# PRD – Modernización de Parlante Philips con Bluetooth

## 1. Objetivo del proyecto
Modernizar un parlante Philips pasivo de mediados de los 2000 para que funcione de manera autónoma respecto a su equipo de audio original, incorporando recepción de audio por Bluetooth, amplificación interna y alimentación desde la red eléctrica mediante una fuente externa.

El proyecto prioriza:
- Reutilización de componentes existentes
- Aprendizaje práctico de electrónica de audio
- Diseño modular y entendible (no solución "todo en uno")

---

## 2. Alcance

### Incluido
- Conversión de parlante pasivo a parlante activo
- Recepción de audio (entrada AUX) vía ficha mini-plug
- Recepción de audio Bluetooth
- Amplificación mono interna
- Alimentación mediante fuente externa DC
- Control de encendido y volumen
- Prototipo funcional probado en protoboard

### Excluido
- Alimentación por baterías
- Portabilidad total
- Potencia de salida tipo "fiesta"

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
- Tipo: Regulardor Lineal
- Modelo: TP7805
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
  - Línea regulada (TP7805) hacia módulo Bluetooth

- Flujo de señal de audio:
  - Bluetooth (L/R) o ficha mini-plug
  - Mezcla estéreo → mono
  - Entrada del AN7522N
  - Potenciómetro para control de volumen
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
- Uso típico: 8 hs de uso contínuo.

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

- Agregado de control de tono
- Diseño de PCB

---

## 12. Lista de componentes sugeridos (etapa protoboard)

### 12.1 Etapa de potencia (AN7522N)
- IC amplificador: **AN7522N**
- Alimentación: **12 V DC**
- Capacitor bulk VCC: **470 µF electrolítico** (entre VCC y GND)
- Capacitor cerámico desacople: **100 nF** (a agregar en etapa de reducción de ruido)
- Disipador de aluminio tipo L (verificado térmicamente)

### 12.2 Stand-by / Soft-start (pin 5)
- Resistencia a VCC: **68 kΩ**
- Resistencia a GND: **330 kΩ ∥ 1 MΩ ≈ 248 kΩ** (o 330 kΩ sola como alternativa válida)
- Capacitor a GND: **10 µF electrolítico**

### 12.3 Control de volumen (según datasheet)
- Potenciómetro lineal: **50 kΩ (slider)**
- Capacitor opcional anti-ruido: **100 nF a GND desde pin VOL**
- Rango de control resultante: **0–1.25 V DC**

### 12.4 Entrada de audio Bluetooth (WIN-668)
- Regulador: TP7805 (12 V → 5 V)
- Capacitor acople audio: **10 µF electrolítico**
- Resistencia serie audio: **2.2 kΩ**
- Nota PRD: *Evaluar subir resistencia serie a **4.7 kΩ** en etapa final de protoboard si no degrada audio*

### 12.5 Entrada AUX por cable
- Jack estéreo común
- Capacitor de acople por canal: **1–10 µF**
- Resistencia serie por canal: **2.2 kΩ** (opción 4.7 kΩ a evaluar)
- Mezcla L/R a mono por resistencias
- Nota PRD: BT y TP7805 permanecen siempre energizados
- Analizar si conmutación de señal mediante switch SPDT
- Analizar si convendría ir directo al chanel 2 del AN7522N (y mezclar con chanel 1 a su salida)

### 12.6 Pendientes de la etapa protoboard
- Reducción de ruido / fritura:
  - Agregar cerámicos de desacople
  - Filtrado de alimentación BT y amplificador
  - Evaluar impacto de resistencias serie mayores
- Validar entrada AUX
- Medir ruido con BT desconectado vs conectado


