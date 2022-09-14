#include<16f877.h>

#device ADC=10;

#fuses XT,NOWDT,NOPROTECT,NOBROWNOUT,NOLVP,NOPUT,NOWRT,NODEBUG,NOCPD

#use delay(clock=4000000)

#use fast_io(a)
#use fast_io(b)
#use fast_io(c)

#use rs232 (baud=9600, xmit=pin_c6, rcv=pin_c7, parity=N, stop=1, stream=bluetooth)

int deger;
float gerilim,sicaklik;

void main(){

setup_psp(PSP_DISABLED);
setup_timer_1(T1_DISABLED);
setup_ccp2(CCP_OFF);

setup_timer_2(T2_DIV_BY_16,255,1);

setup_adc_ports(ALL_ANALOG);
setup_adc(adc_clock_div_32);
set_adc_channel(0);
delay_us(20);

setup_ccp1(CCP_PWM);

set_tris_a(0x01);
set_tris_b(0x00);
set_tris_c(0b11000000);

output_b(0x00);



while(true){

deger=read_adc();

gerilim=(deger*0.0048828125)*1000; //analog deðerin mV cinsinden gerilim karþýlýðý hesaplatýlýyor

sicaklik=(gerilim/10)+2; //LM35 her 10mV'ta 1 derece sýcaklýk artýþý saðladýðý için bu iþlem yaptýrýlýyor. +2'nin nedeni kullandýðýmýz sensörde 2 derece 0mV'a tekabül etmektedir.


if(sicaklik >= 0 && sicaklik <= 15){

  printf("Sicaklik = %.1f C\r",sicaklik);
  printf("Hava Soguk\r");
  delay_ms(500);
  
  output_high(pin_b0);
  output_low(pin_b3);
  output_low(pin_b6);
  
  set_pwm1_duty(deger);
  
}

if(sicaklik >= 16 && sicaklik <= 25){

  printf("Sicaklik = %.1f C\r",sicaklik);
  printf("Hava Ilik\r");
  delay_ms(500);
  
  output_high(pin_b6);
  output_low(pin_b0);
  output_low(pin_b3);
  
  set_pwm1_duty(deger);

}

if(sicaklik >= 26){

  printf("Sicaklik = %.1f C\r",sicaklik);
  printf("Hava Sicak\r");
  delay_ms(500);
  
  output_high(pin_b3);
  output_low(pin_b6);
  output_low(pin_b0);
  
  set_pwm1_duty(deger);
}
 
}

}
