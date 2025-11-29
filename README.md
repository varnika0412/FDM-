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
clc; 
clear; 
close;

fs = 50000;
t = 0:1/fs:0.05;

m1 = 3.7*sin(2*%pi*303*t);
m2 = 3.8*sin(2*%pi*313*t);
m3 = 3.9*sin(2*%pi*323*t);
m4 = 4.0*sin(2*%pi*333*t);
m5 = 4.1*sin(2*%pi*343*t);
m6 = 4.2*sin(2*%pi*353*t);

c1 = cos(2*%pi*2000*t);
c2 = cos(2*%pi*4000*t);
c3 = cos(2*%pi*6000*t);
c4 = cos(2*%pi*8000*t);
c5 = cos(2*%pi*10000*t);
c6 = cos(2*%pi*12000*t);

s1 = m1 .* c1;
s2 = m2 .* c2;
s3 = m3 .* c3;
s4 = m4 .* c4;
s5 = m5 .* c5;
s6 = m6 .* c6;

fdm = s1 + s2 + s3 + s4 + s5 + s6;

r1 = fdm .* c1;
r2 = fdm .* c2;
r3 = fdm .* c3;
r4 = fdm .* c4;
r5 = fdm .* c5;
r6 = fdm .* c6;


cutoff_hz = 400;
norm_cutoff = cutoff_hz/(fs/2);

M = 101; 

[h, w] = wfir('lp', M, [norm_cutoff, 0], 'hm', 0);  

d1 = conv(r1, h, 'same');
d2 = conv(r2, h, 'same');
d3 = conv(r3, h, 'same');
d4 = conv(r4, h, 'same');
d5 = conv(r5, h, 'same');
d6 = conv(r6, h, 'same');

d1 = 2 * d1;
d2 = 2 * d2;
d3 = 2 * d3;
d4 = 2 * d4;
d5 = 2 * d5;
d6 = 2 * d6;

figure(1);
subplot(3,2,1); plot(t,m1); title("Message 1");
subplot(3,2,2); plot(t,m2); title("Message 2");
subplot(3,2,3); plot(t,m3); title("Message 3");
subplot(3,2,4); plot(t,m4); title("Message 4");
subplot(3,2,5); plot(t,m5); title("Message 5");
subplot(3,2,6); plot(t,m6); title("Message 6");

figure(2);
plot(t,fdm); title("Multiplexed FDM");

figure(3);
subplot(3,2,1); plot(t,d1); title("Demod 1");
subplot(3,2,2); plot(t,d2); title("Demod 2");
subplot(3,2,3); plot(t,d3); title("Demod 3");
subplot(3,2,4); plot(t,d4); title("Demod 4");
subplot(3,2,5); plot(t,d5); title("Demod 5");
subplot(3,2,6); plot(t,d6); title("Demod 6");

```

# Model Graph:

<img width="736" height="896" alt="image" src="https://github.com/user-attachments/assets/6010e09f-79c1-4573-b709-48e2a832db21" />

# output:
<img width="1916" height="1020" alt="image" src="https://github.com/user-attachments/assets/1940cd17-72e0-4762-8f0a-aff462b91bbc" />
<img width="1915" height="927" alt="image" src="https://github.com/user-attachments/assets/431f6508-85c1-4967-a6d3-3219c904f3dd" />
<img width="1906" height="1010" alt="image" src="https://github.com/user-attachments/assets/a7dab45c-7768-42ce-b36b-28e66dd81134" />




# Tabulation:
<img width="976" height="1600" alt="image" src="https://github.com/user-attachments/assets/614e2f8b-a04f-4691-9ecb-597e96c928d3" />
<img width="962" height="1462" alt="image" src="https://github.com/user-attachments/assets/9b771492-1975-43b1-a865-f7278a7a7123" />



# Result:
Thus the time division multiplexing is done experimentally and output is verifie
