# Capítulo 3. Transmisión Digital (Deep Dive)

Objetivo
Pasar de bits a formas de onda que puedan viajar por el medio con las propiedades deseadas (sincronismo, eficiencia espectral, robustez).

Códigos de línea (banda base)
- Qué son: reglas para mapear bits a niveles/tiempos en banda base.
- Requisitos habituales: 
  - Sincronismo de reloj (suficientes transiciones).
  - Baja componente DC (para acoplos capacitivos).
  - Eficiencia espectral razonable y baja BER.
- Ejemplos (intuición):
  - NRZ: simple; problema con largas rachas sin transiciones.
  - Manchester: transición en mitad de cada bit → reloj excelente; “cuesta” el doble de ancho de banda (eficiencia media de 0.5 frente a NRZ).
  - AMI (Alternate Mark Inversion): ‘1’ alterna polaridad, ‘0’ es cero → reduce DC.
  - 4B5B, 8B10B: añaden bits de control para asegurar transiciones y balance DC (overhead 25%).
  - MLT-3 y PAM-M: reducen banda o elevan bits por símbolo con múltiples niveles (a costo de SNR).
- Nota de modelos OSI:
  - Manchester pertenece a Capa Física (codificación de línea es fenómeno físico/eléctrico).

Modulación paso banda (radio, FO coherente)
- PSK/FSK/ASK, QAM.
- En M-aria: bits por símbolo ideales $r=\log_2 M$.
- Tasa de símbolos $R_s$ vs bit rate $R_b$:
  - $R_b=R_s\cdot r$ (si no hay codificación extra).
  - Eficiencia espectral $\eta_{spec}=R_b/B$ [bit/(s·Hz)].

Sincronización y reloj
- El receptor necesita recuperar temporización (PLL, transiciones garantizadas).
- Códigos con “densidad de transiciones” ayudan (Manchester, 8B10B).

A/D y cuantificación (cuando digitalizamos analógico)
- Muestreo: $f_s \ge 2B$.
- Cuantificación n bits → $SNR_q(\mathrm{dB}) \approx 6.02n + 1.76$.

Cómo razonar preguntas típicas
- “¿En qué capa ubicas Manchester?” → Física.
- “Eficiencia/anchos” → compara NRZ vs Manchester vs esquemas con overhead (4B5B, 8B10B).
- “¿Por qué más niveles (PAM-4) requieren más SNR?” → menor distancia entre niveles ⇒ más sensible a ruido.

Errores típicos
- Confundir $R_b$ con $R_s$.
- Ignorar el overhead de line codes con codificación en bloque (4B5B/8B10B).