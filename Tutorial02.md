## Unittest Framework
De por sí, selenium no trae ninguna herramineta para hacer testing, por lo que usaremos este módulo de python para hacerlo.
Esta nos permite una forma de comprobar el correcto funcionamiento del código y crear pruebas en el mismo.

Primero en el ```main.py``` importaremos ```unittest``` (no hay que instalar nada), después crearemos una clase donde iran nuestros casos de prueba y la heredaremos de ```TestCase```. Estará compuesta de tres partes digamos, tendrá dos métodos(```setUp``` y ```tearDown```) que nos van a permitir definir instrucciones que van a ser ejecutadas antes o después de cada test. Y por último, al medio de estos métodos tendremos más, los cuales serán todos nuestros tests.

También habrá dos documentos más, el ```page.py``` y el ```locator.py```. En el primero usaríamos el modelo pageObjects, en donde se refiere a crear objetos por cada página de nuestra web. Y el segundo file se encontrarán todas las locaciones de nuestro elementos.

El ```main.py``` nos quedaría algo así:

```python

import unittest
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
import page # importamos el page.py

class VilladaTest(unittest.TestCase):

    def setUp(self):
        self.driver = webdriver.Chrome('chromedriver.exe')
        self.driver.get('https://www.itsv.edu.ar/itsv/index.php/')
        # creamos instancias de nuestras páginas
        self.index_page = page.IndexPage(self.driver) 
        self.search_result_page = page.SearchResultsPage(self.driver)


    def test_search(self):
        # usamos las funcionalidades de nuestros pageObjects
        # lo que estamos haciendo es buscar formularios y verificar si encontró algo
        self.index_page.search('formularios')
        self.driver.implicitly_wait(3)
        assert self.search_result_page.is_results_found()
    
    
    def tearDown(self):
        self.driver.quit()


if __name__ == '__main__':
    unittest.main() 
    # manera simple de correr nuestros tests 
    # y nos provee una interfaz del script de los test en la terminal

```

El ```page.py``` nos quedaría así:

```python

from selenium.webdriver.common.keys import Keys
from locator import IndexPageLocators, SearchResultsPageLocators #importamos locator.py

class BasePage():
    def __init__(self, driver):
        self.driver = driver
        # clase padre para iniciarlizar la base de la página

class IndexPage(BasePage): # heredamos de BasePage

    def search(self, name):
        self.search_box = self.driver.find_element(*IndexPageLocators.SEARCH_BOX)
        self.search_box.send_keys(name + Keys.ENTER)

class SearchResultsPage(BasePage):

    def is_results_found(self):
        return "Formularios para el Alumno" in self.driver.find_element(*SearchResultsPageLocators.LINK_FORM).text

```

Y el ```locator.py``` quedaría así:

```python

from selenium.webdriver.common.by import By

class IndexPageLocators(object):
    SEARCH_BOX = (By.NAME, 'searchword')


class SearchResultsPageLocators(object):
    LINK_FORM = (By.LINK_TEXT, 'Formularios para el Alumno')

```
