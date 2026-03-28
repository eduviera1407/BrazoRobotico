# Diseño y Control de un Brazo Robótico en CoppeliaSim

Este repositorio contiene la Actividad de Desarrollo 1, que consiste en un tutorial técnico para el diseño, ensamblaje y programación de un brazo robótico básico utilizando CoppeliaSim[cite: 1, 18]. 

**Autor:** Eduardo Roque Viera Santana [cite: 3]

## 📖 Descripción del Proyecto

CoppeliaSim es un potente simulador de robótica de propósito general que permite crear, programar y probar sistemas complejos en un entorno 3D realista con físicas integradas[cite: 9]. [cite_start]Actúa como una máquina de previsión que permite solucionar errores en el programa antes de implementarlo en un robot real[cite: 15].

Este proyecto documenta el proceso completo de creación de un brazo robótico, abarcando desde la generación de primitivas hasta la implementación de scripts de control[cite: 18, 423].

## ⚙️ Conceptos Clave Implementados

* **Primitivas 3D:** Uso de formas básicas como cilindros (para la base) y cuboides (para los eslabones)[cite: 57, 58, 231].
* **Articulaciones (Joints):** Implementación de articulaciones de tipo *Revolute* (rotacionales), que funcionan como bisagras permitiendo el giro alrededor de un eje[cite: 94, 101].
* **Jerarquía y Cinemática:** Estructuración del robot mediante relaciones Padre-Hijo en el árbol jerárquico (*Scene Hierarchy*)[cite: 202, 204, 215]. [cite_start]Cualquier transformación del objeto padre se transmite automáticamente a sus objetos hijos[cite: 216].
* **Programación (Scripts):** Control de los actuadores mediante un script de tipo *Threaded* (hilado) escrito en Lua, el cual se ejecuta en paralelo a la simulación principal permitiendo rutinas secuenciales y el uso de comandos de espera[cite: 433, 439].

## 💻 Código de Control

El "cerebro" del robot es un script asociado a la base fija que controla los dos motores (base y codo)[cite: 423, 437]. [cite_start]El siguiente código genera una secuencia de movimiento continuo[cite: 517]:

```lua
function sysCall_thread()
    -- Obtener los identificadores con la ruta completa (Absoluta)
    local motorBase = sim.getObjectHandle('/Base_Fija/Motor1')
    local motorCodo = sim.getObjectHandle('/Base_Fija/Motor1/Eslabon1/Motor2')

    -- Bucle infinito de movimiento
    while true do
        -- Gira la base 90 grados y levanta el codo
        sim.setJointTargetPosition(motorBase, 1.57)
        sim.wait(2)
        
        sim.setJointTargetPosition(motorCodo, 0.78)
        sim.wait(2)

        -- Devuelve ambos motores a la posición inicial (0 grados)
        sim.setJointTargetPosition(motorBase, 0)
        sim.setJointTargetPosition(motorCodo, 0)
        sim.wait(2)
    end
end
