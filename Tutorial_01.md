# Tutorial_Escrito_Selenium

Selenium Python nos provee una simple API para automatizar paginas web.
Con esta podemos acceder a todas las funcionalidades que nos provee Selenium Webdriver, con la cual podemos usar browsers tales como chorme, firefox, etc.

## Instalación
Primero deberemos instalar selenium usando el gestor de paquetes de python (pip)

<pre>
pip install selenium
</pre>

Después, selenium requiere un driver para interactuar con el browser. Por lo que tenderemos que proceder a instalar la misma versión que estemos usando en nuestra computadora. [Instale aqui](https://www.selenium.dev/documentation/en/webdriver/driver_requirements/#quick-reference) 

## Empezando
Una vez que ya hemos instalado todo, procederemos a importar todas las librerias de selenium. Luego crearemos una instancia de nuestro webdriver, le decimos que vamos a trabajar con Chrome, y le pasamos el archivo.exe que hemos descargado anteriormente.

```python
from selenium import webdriver

driver = webdriver.Chrome('chromedriver.exe') # Especificamos la ruta en donde se encuentra nuestro .exe

```
