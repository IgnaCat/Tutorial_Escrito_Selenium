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

Ahora lo que vamos a hacer es decirle al webdriver que página vamos a usar, en este caso sera https://www.itsv.edu.ar/itsv/index.php y lo que hará es llevarnos a la misma.

Una vez cargada, podemos acceder al DOM usando métodos del objecto driver recién creado.
La mejor manera es abriendo el web browser y usar las herramientas de desarrollador para inspeccionar el contenido de la página.

Lo que queremos hacer es que el script escriba en el buscador "formularios" y que luego se apriete el botón de buscar para ver los resultados. Entonces vamos a empezar a identificar los elementos usando lo mencionado anteriormente(click derecho, inspeccionar), donde podemos ver que el elemento ```<input>``` tiene un atributo ```name="searchword"```. Luego usaremos el método ```find_element_by_name()``` pasandole esto como parámetro y le diremos que le escriba "formularios", a través del método ```send_keys()```.
Después usando el mismo procedimiento, haremos lo mismo con el botón de buscar y le diremos que haga click.

Quedaría algo así:

```python
import time
from selenium import webdriver

driver = webdriver.Chrome('chromedriver.exe')
driver.get('https://www.itsv.edu.ar/itsv/index.php/')
time.sleep(3)

search_box = driver.find_element_by_name('searchword')
search_box.send_keys('formularios')
search_btn = driver.find_element_by_name('search')
search_btn.submit()

driver.quit() # Cierra el webdriver

```

Usamos el módulo time por si la página carga lento, que no empieze a interactuar con elementos que no existen y no salte ningún error.
Esto es una mala práctica pero veremos más adelante como se hace correctamente.
