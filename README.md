# Emulador de CPU Simple

## Descripción
Este proyecto es un emulador de CPU educativo escrito en C que simula el funcionamiento de un procesador simple con arquitectura propia. El emulador incluye un conjunto de instrucciones básicas, múltiples modos de direccionamiento y un sistema de flags de estado.

## Características

### 🏗️ Arquitectura
- **Memoria**: 4096 palabras de 16 bits
- **Registros**:
  - `ACC`: Acumulador (16 bits)
  - `X`: Registro índice (16 bits)
  - `PC`: Contador de programa (16 bits)
- **Flags de estado**:
  - `Z`: Zero Flag - Resultado cero
  - `N`: Negative Flag - Resultado negativo
  - `C`: Carry Flag - Acarreo
  - `I`: Interrupt Flag - Interrupciones
  - `V`: Overflow Flag - Desbordamiento
  - `H`: Halt Flag - CPU detenida

### 📟 Conjunto de Instrucciones

#### Instrucciones Básicas
| Mnemónico | Opcode | Descripción |
|-----------|--------|-------------|
| `st` | 0 | Store - Almacena registro en memoria |
| `ld` | 1 | Load - Carga memoria en registro |
| `add` | 2 | Add - Suma memoria a registro |
| `br` | 3 | Branch - Salto incondicional |
| `bz` | 4 | Branch if Zero - Salto si Z=1 |
| `clr` | 5 | Clear - Limpia registro |
| `dec` | 6 | Decrement - Decrementa registro |

#### Instrucciones Extendidas (Opcode 7)
| Mnemónico | Ext Opcode | Descripción |
|-----------|------------|-------------|
| `halt` | 0 | Detiene la CPU |
| `ei` | 1 | Enable Interrupts |
| `di` | 2 | Disable Interrupts |

### 🎯 Modos de Direccionamiento
1. **Directo** (00): `EA = data`
2. **Indirecto** (01): `EA = mem[address]`
3. **Indexado** (10): `EA = address + X`
4. **Indirecto Indexado** (11): `EA = mem[address + X]`

## Formato de Instrucción
- [000] --> No usado (3 bits)
- [OPCODE] --> Código operación (4 bits)
- [R] --> Registro (0=X, 1=ACC)
- [DI] --> Modo direccionamiento (2 bits)
- [CDCDCDCDCD] --> Constante Dirección o Datos (6 bits)

## Compilación y Ejecución

### Requisitos
- Compilador C (GCC recomendado)
- Sistema Linux/Windows/macOS

### Compilar
```bash
gcc -o emulador emulador.c
```

### Ejecutar
```bash
./emulador
```

# Ejemplo de Programa

El emulador incluye un programa de demostración:

```c
uint16_t programa[] = {
    0x24A,  // LD X, [0x0A] - Carga X con valor en mem[0x0A]
    0x44B,  // ST X, [0x0B] - Almacena X en mem[0x0B]  
    0x04C,  // ADD ACC, [0x0C] - Suma mem[0x0C] a ACC
    0xE00,  // HALT - Detiene la CPU
    
    // Datos
    0x0005,  // mem[0x0A] = 5
    0x0006,  // mem[0x0B] = 6
    0x0002   // mem[0x0C] = 2
};
```

# Estructura del Código

## Archivos
- **`emulador.c`** - Código fuente principal

## Funciones Principales

### Gestión de CPU
- **`resetCPU()`** - Inicializa la CPU
- **`execute_instruction()`** - Ejecuta una instrucción
- **`cpu_loop()`** - Bucle principal de ejecución

### Fases de Ejecución
- **`fetch_and_decode()`** - Obtiene y decodifica instrucciones
- **Funciones específicas por instrucción** (`load_data`, `store_data`, etc.)

### Utilidades
- **`printCPUState()`** - Muestra estado de registros y memoria

## Ejemplo de Salida

```bash
DEBUG: op: 1, reg: 0, dirm: 1, cd: a, ea: 5, data: 5
Executing ld 0, 5
PC:1 X:5 ACC:10
STATUS: [Z:0 N:0 C:0 I:0 V:0 H:0]
Memory [0-29]: 24a 44b 4c e00 0 0 0 0 0 0
5 6 2 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
```

## Uso Educativo

Este emulador es ideal para:
- Entender conceptos básicos de arquitectura de computadoras
- Aprender sobre modos de direccionamiento
- Estudiar ejecución pipeline (fetch-decode-execute)
- Experimentar con diseño de instrucciones

## Limitaciones

- Conjunto de instrucciones limitado
- Memoria pequeña (4K palabras)
- No manejo real de interrupciones
- Sin E/S del sistema

## Mejoras Futuras

- [ ] Implementar más instrucciones
- [ ] Añadir manejo de interrupciones
- [ ] Incluir E/S básica
- [ ] Soporte para carga de programas desde archivo
- [ ] Modo depuración paso a paso
