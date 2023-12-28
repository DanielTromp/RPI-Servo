from machine import Pin, PWM, ADC
from time import sleep

def stel_servo_in(pin_nummer):
    servo = PWM(Pin(pin_nummer))
    servo.freq(50)
    return servo

def stel_potentiometer_in(pin_nummer):
    return ADC(pin_nummer)

def waarde_mapperen(waarde, in_min, in_max, uit_min, uit_max):
    return (waarde - in_min) * (uit_max - uit_min) / (in_max - in_min) + uit_min

def afronden_naar_stap(waarde, stap_grootte):
    return round(waarde / stap_grootte) * stap_grootte

# Initialiseer servo's en potentiometers
koppeling_servo = stel_servo_in(28)
gas_servo = stel_servo_in(26)
koppeling_potentiometer = stel_potentiometer_in(PinnummerVoorKoppelingPot)  # Vervang door het juiste pinnummer
gas_potentiometer = stel_potentiometer_in(PinnummerVoorGasPot)  # Vervang door het juiste pinnummer

# Constanten voor PWM, servo hoeken en stapgrootte
PWM_MIN, PWM_MAX = 0, 65535
SERVO_MIN, SERVO_MAX = 1000, 9000
STAP_GROOTTE = 128

laatste_koppeling_positie = 0
laatste_gas_positie = 0

while True:
    # Lees en verwerk koppeling potentiometer
    koppeling_waarde = koppeling_potentiometer.read_u16()
    koppeling_gemapt = waarde_mapperen(koppeling_waarde, PWM_MIN, PWM_MAX, SERVO_MIN, SERVO_MAX)
    koppeling_afgerond = afronden_naar_stap(koppeling_gemapt, STAP_GROOTTE)

    if koppeling_afgerond != laatste_koppeling_positie:
        koppeling_servo.duty_u16(int(koppeling_afgerond))
        laatste_koppeling_positie = koppeling_afgerond

    # Lees en verwerk gas potentiometer
    gas_waarde = gas_potentiometer.read_u16()
    gas_gemapt = waarde_mapperen(gas_waarde, PWM_MIN, PWM_MAX, SERVO_MIN, SERVO_MAX)
    gas_afgerond = afronden_naar_stap(gas_gemapt, STAP_GROOTTE)

    if gas_afgerond != laatste_gas_positie:
        gas_servo.duty_u16(int(gas_afgerond))
        laatste_gas_positie = gas_afgerond

    sleep(0.1)
