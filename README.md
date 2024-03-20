# Practica3

# Práctica 3A - Generación de una pagina web
## Objetivo

El objetivo de este proyecto es crear un servidor web en un dispositivo ESP32 utilizando el sistema de archivos SPIFFS para alojar una página web. La página web contiene elementos HTML básicos y se accede a través de la red WiFi.

## Procedimiento 

1. Se cargó el código proporcionado en el dispositivo ESP32 utilizando el entorno de desarrollo PlatformIO.
2. Se aseguró que el archivo `web.html` estuviera en el directorio data del proyecto.
3. Se verificó la conexión WiFi y el funcionamiento del servidor web.
4. Se abrió un navegador web en un ordenador o dispositivo móvil conectado a la misma red WiFi.
5. Se ingresó la dirección IP del ESP32 en la barra de direcciones del navegador.

## Código utilizado
### `main.cpp`

```cpp
#include <Arduino.h>
#include <SPIFFS.h>
// #include "SPIFFS.h"
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "MOVISTAR_D220"; 
const char* password = "*********"; 

WebServer server;

String webContent; 

void handle_root() {
  server.send(200, "text/html", webContent);
}

String loadFile(String fileName) { 
  File file = SPIFFS.open(fileName);
  String content = "";

  if (!file) {
    Serial.println("Error al abrir el archivo");
    return content;
  }

  while (file.available()) {
    char c = file.read();
    content += c;
  }

  file.close();
  return content;
}

void setup() {
  Serial.begin(115200);

  if (!SPIFFS.begin(true)) {
    Serial.println("Error al montar SPIFFS");
    return;
  }

  webContent = loadFile("/web.html");
  delay(1000);

  Serial.println("Conectando a la red WiFi...");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conexión WiFi establecida");
  Serial.print("Dirección IP: ");
  Serial.println(WiFi.localIP());

  server.begin();
  Serial.println("Servidor HTTP iniciado");
  
  server.on("/", handle_root);
}

void loop() {
  server.handleClient();
}


```

## Fichero .html utilizado
### `web.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página Web</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
        }
        header {
            background-color: #333;
            color: #fff;
            padding: 20px;
            text-align: center;
        }
        nav {
            background-color: #666;
            padding: 10px;
            text-align: center;
        }
        nav a {
            color: #fff;
            text-decoration: none;
            margin: 0 10px;
        }
        section {
            padding: 20px;
            margin: 20px;
            background-color: #fff;
        }
        footer {
            background-color: #333;
            color: #fff;
            padding: 10px;
            text-align: center;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
</head>
<body>
    <header>
        <h1>Mi Página Web</h1>
    </header>
    <nav>
        <a href="#">Inicio</a>
        <a href="#">Acerca de</a>
        <a href="#">Contacto</a>
    </nav>
    <section>
        <h2>¡Bienvenido!</h2>
        <p>Horario.</p>
        <img src="https://upcfabterrassa.upc.edu/ca/shared/img-2545-1.jpg" alt="Mi imagen">
    </section>
</body>
</html>

```
## Resultados y salida del terminal


Se observó que la página web se cargó correctamente en el navegador web.

### Salida del terminal:

```cpp
Conectando a la red WiFi...
MOVISTAR_D220
....
Conexión WiFi establecida
Dirección IP: 192.168.1.41
Servidor HTTP iniciado
```

# Práctica 3B - Comunicación bluetooth con el movil

## Objetivo

## Procedimiento

## Resultados y salida del terminal

