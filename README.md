A largo de este repositorio se mostrara la elaboracion de un programa para la creacion de un minijuego, en este caso sera la creacion de un BuscaMinas.
Para ello se necesitara 3 lenguajes, los cuales son HTML, JavaScrip (JS) y CSS.
A continuacion se lo que debe contener.

Para el cuerpo de index.html se ocupo el siguiente.

    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="estilo.css">
        <title>Buscaminas</title>
    </head>
    <body>
        <h1>Buscaminas</h1>
        <div id="mensaje"></div>
        <div id="tablero" class="tablero"></div>
        <button class="Boton_Reinicio" onclick="reiniciarJuego()">Reiniciar</button>
    </body>
    </html>

Haciendo uso de JavaScrip para las funciones para la creacion del juego se utilizo lo siguiente.

Para comenzar deben de existir las siguientes variables, las cuales contienen informacion sobre lo que se debe de realizar.

        const ROWS = 8;
        const COLS = 8;
        const MINAS = 10;
A continuacion se muestran algunas variables que se ocuparan para la manipulacion del tablero, de la colocacion de las minas y de las casillas.

        let tablero = document.getElementById('tablero');
        let casillas = [];
        let ubicacionMinas = [];
        let casillasReveladas = 0;

Aqui se presenta la funcion para crear e inicializar el tablero.

        function inicializaTablero() {
            tablero.innerHTML = '';
            casillas = [];
            for (let i = 0; i < ROWS; i++) {
                casillas[i] = [];
                for (let j = 0; j < COLS; j++) {
                    let casilla = document.createElement('div');
                    casilla.classList.add('casilla');
                    casilla.dataset.row = i;
                    casilla.dataset.col = j;
                    casilla.dataset.state = 'hidden';
                    casilla.addEventListener('click', manejarClickCasilla);
                    tablero.appendChild(casilla);
                    casillas[i][j] = casilla;
                }
            }
        }

Con esta funcion se hace la colocacion (obtencion) de las ubicaciones de donde se colocaran las minas de manera aleatoria haciendo uso de la opcion Math.random().

        function colocarMinas() {
            ubicacionMinas = [];
            let minasColocadas = 0;
            while (minasColocadas < MINAS) {
                let row = Math.floor(Math.random() * ROWS);
                let col = Math.floor(Math.random() * COLS);
                if (!casillas[row][col].dataset.mine) {
                    casillas[row][col].dataset.mine = 'true';
                    ubicacionMinas.push({ row, col });
                    minasColocadas++;
                }
            }
        }

En esta funcion se hace uso de eventos para cuando una casilla se le hace click.

        function manejarClickCasilla(event) {
            let casilla = event.target;
            let row = parseInt(casilla.dataset.row);
            let col = parseInt(casilla.dataset.col);
            if (casilla.dataset.state === 'hidden') {
                if (casilla.dataset.mine === 'true') {
                    revelarMinas();
                    mostrarMensaje('¡Has perdido!', true);
                } else {
                    let count = contarMinasCercanas(row, col);
                    casilla.textContent = count || '';
                    casilla.dataset.state = 'revealed';
                    casilla.classList.add('revealed');
                    casillasReveladas++;
                    if (count === 0) {
                        revelarCasillasAdyacentes(row, col);
                    }
                    if (casillasReveladas === ROWS * COLS - MINAS) {
                        mostrarMensaje('¡Has ganado!', false);
                    }
                }
            }
        }
![image](https://github.com/user-attachments/assets/611d7913-9ebb-4e91-ad67-ec697dbd1fa1)


Con esta funcion hace el conteo de las minas existentes en el tablero.

        function contarMinasCercanas(row, col) {
            let count = 0;
            for (let i = Math.max(0, row - 1); i <= Math.min(row + 1, ROWS - 1); i++) {
                for (let j = Math.max(0, col - 1); j <= Math.min(col + 1, COLS - 1); j++) {
                    if (casillas[i][j].dataset.mine === 'true') {
                        count++;
                    }
                }
            }
            return count;
        }

Descubre las casillas alrededor de la casilla presionada.

        function revelarCasillasAdyacentes(row, col) {
            for (let i = Math.max(0, row - 1); i <= Math.min(row + 1, ROWS - 1); i++) {
                for (let j = Math.max(0, col - 1); j <= Math.min(col + 1, COLS - 1); j++) {
                    let casilla = casillas[i][j];
                    if (casilla.dataset.state === 'hidden') {
                        let count = contarMinasCercanas(i, j);
                        casilla.textContent = count || '';
                        casilla.dataset.state = 'revealed';
                        casilla.classList.add('revealed');
                        casillasReveladas++;
                        if (count === 0) {
                            revelarCasillasAdyacentes(i, j);
                        }
                    }
                }
            }
        }
![image](https://github.com/user-attachments/assets/185aca29-7e7c-4487-870f-989c6a57d067)


Revela las minas en el tablero.

        function revelarMinas() {
            ubicacionMinas.forEach(location => {
                let { row, col } = location;
                casillas[row][col].textContent = '*';
                casillas[row][col].classList.add('mine', 'revealed');
            });
        }
        
![image](https://github.com/user-attachments/assets/a7d5d09a-521a-4258-9e15-01f3782e2762)


Con esta funcion se genera un mensaje, ya sea que el jugador haya ganado o haya perdido.

        function mostrarMensaje(message, isGameOver) {
            let mensaje = document.getElementById('mensaje');
            mensaje.textContent = message;
            mensaje.style.color = isGameOver ? 'red' : 'green';
        }
![image](https://github.com/user-attachments/assets/c673d8dc-9fa9-4dc4-8626-88d9ca0d1db6) ![image](https://github.com/user-attachments/assets/cba1fa81-af3d-430c-bdb4-8d653480e5fd)



Funcion para reinicial el programa tras precionar el boton de reinicio.

        function reiniciarJuego() {
            casillasReveladas = 0;
            mostrarMensaje('');
            inicializaTablero();
            colocarMinas();
        }

![image](https://github.com/user-attachments/assets/b3884a54-9a90-4c48-ae59-3c94b7d725d9) ![image](https://github.com/user-attachments/assets/beeaef22-a182-4b08-93a0-6cd04546b21e)



Con estas funciones se da inicio a todo lo que es la ejecucion del programa.

        inicializaTablero();
        colocarMinas();

![image](https://github.com/user-attachments/assets/0bd48771-063d-4faf-89a7-e2d5d6a1928f)

A su vez se hace uso de los siguientes estilos para la visualizacion tanto del titulo y el tablero, asi como tambien de todo lo que se puede visualizar a lo largo del juego.


Para cuarpo (body) se uso el siguiente estilo.

    body {
        font-family: Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        min-height: 100vh;
        margin: 0;
        background-color: #000;
    }

Para el titulo de Buscaminas.

    h1 {
        text-align: center;
        color: #f4f4f9;
    }

Para los mensajes

    #mensaje {
        margin-bottom: 10px;
        font-size: 18px;
        font-weight: bold;
    }

Para el tablero.

    .tablero {
    display: grid;
    grid-template-columns: repeat(8, 30px);
    gap: 2px;
    margin-bottom: 10px;
    }

Para las casillas sin descubrir.

    .casilla {
        width: 30px;
        height: 30px;
        background-color: #198434;
        border: 1px solid #58413b;
        text-align: center;
        line-height: 30px;
        font-size: 18px;
        cursor: pointer;
    }

Para las casillas descubiertas.

    .casilla.revealed {
        background-color: #e1a66b;
    }

Para las casillas que contienen minas.

    .casilla.mine {
        background-color: red;
        color: white;
    }

Para el boton de reinicio.

    button {
        padding: 10px 20px;
        font-size: 16px;
        font-weight: bold;
        color: white;
        background-color: #007BFF;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }
    
    button:hover {
        background-color: #0056b3;
    }
    
    button:active {
        background-color: #003f7f;
    }
