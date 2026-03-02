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
- Recepción de audio Bluetooth
- Recepción de audio (entrada AUX) vía ficha mini-plug
- Amplificación mono interna
- Alimentación mediante fuente externa DC
- Control de encendido y volumen
- Prototipo funcional probado en protoboard

### Excluido
- Alimentación por baterías
- Portabilidad total
- Potencia de salida nivel "fiesta"

---

## 3. Requisitos funcionales

### RF-01 – Entrada de audio
- El sistema debe recibir audio de forma inalámbrica vía Bluetooth.
- El audio recibido debe ser compatible con fuentes estéreo estándar (teléfono, PC, etc.).
- El sistema debe recibir audio de forma cableada a través de entrada AUX con un jack 3.5 mm.

### RF-02 – Conversión estéreo a mono
- El audio estéreo debe mezclarse correctamente a mono.
- No se permite la unión directa de canales L y R.
- Se utilizará una red resistiva para la mezcla (ej. 2 resistencias).

### RF-03 – Amplificación
- El sistema debe amplificar la señal de audio mono y excitar un parlante de 6 Ω.
- La amplificación se realizará mediante un IC clase AB reutilizado.

### RF-04 – Control de volumen
- El usuario debe poder regular el volumen mediante un potenciómetro externo.
- Se utilizará el control de volumen interno del IC amplificador.

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
- Se reutiliza la caja original
- Tamaño: 20.4 x 30.8 x 11.5 cm

### 4.2 Amplificador
- Modelo: AN7522N (Panasonic / Matsushita)
- Tipo: Amplificador de audio clase AB
- Configuración: BTL interno por canal
- Uso previsto: 1 canal en modo mono
- Potencia estimada: ~5 W sobre 6–8 Ω

### 4.3 Módulo Bluetooth
- Modelo: WIN-668
- Alimentación: 5 V
- Función: Fuente de señal de audio

### 4.4 Regulación de tensión
- Tipo: Regulardor Lineal
- Modelo: TP7805
- Función: Conversión de 12 V → 5 V para el módulo Bluetooth
- Justificación: evitar introducir ruido de switching

---

## 5. Arquitectura del sistema

### Diagrama conceptual

- Fuente externa DC (≈12 V)
  - Interruptor ON/OFF
  - Línea directa al amplificador
  - Línea regulada (TP7805) hacia módulo Bluetooth

- Flujo de señal de audio:
  - Bluetooth (L/R) o ficha mini-plug (L/R)
  - Mezcla estéreo → mono
  - Potenciómetro para control de volumen (pin 9 del IC)
  - Entrada de audio del AN7522N (pin 6 del IC)
  - Salida BTL hacia el parlante (pines 2 y 4 del IC)

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
- Capacitor electrolítico de bulk en VCC del módulo BT
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

- Largo de los cables de potencia (amplificador → bocina) no debería superar los 8-10 cms
para minimizar ruido en el audio (cables más largos pueden hacer las veces de antena).

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
6. Reducción de ruido (si lo hubiera)
7. Ajustes fino del diseño
8. Diseño del PCB (en KiCAD 9)

---

## 11. Consideraciones futuras (fuera de alcance actual)

- Agregado de control de tono

---

## 12. Lista de componentes sugeridos (etapa protoboard)

### 12.1 Etapa de potencia (AN7522N)
- IC amplificador: **AN7522N**
- Alimentación: **12 V DC**
- Capacitor bulk VCC: **2200 µF electrolítico** (entre VCC y GND)
- Capacitor cerámico desacople: **100 nF** (a agregar en etapa de reducción de ruido)
- Disipador de aluminio tipo 'T' (verificado térmicamente)

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
- Resistencia para Mix L/R: **3.3 kΩ** (opción 4.7 kΩ a evaluar)

### 12.5 Entrada AUX por cable
- Jack estéreo común
- Capacitor de acople por canal: **1–10 µF**
- Resistencia para Mix L/R: **3.3 kΩ** (opción 4.7 kΩ a evaluar)
- Nota PRD: **Por simplicidad del diseño, módulo BT y TP7805 permanecen siempre energizados**

### 12.6 Manejo de las fuentes de audio
- Conmutación de señal mediante switch SPDT

### 12.7 Pendientes de la etapa protoboard
- Reducción de ruido / fritura:
  - Agregar cerámicos de desacople
  - Filtrado de alimentación BT y amplificador
  - Evaluar impacto de resistencias serie mayores
- Validar entrada AUX
- Medir ruido con BT desconectado vs conectado
