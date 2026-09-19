# ASK & FSK
# Aim
 Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
 Google coLab
# Program
# ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, fc, br = 1000, 50, 10
t = np.linspace(0, 1, fs, endpoint=False)

bits = np.random.randint(0,2,br)

spb = fs // br
m = np.repeat(bits, spb)

c = np.sin(2*np.pi*fc*t)
ask = m * c

dem = ask * c
b, a = butter(5, fc/(fs/2))
f = lfilter(b, a, dem)

dec = (f[::spb] > 0.25).astype(int)

plt.figure(figsize=(12,8))

plt.subplot(4,1,1)
plt.plot(t, m)
plt.title("Message Signal")
plt.grid()

plt.subplot(4,1,2)
plt.plot(t, c)
plt.title("Carrier Signal")
plt.grid()

plt.subplot(4,1,3)
plt.plot(t, ask)
plt.title("ASK Modulated Signal")
plt.grid()

plt.subplot(4,1,4)
plt.step(np.arange(len(dec)), dec, marker='x')
plt.title("Decoded Bits")
plt.grid()

plt.tight_layout()
plt.show()
```
# FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter
fs, f1, f2, br = 1000, 30, 70, 10
t = np.linspace(0, 1, fs, endpoint=False)
bits = np.random.randint(0, 2, br)
spb = fs // br
m = np.repeat(bits, spb)
c1 = np.sin(2*np.pi*f1*t)
c2 = np.sin(2*np.pi*f2*t)
fsk = np.zeros_like(t)
for i, b in enumerate(bits):
    s = i * spb
    fsk[s:s+spb] = np.sin(2*np.pi*(f2 if b else f1)*t[s:s+spb])
b, a = butter(5, f2/(fs/2))
c1d = lfilter(b, a, fsk * c1)
c2d = lfilter(b, a, fsk * c2)
dec = []
for i in range(br):
    s = i * spb
    e1 = np.sum(c1d[s:s+spb]**2)
    e2 = np.sum(c2d[s:s+spb]**2)
    dec.append(e2 > e1)
dec = np.array(dec).astype(int)
dem = np.repeat(dec, spb)
plt.figure(figsize=(12,12))
plt.subplot(6,1,1)
plt.plot(t, m)
plt.title('Message Signal')
plt.grid()
plt.subplot(6,1,2)
plt.plot(t, c1)
plt.title('Carrier Signal for bit 0 (f1)')
plt.grid()
plt.subplot(6,1,3)
plt.plot(t, c2)
plt.title('Carrier Signal for bit 1 (f2)')
plt.grid()

plt.subplot(6,1,4)
plt.plot(t, fsk)
plt.title('FSK Modulated Signal')
plt.grid()

plt.subplot(6,1,5)
plt.plot(t, dem)
plt.title('Final Demodulated Signal')
plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform
# ASK
<img width="1190" height="790" alt="image" src="https://github.com/user-attachments/assets/175ca794-a682-47b3-9b0f-137cbe14f1db" />

# FSK
<img width="1201" height="1012" alt="image" src="https://github.com/user-attachments/assets/40059f96-793c-408f-b773-a6f281a54603" />

# Results
Thus, the ASK and FSK performed using python in Google CoLab
