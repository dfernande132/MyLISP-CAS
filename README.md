# Diseño y Estructura de un CAS: Programación simbólica en LISP

### *(repositorio MyLISP-CAS)*

**[English version below](#mylisp-cas-english)**

> ¿Qué necesitamos para poder decirle a nuestro ordenador que `(x + 1)² − (x² + 2x)` → `1`, o que `d/dx(3x²)` → `6x`, sin que sepa cuál es el valor numérico de `x`?

Un sistema de álgebra computacional (CAS) simbólico, escrito enteramente en [MyLISP](https://github.com/dfernande132/MyLISP), corriendo sobre un **Sinclair QL real de 1984** (y compatible con el port de ZX Spectrum Next). Deriva, sustituye, simplifica y reordena expresiones algebraicas —con variables simbólicas, no solo números— construido paso a paso, capítulo a capítulo, en un manual completo que enseña LISP desde cero y termina construyendo, con tus propias manos, el mismo tipo de motor simbólico que hay debajo de Mathematica, Maple o SymPy.

```lisp
(DEFINE E (MAKEPROD 3 (MAKEPOW 'X 2)))     ; 3x²

(PRINTMAT E)                                ; 3x^2
(PRINTMAT (DERIVA E 'X))                    ;   6x
(DERIVAEN E 'X 5)                           ;   75   (3 · 5² evaluado en x=5)
```

No hay ninguna librería de álgebra simbólica escondida detrás. Ningún `eval` haciendo trampa. Cada una de esas tres líneas se apoya en código que se explica, función a función, en el libro que acompaña a este repositorio — y que puedes leer entero antes de escribir una sola línea.

---

## Por qué merece la pena leerlo

Todo el mundo que ha usado Mathematica, Wolfram Alpha o SymPy se ha preguntado alguna vez *cómo sabe el ordenador que `x + x` es `2x`*. La respuesta casi nunca se explica desde cero: se da por hecho, como una caja negra. Este proyecto es la caja negra abierta, pieza a pieza, hasta el último `CAR` y `CDR`.

- **Aprendes LISP de verdad, no con ejemplos de juguete.** La Parte I del libro (9 capítulos) enseña S-expressions, homoiconicidad, closures, recursión, listas y `LAMBDA` — pero cada concepto se explica *para que sirva luego* en la Parte II, no como ejercicio aislado.
- **Construyes un CAS real, capítulo a capítulo, viendo por qué cada pieza hace falta.** No se presenta el simplificador terminado: primero ves la derivada "sucia" y redundante que produce una implementación ingenua (`(+ (* 1 X) (* X 1))` en vez de `2x`), y solo entonces se construye, con motivo, la pieza que la arregla.
- **Todo corre en hardware de 1984.** Sinclair QL, CPU de 7,5 MHz, microdrives, un heap de 24.000 celdas. Cada límite real de esa plataforma —truncado de símbolos a 8 caracteres, buffers de 80 y 500 caracteres, cuándo se dispara el recolector de basura— aparece documentado en el momento exacto en que se descubre, no escondido bajo la alfombra.
- **Nada de magia.** El árbol de sintaxis abstracta de una expresión algebraica es, literalmente, una lista de Lisp sin evaluar. `x + 3` es `(+ X 3)`. Eso es todo el "truco": derivar y simplificar es recorrer esa lista con las mismas herramientas —`COND`, recursión, `CAR`/`CDR`— que aprendiste en el capítulo 1.
- **Honestidad sobre lo que no hace.** El manual cierra explicando, con la misma claridad que el resto, qué se queda fuera (integración, resolución de ecuaciones, funciones trascendentes) y por qué — para que entiendas la diferencia real entre este CAS didáctico y uno de producción, no solo que "es más simple".

Si alguna vez quisiste entender el álgebra simbólica desde los cimientos, sin fingir que la respuesta es obvia, este es el proyecto.

## El libro

El corazón de este repositorio no es el código — es el libro que lo explica. **"Diseño y Estructura de un CAS: Programación simbólica en LISP"**, disponible completo en castellano e inglés, en dos partes:

| | |
|---|---|
| **Parte I — Aprender LISP con MyLISP** | 9 capítulos: qué es MYLISP, el modelo de datos de Lisp, aritmética exacta, listas, control de flujo, funciones y clausuras, cadenas y ficheros, y una galería de programas completos (Fibonacci, quicksort, la criba de Eratóstenes, Newton-Raphson...) |
| **Parte II — Construyendo un CAS** | 10 capítulos + resumen: representación de expresiones, sustitución, derivación cruda, constructores inteligentes, términos semejantes, simplificación de árbol completo, orden canónico, y una impresora en notación matemática legible |
| **Apéndices** | Referencia rápida del lenguaje, arquitectura y límites de la plataforma, mensajes de error, y una referencia función-por-función de las 59 funciones de `CAS.LSP` |

📖 **[Leer el libro en castellano](./book/Diseno_y_Estructura_de_un_CAS.md)**
📖 **[Read the book in English](./book/Design_and_Structure_of_a_CAS.md)**

No hace falta saber LISP para empezar. Sí hace falta curiosidad por ver, de verdad, cómo se construye algo así desde cero.

## Ficheros

| Fichero | Contenido |
|---|---|
| `book/Diseno_y_Estructura_de_un_CAS.md` | El manual completo, en castellano |
| `book/Design_and_Structure_of_a_CAS.md` | El manual completo, en inglés |
| `CAS.LSP` | El CAS completo, en un único fichero acumulativo: cada sección construye sobre la anterior, en el mismo orden en que se explica en el libro |
| `PCAS.LSP` | Ejemplos y pruebas: reproduce, capítulo a capítulo, cada resultado que aparece en el libro — para ir cargándolo a medida que avanzas en la lectura |

## Requisitos

- El intérprete **MyLISP** (v1.0 o posterior) para QL o para ZX Spectrum Next: [github.com/dfernande132/MyLISP](https://github.com/dfernande132/MyLISP)
- Nada más. El CAS es un único fichero fuente en LISP puro, sin dependencias.

## Cómo usarlo

Desde el REPL de MyLISP:

```lisp
(LOAD "CAS.LSP")
(LOAD "PCAS.LSP")   ; opcional: reproduce los ejemplos del libro, capítulo a capítulo
```

O, si prefieres ir siguiendo el libro con tus propias manos: carga solo `CAS.LSP`, abre el manual por el capítulo 10, y ve tecleando cada ejemplo tú mismo antes de mirar `PCAS.LSP`.

## Recorrido por el CAS (Parte II del libro)

1. **Representación y abstracción de datos** — predicados (`ISCONST?`, `ISVAR?`, `ISSUM?`...), selectores y constructores inteligentes (`MAKESUM`, `MAKEPROD`, `MAKEPOW`)
2. **Sustitución y evaluación numérica** — `SUBST`, `EVALEXPR`, y el centinela `VARLIBRE` para variables libres — la técnica de "valor que significa error" en un Lisp sin excepciones
3. **Derivación simbólica cruda** — `DERIV`, aplicando las reglas de suma, producto y potencia — y viendo con tus propios ojos la explosión de basura que produce sin simplificar
4. **Constructores inteligentes** — simplificaciones automáticas al construir (`x+0 → x`, `x·1 → x`, `x·0 → 0`...), sin tocar una sola línea de `DERIV`
5. **Orden canónico y términos semejantes** — `SIMPSUM`: aplana, agrupa y reconstruye para resolver `x + x → 2x`
6. **Simplificación de árbol completo** — `SIMP`, recorriendo la expresión entera de abajo hacia arriba
7. **Simplificador de productos** — `SIMPPROD`: agrupa factores repetidos en potencias (`x·x → x²`)
8. **El orquestador** — `DERIVA` y `DERIVAEN`: la derivada ya simplificada, con o sin evaluación numérica final — composición de funciones pequeñas, sin mega-funciones con banderas
9. **Orden canónico estable** — `EXPLESS?`: orden alfabético real entre símbolos, para que el resultado no dependa del orden en que escribiste la expresión
10. **Impresión infija** — `PRINTMAT`: muestra las expresiones en notación matemática normal (`2x + y`, no `(+ (* 2 X) Y)`), con la puntuación mínima de paréntesis según la precedencia

Y todo eso, apoyado en 9 capítulos previos de Parte I que te llevan de cero a manejar closures y recursión con soltura — sin que en ningún momento el libro asuma que ya sabías LISP.

## La historia detrás del proyecto

Escrito, probado y verificado en un Sinclair QL real —no en un emulador ni de memoria—, con cada límite de la plataforma (heap de 24.000 celdas, símbolos truncados a 8 caracteres, buffers de 80 y 500 caracteres) descubierto y documentado sobre la marcha, tal como le habría pasado a cualquiera programando esto en 1984.

## Colaborar

El código y el libro están aquí para quien quiera leerlos, aprender de ellos, usarlos o mejorarlos. Si encuentras un error, quieres proponer una ampliación del CAS (integración simbólica, funciones trascendentes...) o tienes ideas para el libro, abre un *issue* o contacta conmigo directamente.

## Licencia

Ver el repositorio para condiciones de uso.

---

<a name="mylisp-cas-english"></a>
# Design and Structure of a CAS: Symbolic Programming in LISP

### *(MyLISP-CAS repository)*

> What does it take to tell a computer that `(x + 1)² − (x² + 2x)` → `1`, or that `d/dx(3x²)` → `6x`, without it ever knowing the numeric value of `x`?

A symbolic Computer Algebra System (CAS), written entirely in [MyLISP](https://github.com/dfernande132/MyLISP), running on **real 1984 Sinclair QL hardware** (and compatible with the ZX Spectrum Next port). It differentiates, substitutes, simplifies, and reorders algebraic expressions — with symbolic variables, not just numbers — built step by step, chapter by chapter, in a complete manual that teaches LISP from scratch and ends up building, with your own hands, the same kind of symbolic engine that powers Mathematica, Maple, or SymPy underneath.

```lisp
(DEFINE E (MAKEPROD 3 (MAKEPOW 'X 2)))     ; 3x^2

(PRINTMAT E)                                ; 3x^2
(PRINTMAT (DERIVA E 'X))                    ;   6x
(DERIVAEN E 'X 5)                           ;   75   (3 * 5^2 evaluated at x=5)
```

There's no hidden symbolic-algebra library behind this. No `eval` cheating its way through. Every one of those three lines rests on code that's explained, function by function, in the book that accompanies this repository — which you can read in full before writing a single line yourself.

---

## Why it's worth reading

Everyone who's used Mathematica, Wolfram Alpha, or SymPy has, at some point, wondered *how does the computer know `x + x` is `2x`*? The answer is almost never explained from the ground up — it's taken for granted, as a black box. This project is that black box, opened piece by piece, down to the last `CAR` and `CDR`.

- **You learn real LISP, not toy examples.** Part I of the book (9 chapters) teaches S-expressions, homoiconicity, closures, recursion, lists, and `LAMBDA` — but every concept is explained *so it can be used later*, in Part II, not as an isolated exercise.
- **You build a real CAS, chapter by chapter, seeing why each piece is needed.** The simplifier isn't presented finished: you first see the "dirty," redundant derivative a naive implementation produces (`(+ (* 1 X) (* X 1))` instead of `2x`), and only then, with a clear reason, do you build the piece that fixes it.
- **Everything runs on 1984 hardware.** Sinclair QL, a 7.5 MHz CPU, microdrives, a 24,000-cell heap. Every real limit of that platform — symbols truncated to 8 characters, 80- and 500-character buffers, when the garbage collector fires — is documented at the exact moment it's discovered, never swept under the rug.
- **No magic.** The abstract syntax tree of an algebraic expression is, literally, an unevaluated Lisp list. `x + 3` is `(+ X 3)`. That's the whole "trick": differentiating and simplifying means walking that list with the same tools — `COND`, recursion, `CAR`/`CDR` — you learned back in chapter 1.
- **Honest about what it doesn't do.** The manual closes by explaining, with the same clarity as the rest, what's left out (integration, equation solving, transcendental functions) and why — so you understand the real difference between this didactic CAS and a production one, not just that "it's simpler."

If you've ever wanted to understand symbolic algebra from the foundations up, without pretending the answer is obvious, this is the project.

## The book

The heart of this repository isn't the code — it's the book that explains it. **"Design and Structure of a CAS: Symbolic Programming in LISP,"** available in full in both Spanish and English, in two parts:

| | |
|---|---|
| **Part I — Learning LISP through MyLISP** | 9 chapters: what MYLISP is, Lisp's data model, exact arithmetic, lists, control flow, functions and closures, strings and files, and a gallery of complete programs (Fibonacci, quicksort, the Sieve of Eratosthenes, Newton-Raphson...) |
| **Part II — Building a CAS** | 10 chapters + summary: representing expressions, substitution, raw differentiation, smart constructors, like terms, full-tree simplification, canonical order, and a printer for readable math notation |
| **Appendices** | Language quick reference, platform architecture and limits, error messages, and a function-by-function reference for all 59 functions in `CAS.LSP` |

📖 **[Read the book in Spanish](./book/Diseno_y_Estructura_de_un_CAS.md)**
📖 **[Read the book in English](./book/Design_and_Structure_of_a_CAS.md)**

You don't need to know LISP to start. You do need the curiosity to see, for real, how something like this gets built from nothing.

## Files

| File | Contents |
|---|---|
| `book/Diseno_y_Estructura_de_un_CAS.md` | The complete manual, in Spanish |
| `book/Design_and_Structure_of_a_CAS.md` | The complete manual, in English |
| `CAS.LSP` | The complete CAS, in one cumulative source file: each section builds on the previous one, in the same order it's explained in the book |
| `PCAS.LSP` | Examples and tests: reproduces, chapter by chapter, every result that appears in the book — so you can load it as you go along |

## Requirements

- The **MyLISP** interpreter (v1.0 or later) for QL or ZX Spectrum Next: [github.com/dfernande132/MyLISP](https://github.com/dfernande132/MyLISP)
- Nothing else. The CAS is a single pure-LISP source file, with no dependencies.

## Usage

From the MyLISP REPL:

```lisp
(LOAD "CAS.LSP")
(LOAD "PCAS.LSP")   ; optional: reproduces the book's examples, chapter by chapter
```

Or, if you'd rather follow the book with your own hands: load only `CAS.LSP`, open the manual at chapter 10, and type each example in yourself before checking `PCAS.LSP`.

## Tour of the CAS (Part II of the book)

1. **Representation and data abstraction** — predicates (`ISCONST?`, `ISVAR?`, `ISSUM?`...), selectors, and smart constructors (`MAKESUM`, `MAKEPROD`, `MAKEPOW`)
2. **Substitution and numeric evaluation** — `SUBST`, `EVALEXPR`, and the `VARLIBRE` sentinel for free variables — the "value that means error" technique in a Lisp with no exceptions
3. **Raw symbolic differentiation** — `DERIV`, applying the sum, product, and power rules — and seeing with your own eyes the garbage explosion it produces unsimplified
4. **Smart constructors** — automatic simplification at construction time (`x+0 → x`, `x·1 → x`, `x·0 → 0`...), without touching a single line of `DERIV`
5. **Canonical order and like terms** — `SIMPSUM`: flatten, group, and rebuild to solve `x + x → 2x`
6. **Full-tree simplification** — `SIMP`, walking the entire expression bottom-up
7. **Product simplifier** — `SIMPPROD`: groups repeated factors into powers (`x·x → x²`)
8. **The orchestrator** — `DERIVA` and `DERIVAEN`: the already-simplified derivative, with or without final numeric evaluation — composing small functions, no giant functions with flags
9. **Stable canonical order** — `EXPLESS?`: real alphabetical order between symbols, so the result doesn't depend on the order you wrote the expression in
10. **Infix printing** — `PRINTMAT`: displays expressions in normal math notation (`2x + y`, not `(+ (* 2 X) Y)`), with the minimum parenthesization needed based on precedence

And all of that, resting on 9 prior Part I chapters that take you from zero to handling closures and recursion comfortably — the book never assumes you already knew LISP.

## The story behind the project

Written, tested, and verified on real Sinclair QL hardware — not an emulator, not from memory — with every platform limit (a 24,000-cell heap, symbols truncated to 8 characters, 80- and 500-character buffers) discovered and documented along the way, exactly as it would have happened to anyone programming this in 1984.

## Contributing

The code and the book are here for anyone who wants to read them, learn from them, use them, or improve them. If you find a bug, want to propose an extension to the CAS (symbolic integration, transcendental functions...), or have ideas for the book, open an issue or get in touch with me directly.

## License

See the repository for usage terms.
