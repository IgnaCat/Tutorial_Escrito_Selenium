# Tutorial_Escrito_Selenium

Selenium Python nos provee una simple API para automatizar paginas web.
Con esta podemos acceder a todas las funcionalidades que nos provee Selenium Webdriver, con la cual podemos usar browsers tales como chorme, firefox, etc.

## Instalación
Primero deberemos instalar selenium usando el gestor de paquetes de python (pip)

<pre>
pip install selenium
</pre>

Después, selenium requiere un driver para interactuar con el browser, por lo que tenderemos que proceder a instalar la misma versión que estemos usando en nuestra computadora. [Instale aqui](https://www.selenium.dev/documentation/en/webdriver/driver_requirements/#quick-reference) 

## Empezando
Una vez que ya hemos instalado todo, procederemos a importar todas las librerias de selenium. Luego crearemos una instancia de nuestro webdriver, le decimos que vamos a trabajar con Chrome, y le pasamos el archivo.exe que hemos descargado anteriormente.

```python
from selenium import webdriver

driver = webdriver.Chrome('/PATH/chromedriver.exe') # Especificamos el directorio en donde se encuentra nuestro .exe

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

## Ejemplo Villada
Ahora seguiremos con lo hecho anteriormente, pero le agregaremos que después de buscar, apriete en "Formularios para el Alumno" y nos escriba en la terminal los distintos tipos de formulario que existen.

Seguiriamos usando el mismo procedimiento dicho anteriormente y nos quedaría algo asi:

```python
import time
from selenium import webdriver

driver = webdriver.Chrome('chromedriver.exe')
driver.get('https://www.itsv.edu.ar/itsv/index.php/')

search_box = driver.find_element_by_name('searchword')
search_box.send_keys('formularios')
search_btn = driver.find_element_by_name('search')
search_btn.submit()

form_link = driver.find_element_by_link_text('Formularios para el Alumno').click()
time.sleep(2)
article = driver.find_element_by_xpath('//*[@id="art-main"]/div/div[1]/div/div/div/div/article/div/div')
forms_ul = article.find_elements_by_tag_name('ul')
for form in forms_ul:
    form_li = form.find_element_by_tag_name('li')
    form_strong = form_li.find_element_by_tag_name('strong')
    print(form_strong.text)

driver.quit()

```


## Waits
Como dijimos anteriormente, usar el time.sleep no es la mejor manera para esperar a que carguen ciertos elementos de nuestra página web, ya que estamos esperano un tiempo fijo, osea estamos pausando la ejecucíon del script unos segundos.
Para eso Selenium Webdiver nos provee dos tipos de waits, el implicit wait y el explicit wait, para que la ejecución sea mas rápida  y dinámica. 
El implicit wait espera a q el doom cargue un elemento en una cantidad n de tiempo antes de que te tire un exception error.

Su utilización sería asi:

```python
driver.implicitly_wait(5) # Espere 5 segundos

```

El explicit wait no solo espera a q el elemento exista, sino q tenga una condición dada antes de tirar un ElementNotVisibleException error, es decir que el webdriver espera por cierta condición.

Su implementación sería algo así:

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

search_btn = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.NAME, 'search')))
# Esperamos 5 segundos a que el botón sea clickable
```

## Asserts
Los asserts son verificaciones, pero tienen una diferencia, cuando algo no funciona se corta la ejecución del script. Entonces, lo que hace es devolverte un true o un false, si es true la prueba sigue, si es false se corta.

Los asserts que trae Selenium Webdriver son algo asi:

```python
assert form_strong.text.startswith('Formulario')
assert form_strong.tag_name == 'strong'

```

## Teclas Especiales
Selenium Webdriver nos proporciona la "ventaja" de poder usar las teclas especiales, no simplemente las alfanumericas del teclado.
Con esto un ejemplo de lo que podemos hacer, es cuando buscamos formularios en el buscador, que se aprete enter, en vez de tener que identficar el botón de buscar y de hacer que haga click. Entonces lo que estaríamos haciendo es ahorrar un poco más de código y simplificarlo.

Seria:

```python
from selenium.webdriver.common.keys import Keys

search_box = driver.find_element_by_name('searchword')
search_box.send_keys('formularios' + Keys.ENTER)

```


Entonces la totalidad del código usando los diferentes temas explicados y el ejemplo del villada, haría que el el script quedara asi:

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

driver = webdriver.Chrome('chromedriver.exe')
driver.get('https://www.itsv.edu.ar/itsv/index.php/')

search_box = driver.find_element_by_name('searchword')
search_box.send_keys('formularios' + Keys.ENTER)

form_link = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.LINK_TEXT, 'Formularios para el Alumno'))).click()
driver.implicitly_wait(3)
article = driver.find_element_by_xpath('//*[@id="art-main"]/div/div[1]/div/div/div/div/article/div/div')
forms_ul = article.find_elements_by_tag_name('ul')
for form in forms_ul:
    form_li = form.find_element_by_tag_name('li')
    form_strong = form_li.find_element_by_tag_name('strong')
    print(form_strong.text)
    assert form_strong.text.startswith('Formulario')
    assert form_strong.tag_name == 'strong'

driver.quit()

```

