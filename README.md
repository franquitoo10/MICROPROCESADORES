## MICROPROCESADORES


GRUPO: JHON FERNANDO FRANCO CALVO, SERGIO STIVEN CAICEDO GUIZA, DAVID FERNANDO SIERRA ESTUPIÑAN

# CODIGO

```javascript
#include <xc.h>
#define _XTAL_FREQ 4000000 

void CONF_PUERTOS(void);
void BORRAR_BASURA(void);
void RUTINA (void);
void LEER_OPERANDO_1(void);
void LEER_OPERANDO_2(void);
void LEER_OPERACION(void);
void FUNCION_ENTER(void);
void F_S(void);
void F_R(void);
void F_A(void);
void F_O(void);
void F_M(void);
void F_D(void);
void MOSTRAR_RESULTADO(void);


unsigned char OPERANDO_1;
unsigned char OPERANDO_2;
unsigned char selector;
unsigned char RESULTADO;


void main(void) 
{
    CONF_PUERTOS();
    BORRAR_BASURA();
    RUTINA();    
}


void CONF_PUERTOS(void)
{
            //76543210    
    TRISB = 0b00011111;
    
            //76543210
    TRISC = 0b00000000;
    
            //76543210    
    TRISD = 0b00000000;
    
    ANSELB =0b00000000; 
}

void BORRAR_BASURA(void)
{
   LATC = LATD = 0; 
   OPERANDO_1 = OPERANDO_2 = selector = RESULTADO = 0;
}

void RUTINA(void)
{
  FUNCION_ENTER();  
  LEER_OPERANDO_1();
  
  FUNCION_ENTER();
  LEER_OPERACION();
  
  FUNCION_ENTER();
  LEER_OPERANDO_2();  
 
  while(1)
  {
      MOSTRAR_RESULTADO();
  }    
  
  
}

void LEER_OPERANDO_1(void)
{
    OPERANDO_1 = PORTB;
    OPERANDO_1 = OPERANDO_1 & 0b00001111; 
}

void LEER_OPERACION (void)
{        
    selector = PORTB;
    selector = selector & 0b00001111; 
    
    switch(selector)
    {
        case 0:F_S();break;
        case 1:F_R();break;
        case 2:F_A();break;
        case 3:F_O();break;
        case 4:F_M();break;
        case 5:F_D();break;
        default:break;    
        
    }        
}

void LEER_OPERANDO_2(void)
{
    OPERANDO_2 = PORTB;
    OPERANDO_2 = OPERANDO_2 & 0b00001111;
}

void FUNCION_ENTER(void)
{
    while(PORTBbits.RB4 ==1)
    {}
    __delay_ms (50);
    
    while(PORTBbits.RB4 ==  0)
    {}
    __delay_ms (50);
    
}


void F_S(void)
{
    RESULTADO = OPERANDO_1 + OPERANDO_2;      
}
void F_R(void)
{
    RESULTADO = OPERANDO_1 - OPERANDO_2;      
}
void F_A(void)
{
    RESULTADO = OPERANDO_1 & OPERANDO_2;      
}
void F_O(void)
{
    RESULTADO = OPERANDO_1 | OPERANDO_2;        
}
void F_M(void)
{
    RESULTADO = OPERANDO_1 * OPERANDO_2;      
}
void F_D(void)
{
    RESULTADO = OPERANDO_1 / OPERANDO_2;      
}

void MOSTRAR_RESULTADO(void)
{
 LATC = selector;
     
 LATD = 0;
 __delay_ms (250);
 
 LATD = OPERANDO_1;
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250);
         
  LATD = 0;
 __delay_ms (250);

 
 LATD = OPERANDO_2;
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250); 
 
   LATD = 0;
 __delay_ms (250);
 
  LATD = RESULTADO;
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250);
 __delay_ms (250); 
 
    LATD = 0;
 __delay_ms (250);
 

}
}
```
![image](https://github.com/user-attachments/assets/13a073b5-c9f6-4f70-8631-8109c82c7854)
