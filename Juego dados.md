# fundamentos_pseint
Proyectos de parte de fundamentos de programación pseint 

Proceso ProyectoII
	// Definici�n de variables
	Definir puntoJ1, puntoJ2, turno, tablero, dado1, dado2, suma, opcMenu Como Entero;
	Definir triadasJ1, triadasJ2, i, j, oportunidades, fila, columna Como Entero;
	Definir visible, iniJ1, iniJ2 Como Caracter;
	Definir encontroTriadaJ1, encontroTriadaJ2 ,tableroLleno, sumaValida, acerto Como Logico;
	
	// Inicializaci�n de dimensiones y variables base
	Dimension tablero[4,6];
	Dimension visible[4,6];
	
	encontroTriadaJ1 <- Falso;
	encontroTriadaJ2 <- Falso;
	triadasJ1 <- 0;
	triadasJ2 <- 0;
	opcMenu <- 0;
	
	animacionCarga;
	// --- MEN� PRINCIPAL ---
	Mientras opcMenu <> 3 Hacer
		Limpiar Pantalla;
		Escribir "=====================================";
		Escribir "      FINDING THE NUMBER - MEN�      ";
		Escribir "=====================================";
		Escribir "1. Jugar";
		Escribir "2. Acerca de...";
		Escribir "3. Salir";
		Escribir "Seleccione una opci�n: ";
		Leer opcMenu;
		
		Segun opcMenu Hacer
			1:
				// Inicio del juego
				Limpiar Pantalla;
				mostrarPortada;
				pedirIniciales(iniJ1, iniJ2);
				IniciarTablero(tablero);
				
				// Inicializar matriz visible
				Para i <- 0 Hasta 3 Hacer
					Para j <- 0 Hasta 5 Hacer
						visible[i,j] <- "??";
					FinPara
				FinPara
				
				// Configuraci�n inicial de partida
				turno <- azar(2);
				puntoJ1 <- 0;
				puntoJ2 <- 0;
				tableroLleno <- Falso;
				
				// BUCLE PRINCIPAL DE LA PARTIDA
				Mientras tableroLleno == Falso Hacer
					
					Limpiar Pantalla;
					Escribir "Puntos     ", iniJ1, ":", puntoJ1,"    ", iniJ2, ": ", puntoJ2;
					mostrarTablero(visible);
					
					Si turno == 0 Entonces
						Escribir "TURNO DE: ", iniJ1;
					SiNo
						Escribir "TURNO DE: ", iniJ2;
					FinSi
					
					// Lanzar dados con garant�a de existencia (Punto #11)
					Repetir
						sumaValida <- Falso;
						dado1 <- azar(12);
						dado2 <- azar(12);
						suma <- dado1 + dado2;
						
						Si suma == 0 Entonces
							sumaValida <- Verdadero;
						SiNo
							Para i <- 0 Hasta 3 Hacer
								Para j <- 0 Hasta 5 Hacer
									Si tablero[i,j] == suma Y visible[i,j] == "??" Entonces
										sumaValida <- Verdadero;
									FinSi
								FinPara
							FinPara
						FinSi
					Hasta Que sumaValida == Verdadero;
					
					Escribir "Presione una tecla para lanzar el dado 1";
					Esperar Tecla;
					Escribir "DADO 1: ", dado1;
					Escribir "";
					Escribir "Presione una tecla para lanzar el dado 2";
					Esperar Tecla;
					Escribir "DADO 2: ", dado2;
					Escribir "==============";
					Escribir "Suma: ", suma;
					Escribir "";
					
					//Regla del doble cero
					Si suma == 0 Entonces
						Escribir "�Doble cero! No ganas puntos y pierdes el turno.";
						Esperar 4 Segundos;
					SiNo
						oportunidades <- 0;
						acerto <- Falso;
						
						Mientras oportunidades < 2 Y acerto == Falso Hacer
							Escribir "--- OPORTUNIDAD ", oportunidades + 1, " ---";
							
							//Valida que el # ingresado est� dentro del rango de la matriz
							Repetir
								Escribir "Ingrese fila (1-4): ";
								Leer fila;
								
								Si fila < 1 O fila > 4 Entonces
									Escribir "ERROR: Fila fuera de rango.";
								FinSi
							Hasta Que fila >= 1 Y fila <= 4;
							
							Repetir
								Escribir "Ingrese columna (1-6): ";
								Leer columna;
								
								Si columna < 1 O columna > 6 Entonces
									Escribir "ERROR: Columna fuera de rango.";
								FinSi
							Hasta Que columna >= 1 Y columna <= 6;
							
							Si tablero[fila-1, columna-1] == suma Entonces
								acerto <- Verdadero;
								
								Si turno == 0 Entonces
									visible[fila-1, columna-1] <- iniJ1;
									puntoJ1 <- puntoJ1 + 1;
									Escribir "�Correcto! +1 punto para ", iniJ1;
								SiNo
									visible[fila-1, columna-1] <- iniJ2;
									puntoJ2 <- puntoJ2 + 1;
									Escribir "�Correcto! +1 punto para ", iniJ2;
								FinSi
								
								// Aqu� ir�a el conteo de triadas final (Punto #15)
								Esperar 2 Segundos;
							SiNo
								Escribir "Incorrecto, intent� de nuevo";
								oportunidades <- oportunidades + 1;
							FinSi
						FinMientras
					FinSi
					
					// Verifica si el tablero est� lleno
					tableroLleno <- Verdadero;
					Para i <- 0 Hasta 3 Hacer
						Para j <- 0 Hasta 5 Hacer
							Si visible[i,j] == "**" Entonces
								tableroLleno <- Falso;
							FinSi
						FinPara
					FinPara
					
					// Cambio de turno
					Si turno == 0 Entonces 
						turno <- 1; 
					SiNo 
						turno <- 0;
					FinSi
				FinMientras
				
				triadasJ1 <- 0; triadasJ2 <- 0;
				
				//Triadas en filas
				Para i <- 0 Hasta 3 Con Paso 1 Hacer
					encontroTriadaJ1 <- Falso;
					encontroTriadaJ2 <- Falso;
					
					Para j <- 0 Hasta 3 Con Paso 1 Hacer
						Si (encontroTriadaJ1 == Falso) Y (visible[i,j] == iniJ1 Y visible[i,j+1] == iniJ1 Y visible[i,j+2] == iniJ1) Entonces
							triadasJ1 <-triadasJ1 + 1;
							encontroTriadaJ1 <- Verdadero;
						FinSi
						
						Si (encontroTriadaJ2 == Falso) Y (visible[i,j] == iniJ2 Y visible[i,j+1] == iniJ2 Y visible[i,j+2] == iniJ2) Entonces
							triadasJ2 <- triadasJ2 + 1;
							encontroTriadaJ2 <- Verdadero;
						FinSi
					FinPara
				FinPara
				
				//Triadas en columnas
				Para j <- 0 Hasta 5 Con Paso 1 Hacer
					encontroTriadaJ1 <- Falso; 
					encontroTriadaJ2 <- Falso;
					Para i <- 0 Hasta 1 Con Paso 1 Hacer
						Si (encontroTriadaJ1 == Falso) Y (visible[i,j] == iniJ1 Y visible[i+1,j] == iniJ1 Y visible[i+2,j] == iniJ1) Entonces
							triadasJ1 <- triadasJ1 + 1; encontroTriadaJ1 <- Verdadero;
						FinSi
						
						Si (encontroTriadaJ2 == Falso) Y (visible[i,j] == iniJ2 Y visible[i+1,j] == iniJ2 Y visible[i+2,j] == iniJ2) Entonces
							triadasJ2 <- triadasJ2 + 1; encontroTriadaJ2 <- Verdadero;
						FinSi
					FinPara
				FinPara
				
				puntoJ1 <- puntoJ1 + (triadasJ1 * 3);
				puntoJ2 <- puntoJ2 + (triadasJ2 * 3);
				
				// Final del juego
				Limpiar Pantalla;
				Escribir "=============================";
				Escribir "        FIN DEL JUEGO        ";
				Escribir "=============================";
				Escribir "RESULTADOS ", iniJ1,":";
				Escribir "Puntos de juego: ", puntoJ1 - (triadasJ1 * 3), " | Triadas: ", triadasJ1 * 3, " | Total: ", puntoJ1;
				Escribir "==============================";
				Escribir "RESULTADOS ", iniJ2,":";
				Escribir "Puntos de juego: ", puntoJ2 - (triadasJ2 * 3), " | Triadas: ", triadasJ2 * 3, " | Total: ", puntoJ2;
				Escribir "=============================";
				
				Si puntoJ1 > puntoJ2 Entonces
					Escribir "�GANADOR: ", iniJ1, "!";
				SiNo
					Si puntoJ2 > puntoJ1 Entonces
						Escribir "�GANADOR", iniJ2, "!";
					SiNo
						Escribir "�Empate!";
					FinSi
				FinSi
				Escribir "Presione una tecla para volver al men�...";
				Esperar Tecla;
				
			2:
				acercaDe;
				Escribir "Presione una tecla para volver al men�...";
				Esperar Tecla;
			3:
				Escribir "Saliendo del sistema...";
			De Otro Modo:
				Escribir "Opci�n no v�lida.";
				Esperar 1 Segundo;
		FinSegun
	FinMientras
FinProceso

subproceso mostrarPortada // Reglas del juego
	Escribir "=========================================";
	Escribir"    CENTRO DE APRENDIZAJE HOUSE SCHOOL    ";
	Escribir "=========================================";
	Escribir" Bienvenidos al juego: Finding the number ";
	Escribir"=========================================";
	Escribir"";
	Escribir" Intrucciones del juego:                  ";
	Escribir"";
	Escribir"1. El juego es para 2 jugadores.";
	Escribir"2. Se sortea aleatoriamente quien inicia.";
	Escribir"3. En un turno lanzas 2 dados(cada uno del 0 al 12).";
	Escribir"4. La suma de los dados es el numero que debes localizar en el tablero.";
	Escribir"5. Debes indicar la fila y columna donde crees que esta el numero.";
	Escribir"6. Si aciertas, ganas 1 punto y tu inicial aparece en el tablero.";
	Escribir"7. Tienes 2 oportunidades por turno para acertar";
	Escribir"8. Triadas de 3 iniciales consecutivas en fila o columna valen 3 puntos extra.";
	Escribir"9. Gana quien acumule mas puntos.";
	Escribir"";
	Escribir"Presione una tecla para continuar...";
	Esperar Tecla;
	Limpiar Pantalla;
	
FinSubProceso

Subproceso pedirIniciales(iniJ1 por referencia, iniJ2 por referencia) //Condicciones de las iniciales
	Definir j1, j2 como caracter;
	Definir valido Como Logico;
	
	valido <- Falso;
	
	//Iniciales jugador 1
	Escribir "===========================================";
	Mientras valido == Falso hacer
		Escribir "Ingrese las iniciales del jugador 1";
		Escribir "Ejemplo: Jonathan Moreno -> JM";
		Leer j1;
		
		Si Longitud(j1) == 2 entonces 
			iniJ1 <- mayusculas(j1);
			valido <- Verdadero;
		SiNo 
			Escribir "";
			Escribir"ERROR...Debe de ingresar �nicamente las iniciales...";
		FinSi
	FinMientras
	
	valido <- Falso;
	
	//Iniciales jugador 2
	Escribir "===========================================";
	Mientras valido == Falso hacer 
		Escribir "Ingrese las iniciales del jugador 2";
		Escribir "Ejemplo: Jonathan Moreno -> JM";
		Leer j2;
		
		Si Longitud(j2) == 2 entonces 
			iniJ2 <- mayusculas(j2);
			valido <- Verdadero;
		SiNo 
			Escribir "";
			Escribir"ERROR...Debe de ingresar �nicamente las iniciales...";
		FinSi
	FinMientras
	
	Limpiar Pantalla;
	Escribir "===========================================";
	Escribir " Jugador 1: ", iniJ1;
	Escribir " Jugador 2: ", iniJ2;
	Escribir "===========================================";
	Escribir "Presione una tecla para continuar...";
	Esperar Tecla;
	
FinSubProceso

Subproceso acercaDe
	
    Limpiar Pantalla;
    Escribir "===========================================";
    Escribir "       INFORMACI�N DEL PROYECTO            ";
    Escribir "===========================================";
	Escribir "Proyecto: Finding the number";
    Escribir "Integrantes:";
    Escribir "1. Erick Lopez";
    Escribir "2. Anais Martinez";
    Escribir "3. Alejandra [Apellido]";
	Escribir "Curso: Fundamentos de Programaci�n";
	Escribir "Profesor: Jonathan Moreno N��ez";
	Escribir "Fecha de entrega: 27/04/2026";
    Escribir "===========================================";
	
FinSubProceso

SubProceso animacionCarga
    Definir i, j, retraso Como Entero;
    Definir textoCarga Como Cadena;
	
	textoCarga <- "               Cargando";
	
    Para i <- 1 Hasta 16 Hacer
        Limpiar Pantalla;
        Escribir "";
        Escribir "";
        Escribir "";
        
        // Alternamos el texto para el efecto de parpadeo
        Si (i MOD 2 = 0) Entonces
            
            Escribir Sin Saltar textoCarga;
            Escribir Sin Saltar "...";
        SiNo
			
            Escribir Sin Saltar textoCarga;
        FinSi
        Escribir ""; 
		
        //Este sirve para hacer tiempo entre cada parpadeo
		Para retraso <- 1 Hasta 220000 Hacer
            
        FinPara
        
    FinPara
    
    Escribir "";
    Escribir "               �Ready!";
    Esperar 1.5 Segundos;
    Limpiar Pantalla;
FinSubProceso

Subproceso IniciarTablero(tablero Por Referencia)
    Definir nums, i, j, k, temp, pos Como Entero;
	
	Dimension nums[24];
	
    // Llenar con n�meros del 1 al 24
    Para i <- 0 Hasta 23 Hacer
        nums[i] <- i + 1;
    FinPara
	
    // Mezclar los n�meros
    Para i <- 0 Hasta 23 Hacer
        pos <- i + azar(24 - i);
        temp <- nums[i];
        nums[i] <- nums[pos];
        nums[pos] <- temp;
    FinPara
	
    // Colocar en el tablero
    k <- 0;
	
    Para i <- 0 Hasta 3 Hacer
        Para j <- 0 Hasta 5 Hacer
            tablero[i,j] <- nums[k];
            k <- k + 1;
        FinPara
    FinPara
	
FinSubProceso

Subproceso mostrarTablero(visible Por Referencia)
    Definir i, j Como Entero;
    
	// Mostrar tablero
	Escribir "=====================================";
	Escribir "         TABLERO DEL JUEGO           ";
	Escribir "=====================================";
	Escribir "     C1    C2    C3    C4    C5    C6 ";
	
	Para i <- 0 Hasta 3 Hacer
		Escribir Sin Saltar "F", i + 1, " ";
		Para j <- 0 Hasta 5 Hacer
			Escribir Sin Saltar "  ", visible[i,j], "  ";
		FinPara
		Escribir "";
	FinPara
	Escribir "=====================================";
FinSubProceso
