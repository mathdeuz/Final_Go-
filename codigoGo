using System;


class JuegoGo
{
    const int TAMANO = 9;
    int[,] tablero = new int[TAMANO, TAMANO];
    int puntajeNegro = 0;
    int puntajeBlanco = 0;
   
    // Inicializa el tablero con todas las posiciones en 0 (vacías)
    void InicializarTablero()
    {
        for (int i = 0; i < TAMANO; i++)
        {
            for (int j = 0; j < TAMANO; j++)
            {
                tablero[i, j] = 0;
            }
        }
    }


    // Muestra el tablero en la consola
    void MostrarTablero()
    {
        Console.WriteLine("  0 1 2 3 4 5 6 7 8");
        for (int i = 0; i < TAMANO; i++)
        {
            Console.Write(i + " ");
            for (int j = 0; j < TAMANO; j++)
            {
                if (tablero[i, j] == 0)
                    Console.Write(". ");
                else if (tablero[i, j] == 1)
                    Console.Write("N ");
                else
                    Console.Write("B ");
            }
            Console.WriteLine();
        }
    }


    // Coloca una piedra en el tablero si la posición está vacía
    bool ColocarPiedra(int fila, int columna, int jugador)
    {
        if (tablero[fila, columna] != 0)
        {
            Console.WriteLine("La posición ya está ocupada. Intente nuevamente.");
            return false;
        }


        tablero[fila, columna] = jugador;
        CapturarPiedras(fila, columna, jugador);
        return true;
    }


    // Verifica si una piedra tiene libertades
    bool TieneLibertades(int fila, int columna, int jugador)
    {
        if (fila < 0 || fila >= TAMANO || columna < 0 || columna >= TAMANO || tablero[fila, columna] == 0)
            return true;
       
        if (tablero[fila, columna] != jugador)
            return false;


        tablero[fila, columna] = -jugador; // Marca temporalmente para evitar revisiones repetidas


        bool tieneLibertad = TieneLibertades(fila - 1, columna, jugador) || TieneLibertades(fila + 1, columna, jugador) ||
                             TieneLibertades(fila, columna - 1, jugador) || TieneLibertades(fila, columna + 1, jugador);


        tablero[fila, columna] = jugador; // Desmarca después de revisar
        return tieneLibertad;
    }


    // Captura piedras del oponente que no tienen libertades
    void CapturarPiedras(int fila, int columna, int jugador)
    {
        int oponente = jugador == 1 ? 2 : 1;


        if (fila > 0 && tablero[fila - 1, columna] == oponente && !TieneLibertades(fila - 1, columna, oponente))
            RemoverPiedras(fila - 1, columna, oponente);


        if (fila < TAMANO - 1 && tablero[fila + 1, columna] == oponente && !TieneLibertades(fila + 1, columna, oponente))
            RemoverPiedras(fila + 1, columna, oponente);


        if (columna > 0 && tablero[fila, columna - 1] == oponente && !TieneLibertades(fila, columna - 1, oponente))
            RemoverPiedras(fila, columna - 1, oponente);


        if (columna < TAMANO - 1 && tablero[fila, columna + 1] == oponente && !TieneLibertades(fila, columna + 1, oponente))
            RemoverPiedras(fila, columna + 1, oponente);
    }


    // Elimina las piedras capturadas del tablero y suma puntos
    void RemoverPiedras(int fila, int columna, int jugador)
    {
        if (fila < 0 || fila >= TAMANO || columna < 0 || columna >= TAMANO || tablero[fila, columna] != jugador)
            return;


        tablero[fila, columna] = 0;
        if (jugador == 1)
            puntajeBlanco++;
        else
            puntajeNegro++;


        RemoverPiedras(fila - 1, columna, jugador);
        RemoverPiedras(fila + 1, columna, jugador);
        RemoverPiedras(fila, columna - 1, jugador);
        RemoverPiedras(fila, columna + 1, jugador);
    }


    // Calcula el puntaje al final del juego
    void CalcularPuntaje()
    {
        puntajeNegro += ContarAreaControlada(1);
        puntajeBlanco += ContarAreaControlada(2);


        Console.WriteLine("Puntaje final:");
        Console.WriteLine("Jugador Negro: " + puntajeNegro);
        Console.WriteLine("Jugador Blanco: " + puntajeBlanco);


        if (puntajeNegro > puntajeBlanco)
            Console.WriteLine("¡Jugador Negro gana!");
        else if (puntajeBlanco > puntajeNegro)
            Console.WriteLine("¡Jugador Blanco gana!");
        else
            Console.WriteLine("Empate.");
    }


    // Cuenta el área controlada por un jugador
    int ContarAreaControlada(int jugador)
    {
        int areaControlada = 0;
        for (int i = 0; i < TAMANO; i++)
        {
            for (int j = 0; j < TAMANO; j++)
            {
                if (tablero[i, j] == 0 && EstaControlada(i, j, jugador))
                    areaControlada++;
            }
        }
        return areaControlada;
    }


    // Verifica si una intersección vacía está controlada por un jugador
    bool EstaControlada(int fila, int columna, int jugador)
    {
        if (fila < 0 || fila >= TAMANO || columna < 0 || columna >= TAMANO)
            return true;


        if (tablero[fila, columna] == 0)
        {
            tablero[fila, columna] = -1; // Marca temporalmente para evitar revisiones repetidas
            bool estaControlada = EstaControlada(fila - 1, columna, jugador) && EstaControlada(fila + 1, columna, jugador) &&
                                  EstaControlada(fila, columna - 1, jugador) && EstaControlada(fila, columna + 1, jugador);
            tablero[fila, columna] = 0; // Desmarca después de revisar
            return estaControlada;
        }


        return tablero[fila, columna] == jugador;
    }


    // Lógica principal del juego
    public void Jugar()
    {
        InicializarTablero();
        int jugador = 1;


        while (true)
        {
            MostrarTablero();
            Console.WriteLine("Turno del jugador " + (jugador == 1 ? "Negro" : "Blanco"));
           
            int fila, columna;


            // Manejo de entrada para fila
            Console.Write("Ingrese la fila (0-8): ");
            while (!int.TryParse(Console.ReadLine(), out fila) || fila < 0 || fila >= TAMANO)
            {
                Console.WriteLine("Entrada inválida. Ingrese un número entre 0 y 8.");
            }


            // Manejo de entrada para columna
            Console.Write("Ingrese la columna (0-8): ");
            while (!int.TryParse(Console.ReadLine(), out columna) || columna < 0 || columna >= TAMANO)
            {
                Console.WriteLine("Entrada inválida. Ingrese un número entre 0 y 8.");
            }


            if (ColocarPiedra(fila, columna, jugador))
                jugador = 3 - jugador; // Cambia de jugador


            Console.Write("¿Quieres pasar tu turno? (s/n): ");
            if (Console.ReadLine().ToLower() == "s")
            {
                Console.Write("¿El siguiente jugador también quiere pasar su turno? (s/n): ");
                if (Console.ReadLine().ToLower() == "s")
                    break;
            }
        }


        CalcularPuntaje();
    }


    static void Main()
    {
        JuegoGo juego = new JuegoGo();
        juego.Jugar();
    }
}
