# FDM

# Aim:
Study of TDM pulse amplitude modulation/ demodulation with transmitter block (clock) and channel identification information linked directly to the receivers.

# Apparatus Required:

1) Experimental kit DCL-02

2) Connecting chord

3) Power supply

4) 20 MHz dual trace oscilloscope.


# Procedure:

1) Refer to the block diagram and carry out the following connections and switch setting.

2) Connect power supply in proper polarity DCL-02 and s it switch it on.

3) Connect 250hz, 500hz, 1khz and 2khz sine wave signals from the function generator to the multiplexer input channels CH0, CH1,CH3 by means of connecting chords.

4) Connect the multiplexer output txd of the transmitted section to the de multiplexer input rxd of the receiver section.

5) Connect the output of the receiver section CH0, CH1, CH2,CH3 to the IN0, IN1, IN2. IN3 of the filter section.

6) Connect the sampling clock TX CLK channel identification clock TXSYNC of the transmitter section to the corresponding RXCLK, RXSYNC of the receiver section respectively.

7) Set the amplitude of the input sine wave desired.
 
8)  Take the observations as mentioned below.


# program :
```
clc; clear; close;

fs = 100000;
T = 0.02;             
t = 0:1/fs:T-1/fs;   
N = length(t);          

m1 = sin(2*%pi*200*t);
m2 = sin(2*%pi*400*t);
m3 = sin(2*%pi*600*t);
m4 = sin(2*%pi*800*t);
m5 = sin(2*%pi*1000*t);
m6 = sin(2*%pi*1200*t);

fc = [6000 11000 16000 21000 26000 31000]; // Hz

s1 = m1 .* cos(2*%pi*fc(1)*t);
s2 = m2 .* cos(2*%pi*fc(2)*t);
s3 = m3 .* cos(2*%pi*fc(3)*t);
s4 = m4 .* cos(2*%pi*fc(4)*t);
s5 = m5 .* cos(2*%pi*fc(5)*t);
s6 = m6 .* cos(2*%pi*fc(6)*t);

fdm = s1 + s2 + s3 + s4 + s5 + s6;

F = fft(fdm);
freq = (0:N-1)*(fs/N);

fshift = freq;
idx = find(freq > fs/2);
if ~isempty(idx) then
    fshift(idx) = freq(idx) - fs;
end

msg_bw = 1500;
halfband = msg_bw + 300; 

recovered = zeros(6, N);

for k = 1:6
    BPmask = zeros(1, N);
    BPmask = (abs(fshift - fc(k)) <= halfband) | (abs(fshift + fc(k)) <= halfband);
    Fbp = F .* BPmask;
    xbp = fft(Fbp, -1) / N;
    down = 2 .* xbp .* cos(2*%pi*fc(k)*t);
    D = fft(down);
    LPmask = (abs(fshift) <= msg_bw);
    Dlp = D .* LPmask;
    base = fft(Dlp, -1) / N;
    recovered(k, :) = real(base);
end

figure(1);
subplot(3,2,1); plot(t, m1); title("Original m1 (200 Hz)");
subplot(3,2,2); plot(t, m2); title("Original m2 (400 Hz)");
subplot(3,2,3); plot(t, m3); title("Original m3 (600 Hz)");
subplot(3,2,4); plot(t, m4); title("Original m4 (800 Hz)");
subplot(3,2,5); plot(t, m5); title("Original m5 (1 kHz)");
subplot(3,2,6); plot(t, m6); title("Original m6 (1.2 kHz)");
xlabel("Time (s)");

figure(2);
plot(t, fdm);
title("FDM Composite Signal (time domain)");
xlabel("Time (s)");

figure(3);
subplot(3,2,1); plot(t, recovered(1,:)); title("Recovered m1");
subplot(3,2,2); plot(t, recovered(2,:)); title("Recovered m2");
subplot(3,2,3); plot(t, recovered(3,:)); title("Recovered m3");
subplot(3,2,4); plot(t, recovered(4,:)); title("Recovered m4");
subplot(3,2,5); plot(t, recovered(5,:)); title("Recovered m5");
subplot(3,2,6); plot(t, recovered(6,:)); title("Recovered m6");
xlabel("Time (s)");

disp("✅ FFT-based demux complete. Compare originals vs recovered visually or compute MSE:");
for k=1:6
    mse = sum((recovered(k,:) - eval("m"+string(k))).^2)/N;
    disp("Channel "+string(k)+" MSE = "+string(mse));
end
```

# Model Graph:

<img width="736" height="896" alt="image" src="https://github.com/user-attachments/assets/6010e09f-79c1-4573-b709-48e2a832db21" />

# output:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/03d2b3f0-c494-41dc-b5b5-b5bc3a1ac4b0" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/68fa1c1a-c28f-41f7-b7c2-61869dadb240" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/44e92d8f-2053-40c6-8222-deda084d2e95" />



# Tabulation:
<img width="824" height="1280" alt="image" src="https://github.com/user-attachments/assets/50ea056d-f9fd-4faf-b499-86e23350ebd7" />
<img width="849" height="1280" alt="image" src="https://github.com/user-attachments/assets/59be4eec-b719-414e-a312-a2036457e4dd" />


# Result:
Thus the time division multiplexing is done experimentally and output is verifie
