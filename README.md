# collatz-runjump-acelerando-la-verificaci-n-de-Collatz-con-saltos-de-racha-y-tablas-de-ventana-fija
Mapa acelerado de Collatz tipo 2.1 para peores órbitas iniciales
Un experimento chico sobre cómo acelerar la verificación de trayectorias de la conjetura de Collatz (3n+1), sin pretender romper ningún récord de verificación exhaustiva (el récord real es 2^71, de David Barina, enero 2025 — años de cómputo dedicado).
Tres piezas:
Una fórmula cerrada de salto de racha. Para todo impar n con n+1 = m·2^x, x pasos acelerados seguidos se resuelven en una sola operación: n_nuevo = m·3^x − 1. Demostrada e implementada.
Comparación honesta de 4 métodos (naive, T-map estándar, salto de racha, tabla de ventana fija), con validación cruzada de que los cuatro cuentan exactamente los mismos pasos elementales. Resultado interesante: el salto de racha reduce las iteraciones a la mitad, pero es más lento en reloj real que el T-map simple — la multiplicación variable por 3^x cuesta más de lo que ahorra en iteraciones. La tabla de ventana fija sí gana, y hay un punto óptimo de tamaño (≈16 bits) antes de que los fallos de caché empiecen a doler.
Verificación puntual de 2^k − 1 (el caso "peor comportado" estructuralmente) para k muy por encima de 71 — se probó hasta k=300.000 (números de ~90.000 dígitos) y todos convergen a 1.
No es una prueba de nada. Es una bitácora de experimentos con benchmarks reproducibles.
https://claude.ai/public/artifacts/98a78307-ecfa-4add-8661-e95a711a04b5
"""
Verifica de forma puntual que 2^k - 1 converge a 1 bajo Collatz, para k por
encima del record de verificacion exhaustiva (2^71, Barina 2025).

Usa la formula de salto de racha: para estos numeros m=1 siempre (ya que
n+1 = 2^k exactamente), asi que el primer salto es 3^k - 1 calculado de un
tiron, sin ninguna multiplicacion intermedia.

Uso:
    python3 verify_extreme.py            # corre un barrido por defecto
    python3 verify_extreme.py 500000     # corre un k especifico
"""
import sys
import time


def v2(n):
    """Valoracion 2-adica de n (numero de ceros finales en binario)."""
    return (n & -n).bit_length() - 1


def verify(n0, max_steps=10_000_000):
    """Aplica el mapa acelerado (salto de racha + division de ceros) hasta
    llegar a 1 o agotar max_steps. Devuelve (convergio, pasos, bits_maximos)."""
    n = n0
