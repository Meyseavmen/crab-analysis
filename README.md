# 🦀 DST: Algoritmo Universal de Solución Determinística (Análisis Cangrejo)

## 🎯 Introducción y Fundamentos

El **Algoritmo de Solución Determinística (DST)** es un marco analítico universal, diseñado para resolver completamente cualquier juego de suma cero, de información perfecta y de turnos alternos. Este método, conocido como **Análisis Cangrejo** (Inducción Hacia Atrás), etiqueta cada estado del juego con una solución forzada: **Victoria (Gn), Derrota (Px), o Tablas (Tz)**.

El objetivo del algoritmo no es solo encontrar la solución, sino también determinar el **camino de máxima resistencia** que debe anticipar el jugador para garantizar la victoria o minimizar la derrota.

---

## 1. Estructura y Notación

### A. La Isla (Estado Único)
Una **Isla** es un nodo único en el grafo de juego, representando una configuración de tablero y el turno. Su clave se define para mantener la consistencia del turno y manejar ciclos:

> Clave Única = ID Progresivo de la Foto + Letra del Jugador que **Acaba de Mover**

Esto implica que el **color de la Isla** indica al jugador que acaba de llegar, y el **turno siguiente** es del color opuesto (ej: '1B' indica que el Blanco movió, y ahora le toca al Negro).

---

## 2. Propagación Gn/Px (El Bucle de Inducción)

El algoritmo se resuelve desde los estados terminales hacia atrás (G1). La **Acomodación Global** (actualización de la Base de Tablas) es crucial después de cada asignación Gn o Px.

| Asignación | Condición de Sucesores | Regla de Índice (Índice en el Antecesor) | Lógica (Máxima Resistencia) |
| :--- | :--- | :--- | :--- |
| **Pérdida (Px)** | **TODOS** los sucesores son **Gn**. | **x = G_mínima + 1** | El oponente (ganador) elegirá la ruta Gn más corta para finalizar la partida lo más rápido posible. |
| **Victoria (Gn)** | **TODOS** los sucesores son **Px**. | **n = P_máxima + 1** | El jugador debe anticipar que el oponente (perdedor) elegirá la ruta Px más larga para maximizar la duración del juego. |

---

## 3. Asignación Tz (Tablas)

La etiqueta **Tz** se asigna a las Islas que quedan sin resolver después de que el bucle Gn/Px se ha detenido (Regla de Exclusión).

### A. Indexación de $z$ (Información Defensiva)

El índice $z$ proporciona un valor informativo sobre el riesgo de error o la duración mínima del ciclo de tablas.

| Escenario | Sucesores Incluyen | Regla de Índice (Defensiva) | Lógica |
| :--- | :--- | :--- | :--- |
| **Riesgo** | Al menos una Px | **z = P_mínima** | Indica el riesgo mínimo de pérdida si el jugador se desvía del camino de tablas (Gravedad del Error). |
| **Ciclo Puro** | SOLO T | **z = T_mínima** | Representa el camino más corto para establecer las tablas (para mantener la coherencia con el principio de juego óptimo en T). |

---

## 4. La Jerarquía de Prioridades (Movimiento Óptimo)

La decisión del jugador al momento de moverse se basa en una estricta jerarquía de prioridades que define el juego óptimo:

1.  **Prioridad Máxima: Ganar (G)**
    * **Acción:** Elegir la **G** con el valor $n$ **MÁS BAJO** (ganar más rápido).
2.  **Prioridad Intermedia: Tablas (T)**
    * **Acción:** Elegir la **T** con el valor $z$ **MÁS ALTO** para maximizar la duración del empate.
3.  **Prioridad Mínima: Perder (P)**
    * **Acción:** Elegir la **P** con el valor $x$ **MÁS ALTO** para minimizar la velocidad de la derrota.

---

## 5. Pseudocódigo Universal del DST

Este es el algoritmo central que implementa la teoría anterior. 

```pseudocode
// ----------------------------------------------------------------------
// ESTRUCTURAS GLOBALES
// ----------------------------------------------------------------------
BASE_DE_TABLAS = Diccionario<Clave_Isla, Solucion> // Almacena (1N : G3, 2B : P4, etc.)
CONTEO_FOTOS = 0 // Contador progresivo para ID único de la 'Foto'

// ----------------------------------------------------------------------
// FUNCIONES DEL DST
// ----------------------------------------------------------------------

FUNCION Resolver_DST(Estado_Inicial)
    // 1. Construcción del Grafo y Asignación de IDs
    Expandir_Grafo(Estado_Inicial)
    
    // 2. Solución de Victoria/Derrota (Gn y Px)
    Bucle_Gn_Px() 
    
    // 3. Solución de Tablas (Tz)
    Bucle_Tz()

    RETORNA BASE_DE_TABLAS[Estado_Inicial] 
FIN FUNCION

// Construye la estructura de la Isla y sus Puentes (utiliza la función game-specific de Generar_Puentes_de_Salida)
FUNCION Expandir_Grafo(Serializacion_Actual)
    // ... Lógica para asignar el ID progresivo y la Clave Única ...
    // ... Lógica recursiva para explorar todos los Puentes y construir el GRAFO_INVERSO ...
FIN FUNCION

// Aplica las reglas Gn/Px (P_max + 1, G_min + 1) de forma iterativa y aplica Acomodación Global.
FUNCION Bucle_Gn_Px()
    MIENTRAS Haya_Cambios_En_BASE_DE_TABLAS
        // ... Lógica de Propagación Gn y Px ...
        // ... Si se asigna, aplicar Acomodación (actualizar antecesores) ...
    FIN MIENTRAS
FIN FUNCION

// Resuelve las Tz usando la Regla de Exclusión y el índice z (P_minima o T_minima).
FUNCION Bucle_Tz()
    MIENTRAS Haya_Cambios_En_Tz_Asignados
        // ... Lógica de Propagación Tz y asignación del índice z ...
    FIN MIENTRAS
FIN FUNCION

---

## Autores

* Alex
* Logos

---

## Licencia

Este proyecto está bajo la **Licencia MIT**. Puedes usar, copiar, modificar y distribuir el código siempre y cuando se incluya la nota de copyright original.
