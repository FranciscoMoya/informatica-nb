# Gráficos de tortuga

Esta práctica de laboratorio tiene dos objetivos:

* Que el alumno aprenda a utilizar funciones auxiliares con una interfaz de programación impuesta.  En términos académicos sería introducir la variante *middle-outwards* en el diseño *top-down*.

* Que el alumno se familiarice con los problemas de las funciones impuras y aprenda a minimizar su impacto.

Esfuérzate por realizar los ejercicios de la forma más limpia posible, de manera que puedan reutilizarse para otros fines.

## Hola mundo de las tortugas

El dibujo con tortugas es simple, solo hay que dirigir el camino de una tortuga virtual, que inicialmente está en el origen, mirando hacia la derecha de la pantalla.

Este podría ser un pequeño programa que dibuja una L en la pantalla.

```
start()
forward(50)
rotate(-pi/2)
forward(100)
show()
```

El conjunto completo de funciones disponibles es el siguiente:

**Función** | **Descripción**
--- | ---
`start()`   | Empieza el dibujo. Usa esta función solo en tus pruebas, no en el código entregado.
`show()`    | Muestra el dibujo en una ventana y espera a que se cierre. Usa esta función solo en tus pruebas, no en el código entregado.
`forward(L)` | Avanza `L` unidades pintando una línea.
`rotate(a)` | Gira el ángulo indicado (en radianes). Si es negativo gira hacia la izquierda.
`skip(L)`   | Igual que `forward` pero sin pintar línea.
`home()`    | Lleva el lápiz al origen y se orienta mirando hacia la derecha.
`goto(x,y)` | Lleva el lápiz a la posición indicada, sin cambiar la orientación.
`color(c)`  | Cambia el color del trazo al indicado (en inglés, e.g. `'black'`).
`width(w)`  | Cambia el grosor del trazo al indicado.



## Funciones para IDLE/PyCharm

``` python
from math import pi
from turtle import forward, color, width
from turtle import rt as rotate
import turtle

def start():
    turtle.setup()
    turtle.speed(0)
    clear()

def clear():
    turtle.clearscreen()
    turtle.hideturtle()
    turtle.radians()
    turtle.tracer(0,0)

def home():
    turtle.penup()
    turtle.home()
    turtle.pendown()

def goto(x,y):
    turtle.penup()
    turtle.goto(x,y)
    turtle.pendown()

def skip(units):
    turtle.penup()
    turtle.forward(units)
    turtle.pendown()

def show():
    turtle.update()
    turtle.done()
```

## Funciones para Jupyter Notebook

``` python
from IPython.display import SVG,display
from math import pi, sin, cos

class Screen(object):    
    def __init__(self, width = 0., height = 0.):
        self.ll = self.ur = width, height
        self.start()
        
    def start(self):
        self._width = 1
        self._color = 'black'
        self.clear()        

    def width(self, w):
        self._width = w

    def color(self, c):
        self._color = c

    def clear(self):
        self.drawing = ''
        self.home()

    def home(self):
        self.pos = (0., 0.)
        self.angle = pi

    def forward(self, units):
        pos1 = self.pos
        self.skip(units)
        self.drawing += self.line(pos1, self.pos)

    def rotate(self, radians):
        self.angle += radians

    def goto(self, x, y):
        self.pos = -x,y
        self.fit()

    def skip(self, units):
        x, y = self.pos
        self.pos = x + units*cos(self.angle), y + units*sin(self.angle)
        self.fit()

    def fit(self):
        x,y = self.pos
        self.ll = min(x,self.ll[0]), min(y,self.ll[1])
        self.ur = max(x,self.ur[0]), max(y,self.ur[1])
        #print(self.ll,self.ur)

    def show(self):
        x1, y1 = self.ll[0] - 5, self.ll[1] - 5
        x2, y2 = self.ur[0] + 5, self.ur[1] + 5
        ret = SVG('''<svg width="{}" height="{}" viewBox="{} {} {} {}"
                      style="transform:rotate(180deg)"
                      xmlns="http://www.w3.org/2000/svg">{}</svg>'''\
               .format(x2 - x1,
                       y2 - y1,
                       x1, y1,
                       x2 - x1, y2 - y1,
                       self.drawing))
        self.clear()
        display(ret)

    def line(self, p1, p2):
        (x1, y1), (x2, y2) = p1, p2
        return '''<line x1="{}" y1="{}" 
                        x2="{}" y2="{}" 
                        stroke-width="{}" stroke="{}"/>'''.format(x1,y1,x2,y2,self._width,self._color)
    
# globals
_screen = Screen()
def forward(L): _screen.forward(L)
def color(c):   _screen.color(c)
def width(w):   _screen.width(w)
def rotate(a):  _screen.rotate(a)
def start():    _screen.start()
def clear():    _screen.clear()
def home():     _screen.home()
def goto(x,y):  _screen.goto(x,y)
def skip(L):    _screen.skip(L)
def show():     _screen.show()
```
