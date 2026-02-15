# PRD – Modernización de Parlante Philips con Bluetooth

## 1. Objetivo del proyecto
Modernizar un parlante Philips pasivo de mediados de los 2000 para que funcione de manera autónoma respecto a su equipo de audio original, incorporando recepción de audio por Bluetooth, amplificación interna y alimentación desde la red eléctrica mediante una fuente externa.

El proyecto prioriza:
- Reutilización de componentes existentes
- Aprendizaje práctico de electrónica de audio
- Diseño modular y entendible (no solución “todo en uno”)

---

## 2. Alcance

### Incluido
- Conversión de parlante pasivo a parlante activo
- Recepción de audio Bluetooth
- Amplificación mono interna
- Alimentación mediante fuente externa DC
- Control de encendido y volumen
- Prototipo funcional probado en protoboard

### Excluido
- Alimentación por baterías
- Portabilidad total
- Potencia de salida tipo “fiesta / PA”
- Diseño de PCB en esta etapa

---

## 3. Requisitos funcionales

### RF-01 – Entrada de audio
- El sistema debe recibir audio de forma inalámbrica vía Bluetooth.
- El audio recibido debe ser compatible con fuentes estéreo estándar (teléfono, PC, etc.).

### RF-02 – Conversión estéreo a mono
- El audio estéreo Bluetooth debe mezclarse correctamente a mono.
- No se permite la unión directa de canales L y R.
- Se utilizará una red resistiva para la mezcla (ej. 2 resistencias).

### RF-03 – Amplificación
- El sistema debe amplificar la señal de audio mono y excitar un parlante de 6 Ω.
- La amplificación se realizará mediante un IC clase AB reutilizado.

### RF-04 – Control de volumen
- El usuario debe poder regular el volumen mediante un potenciómetro externo.
- El control de volumen interno del IC amplificador no será utilizado.

### RF-05 – Alimentación
- El sistema debe alimentarse desde una fuente externa DC.
- Debe incluir interruptor físico de encendido/apagado.

---

## 4. Componentes principales

### 4.1 Parlante
- Marca: Philips
- Tipo: Pasivo
- Potencia nominal: 50 W (no RMS)
- Impedancia: 6 Ω
- Caja original reutilizada

### 4.2 Módulo Bluetooth
- Modelo: WIN-668
- Alimentación: 5 V
- Función: Fuente de señal de audio

### 4.3 Amplificador
- Modelo: AN7522N (Panasonic / Matsushita)
- Tipo: Amplificador de audio clase AB
- Configuración: BTL interno por canal
- Uso previsto: 1 canal en modo mono
- Potencia estimada: ~5 W sobre 6–8 Ω

### 4.4 Regulación de tensión
- Tipo: DC/DC buck converter
- Modelo: LM2596
- Función: Conversión de 12 V → 5 V para el módulo Bluetooth
- Justificación:
  - Alta eficiencia
  - Baja disipación térmica
  - Mayor margen para expansiones futuras

---

## 5. Arquitectura del sistema

### Diagrama conceptual

- Fuente externa DC (≈12 V)
  - Interruptor ON/OFF
  - Línea directa al amplificador
  - Línea regulada (LM2596) hacia módulo Bluetooth

- Flujo de señal de audio:
  - Bluetooth (L/R)
  - Mezcla estéreo → mono
  - Potenciómetro de volumen
  - Entrada del AN7522N
  - Salida BTL hacia el parlante

---

## 6. Requisitos eléctricos

### Alimentación
- Tensión principal: 12 V DC nominal
- Corriente estimada total: < 2 A

### Regulación
- Salida regulada: 5 V DC
- Corriente esperada módulo BT: < 100 mA

### Filtrado
- Capacitor electrolítico de bulk en línea de 12 V
- Capacitores de desacople (cerámicos) cerca de cada IC

---

## 7. Requisitos térmicos

- El AN7522N debe contar con disipador térmico.
- El diseño debe contemplar ventilación pasiva dentro de la caja.
- No se permite operación sostenida sin disipación adecuada.

---

## 8. Requisitos mecánicos

- Todos los componentes deben alojarse dentro de la caja original del parlante.
- Elementos ya incorporados:
  - Jack DC lateral
  - Interruptor ON/OFF
  - Manija superior

- Se debe preservar la integridad estructural y estética del gabinete.

---

## 9. Requisitos de calidad de audio

- Distorsión baja a volumen doméstico.
- Ruido de fondo mínimo.
- Sin zumbidos audibles provenientes de la fuente o del módulo BT.

---

## 10. Etapas del proyecto

1. Análisis de componentes y arquitectura
2. Diseño del esquema eléctrico mínimo
3. Prototipo en protoboard del amplificador AN7522N
4. Integración del módulo Bluetooth y regulación
5. Pruebas funcionales y térmicas
6. Ajustes finales

---

## 11. Consideraciones futuras (fuera de alcance)

- Migración a amplificador clase D
- Diseño de PCB
- Agregado de control de tono
- Entrada auxiliar por jack

---

## 12. Lista de componentes sugeridos (BOM preliminar)

> Nota: Los valores indicados son sugeridos para la etapa de prototipo y pueden ajustarse durante las pruebas.

### 12.1 Alimentación
- Fuente externa DC: 12 V / 2 A (mínimo)
- Jack DC: 5.5 mm × 2.1 mm
- Interruptor ON/OFF: SPST, 12 V DC

### 12.2 Regulación de tensión
- Módulo DC/DC buck: LM2596 ajustable
  - Entrada: 12 V
  - Salida ajustada: 5.0 V
- Capacitor electrolítico entrada buck: 470 µF / 25 V
- Capacitor electrolítico salida buck: 220 µF / 10 V
- Capacitor cerámico: 100 nF (entrada y salida)

### 12.3 Módulo Bluetooth
- Módulo BT: WIN-668
- Capacitor de desacople local: 100 nF + 10 µF

### 12.4 Mezcla estéreo a mono
- Resistencias mezcla L y R:
  - 2 × 2.2 kΩ (1/4 W, 5 %)

### 12.5 Control de volumen
- Potenciómetro: 10 kΩ logarítmico (A10k)
- Configuración: divisor de señal antes del amplificador

### 12.6 Amplificador
- IC amplificador: AN7522N
- Configuración: uso de un canal BTL
- Disipador térmico: ≤ 20 °C/W
- Capacitor de alimentación Vcc:
  - 470 µF / 25 V (mínimo)
  - 100 nF cerámico cercano al pin Vcc

### 12.7 Entrada de audio al amplificador
- Capacitor de acople de entrada:
  - 1 µF (electrolítico o film)
- Resistencia de referencia a masa (si aplica): 10 kΩ

### 12.8 Salida a parlante
- Cableado de salida: AWG22 o mayor
- Parlante: Philips 6 Ω

### 12.9 Misceláneos
- Protoboard para pruebas
- Cables Dupont
- Tornillería y aislación térmica para disipador

---

## 13. Etapas del proyecto

1. Análisis de componentes y arquitectura
2. Diseño del esquema eléctrico mínimo
3. Prototipo en protoboard del amplificador AN7522N
4. Integración del módulo Bluetooth y regulación
5. Pruebas funcionales y térmicas
6. Ajustes finales

---

## 14. Consideraciones futuras (fuera de alcance)

- Migración a amplificador clase D
- Diseño de PCB
- Agregado de control de tono
- Entrada auxiliar por jack

