# RC High-Pass (Subsonic) Filter PCB

A simple, single-pole (6 dB/oct) RC **high-pass** filter PCB designed in EasyEDA, intended for subwoofer or full-range signal protection by removing unwanted low-frequency content (subsonic).  
Current configuration is tuned for ~33.9 Hz when used with a high-impedance amplifier input.

---

## 📌 Purpose

This board filters out **very low frequencies** that can cause:
- Woofer over-excursion below enclosure tuning (e.g., in bandpass boxes like the Hexibase 7th-order design)
- Wasted amplifier power on inaudible bass
- Distortion and muddiness in low-end playback

Typical application:
- Place between **AVR sub-out** and **power amplifier**.
- Example setup: AVR151 → Sub out → **This filter** → TPA3116D2 amp → Parallel 8 Ω subs (4 Ω total).

---

## ⚙️ How It Works

This is a **1st-order passive RC high-pass filter**:
- **C2** in series with signal  
- **R2** to ground after C2  
- **R1** (bleed resistor) to ground before C2 — prevents DC build-up and input pops.

**Cutoff frequency formula**:
\[
f_c = \frac{1}{2 \pi R_{\text{eff}} C}
\]
Where:
- \(C\) = series capacitor value (C2)
- \(R_{\text{eff}}\) = R2 in parallel with amplifier input impedance

---

## 🔧 Current Build (as per PCB design)

| Component | Value     | Notes                                    |
|-----------|-----------|------------------------------------------|
| R1        | 100 kΩ    | Bleed resistor, 1% metal film, through-hole |
| R2        | 10 kΩ     | Shunt resistor, 1% metal film, through-hole |
| C2        | 0.47 µF   | Film capacitor, ±5%, through-hole        |

**Expected cutoff**: ~33.9 Hz with high-Z amp input (~100 kΩ).  
If amp Rin = 47 kΩ → ~41 Hz.  
If amp Rin = 20 kΩ → ~50.8 Hz.

---

## 🎯 Retuning for Different Frequencies

**Method 1: Keep R2 fixed, change C2**
\[
C2 = \frac{1}{2\pi R2 f_c}
\]
Example (R2 = 10 kΩ):

| Target fc | C2 (µF) |
|-----------|---------|
| 25 Hz     | 0.637   |
| 30 Hz     | 0.531   |
| 40 Hz     | 0.398   |
| 50 Hz     | 0.318   |
| 80 Hz     | 0.199   |

---

**Method 2: Keep C2 fixed, change R2**
\[
R2 = \frac{1}{2\pi C2 f_c}
\]
Example (C2 = 0.47 µF):

| Target fc | R2 (Ω) |
|-----------|--------|
| 25 Hz     | 13.55 kΩ |
| 33 Hz     | 10.26 kΩ |
| 40 Hz     | 8.47 kΩ |
| 50 Hz     | 6.77 kΩ |
| 80 Hz     | 4.23 kΩ |

---

**Method 3: Account for amplifier input impedance**

When the amplifier input impedance ($R_\mathrm{in}$) is not much larger than $R_2$, you must use the parallel combination:

$$
R_\mathrm{eff} = R_2 \parallel R_\mathrm{in} = \frac{R_2 \cdot R_\mathrm{in}}{R_2 + R_\mathrm{in}}
$$

Use $R_\mathrm{eff}$ in the cutoff formula:

$$
f_c = \frac{1}{2\pi\,R_\mathrm{eff}\,C_2}
$$

---

**To find $R_2$ for a target $f_c$ (with fixed $C_2$):**

1. Compute the required effective resistance:

$$
R_\mathrm{eff} = \frac{1}{2\pi\,C_2\,f_c}
$$

2. Solve for the actual shunt resistor:

$$
R_2 = \frac{1}{\frac{1}{R_\mathrm{eff}} - \frac{1}{R_\mathrm{in}}}
$$

---

**Example**

Target: $f_c = 33.9\ \text{Hz}$, $C_2 = 0.47\ \mu\text{F}$, $R_\mathrm{in} = 47\ \text{k}\Omega$

Step 1:

$$
R_\mathrm{eff} = \frac{1}{2\pi \cdot 0.47\times 10^{-6} \cdot 33.9} \approx 10\ \text{k}\Omega
$$

Step 2:

$$
R_2 = \frac{1}{\frac{1}{10\ \text{k}\Omega} - \frac{1}{47\ \text{k}\Omega}} \approx 12.7\ \text{k}\Omega
$$


---

## 🛠️ Build Notes
- Use **film capacitors** for C2 (better stability and sound quality).
- Keep traces short and use a **ground plane** for best noise rejection.
- RCA jacks on input, 2-pin terminal block on output.
- Clearly label `OUT+` and `GND` on silkscreen.
- Recommended trace width: ≥ 0.5 mm for signal, ≥ 1 mm for ground.

---

## 📂 Files
- **EasyEDA project** – Schematic and PCB layout.
- **Gerbers** – For JLCPCB fabrication.
- **BOM** – JLCPCB Basic/Extended parts.

---

## 📷 Example Board Layout
(https://github.com/barnard704344/1st-order-high-pass/blob/main/images/board.png)

---

## 📜 License
This project is open-source under the MIT License — feel free to adapt for your own frequencies.
