# FirmWare TFX007

------------

# main.c

```c
/*
 * File:   main.c
 * Author: Luca Fidanza "The Fidax"
 *
 * Created on 12/4/2020 7:54:48 AM UTC
 * "Created in MPLAB Xpress"
 */

#define F_CPU   8000000     // Frequenza di funzionamento
#include <avr/interrupt.h>  // ISR per gli interrupt
#include <util/delay.h>     // per _delay_ms
#include <stdint.h>         // uintxx_t in C

#include "Frequency.h"                  // libreria con i valori di accensione spegimento lanterne
#include "TIMER_ATTINY10_DEFINES.h"     // comandi Timer

/* Tempistiche Timer */
// Durata del 'Tick' = 256 (256 cicli per un 1 tick del timer) / 8000000 = 3,2 x 10^-5 S = 32 * 10^-6 uS
#define TICKS_TIMING            ( 32 )
// Calcola il numero Ticks partendo da un generico tempo in mS: T=XmS = X * 10^-3 -> n' Tick = ( (X * (10^(-3))) / (32 * 10^(-6)) )
// Lavoro SOLO in positivo quindi: n' Tick = ( (X * (10^(3))) / (32 ) )
#define TICKS_GENERIC_MS(x)     ( ((uint32_t)x * 1000 ) / TICKS_TIMING )
#define TICKS_T_ON              TICKS_GENERIC_MS(T_ON)  //( 625 )
#define TICKS_T_OFF             TICKS_GENERIC_MS(T_OFF) //( 20625 ) 

/* Comandi 'rapidi' */
#define LANT1_ON    ( PORTB |= (1 << PORTB0) )
#define LANT1_OFF   ( PORTB &= ~( 1 << PORTB0) )
#define LANT2_ON    ( PORTB |= (1 << PORTB1) )
#define LANT2_OFF   ( PORTB &= ~( 1 << PORTB1) )

/* Prototipi */
uint8_t getRandom(void);

/* Variabili Globali */ 
static uint32_t asyncDelayTicks;
    
/* MAIN */
int main() {    
    CCP = 0xD8;         // abilito la modifica dei registri protetti (dal datasheet)
    CLKPSR = 0b0000;    // imposto il 'prescaler' a 1 : frequenza 8MHz
    
    /* Calcolo un valore 'random' */
    asyncDelayTicks = TICKS_GENERIC_MS(2 * getRandom());

    /* Una volta usato l'ADC per il calcolo del valore 'random' lo disattivo*/
    ADCSRA = 0;
    PRR = (1<<PRADC);
        
    /* SETUP */
    DDRB = 0b0011;
        
    /* Imposto il Timer0 per gestire il lampeggio */
    TIMER_DISABLE_OVERFLOW_INTERRUPT;   // disattivo il timer
    TIMER_RESET_CCONTROLS_REGISTERS;    // reset dei registri di controllo
    TIMER_SET_PRESCALER_256;
    TIMER_ENABLE_OVERFLOW_INTERRUPT;    // attivo l' ISR per Overflow
    sei();                              // abilito gli interrupt globali
    TIMER_SET_COUNTER_REGISTER(MAX_TIMER_COUNTER);       // attivo subito l'ISR
    /* FINE SETUP */
        
    /* LOOP */
    while(1) { 
        /* Non faccio nulla perche' tutto gestito dal Timer */ 
        SMCR = 0b1001;  // Metto il micro in Standby
    }
    /* FINE LOOP */
    
    return 0;
}

typedef enum {      // identifica lo stato del Lampeggio
    lant1_On = 0,   // accensione   Lanterna 1: devo attendere il tempo di accesione
    lant1_Off,      // spegnimento  Lanterna 1: devo attendere il tempo di "sfasamento" prima di accendere la lanterna 2
    lant2_On,       // accensione   Lanterna 2: devo attendere il tempo di accesione
    lant2_Off       // spegnimento  Lanterna 2: devo attendere il tempo di "sfasamento" prima di Riaccendere la lanterna 1
} blinkPhase;


ISR(TIM0_OVF_vect) {
    static blinkPhase nextPhase = lant1_On;
    
    switch(nextPhase) {
        case lant1_On: {
            LANT1_ON;
            nextPhase = lant1_Off;
            
            TIMER_SET_COUNTER_REGISTER((uint16_t)(MAX_TIMER_COUNTER - TICKS_T_ON));
            break;
        }
        case lant1_Off: {
            LANT1_OFF;
            nextPhase = lant2_On;
            
            TIMER_SET_COUNTER_REGISTER((uint16_t)(MAX_TIMER_COUNTER - asyncDelayTicks));
            break;
        }
        case lant2_On: {
            LANT2_ON;
            nextPhase = lant2_Off;
            
            TIMER_SET_COUNTER_REGISTER((uint16_t)(MAX_TIMER_COUNTER - TICKS_T_ON));
            break;
        }
        case lant2_Off: {
            LANT2_OFF;
            nextPhase = lant1_On;
            
            TIMER_SET_COUNTER_REGISTER((uint16_t)(MAX_TIMER_COUNTER - (TICKS_T_OFF - asyncDelayTicks)));
            break;
        }
    }
}

#define  ITERATIONS  10
uint8_t getRandom(void) {
    uint8_t value = 0;  
    
    /*Imposto il ADC per ricavare il valore casuale di Ritardo*/
    ADMUX = 0b00000010;             // ADC2 (PB2)
    ADCSRA = 1<<ADEN | 1<<ADPS0; 

    for(uint8_t i = 0; i < ITERATIONS; ++i){
        int temp;
        ADCSRA = ADCSRA | 1<<ADSC;      // Start
        while (ADCSRA & 1<<ADSC);       // Wait while conversion in progress
        temp = ADCL;                    // ADCL = registro in cui l'ADC salva la conversione, ADC a 8bit su ATtiny10

        value += ( (value ^ temp) / (value % ITERATIONS) );
        
        _delay_ms(5);    
    }
    
    return value;
}
```

------------

# Frequency.h

```c
/* 
 * File:   Frequency.h
 * Author: lfidanza997@gmail.com
 *
 * Created on 4/7/2021 5:00:26 PM UTC
 * "Created in MPLAB Xpress"
 *
 *
 *  33  x   1/50 spento = 0,66S = 660mS
 *  1   x   1/50 accesa = 0,02S = 20mS
 *
 *  1000mS = 1S
 *
 */

#ifndef FREQUENCY_H
#define	FREQUENCY_H

//i  seguenti valori identificano il Timing della singola lanterna:
#define T_OFF       660     //(ms) = tempo in cui la lanterna e' 'SPENTA'
#define T_ON        20      //(ms) = tempo in cui la lanterna e' 'ACCESA'

#define ASYNC_DELAY 160

#endif	/* FREQUENCY_H */

```

------------

# TIMER_ATTINY10_DEFINES.h

```c
#ifndef TIMER_ATTINY10_DEFINES_h
#define TIMER_ATTINY10_DEFINES_h

#if defined(__AVR_ATtiny10__) || defined(__AVR_ATtiny9__) || defined(__AVR_ATtiny5__) || defined(__AVR_ATtiny4__)

#define MAX_TIMER_COUNTER ( 65536 )

// DEFINE che controllano il Timer0
      /*
      * Esegue un reset dei registri del Counter del Timer
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_RESET_COUNTER_REGISTER       ( TCNT0H = TCNT0L = 0 )
      /*
      * Imposta un valore nei registri del Counter del Timer
      * Input:
      *   - il valore da impostare
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_COUNTER_REGISTER(x)       TCNT0H = (((int)x)>>8); TCNT0L = (((int)x) & 0xFF)
      /*
      * Imposta un valore nei registri del Output Compare Register del Timer
      * Input:
      *   - il valore da impostare
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_OUTPUT_COMPARE_REGISTER(x)       OCR0AH = (((int)x)>>8); OCR0AL = (((int)x) & 0xFF)
      /*
      * Esegue un reset dei registri di controllo del Timer
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_RESET_CCONTROLS_REGISTERS    ( TCCR0A = TCCR0B = 0x00 )
      /*
      * Esegue un reset del Prescaler del Timer3 con conseguenza fermata del Timer
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_RESET_PRESCALER              ( TCCR0B &= ~( (1 << CS00) | (1 << CS01) | (1 << CS02) ) )
      /*
      * Imposta il Prescaler del Timer a 1
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_PRESCALER_1              ( TCCR0B |= (1 << CS00) )
      /*
      * Imposta il Prescaler del Timer a 8
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_PRESCALER_8              ( TCCR0B |= (1 << CS01) )
      /*
      * Imposta il Prescaler del Timer a 64
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_PRESCALER_64              ( TCCR0B |= (1 << CS00) | (1 << CS01) )
      /*
      * Imposta il Prescaler del Timer a 256
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_PRESCALER_256             ( TCCR0B = (1 << CS02) )
      /*
      * Imposta il Prescaler del Timer a 1024
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_SET_PRESCALER_1024            ( TCCR0B |= (1 << CS00) | (1 << CS02) )
      /*
      * Enable Timer/Counter, Overflow Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_ENABLE_OVERFLOW_INTERRUPT     ( TIMSK0 = (1 << TOIE0) )
      /*
      * Disable Timer/Counter, Overflow Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_DISABLE_OVERFLOW_INTERRUPT   ( TIMSK0 &= ~( (1 << TOIE0) ) )
      /*
      * Enable Timer/Counter, Output Compare B Match Interrupt Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_ENABLE_OUTPUT_COMPARE_A_INTERRUPT     TCCR0A = (1<<COM0A0); ( TIMSK0 = (1 << OCIE0A) )
      /*
      * Disable Timer/Counter, Output Compare B Match Interrupt Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_DISABLE_OUTPUT_COMPARE_A_INTERRUPT     TCCR0A &= ~( (1 << COM0A0) ); ( TIMSK0 &= ~( (1 << OCIE0A) ) )
      /*
      * Enable Timer/Counter, Output Compare B Match Interrupt Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_ENABLE_OUTPUT_COMPARE_B_INTERRUPT     TCCR0B = (1<<COM0B0); ( TIMSK0 = (1 << OCIE0B) )
      /*
      * Disable Timer/Counter, Output Compare B Match Interrupt Interrupt 
      * Input:
      *   - Nulla
      * Restituisce:
      *   - Nulla
      */
#define TIMER_DISABLE_OUTPUT_COMPARE_B_INTERRUPT     TCCR0B &= ~( (1 << COM0B0) ); ( TIMSK0 &= ~( (1 << OCIE0B) ) )

#else

#error "Compatibile SOLO CON ATTINY10"

#endif

#endif
```
