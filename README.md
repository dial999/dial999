from machine import Pin, PWM
import time

# ---------------------------
# Configuración Motores (L298N)
# ---------------------------

# Motor A
ENA = PWM(Pin(19), freq=1000)
IN1 = Pin(5, Pin.OUT)
IN2 = Pin(18, Pin.OUT)

# Motor B
ENB = PWM(Pin(25), freq=1000)
IN3 = Pin(27, Pin.OUT)
IN4 = Pin(26, Pin.OUT)

# Motor C

ENC = PWM(Pin(17), freq=1000)
IN5 = Pin(15, Pin.OUT) #IN1-3
IN6 = Pin(2, Pin.OUT) #IN2

# Motor D

END = PWM(Pin(14), freq=1000)
IN7 = Pin(13, Pin.OUT) #IN3-3
IN8 = Pin(21, Pin.OUT) #IN4

def motorA_forward(speed=1000):
    IN1.value(1)
    IN2.value(0)
    ENA.duty(speed)

def motorA_backward(speed=512):
    IN1.value(0)
    IN2.value(1)
    ENA.duty(speed)

def motorB_forward(speed=1000):
    IN3.value(1)
    IN4.value(0)
    ENB.duty(speed)

def motorB_backward(speed=512):
    IN3.value(0)
    IN4.value(1)
    ENB.duty(speed)

def motorC_forward(speed=1000):
    IN5.value(1)
    IN6.value(0)
    ENC.duty(speed)

def motorC_backward(speed=512):
    IN5.value(0)
    IN6.value(1)
    ENC.duty(speed)

def motorD_forward(speed=1000):
    IN7.value(1)
    IN8.value(0)
    END.duty(speed)

def motorD_backward(speed=512):
    IN7.value(0)
    IN8.value(1)
    END.duty(speed)

def motors_stop():
    ENA.duty(0)
    ENB.duty(0)
    ENC.duty(0)
    END.duty(0)
    IN1.value(0)
    IN2.value(0)
    IN3.value(0)
    IN4.value(0)
    IN5.value(0)
    IN6.value(0)
    IN7.value(0)
    IN8.value(0)

# ---------------------------
# Configuración Sensor Ultrasónico HC-SR04
# ---------------------------
TRIG = Pin(16, Pin.OUT)
ECHO = Pin(4, Pin.IN)

def medir_distancia():
    # Generar pulso de 10us en TRIG
    TRIG.value(0)
    time.sleep_us(2)
    TRIG.value(1)
    time.sleep_us(10)
    TRIG.value(0)

    # Esperar respuesta en ECHO
    while ECHO.value() == 0:
        start = time.ticks_us()
    while ECHO.value() == 1:
        end = time.ticks_us()

    duracion = time.ticks_diff(end, start)
    distancia = (duracion / 2) / 29.1  # en cm
    return distancia

# ---------------------------
# Programa Principal
# ---------------------------
while True:
    d = medir_distancia()
    print("Distancia:", d, "cm")

    if d < 40:  # obstáculo cerca
        print("Obstáculo detectado
        +.3666666666666-
        +-
        ")
        #motorA_backward(600)
        #motorB_backward(600)
        #time.sleep(0.5)
        motors_stop()
    else:
        print("Avanzando...")
        motorA_forward(300)
        motorB_forward(300)
        motorC_forward(300)
        motorD_forward(300)

    time.sleep(0.2)
