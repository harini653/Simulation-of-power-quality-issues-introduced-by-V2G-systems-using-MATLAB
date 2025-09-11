# Simulation-of-power-quality-issues-introduced-by-V2G-systems-using-MATLAB

## AIM
To Simulate power quality issues introduced by V2G systems using MATLAB

## APPARATUS REQUIRED
•	MATLAB

## MATLAB CODING

% V2G Power Quality Issues Simulation

% Shows harmonics, THD, and flicker due to V2G integration

clear; 

close all;

clc;

%% Parameters

f0 = 50; % fundamental frequency (Hz)

Fs = 5000; % sampling frequency (Hz)

T = 0.2; % simulation time (s)

t = 0:1/Fs:T; % time vector

%% Grid voltage (clean sine)

V_grid = 230*sqrt(2)*sin(2*pi*f0*t);

%% V2G distorted current (fundamental + harmonics)

I_fund = 10*sin(2*pi*f0*t); % fundamental current

I_3rd = 3*sin(2*pi*3*f0*t); % 3rd harmonic

I_5th = 2*sin(2*pi*5*f0*t); % 5th harmonic

I_7th = 1.5*sin(2*pi*7*f0*t); % 7th harmonic

I_11th = 1*sin(2*pi*11*f0*t); % 11th harmonic

I_v2g = I_fund + I_3rd + I_5th + I_7th + I_11th;

%% Sudden V2G injection effect (flicker simulation)

V_flicker = V_grid;

V_flicker(t > 0.1) = V_flicker(t > 0.1) * 0.85; % voltage drop after 0.1s

%% FFT for harmonic analysis

N = length(t);

Y = fft(I_v2g);

f = (0:N-1)*(Fs/N);

mag = abs(Y)/N;

%% THD calculation (manual method)
fundamental_mag = max(abs(fft(I_fund)))/N;

harmonic_mag = sqrt(sum((mag(2:200).^2))) - fundamental_mag;

THD = (harmonic_mag / fundamental_mag)*100;

%% Plots

figure;

% 1. Grid Voltage (clean)

subplot(3,2,1);

plot(t, V_grid, 'b','LineWidth',1.2);

xlabel('Time (s)'); ylabel('Voltage (V)');

title('Grid Voltage (Clean Sine)');

grid on;

% 2. V2G Current (Distorted)

subplot(3,2,2);

plot(t, I_v2g, 'r','LineWidth',1.2);

xlabel('Time (s)'); ylabel('Current (A)');

title('V2G Current with Harmonics');

grid on;

% 3. Harmonic Spectrum of Current

subplot(3,2,3);

stem(f(1:500), mag(1:500),'r','filled');

xlabel('Frequency (Hz)'); ylabel('Magnitude');

title('Harmonic Spectrum of V2G Current');

grid on;

% 4. Voltage Flicker (Sudden Drop due to V2G Injection)

subplot(3,2,4);

plot(t, V_flicker, 'm','LineWidth',1.2);

xlabel('Time (s)'); ylabel('Voltage (V)');

title('Voltage Flicker due to Sudden V2G Injection');

grid on;

% 5. THD Display (Bar Graph)

subplot(3,2,5);

bar([fundamental_mag harmonic_mag]);

set(gca,'XTickLabel',{'Fundamental','Harmonics'});

ylabel('Magnitude');

title(['Current THD = ', num2str(THD,'%.2f'), ' %']);

grid on;


## OUTPUT

<img width="696" height="625" alt="image" src="https://github.com/user-attachments/assets/e05cfad4-3d89-491c-959e-5f3762a3c863" />


## RESULT

Thus the power quality issues introduced by V2G systems is simulated AND verified using MATLAB 
