# EXP.NO.11-Simulation-of-Spread-Spectrum-Modulation-Techniques

11.Simulation of Spread Spectrum Modulation Techniques
Simulation of Spread Spectrum Modulation Techniques

## AIM
To simulate and analyze various Spread Spectrum Modulation techniques such as Direct Sequence Spread Spectrum (DSSS) and Frequency Hopping Spread Spectrum (FHSS), and to observe their performance in terms of signal spreading and bandwidth.


## SOFTWARE REQUIRED
Google Colab (online Python environment with zero installation)

Python Libraries:

NumPy – for signal and PN sequence generation

Matplotlib – for plotting and visualization


## ALGORITHMS
```
FOR (DSSS) Direct Sequence Spread Spectrum: 
1. Generate a digital baseband signal (binary data stream).
2. Create a pseudo-noise (PN) sequence with a higher chip rate than the data rate.
3. Spread the signal by performing element-wise multiplication of the baseband signal with the PN sequence.
4. Transmit the spread spectrum signal over the channel.
5. At the receiver, apply the same synchronized PN sequence to the received signal for despreading.
6. Integrate the despread signal over each bit period and make a decision based on a threshold to recover the original data.
FOR (FHSS)Frequency Hopping Spread Spectrum:
1. Generate a binary data stream as the baseband input signal.
2. Define a set of carrier frequencies for frequency hopping.
3. Use a pseudo-random sequence to determine the hopping pattern.
4. Modulate each data symbol using the carrier frequency selected by the PN sequence at that time slot.
5. Transmit the frequency-hopped signal through the communication channel.
6. At the receiver, synchronize with the transmitter’s hopping pattern using the same PN sequence.
7. Demodulate the received signal and reconstruct the original data stream.
```
## PROGRAM
```
import numpy as np
import matplotlib.pyplot as plt

# System Parameters
data_length = 4                # Number of bits
chips_per_bit = 8              # PN sequence length
bit_rate = 1e3                 # 1 kbps
chip_rate = bit_rate * chips_per_bit
carrier_freq = 20e3            # 20 kHz carrier
sample_rate = 160e3            # 160 kHz sampling rate
samples_per_chip = int(sample_rate / chip_rate)

# Generate random binary data
def generate_data(length):
    return np.random.randint(0, 2, length)

# Generate PN sequence: ±1 chips
def generate_pn_sequence(length):
    return np.random.choice([-1, 1], length)

# BPSK mapping: 0 → -1, 1 → +1
def bpsk_modulate(bit):
    return 2 * bit - 1

# DSSS spreading
def dsss_spread(data, pn_sequence):
    spread = []
    for bit in data:
        bpsk_bit = bpsk_modulate(bit)
        spread.extend(bpsk_bit * pn_sequence)
    return np.array(spread)

# BPSK carrier modulation of spread signal
def carrier_modulate(spread_signal, carrier_freq, sample_rate, samples_per_chip):
    total_samples = len(spread_signal) * samples_per_chip
    t = np.arange(total_samples) / sample_rate
    carrier_wave = np.cos(2 * np.pi * carrier_freq * t)

    # Repeat each chip to match carrier sampling
    chip_samples = np.repeat(spread_signal, samples_per_chip)
    return chip_samples * carrier_wave, t  # <-- Fixed indentation here

# Main function
if __name__ == "__main__":
    # Generate input
    data = generate_data(data_length)
    pn_seq = generate_pn_sequence(chips_per_bit)
    print("Original Data Bits:     ", data)
    print("PN Sequence:            ", pn_seq)

    # DSSS spreading
    spread_signal = dsss_spread(data, pn_seq)

    # BPSK Carrier modulation
    bpsk_waveform, t = carrier_modulate(spread_signal, carrier_freq, sample_rate, samples_per_chip)

    # Plot DSSS spread signal (chip values)
    plt.figure(figsize=(12, 3))
    plt.plot(spread_signal, drawstyle='steps-mid')
    plt.title("DSSS Spread Signal (Baseband)")
    plt.xlabel("Chip Index")
    plt.ylabel("Amplitude")
    plt.grid(True)
    plt.tight_layout()

    # Plot BPSK modulated carrier waveform
    plt.figure(figsize=(12, 3))
    plt.plot(t, bpsk_waveform)
    plt.title("BPSK Modulated Waveform (Carrier)")
    plt.xlabel("Time (s)")
    plt.ylabel("Amplitude")
    plt.grid(True)
    plt.tight_layout()

    plt.show()


```
## OUTPUT

![Screenshot 2025-05-19 182835](https://github.com/user-attachments/assets/dae670f2-2628-48da-a8ef-d9a8ca909c65)


## RESULT / CONCLUSIONS
Simulation of Spread Spectrum Modulation Techniques was verified successfully.
