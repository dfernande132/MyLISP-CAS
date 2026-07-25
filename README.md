# MyLISP-CAS

**[English version below](#mylisp-cas-english)**

Un sistema de álgebra computacional (CAS) simbólico, escrito enteramente en [MyLISP](https://github.com/dfernande132/MyLISP), corriendo sobre un Sinclair QL real (y compatible con el port de ZX Spectrum Next).

Deriva, sustituye, simplifica y reordena expresiones algebraicas —con variables simbólicas, no solo números— construido paso a paso, capítulo a capítulo, como ejercicio de aprendizaje de LISP y de diseño de un CAS desde cero.

```lisp
(DEFINE E (MAKEPROD 3 (MAKEPOW 'X 2)))
(DERIVA E 'X)              ; -> (* 6 X)
(PRINTMAT (DERIVA E 'X))   ; 6x
```

---

## ¿Qué es esto?

Un CAS (Computer Algebra System) manipula expresiones matemáticas como *símbolos*, no como números: sabe derivar `3x²` y devolver `6x` sin que le digas cuánto vale `x`. MyLISP-CAS es una implementación completa de esa idea, escrita en el propio lenguaje MyLISP, sin ninguna dependencia externa: es LISP puro corriendo sobre un ordenador de los años 80.

El proyecto nació como excusa para aprender LISP en serio: closures, funciones de orden superior, recursión sobre listas y azúcar sintáctico, todo aplicado a un problema real en vez de a ejemplos de juguete.

## Requisitos

- El intérprete **MyLISP** (v1.0 o posterior) para QL o para ZX Spectrum Next: [github.com/dfernande132/MyLISP](https://github.com/dfernande132/MyLISP)
- Nada más. El CAS es un único fichero fuente en LISP.

## Ficheros

| Fichero | Contenido |
|---|---|
| `CAS.LSP` | El CAS completo, en un único fichero acumulativo: los 10 capítulos, cada uno construyendo sobre el anterior |
| `PCAS.LSP` | Batería de pruebas/demostración: reproduce paso a paso los ejemplos de cada capítulo |

## Cómo usarlo

Desde el REPL de MyLISP:

```lisp
(LOAD "CAS.LSP")
(LOAD "PCAS.LSP")   ; opcional: ejecuta los ejemplos de demostración
```

## Recorrido por los capítulos

1. **Representación y abstracción de datos** — predicados (`ISCONST?`, `ISVAR?`, `ISSUM?`...), selectores y constructores inteligentes (`MAKESUM`, `MAKEPROD`, `MAKEPOW`)
2. **Sustitución y evaluación numérica** — `SUBST`, `EVALEXPR`, y el centinela `VARLIBRE` para variables libres
3. **Derivación simbólica cruda** — `DERIV`, aplicando las reglas de suma, producto y potencia
4. **Constructores inteligentes** — simplificaciones automáticas al construir (`x+0 → x`, `x·1 → x`, etc.)
5. **Orden canónico y términos semejantes** — `SIMPSUM`: agrupa y reduce términos semejantes
6. **Simplificación de árbol completo** — `SIMP`, recorriendo la expresión entera
7. **Simplificador de productos** — `SIMPPROD`: agrupa factores repetidos en potencias
8. **Orquestador** — `DERIVA` y `DERIVAEN`: la derivada ya simplificada, con o sin evaluación numérica final
9. **Orden canónico estable** — `EXPLESS?`: orden alfabético real entre símbolos, para que el resultado no dependa del orden de entrada
10. **Impresión infija** — `PRINTMAT`: muestra las expresiones en notación matemática normal (`2x + y`, no `(+ (* 2 X) Y)`)

Para la historia completa de por qué existe este proyecto y cómo se llegó hasta aquí, hay un artículo aparte: *"De la HP48 al QL: cómo acabé escribiendo mi propio LISP"*.

## Colaborar

El código está aquí para quien quiera leerlo, aprender de él, usarlo o mejorarlo. Si quieres traducir la documentación a otros idiomas o tienes ideas para ampliar el CAS, contacta conmigo directamente.

## Licencia

Ver el repositorio para condiciones de uso.

---

<a name="mylisp-cas-english"></a>
# MyLISP-CAS (English)

A symbolic Computer Algebra System (CAS), written entirely in [MyLISP](https://github.com/dfernande132/MyLISP), running on real Sinclair QL hardware (and compatible with the ZX Spectrum Next port).

It differentiates, substitutes, simplifies and reorders algebraic expressions — with symbolic variables, not just numbers — built step by step, chapter by chapter, as an exercise in learning LISP and designing a CAS from scratch.

```lisp
(DEFINE E (MAKEPROD 3 (MAKEPOW 'X 2)))
(DERIVA E 'X)              ; -> (* 6 X)
(PRINTMAT (DERIVA E 'X))   ; 6x
```

## What is this?

A CAS manipulates mathematical expressions as *symbols*, not numbers: it knows how to differentiate `3x²` and return `6x` without ever being told what `x` is worth. MyLISP-CAS is a full implementation of that idea, written in MyLISP itself, with no external dependencies — pure LISP running on 1980s hardware.

The project started as an excuse to properly learn LISP: closures, higher-order functions, recursion over lists, and syntactic sugar, all applied to a real problem instead of toy examples.

## Requirements

- The **MyLISP** interpreter (v1.0 or later) for QL or ZX Spectrum Next: [github.com/dfernande132/MyLISP](https://github.com/dfernande132/MyLISP)
- Nothing else. The CAS is a single LISP source file.

## Files

| File | Contents |
|---|---|
| `CAS.LSP` | The complete CAS, in one cumulative source file: 10 chapters, each building on the previous one |
| `PCAS.LSP` | Test/demo suite: walks through every chapter's examples step by step |

## Usage

From the MyLISP REPL:

```lisp
(LOAD "CAS.LSP")
(LOAD "PCAS.LSP")   ; optional: runs the demo examples
```

## Chapter walkthrough

1. **Data representation & abstraction** — predicates (`ISCONST?`, `ISVAR?`, `ISSUM?`...), selectors, and smart constructors (`MAKESUM`, `MAKEPROD`, `MAKEPOW`)
2. **Substitution & numeric evaluation** — `SUBST`, `EVALEXPR`, and the `VARLIBRE` sentinel for free variables
3. **Raw symbolic differentiation** — `DERIV`, applying the sum, product and power rules
4. **Smart constructors** — automatic simplification at construction time (`x+0 → x`, `x·1 → x`, etc.)
5. **Canonical order & like terms** — `SIMPSUM`: groups and reduces like terms
6. **Full-tree simplification** — `SIMP`, walking the whole expression
7. **Product simplifier** — `SIMPPROD`: groups repeated factors into powers
8. **Orchestrator** — `DERIVA` and `DERIVAEN`: the already-simplified derivative, with or without final numeric evaluation
9. **Stable canonical order** — `EXPLESS?`: real alphabetical ordering between symbols, so results don't depend on input order
10. **Infix printing** — `PRINTMAT`: displays expressions in normal math notation (`2x + y`, not `(+ (* 2 X) Y)`)

For the full story of why this project exists and how it came to be, there's a separate article: *"From the HP48 to the QL: how I ended up writing my own LISP"*.

## Contributing

The code is here for anyone who wants to read it, learn from it, use it, or improve it. If you'd like to help translate the documentation into other languages, or have ideas for extending the CAS, get in touch with me directly.

## License

See the repository for usage terms.
