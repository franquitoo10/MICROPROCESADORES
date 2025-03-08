# MICROPROCESADORES
## PIC18F45K22

GRUPO: JHON FERNANDO FRANCO CALVO, SERGIO STIVEN CAICEDO GUIZA, DAVID FERNANDO SIERRA ESTUPIÑAN

## CODIGO

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
## DESCRICION DEL CODIGO

El programa inicia su ejecución en la función main(), donde se configuran los puertos y se llama a la función RUTINA(). La configuración de los puertos establece el puerto B como entrada y los puertos C y D como salidas. Esto permite recibir datos del usuario y mostrar el resultado en los LED o displays conectados.

El usuario ingresa los operandos y la operación mediante botones o un teclado matricial, confirmando cada entrada con una pulsación en el botón de "Enter" (PORTBbits.RB4). Una vez capturados los valores, el programa ejecuta la operación seleccionada utilizando una estructura switch-case.

Las funciones de operación (F_S(), F_R(), F_A(), F_O(), F_M(), F_D()) aplican sumas, restas, AND, OR, multiplicación y división sobre los operandos. Finalmente, la función MOSTRAR_RESULTADO() despliega el selector de la operación, los operandos y el resultado en el puerto D con retardos de 250 ms para permitir una correcta visualización secuencial.


El código es eficiente y estructurado, permitiendo la manipulación de datos con una interfaz sencilla basada en botones o switches. Su implementación en un microcontrolador facilita su integración en aplicaciones como calculadoras básicas o sistemas de control digital con operaciones lógicas.

 Descripción General del Código
El código desarrollado en lenguaje C está diseñado para ejecutarse en un microcontrolador PIC utilizando el compilador XC8. Su propósito es realizar operaciones matemáticas y lógicas entre dos operandos ingresados a través del puerto B y mostrar los resultados en los puertos C y D.

El programa se compone de varias funciones encargadas de la configuración de los puertos, la captura de datos de entrada y la ejecución de operaciones aritméticas y lógicas. Al inicio, se configuran los registros de entrada y salida a través de la función CONF_PUERTOS(), y se inicializan las variables a cero con BORRAR_BASURA(). Luego, en la función RUTINA(), se ejecuta un flujo que permite al usuario ingresar los operandos y la operación deseada.

La lectura de los operandos se realiza a través de LEER_OPERANDO_1() y LEER_OPERANDO_2(), que capturan los valores de 4 bits del puerto B. La operación a ejecutar se obtiene con LEER_OPERACION(), que evalúa la entrada y llama a la función correspondiente (F_S(), F_R(), F_A(), F_O(), F_M(), F_D()). Posteriormente, la función MOSTRAR_RESULTADO() presenta los datos en el puerto D en un ciclo continuo.

El código utiliza retardos (__delay_ms(50)) para evitar rebotes en la entrada y garantizar una lectura estable. Finalmente, el microcontrolador realiza la operación seleccionada y muestra los resultados a través de los puertos de salida.



## IMAGEN DEL MONTAJE
![image](https://github.com/user-attachments/assets/24a18bce-ecea-4d8c-bac7-90a500869bc1)

## DESCRIPCION DE EL MONTAJE

Cómo primera parte por medio del dipswitch y el pulsador se envían los datos de la calculadora ALU tanto los dos valores de la operación y el operador es decir en el dipswitch se ingresa el primer valor y luego se oprime el pulsador para que así esté valor quede guardado luego se ingresa el tipo de operador por en dipswitch y se oprime nuevamente el pulsador para que se guarde y por último se ingresa el valor dos y se oprime nuevamente el pulsador para que se guarde.

cada vez que se realiza una de las acciones mencionadas anteriormente estás se van a ver reflejadas en los leds es decir en los 8 leds de la izquierda se mostrará el primer valor ingresado este se muestra por unos segundos dependiendo del programa para después dar paso a la muestra del segundo valor en cambio con los leds del lado derecho en estos siempre se va a estar registrado el tipo de operador que se eligió durante la ejecución del primer calculo y el resultado de este cálculo se muestra en los leds del lado izquierdo como última instancia

### LA PARTE DE IMAGEN DEL MONTAJE Y DESCRIPCION DEL MONTAJE LA REALIZO DAVID SIERRA SOLO QUE POR PROBLEMAS CON SU COMPUTADOR NO PUDO SUBIR NADA 









