A largo de este repositorio se mostrara la elaboracion del programa para la ceracion de un minijuego, en este caso sera la creacion de un Busca Minas.
Para ello se necesitara 3 lenguajes, los cuales son HTML, JavaScrip (JS) y CSS.
A continuacion se muestra el cuerpo del index.html.
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
