📘 Simulink Analog Circuits Lab — Detailed README ⚡️🔧

Welcome! This repo contains hands-on Simulink simulations of several foundational analog circuits I built and analyzed: Half-Wave Rectifier, Full-Wave Rectifier, Audio Modulation Circuit, Non-Inverting Amplifier, Inverting Amplifier, Passive & Active Low-Pass Filters, and Passive & Active High-Pass Filters. Each model is designed to demonstrate real behavior, waveform shapes, frequency response, and practical effects (diode drop, op-amp limitations, loading, etc.). Dive in — lots of plots, insights, and tips below! 🚀👨‍🔬

🔎 Project Summary

Simulated typical analog building blocks in MATLAB Simulink to study time-domain waveforms, frequency response, distortion, and practical nonidealities. Emphasis on:

Realistic component behavior (diode drop, op amp rails, source/load impedances) 🔋

Measurement of peak/rms values, ripple, gain, phase shift 📈

Bode plots for active/passive filters and amplifier stability considerations 🔁

Reproducible Simulink models and clear instructions to run/modify them 🧩

🧩 Files & Structure
/simulink-analog-circuits/
├─ half_wave_rectifier.slx
├─ full_wave_rectifier.slx
├─ audio_modulation.slx
├─ noninverting_amplifier.slx
├─ inverting_amplifier.slx
├─ passive_lowpass.slx
├─ active_lowpass.slx
├─ passive_highpass.slx
├─ active_highpass.slx
├─ README.md   <-- (this file)
├─ docs/       <-- screenshots, plots, notes
└─ models/     <-- subsystem blocks if any

⚙️ How I Simulated — General Notes

Simulink blocks used: Sine Wave (sources), Voltage Measurement / Scope, Transfer Function/Filter blocks, Diode (Simscape or ideal diode), Op-Amp modeled using an ideal op amp block or an Op Amp with finite gain/rail limits (for realism) 🧱

Solver: Fixed-step (for deterministic sampling) or variable-step with sufficiently small tolerances for accuracy (e.g., ode45/ode23tb); sample time set per model depending on highest frequency component. ⏱️

Measurements: Use RMS, Peak, FFT (Spectrum Analyzer), and Bode Plot blocks to extract performance metrics. Save simulation logs for reproducibility. 📊

Nonidealities: Included diode forward voltage (~0.7V for silicon), op-amp supply rails, input bias when needed, source impedance and load impedance. These show real behavior and limitations. ⚠️

🚦 Circuit-by-Circuit Breakdown
1) Half-Wave Rectifier 🧯

Purpose: Convert AC → pulsating DC; teach diode conduction and ripple basics.
Schematic overview: AC source → diode → load resistor → smoothing capacitor (optional).
Key simulation points:

Model the diode with forward threshold (~0.7 V). During positive half-cycle diode conducts; negative half blocked → output zero (or capacitor holds voltage if present).

With capacitor present: measure ripple (ΔV), approximate ripple ≈ I_load / (f * C) for full-wave? (For half-wave, ripple frequency = input frequency so ripple is larger).

Observe charging spikes and diode conduction intervals; measure diode conduction angle if needed.
What to look for in plots:

Input sine vs. output waveform (show clipping to zero).

With smoothing capacitor: envelope with exponential discharge between peaks.
Tips: Increase C to reduce ripple, add series resistor to limit inrush charging current.

2) Full-Wave Rectifier 🔁

Purpose: Improved DC conversion — doubles ripple frequency vs half-wave.
Schematic overview: Bridge rectifier (4 diodes) or center-tap + 2 diodes → load.
Key simulation points:

Use four-diode bridge: during both half-cycles, load sees positive polarity → ripple frequency = 2× input frequency.

Include transformer model or source amplitude appropriate for diode drops.

Measure average DC, peak-to-peak ripple, and diode losses.
What to look for:

Output pulsating DC at double frequency; smoother for same capacitor value vs half-wave.
Tips: Model real diode series resistance to reveal voltage drop at high currents.

3) Audio Modulation Circuit 🎛️🎙️

Purpose: Demonstrate amplitude modulation (AM) basics — carrier × message; observe envelope demodulation.
Schematic overview: Message (low-freq sine) and Carrier (high-freq sine) → multiplier/mixer → AM output; include envelope detector (diode + RC) for demodulation.
Key simulation points:

Use Simulink Product block or dedicated mixer to multiply signals for true AM.

Vary modulation index m (0 ≤ m ≤ 1 recommended) to show under/over modulation.

Demodulate using envelope detector: diode + large R‖C to follow envelope but filter out carrier.
What to view:

Time-domain: carrier with modulated amplitude; zoom to show envelope.

Spectrum: observe carrier ± sidebands (message frequency content mirrored around carrier).
Tips: Avoid over-modulation (m > 1) unless intentional — produces distortion and clipping of the envelope.

4) Non-Inverting Amplifier (Op-Amp) ➕

Purpose: Learn high input impedance amplifier topology; measure voltage gain and bandwidth.
Schematic overview: Op-amp: input to non-inverting (+), feedback network R1 (to ground) and R2 (from output to -) sets gain Av = 1 + R2/R1.
Key simulation points:

Simulate with an op-amp block; set finite GBW to see bandwidth limitations (gain-bandwidth tradeoff).

Include power rails (±Vcc) to observe output saturation/clipping.
What to measure:

DC gain and frequency response (Bode) to find -3 dB bandwidth.

Output slew rate and distortion if input amplitude high.
Tips: Choose R values to keep source loading minimal; low R values increase current draw.

5) Inverting Amplifier ➖

Purpose: Phase inversion, controlled gain with virtual ground at negative input.
Schematic overview: Input → R_in → (-) op-amp input; feedback R_f from output to (-); (+) tied to ground. Av = - R_f / R_in.
Key simulation points:

Study input impedance (≈ R_in), phase shift 180°, and same GBW considerations.

Test with sinusoidal inputs at various amplitudes to verify linear region (no clipping).
What to record:

Gain magnitude and phase vs frequency; confirm DC gain sign flip.
Tips: Use proper grounding and consider input protection in real circuits.

6) Passive Low-Pass Filter (RC) 🧷⬇️

Purpose: Simple frequency-selective network; observe corner frequency and attenuation slope.
Schematic overview: Series resistor R, shunt capacitor C (1st-order). Cutoff fc = 1/(2πRC).
Key simulation points:

Plot magnitude and phase (Bode). Expect -20 dB/decade roll-off after fc and -45° phase shift near fc.

Time-domain: step response shows charging exponential; sinusoidal inputs show amplitude reduction above fc.
What to vary:

R and C to shift fc; load impedance effects if not high compared to R.
Tips: Passive filters do not provide gain >1; use buffer op-amp for isolation.

7) Active Low-Pass Filter (Op-Amp) 🧠⬇️

Purpose: Gain + filtering; higher order designs (Sallen-Key, multiple feedback) for sharper roll-off and controlled Q.
Schematic overview: Sallen-Key or multiple-feedback topology using op-amp and passive R/C network. Choose Butterworth/Chebyshev designs for flatness or steeper passband.
Key simulation points:

Design for desired fc and Q. Observe peaking if Q > 0.707.

Measure passband gain, -3 dB cutoff, and phase response.
What to inspect:

Stability and transient ringing for high Q; step response exhibits overshoot if Q high.
Tips: Use unity-gain buffer for Sallen-Key when you want no gain but isolation. For gain + filter, MFB topology is compact.

8) Passive High-Pass Filter (RC) ⬆️🧷

Purpose: Block low frequencies, pass high frequencies; useful for coupling and noise removal.
Schematic overview: Series capacitor C, shunt resistor R → cutoff fc = 1/(2πRC).
Key simulation points:

At low f → output attenuated; above fc → passes with ~0 dB (for simple first-order).

Check phase shift (approaches +90° at high f relative to low f).
What to test:

Step response shows differentiation behavior for fast edges.
Tips: Beware of DC blocking if you need DC path — add bias network.

9) Active High-Pass Filter (Op-Amp) 🧠⬆️

Purpose: High-pass response with selectable gain and steeper roll-off (higher order).
Schematic overview: Op-amp based topologies (MFB high-pass or Sallen-Key high-pass).
Key simulation points:

Design fc and Q; analyze transient and frequency responses.

Look at noise performance and input bias for coupling networks.
Tips: Active topologies allow buffering and precise gain at passband.

📐 Design Equations & Quick References

RC cutoff: fc = 1 / (2πRC)

Non-inverting gain: Av = 1 + R2/R1

Inverting gain: Av = -R_f / R_in

Ripple (approx, capacitor smoothing): ΔV ≈ I_load / (f_ripple * C) where f_ripple = input freq for half-wave, 2× for full-wave.

For filters, 1st-order slope = -20 dB/decade; each additional pole adds -20 dB/decade.

🧪 Simulation Tips & Best Practices

Start small: verify a block (diode/op-amp) behavior before combining complex subsystems.

Time resolution: sample sufficiently — at least 10–20 points per cycle at highest frequency of interest.

Use scopes & logging: add scopes for time-domain and Spectrum Analyzer/Bode Analyzer for frequency domain.

Parameter sweeps: vary R, C, op-amp gain to see sensitivity and real-world tolerances.

Add measurement blocks: use RMS, mean, and FFT blocks to compute numeric metrics programmatically.

Nonideal op amp: set finite open-loop gain and bandwidth to see realistic roll-off and saturation.

✅ Results & What I Learned

Rectifiers: Bridge rectifier reduces ripple significantly versus half-wave for the same capacitor. Diode forward voltage and series resistance shape conduction spikes and reduce DC.

AM circuit: Multiplying signals produces clear sidebands in the spectrum — envelope detector recovers baseband when modulation index is within limits.

Amplifiers: Real op-amps show finite bandwidth — high closed-loop gain reduces bandwidth (GBW tradeoff). Power rails limit maximum undistorted output.

Filters: Passive filters are simple but load-sensitive; active filters allow gain and sharper responses but need stability/Q considerations.

▶️ How to Run Models

Open the .slx file in MATLAB/Simulink.

Inspect model parameters (source amplitude, frequency, R/C values, op-amp model settings).

Run simulation. Use Simulink Scope or To Workspace to export signals.

For frequency response, use Bode Plot or run a swept-sine and compute FFT of output / input ratio.

Modify component values and re-run to study effects (e.g., change C to see ripple change).

📸 Visuals & Documentation

See /docs/ for screenshots, scope captures, Bode plots, and parameter tables for each circuit. Each doc contains:

Component values used

Plots (time-domain, FFT/Bode)

Observations & numerical metrics (gain, fc, ripple, THD where measured)

💡 Future Improvements (Ideas)

Add temperature-dependent diode models and real op-amp SPICE models for higher fidelity. 🌡️

Implement higher-order active filters (2nd/4th order) and compare Butterworth vs Chebyshev responses. 📚

Add Monte Carlo tolerance analysis to see how component tolerances affect fc and gain. 🎲

Build a small GUI in Simulink to tweak R/C and view real-time responses. 🛠️
