# pwm-led-control



## Getting started
python3 pwmLed.py

## PWM - LED Control

### 1) Selected PIN Number : 2 (GPIO 115)

<img src="images/pinMap.png" alt="alt text" width="600" height="400">

<img src="images/raspPinMap.png" alt="alt text" width="600" height="400">

### 2) Use Breadboard to Connect LED to Board PIN

<img src="images/pwmLed.png" alt="alt text" width="600" height="500">

## Code

```
import time

PWM_CHIP_PATH = '/sys/class/pwm/pwmchip0/'
PWM_PIN = 2

def enable_pwm(pwm_pin):
    with open(PWM_CHIP_PATH + 'export', 'w') as export_file:
        export_file.write(str(pwm_pin))
        
def disable_pwm(pwm_pin):
    with open(PWM_CHIP_PATH + 'unexport', 'w') as unexport_file:
        unexport_file.write(str(pwm_pin))

def set_pwm_period(pwm_pin, period_ns):
    with open(PWM_CHIP_PATH + f'pwm{pwm_pin}/period', 'w') as period_file:
        period_file.write(str(period_ns))

def set_pwm_duty_cycle(pwm_pin, duty_cycle_ns):
    with open(PWM_CHIP_PATH + f'pwm{pwm_pin}/duty_cycle', 'w') as duty_cycle_file:
        duty_cycle_file.write(str(duty_cycle_ns))

def enable_pwm_output(pwm_pin):
    with open(PWM_CHIP_PATH + f'pwm{pwm_pin}/enable', 'w') as enable_file:
        enable_file.write('1')

def disable_pwm_output(pwm_pin):
    with open(PWM_CHIP_PATH + f'pwm{pwm_pin}/enable', 'w') as enable_file:
        enable_file.write('0')

if __name__ == '__main__':
    try:       
        # Enable
        enable_pwm(PWM_PIN)

        # 주기를 1초로 설정하여 1Hz 주파수로 깜빡이기
        set_pwm_period(PWM_PIN, 1000000000)  # 1초 period (1Hz frequency)
        set_pwm_duty_cycle(PWM_PIN, 500000000)  # 0.5초 duty cycle (50% duty cycle)
        enable_pwm_output(PWM_PIN)

        while True:
            print("\nSleep")
            time.sleep(1)

    except KeyboardInterrupt:
        disable_pwm_output(PWM_PIN)
        disable_pwm(PWM_PIN)
        print("\nPWM control stopped.")
```

## Result
![Alt text](images/pwmLed.mp4)