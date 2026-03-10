---
icon: 📐
title: Render-Test
version: 
date: 2026-03-10
tags: [test, markdown, latex, code]
badge: 
---

# Render-Test — Markdown, LaTeX & Code

Dieses File testet alle Rendering-Features von lab.notes.

---

## 1. Markdown Basics

**Fett**, *kursiv*, ~~durchgestrichen~~, `inline code`

> Das ist ein Blockquote. Gut für Hinweise oder Zitate.

Normaler Absatz mit einem [Link zu Google](https://google.com) drin.

### Listen

Ungeordnet:
- Widerstand R₁ = 10 kΩ
- Kondensator C₁ = 100 nF
- MOSFET: IRFZ44N

Geordnet:
1. Schaltung aufbauen
2. Spannung messen
3. Gate-Treiber kalibrieren

### Tabelle

| Bauteil | Wert | Gehäuse | Notiz |
|---------|------|---------|-------|
| R1 | 10 kΩ | 0603 | Pull-up |
| C1 | 100 nF | 0402 | Bypass |
| Q1 | IRFZ44N | TO-220 | N-Ch MOSFET |
| U1 | IR2110 | DIP-14 | Gate Treiber |

---

## 2. LaTeX — Inline

Das Ohmsche Gesetz: $V = I \cdot R$

Leistung: $P = \frac{V^2}{R} = I^2 \cdot R$

Zeitkonstante RC-Glied: $\tau = R \cdot C$

Resonanzfrequenz: $f_0 = \frac{1}{2\pi\sqrt{LC}}$

---

## 3. LaTeX — Block (Display Mode)

Kirchhoffs Spannungsgesetz:

$$\sum_{k=1}^{n} V_k = 0$$

Übertragungsfunktion eines Tiefpasses 1. Ordnung:

$$H(j\omega) = \frac{1}{1 + j\omega RC}$$

Maxwell-Gleichung (Faraday):

$$\nabla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}$$

Fourier-Transformation:

$$X(f) = \int_{-\infty}^{\infty} x(t)\, e^{-j2\pi ft}\, dt$$

---

## 4. Code — C (STM32 / Embedded)

```c
#include "stm32f4xx_hal.h"

#define PWM_FREQ   20000   // 20 kHz
#define DEAD_TIME  75      // ns

void ESC_Init(TIM_HandleTypeDef *htim) {
    HAL_TIM_PWM_Start(htim, TIM_CHANNEL_1);
    HAL_TIMEx_PWMN_Start(htim, TIM_CHANNEL_1);
}

void ESC_SetDutyCycle(TIM_HandleTypeDef *htim, uint8_t duty_pct) {
    uint32_t arr = htim->Instance->ARR;
    uint32_t ccr = (arr * duty_pct) / 100;
    __HAL_TIM_SET_COMPARE(htim, TIM_CHANNEL_1, ccr);
}

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_TIM1_Init();
    ESC_Init(&htim1);

    while (1) {
        for (uint8_t d = 0; d <= 100; d += 5) {
            ESC_SetDutyCycle(&htim1, d);
            HAL_Delay(100);
        }
    }
}
```

## 5. Code — C++ (Arduino-Style)

```cpp
#include <Arduino.h>

class PIDController {
private:
    float kp, ki, kd;
    float integral = 0.0f;
    float prev_error = 0.0f;

public:
    PIDController(float p, float i, float d)
        : kp(p), ki(i), kd(d) {}

    float compute(float setpoint, float measured, float dt) {
        float error = setpoint - measured;
        integral += error * dt;
        float derivative = (error - prev_error) / dt;
        prev_error = error;
        return kp * error + ki * integral + kd * derivative;
    }
};

PIDController pid(1.2f, 0.05f, 0.01f);

void loop() {
    float output = pid.compute(100.0f, analogRead(A0), 0.01f);
    analogWrite(9, constrain(output, 0, 255));
    delay(10);
}
```

## 6. Code — Python (Messdaten)

```python
import numpy as np
import matplotlib.pyplot as plt

def bode_plot(R: float, C: float, f_range=(10, 1e6)):
    """Bode-Plot für RC-Tiefpass 1. Ordnung"""
    f = np.logspace(np.log10(f_range[0]), np.log10(f_range[1]), 500)
    omega = 2 * np.pi * f
    H = 1 / (1 + 1j * omega * R * C)

    magnitude_db = 20 * np.log10(np.abs(H))
    phase_deg = np.degrees(np.angle(H))
    f_cutoff = 1 / (2 * np.pi * R * C)

    print(f"Grenzfrequenz: {f_cutoff:.1f} Hz")

    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(8, 6))
    ax1.semilogx(f, magnitude_db)
    ax1.set_ylabel("Betrag [dB]")
    ax1.grid(True, which="both")
    ax2.semilogx(f, phase_deg)
    ax2.set_ylabel("Phase [°]")
    ax2.set_xlabel("Frequenz [Hz]")
    ax2.grid(True, which="both")
    plt.tight_layout()
    plt.show()

# RC-Tiefpass: R=10kΩ, C=100nF → f_c ≈ 159 Hz
bode_plot(R=10e3, C=100e-9)
```

---

## 7. Kombiniert — LaTeX + Text

Der Bootstrap-Kondensator $C_{boot}$ beim IR2110 muss folgende Bedingung erfüllen:

$$C_{boot} \gg \frac{Q_g}{V_{CC} - V_f - V_{DS(on)}}$$

Mit typischen Werten ($Q_g = 110\,\text{nC}$, $V_{CC} = 15\,\text{V}$, $V_f = 0{,}7\,\text{V}$):

$$C_{boot} \gg \frac{110 \times 10^{-9}}{15 - 0{,}7 - 0{,}1} \approx 7{,}8\,\text{nF}$$

Gewählt: $C_{boot} = 220\,\text{nF}$ (Faktor ~28 Reserve). ✓

---

*Render-Test abgeschlossen. Wenn du das siehst: alles funktioniert.*