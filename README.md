# collatz-runjump-acelerando-la-verificaci-n-de-Collatz-con-saltos-de-racha-y-tablas-de-ventana-fija
Mapa acelerado de Collatz tipo 2.1 para peores órbitas iniciales
Un experimento chico sobre cómo acelerar la verificación de trayectorias de la conjetura de Collatz (3n+1), sin pretender romper ningún récord de verificación exhaustiva (el récord real es 2^71, de David Barina, enero 2025 — años de cómputo dedicado).
Tres piezas:
Una fórmula cerrada de salto de racha. Para todo impar n con n+1 = m·2^x, x pasos acelerados seguidos se resuelven en una sola operación: n_nuevo = m·3^x − 1. Demostrada e implementada.
Comparación honesta de 4 métodos (naive, T-map estándar, salto de racha, tabla de ventana fija), con validación cruzada de que los cuatro cuentan exactamente los mismos pasos elementales. Resultado interesante: el salto de racha reduce las iteraciones a la mitad, pero es más lento en reloj real que el T-map simple — la multiplicación variable por 3^x cuesta más de lo que ahorra en iteraciones. La tabla de ventana fija sí gana, y hay un punto óptimo de tamaño (≈16 bits) antes de que los fallos de caché empiecen a doler.
Verificación puntual de 2^k − 1 (el caso "peor comportado" estructuralmente) para k muy por encima de 71 — se probó hasta k=300.000 (números de ~90.000 dígitos) y todos convergen a 1.
No es una prueba de nada. Es una bitácora de experimentos con benchmarks reproducibles.
https://claude.ai/public/artifacts/98a78307-ecfa-4add-8661-e95a711a04b5
