# Plan de estudio exprés (viernes–domingo)

Objetivo: maximizar puntos en 6 preguntas tipo test + 1 problema de desarrollo.

## Viernes
- Capa física:
  - dB/dBm: SNR(dB) = 10·log10(SNR), SNR = 10^(dB/10)
  - Capacidad de canal (Shannon): C = B·log2(1+SNR)
  - Potencia RMS y suma de potencias en señales senoidales de distinta frecuencia
  - Codificación Manchester (capa física)
- Práctica (115 min totales):
  - 20 min: repaso de fórmulas y unidades
  - 50 min: 3 ejercicios de Shannon (conversión dB↔SNR, unidades) + 2 de potencia RMS y dBm
  - 15 min: codificación Manchester (conceptos, trampas típicas)
  - 30 min: 20 flashcards

## Sábado
- ARQ y ventanas (sin HDLC):
  - Stop&Wait, Go-Back-N (GBN), Selective Repeat (SR)
  - Ttx = L/R, Tprop = d/v, a = Tprop/Ttx
  - Condición de saturación: W ≥ 1 + 2a
  - Límites: GBN Wmax=2^k−1, SR Wmax=2^(k−1)
  - Impacto del BER p: P_ok ≈ (1−p)^L ≈ e^(−pL)
- Red y transporte:
  - MTU/MSS, IPv6+TCP (overhead típico 40+20 B)
  - Throughput vs Goodput; factores de carga
- Práctica (135 min):
  - 60 min: problemas de ventanas, eficiencia y elección de ARQ
  - 45 min: problemas integrados de throughput/goodput con MTU/MSS
  - 30 min: 20 flashcards

## Domingo
- Simulacro cronometrado (60 min): 6 test + 1 problema “drones”
- Corrección y repaso (30 min): fórmulas y errores
- Último bloque (30–45 min): ejercicios cortos de capacidad y ventanas

## Trampas típicas
- Manchester = capa física (no enlace)
- Piggybacking ≠ bit stuffing (puede aparecer conceptual aunque no entre HDLC)
- t en ms → frecuencias en kHz; usa RMS para potencia
- En throughput/goodput no olvides el overhead (enlace y TCP/IPv6)

## Checklist rápido pre-examen
- Unidades: Hz/kHz/MHz; bits/Bytes; dB/dBm
- Convertir SNR(dB) ↔ SNR lineal sin errores
- Fórmulas memorizadas: Shannon, RMS, P=Vrms²/R, a=Tprop/Ttx, límites de ventana
- Factores de carga típicos con MTU 700B e IPv6+TCP 60B