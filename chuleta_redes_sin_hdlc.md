# Chuleta rápida (sin HDLC)

## dB, dBm y SNR
- SNR(dB) = 10·log10(SNR)
- SNR = 10^(SNR(dB)/10)
- P(dBm) = 10·log10(P[mW])  →  0 dBm = 1 mW

## Potencia y señales senoidales
- v(t) = A·sin(2πft + φ) → Vrms = A/√2
- P = Vrms² / R
- Señales con frecuencias distintas: potencias medias se suman (no hay término cruzado en media)
- Suma de senos: si f1 y f2 son múltiplos enteros, la frecuencia fundamental es el MCD(f1,f2)

## Capacidad de canal (Shannon)
- C = B·log2(1 + SNR)
- Unidades: B en Hz → C en bit/s; pasar a kbps/ Mbps según convenga
- Conversión rápida: log2(x) = ln(x)/ln(2)

## Codificación de línea
- Manchester: transición por bit, reloj embebido; pertenece a la capa física

## ARQ y ventanas (modelo general sin HDLC)
- Ttx = L/R        (L en bits, R en bps)
- Tprop = d/v      (v ≈ 2·10^8–3·10^8 m/s en radio/fibra)
- a = Tprop / Ttx
- Utilización sin errores ≈ min( W/(1+2a), 1 )
- Go-Back-N:      Wmax = 2^k − 1
- Selective Repeat: Wmax = 2^(k − 1)

## Impacto del BER (p) sobre una trama de L bits
- P_ok ≈ (1 − p)^L ≈ e^(−pL)  (si pL ≪ 1, usar expansión)

## Throughput y Goodput
- Factor de carga enlace = payload_enlace / Ltrama
- Factor de aplicación = payload_app / Ltrama
- Throughput ≈ R · U · P_ok · factor_carga
- Goodput  ≈ R · U · P_ok · factor_app

## QoS de aplicaciones (conceptos frecuentes)
- Domótica sin alarmas/emergencias: tolera retardo y jitter; exige fiabilidad
- Voz interactiva: muy sensible a retardo/jitter; tolera pérdidas pequeñas
- Vídeo streaming: bufferiza (tolerancia a jitter), moderada sensibilidad a pérdida

## Conversión útil
- 1 KB = 1024 B (si no se indique lo contrario)
- 1 ms = 10^−3 s, 1 μs = 10^−6 s
- 1 kHz = 10^3 Hz, 1 MHz = 10^6 Hz
