# Capítulo 8. Control de Flujo, Control de Errores y Control de Congestión

## 8.1 Control de flujo (capa de enlace; análogo en transporte)
Objetivo: evitar overflow del buffer de RX.

Parámetros y unidades:
- R_b (bit/s), n_bits (bit por trama), d (m), v_prop (m/s).
- t_tx = n_bits/R_b (s), t_prop = d/v_prop (s), a = t_prop/t_tx (adimensional).
- Throughput T = R_b·η (bit/s).

Stop-and-wait (parada y espera):
- TX espera ACK antes de siguiente trama.
- Eficiencia sin errores: η_SW = 1/(1 + 2a) (adim).
- Observaciones: η decrece con d y con R_b si n_bits fijo.

Ventana deslizante (sliding window), tamaño máximo W (adim):
- Permite hasta W tramas sin ACK (RX debe poder bufferizar ≥ W).
- Eficiencia (sin errores):
  - Si W < 2a+1: η = W/(1+2a)
  - Si W ≥ 2a+1: η → 1
- Numeración de tramas con Q bits (0…2^Q−1). Sin ambigüedad típico: W ≤ 2^Q−1 (go-back-N) y W ≤ 2^{Q−1} (selective repeat).

Producto ancho de banda–retardo:
- BDR = R_b·t_prop (bit) = bits “en vuelo”.

## 8.2 Control de errores

### 8.2.1 Esquemas de codificación (detección/corrección) — resumen
- Códigos bloque (n,k): palabra de n bits a partir de k bits de información. Tasa R_c = k/n (adim). Distancia mínima d_min (conteo): detección segura ≤ q si d_min ≥ q+1; corrección segura ≤ t si d_min ≥ 2t+1.
- CRC (detección): división polinómica módulo 2 por generador G(x) (grado r). Excelente detección de ráfagas (prob. fallo ≈ 2^{−r}). FCS típico: 16 o 32 bit.
- Checksums (detección): suma complemento a 1 sobre palabras de m bits (p.ej., 16). Menos potentes que CRC pero computación simple.
- Nota de unidades: todo en bits/bytes; probabilidades adimensional.

### 8.2.2 Mecanismos ARQ (retransmisión)
Suposiciones útiles:
- p_b: prob. error de bit (adim); errores independientes → p_frame = 1 − (1 − p_b)^{n_bits}.
- ACK/NAK sin error (aprox.) para análisis; tiempos de procesado despreciables.

Stop-and-wait ARQ:
- Eficiencia: η_SW_ARQ = (1 − p_frame)/(1 + 2a).

Ventana deslizante con ARQ:
- go-back-N (W ≤ 2^Q−1):
  - Con W ≥ 2a+1: η_GBN = (1 − p_frame)/(1 + 2a·p_frame).
  - Con W < 2a+1: η_GBN = W·(1 − p_frame)/(2a + 1).
- selective repeat (W ≤ 2^{Q−1}):
  - Con W ≥ 2a+1: η_SR ≈ (1 − p_frame).
  - Con W < 2a+1: η_SR = (W·(1 − p_frame))/(2a + 1 − W·p_frame).

Notas:
- Selective repeat mejora eficiencia a costa de más complejidad y mayor requisito de numeración.
- Timeout (s) > α·RTT (α>1) para cubrir pérdidas de ACK/NAK.

## 8.3 Control de congestión
Concepto: regular fuentes cuando la red (o nodo) se congestiona (buffers con ocupación alta, pérdidas, latencias elevadas). Es 1→N (red→múltiples fuentes), distinto del control de flujo (1→1).

Mecanismos:
- Contrapresión (backpressure): nodo congestivo activa control de flujo hacia atrás salto a salto.
- Paquetes de frenado (choke): notificación explícita a la fuente (p.ej., ICMP Source Quench — obsoleto en práctica, pero conceptual).
- Señalización implícita: fuentes detectan congestión por RTT/pérdidas (p.ej., TCP congestion control).
- Señalización explícita: marcas en cabeceras (FECN/BECN en FR/ATM; ECN en IP/TCP).

Criterios:
- Imparcialidad (fairness): reparto equitativo de recursos.
- QoS: prioridades/clasificación; reservas (ATM, RSVP en IP).

Variables y unidades:
- RTT (s), pérdidas (adim), tasas λ/μ (s⁻¹), utilización ρ (adim).

## 8.4 HDLC (orientado a bit) — resumen
- Configuraciones: no balanceada (Primaria P / Secundarias S; modos NRM/ARM) y balanceada (C–C; ABM).
- Tramas (formato común): 
  - Flag (8 bit, 0x7E) | Address (8 bit, extensible) | Control (8/16 bit) | Information (variable) | FCS (16/32 bit) | Flag.
- Tipos:
  - I (Información): datos de capa superior; N(S), N(R) (numeración).
  - S (Supervisión): RR, RNR, REJ (GBN), SREJ (SR).
  - U (No numeradas): gestión (SABM/SABME, DISC, UA, XID, TEST, …).
- Bit P/F (Poll/Final): fuerza respuesta/sella fin de respuesta; semántica según comando/respuesta y dirección.
- Ventanas y piggybacking: ACKs en tramas I opuestas para eficiencia.
- Unidades: todos los campos en bits; tiempos/eficiencia según secciones 8.1–8.2.