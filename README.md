# U1-PROYECTO-INTEGRADOR
------
Este proyecto integrador corresponde a la Unidad 1 y está basado en el desarrollo de un escenario procedural, al cual se le agregó la animación de una cámara que recorre un camino definido dentro del entorno.

El objetivo principal es aplicar los conceptos fundamentales de la graficación por computadora, integrando generación procedural y movimiento dinámico de cámara para mejorar la visualización del entorno

----
1. Importación de librerías
```Python
import bpy
import math
```
• bpy: Permite manipular Blender mediante scripts.

• math: Se usa para funciones trigonométricas como cos(), sin() y radians().

------
2. Función para crear materiales
```Python
def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat
```

Esta función:

• Crea un nuevo material en Blender.

• Asigna un color RGB.

• Retorna el material creado.

Parámetros:

• nombre: Nombre del material.

• color_rgb: Tupla con valores RGB (ejemplo: (0.1, 0.1, 0.1)).

----
3. Función principal: generar_pasillo_curvo()
```Python
def generar_pasillo_curvo():
```
Esta función contiene toda la lógica de creación del escenario.

-----
4. Limpieza de la escena
```Python
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```

• Selecciona todos los objetos existentes.

• Los elimina.

Esto asegura que el script genere la escena desde cero.

----
5. Creación de materiales
```Python
mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))
```
Se crean dos materiales:

• ParedOscura → Gris oscuro.

• ParedDetalle → Color rojizo.

Cada lado del pasillo tendrá un material diferente.

----
6. Parámetros configurables
```Python
radio = 10
ancho_pasillo = 2
segmentos = 20
angulo_total = math.radians(90)
```
Explicación:

Parámetro	Función

• radio:	Tamaño de la curva

• ancho_pasillo: Distancia entre paredes

• segmentos:	Cantidad de bloques que forman la curva

• angulo_total:	Ángulo total del pasillo (90° convertidos a radianes)

-----
7. Bucle principal: generación de paredes
```Python
for i in range(segmentos):
```
Este ciclo crea cada segmento del pasillo.

----
7.1 Cálculo del ángulo actual
```Python
angulo = angulo_total * (i / segmentos)
```
Divide el ángulo total entre los segmentos para distribuirlos uniformemente.

-----
7.2 Posición central en la curva
```Python
x = radio * math.cos(angulo)
y = radio * math.sin(angulo)
```
Aquí se aplican fórmulas trigonométricas de movimiento circular:

• cos() → Coordenada X

• sin() → Coordenada Y

Esto posiciona los cubos formando una curva.

-----
7.3 Cálculo del desplazamiento lateral (offset)
```Python
offset_x = ancho_pasillo * math.cos(angulo + math.pi/2)
offset_y = ancho_pasillo * math.sin(angulo + math.pi/2)
```
Se suma math.pi/2 (90°) para obtener la dirección perpendicular.

Esto permite separar:

• Pared izquierda

• Pared derecha

----
8. Creación de paredes
Pared izquierda
```Python
bpy.ops.mesh.primitive_cube_add(location=(x + offset_x, y + offset_y, 1))
pared_izq = bpy.context.active_object
pared_izq.data.materials.append(mat_pared_a)
pared_izq.scale.z = 1.5
```
• Se crea un cubo.

• Se posiciona usando la curva + desplazamiento.

• Se asigna material.

• Se escala en altura.

----
Pared derecha
```Python
bpy.ops.mesh.primitive_cube_add(location=(x - offset_x, y - offset_y, 1))
pared_der = bpy.context.active_object
pared_der.data.materials.append(mat_pared_b)
pared_der.scale.z = 1.5
```
Misma lógica pero restando el desplazamiento para colocarlo del lado opuesto.

----
9. Creación del suelo
```Python
bpy.ops.mesh.primitive_circle_add(radius=radio+ancho_pasillo, location=(0,0,0))
```
Se agrega un círculo base que actúa como suelo del pasillo.

El radio es mayor para cubrir ambas paredes.

----
10. Ejecución de la función
```Python
generar_pasillo_curvo()
```Python
Llama a la función y ejecuta todo el proceso.
```
-----
https://github.com/user-attachments/assets/d50c1083-1f2d-4979-8287-cf8bdc755335
--




