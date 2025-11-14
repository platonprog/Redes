# Simulacro 1 (6 test + 1 problema largo)

Tiempo recomendado: 60 min.

## Tipo test (elige 1 opción; explica en una línea para autocorrección)

1) ¿En qué capa OSI ubicarías la codificación Manchester?
a) Física  b) Enlace  c) Red  d) Transporte  e) Aplicación  
Solución: a. Es codificación de línea; pertenece a la capa física.

2) Canal banda base con B = 25 kHz y SNR = 15 dB. Capacidad C ≈  
a) 77 kbps  b) 101 kbps  c) 126 kbps  d) 200 kbps  e) 400 kbps  
Cálculo: SNR = 10^(15/10)=31.62; C=25e3·log2(1+31.62)=25e3·5.033≈125.8 kbps.  
Solución: c).

3) v(t)=3·sin(2π·10·t)+4·sin(2π·20·t), t en ms; carga R=50 Ω. Potencia media ≈  
a) 20 dBm  b) 22 dBm  c) 24 dBm  d) 26 dBm  e) 28 dBm  
Cálculo: Vrms1=3/√2=2.121 → P1=Vrms1²/R=4.5/50=0.09 W; Vrms2=4/√2=2.828 → P2=8/50=0.16 W; P=0.25 W=250 mW → 10 log10(250)=23.98 dBm.  
Solución: c) ~24 dBm.

4) En un enlace, L=6000 bits, R=100 Mbps, d=30 km en fibra (v=2·10^8 m/s). ¿k mínimo para SR con U≈1?  
Ttx=L/R=60 μs; Tprop=d/v=1.5e-4 s=150 μs; a=150/60=2.5 → 1+2a=6.  
Para SR: se requiere W≥1+2a y W≤2^(k−1) → 2^(k−1)≥6 ⇒ k−1≥log2(6)=2.585 ⇒ k=4.  
Solución: k=4.

5) Una app domótica sin alarmas ni emergencia es, típicamente:  
a) Muy exigente en retardo  b) Tolerante a retardo y jitter, exige fiabilidad  c) Tolerante a fiabilidad  d) Solo exigente en jitter  e) Muy exigente en retardo y jitter  
Solución: b).

6) Fuente discreta con p={0.3, 0.25, 0.2, 0.15, 0.1}. Eficiencia con Huffman ≈  
Entropía H = −∑p log2 p ≈ 2.197 bits/símb. Longitudes típicas: {2,2,2,3,3} → L=0.3·2+0.25·2+0.2·2+0.15·3+0.1·3=2.25 bits/símb. Eficiencia η=H/L≈0.976.  
Solución: ≈0.976.

---

## Problema largo (drones)

Dos drones se enlazan a R=24 Gbps, separadas horizontalmente 750 m; alturas 550 m y 150 m. MTU de datos de enlace 700 B; overhead enlace 14 B; k bits de secuencia. Se usa TCP/IPv6 (60 B de overhead). BER=1e−5. Velocidad de propagación ≈ c=3·10^8 m/s.

1) Determina la técnica ARQ (GBN o SR) que maximiza la eficiencia en saturación y justifica.  
2) With BER=1e−5, calcula throughput (enlace) y goodput (aplicación).  
3) ¿Cuántas tramas de datos se requieren para transferir 50 KB? (1 KB=1024 B)  
4) Tiempo de transferencia de ese fichero en saturación.  
5) Si se quisiera que SR alcance U≈1, ¿cuántos bits de secuencia serían necesarios?

### Solución-resumen
- Distancia real: d = sqrt(750² + 400²) ≈ 850 m → Tprop ≈ 2.833 μs.  
- Ltrama = 700+14 = 714 B = 5712 bits → Ttx = 5712/24e9 ≈ 0.238 μs → a ≈ 11.9.  
- Ventana necesaria: 1+2a ≈ 24.8. GBN: Wmax=2^k−1; SR: Wmax=2^(k−1). Con k=5 → GBN Wmax=31 (satura), SR Wmax=16 (no satura).  
1) Más eficiente: Go-Back-N.  
2) P_ok ≈ e^(−pL)=e^(−0.05712)=0.9445. Factor carga enlace 700/714=0.9804; factor app 640/714=0.896. Throughput ≈ 24e9·1·0.9445·0.9804 ≈ 22.22 Gbps. Goodput ≈ 24e9·0.9445·0.896 ≈ 20.32 Gbps.  
3) Tramas para 50 KB app: payload_app=640 B → 51200/640=80.  
4) Tiempo ≈ bits_app/Goodput = 409600/20.32e9 ≈ 20 μs.  
5) Para SR con U≈1: 2^(k−1)≥1+2a≈25 ⇒ k≥6.