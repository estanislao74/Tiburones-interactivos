# Tiburones-interactivos
video de como instalarlo:https://www.youtube.com/watch?v=m3kXr_HY_wA

Instalación interactiva en TouchDesigner: una vista aérea de agua oscura con tiburones 3D nadando de forma autónoma, sobre un efecto de agua psicodélico tipo feedback que reacciona al movimiento de las personas frente a un sensor Kinect.

Que hace el proyecto
Detección de movimiento : lee la imagen de profundidad de un sensor Kinect y calcula una máscara de movimiento por diferencia de frames (adaptado para GPUs que no soportan el Optical Flow nativo de Nvidia).
Agua reactiva : un sistema de retroalimentación (blur + edge + zoom + mezcla dry/wet) que genera ondas y un efecto psicodélico tipo "trail" que reacciona al movimiento detectado y decae solo con el tiempo.
Tiburones 3D : 4 modelos de tiburón (malla suavizada en Blender) que nadan de forma autónoma con rumbo propio, orientándose siempre hacia su dirección de avance, virando de vuelta al acercarse al borde del área (nunca desaparecen de la pantalla).
Render final : todo compuesto en una sola imagen (agua + tiburones) lista para proyectar.
Requisitos
Software
TouchDesigner 2025.32460 (No Comercial o Comercial) o superior. Versión recomendada: la misma compilación (2025.32460) para evitar diferencias de comportamiento en operadores como Feedback TOP/POP y Random POP.
Sistema operativo: Windows 10/11 (el proyecto fue desarrollado y probado en Windows 10).
Hardware
GPU: cualquier GPU compatible con TouchDesigner. No hace falta una GPU Nvidia RTX 3000 o superior (el proyecto evita a propuesta el Optical Flow TOP nativo, que si requiere esa gama, y ​​usa diferencia de marcos en su lugar). Probado en NVIDIA GTX 1660 Super.
CPU: cualquier procesador moderno (probado en AMD Ryzen 7).
RAM: 8 GB o más recomendado.
dispositivos externos
Sensor Kinect (v1 o v2). Si usas Kinect v2, necesitas instalar el Kinect SDK v2.0 (Build 1410) de Microsoft. Sin el sensor conectado, el proyecto sigue funcionando (el agua queda en reposo, sin reaccionar), pero no habrá interacción.
Como abrir y ejecutar
Instalar TouchDesigner (ver versión recomendada arriba).
Si vas a usar el sensor Kinect v2, instala el Kinect SDK v2.0 antes de abrir el proyecto.
Abrir el archivo proyecto claude+touchdesinger.toecon TouchDesigner.
Los activos externos (modelo 3D de tiburón) están incluidos en la carpeta assets/con rutas relativas al .toe, no deberia hacer falta reconfigurar nada.
Para ver el resultado final en una ventana dedicada: use el botón de Perform Mode de TouchDesigner (arriba a la izquierda, debajo del menú Archivo). Esta configurado para mostrar el render final ( /project1/FINAL_RENDER/out1).
Revisar la consola de TouchDesigner (barra de errores) al abrir: si el Kinect no está conectado o falta el SDK, va a aparecer un error en el componente KINECT_INPUT/kinect1; esto es esperado si no tienes el sensor a mano y no impide ver el resto de la instalación funcionando (el agua y los tiburones se ven igual, solo sin interacción).
Estructura del proyecto (dentro del .toe)
KINECT_INPUT-- lectura del sensor Kinect (imagen de profundidad).
OPTICAL_FLOW-- detección de movimiento por diferencia de marcos.
container1/feedbackEdgepppp-- sistema de agua (feedback + edge + zoom + mezcla dry/wet).
FISH_SIMULATION-- sistema de peces/tiburones procedurales en GPU (partículas POP), usado en etapas iniciales del proyecto.
FINAL_RENDER-- composición final: agua + los 4 tiburones 3D ( rig1a rig4) + render y salida ( out1).
Configurar parámetros importantes
Todos los parámetros de control están expuestos como páginas de parámetros personalizados en sus respectivos componentes:

KINECT_INPUT (página "Kinect"): Enable, Smoothing, Movementthreshold.
OPTICAL_FLOW (página "Optical Flow"): Flowstrength(sensibilidad al movimiento), Scale, Threshold(umbral mínimo para considerar que hay movimiento).
container1/feedbackEdgepppp (pagina "Feedback"): Edgecolor (color del efecto), Scale(velocidad de zoom del feedback), Feedbackgain(que tan rapido decae el efecto), Drywet(mezcla entre imagen cruda y efecto procesado).
FINAL_RENDER/shark_ai (Execute DAT, dentro del código): velocidad, frecuencia de giro y fase de cada tiburón se configuran en el diccionario rigs_cfgal principio del script. La radio de patrullaje se controla con SOFT_BOUND/ HARD_BOUND.
FINAL_RENDER/light_shark : intensidad ( Dimmer) y color de la luz que ilumina a los tiburones.
