# Como carajo Instalar TensorFlow con GPU para Windows 10/11

Fecha última Actualización: 31 de mayo del 2023

*"Una bella presentación a través de las montañas de la locura y los valles de la desesperación"*

----

## Introducción

----

Este recurso busca facilitar el proceso de uso de GPU en TensorFlow para usuarios de Windows (probablemente el segundo problema más difícil en aprendizaje automático). En este se detallan los requerimientos tanto como pasos necesarios para garantizar el correcto uso de GPU por TensorFlow en un entorno local de Windows.

Este recurso fue generado como material de apoyo para el ramo *"INTRODUCCIÓN A LAS REDES NEURONALES ARTIFICIALES AND DEEP LEARNING"* dictado por el profesor Ricardo Ñanculef en la Universidad Técnica Federico Santa María.

----

## Requerimientos

----

A continuación se detallan algunos de los paquetes que es necesario tener previamente instalados en Windows para el correcto funcionamiento del proceso de instalación detallado.

* [pyenv](https://github.com/pyenv/pyenv) (o cualquier otra herramienta de manejo de ambientes de Python)
* GPU Nvidia
* Cuenta de Microsoft Online
* Cuenta de NVIDIA Developer

----

## Instalación

----

Para la realización de la instalación se trabajará en un entorno con los siguientes detalles técnicos (sin TensorFlow instalado):

* OS: Windows 10
* Python: 3.8.10
* TensorFlow: 2.10.0 (versión más reciente para Windows al momento de realizar el recurso)

### 1. Encontrar la versión de TF a instalar y sus drivers

----

El primer paso consiste en ir a este [link](https://www.tensorflow.org/install/source_windows#gpu) y decidir la versión de TensorFlow a instalar. Dependiendo de que versión se haya elegido se debe determinar la versión de drivers de CUDA y otros softwares compatibles.

Por fines pedagógicos se utilizará la versión 2.10.0 de TensorFlow con el fin de hacer más ilustrativo el manejo y uso de versiones.
Del link indicado se extrae la siguiente tabla:

![Tabla versiones TensorFlow y librerías necesarias](/images/tensorflow_table.png "Tabla versiones TensorFlow y librerías necesarias")
*Tabla versiones TensorFlow y librerías necesarias*

De la tabla observamos que para emplear TensorFlow 2.10.0 con GPU necesitamos una instalación de Python que esté entre las versiones 3.7 y 3.10, de compilador es necesario instalar visual studio community version 2019 (incluye las build tools necesarias), cUDNN 8.1 y CUDA 11.2.

### 2. Instalar Microsoft Visual Studio

----

Para el segundo paso es necesario instalar *Microsoft Visual Studio*. Nótese que *Microsoft Visual Studio* y *Visual Studio Code* son herramientas distintas. **En caso de tener *Microsoft Visual Studio 2022* previamente instalado, desinstálelo antes de continuar con los siguientes pasos.**

Ingrese al siguiente [link](https://visualstudio.microsoft.com/es/vs/older-downloads) y descargue *Microsoft Visual Studio 2019 community edition*, para esto es necesario tener una cuenta en Microsoft (alumnos y funcionarios de la Universidad Técnica Federico Santa María tienen cuenta ya creada, solo deben apretar descargar y realizar la autorización de permisos).

![Descarga *Microsoft Visual Studio 2019*](/images/microsoft_visual_studio_2019_download.png "Descarga Microsoft Visual Studio 2019")
*Descarga Microsoft Visual Studio 2019*

Al apretar descarga se debiese redireccionar a la siguiente página, donde se debe descargar *Visual Studio Community 2019 (version 16.11)* (en caso de estar en un sistema de 32-bit cambiar de x64 a x86):

![Descarga *Microsoft Visual Studio Community 2019 (version 16.11)*](/images/microsoft_visual_studio_2019_community_download.png "Descarga Microsoft Visual Studio Community 2019 (version 16.11)")
*Descarga Microsoft Visual Studio Community 2019 (version 16.11)*

Corra el ejecutable descargado y espere a que termine la instalación. El instalador le pedirá elegir el workload a instalar, no es necesario elegir ninguno por lo cual simplemente instale sin workload y haga clic en continuar. Con la instalación finalizada le pedirá ingresar a su cuenta, es paso no es necesario por lo cual puede saltarse este paso.

### 3. Instalar NVIDIA CUDA toolkit

----

El NVIDIA CUDA toolkit contiene los drivers para las GPU de NVIDIA. Dependiendo del sistema en el que se esté trabajando estos pueden o no estar previamente instalados, si lo están es necesario chequear la compatibilidad con TensorFlow. Esta guía asume que no están instalados, sin embargo, en caso de estarlo se puede seguir el tutorial y no debiese evidenciar problemas.

Para correcta ejecución de GPU en TensorFlow es necesario primero eliminar **TODAS** las versiones de CUDA instaladas, para esto vaya a Settings en Windows e ingrese a *"Apps & Features"*, en buscar introduzca "nvidia cuda" y desinstale **TODAS** las aplicaciones que tengan "nvidia cuda" en su nombre. Luego vaya a `Local Disk (C:) > Program Files > NVIDIA GPU Computing Toolkit > CUDA`, dentro de esta carpeta debiesen estar todas las versiones distintas de cuda previamente instaladas, elimine **TODOS** los contenidos de la carpeta `CUDA`.

Ingrese al siguiente [link](https://developer.nvidia.com/cuda-toolkit-archive) y descargue la versión compatible, para nuestro caso es *CUDA Toolkit 11.2.0* (todas las versiones 11.2.x debiesen funcionar correctamente), al ingresar al link debe ver algo como esto:

![Versiones *CUDA Toolkit*](/images/CUDA_versions.png "Versiones CUDA Toolkit")
*Versiones CUDA Toolkit*

En esta página buscamos las versiones compatibles y luego hacemos clic en alguna de ellas y no redireccionará a otra página. Aquí elegimos el sistema operativo sobre el que trabajaremos (en nuestro caso Windows, particularmente Windows 10) y descargamos el instalado, puede ser la versión de red (más liviana) o el exe (más pesado, pero viene con todo lo necesario para la instalación), en este caso seleccionamos el exe y hacemos clic en descargar, como se puede observar en la imagen a continuación:

![Download *CUDA Toolkit 11.2.0*](/images/CUDA_toolkit_download.png "Download CUDA Toolkit 11.2.0")
*Download CUDA Toolkit 11.2.0*

A continuación ejecute el exe y siga los pasos descritos por el instalador.

----

#### **Errores Comunes**

----

**ERROR** Frameview ya instalado:

![Frameview Error](/images/CUDA_error_frameview.png "Frameview Error")

**Solución** Frameview: ir a Settings en Windows, ingrese a *"Apps & Features"*, en buscar introduzca "nvidia frameview" y desintale **TODAS** las aplicaciones que tengan "nvidia frameview" en su nombre.

**ERROR** No se encontraron versiones compatibles de Visual Studio:

![Visual Studio Error](/images/CUDA_error_visual_studio.png "Visual Error")

**Solución** Visual Studio: ir a Settings en windows, ingrese a *"Apps & Features"*, en buscar introduzca "visual studio" y verifique que tiene instalada la versión del **2019**, desinstale todas las versiones de visual studio instaladas y repita el paso **2**.

### 4. Instalar cuDNN

----

Para TensorFlow 2.10.0, es necesario instalar cuDNN 8.1, ingrese al siguiente [link](https://developer.nvidia.com/cudnn) y haga clic en descargar:

![Download cuDNN](/images/cudnn_download.png "Download cuDNN")

Le pedirá crear una cuenta de desarrollador:

![NVIDIA Developer account](/images/cudnn_developer_account.png "NVIDIA Developer account")

Si no tiene una cuenta previamente creada, haga clic en "join", ingrese su email y llene el formulario, tras lo cual recibirá su cuenta de desarrollador (Esta es  **GRATUITA**). Luego vuelva a la página de descarga de [cuDNN](https://developer.nvidia.com/rdp/cudnn-download) donde debiese aparecer lo siguiente:

![Download cuDNN 2](/images/cudnn_download_2.png "Download cuDNN step 2")

En nuestro caso necesitamos la versión 8.1.0 para *CUDA 11.2* por lo cual debe hacer clic en [Archived cuDNN Releases](https://developer.nvidia.com/rdp/cudnn-archive) (también puede instalar cualquiera de las versiones compatibles con *CUDA 11.x* y no debiese tener problemas):

![Download *cuDNN 8.1.0*](/images/cudnn_archived_version_download.png "Download cuDNN 8.1.0")

Al hacer clic nos muestra lo siguiente y debemos descargar la versión para Windows:

![Download *cuDNN 8.1.0* windows version](/images/cudnn_archived_version_windows_download.png "Download cuDNN 8.1.0 Windows version")

Esto nos debiese descargar una carpeta zip la cual descomprimimos y nuestra carpeta de descargas debiese verse así:

![Download *cuDNN 8.1.0* zip](/images/cudnn_zip.png "Download cuDNN 8.1.0 zip")

Ingresamos a la carpeta `cuda`, copiamos las tres carpetas dentro de ella (`bin`,`include`,`lib`) con todos sus contenidos y las pegamos en `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2`.

Windows Explorer señalará que estas carpetas ya existen, por lo cual debemos escoger la opción *Reemplazar archivos de destino* y seleccionamos esto para todos los archivos.

### 5. Añadir CUDA toolkit al PATH de usuario

----

Para el funcionamiento correcto de CUDA con TensorFlow necesitamos añadir acceso a esta carpeta mediante las variables de ambiente. En la dirección `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2`, hay una carpeta denominada `bin`, ingrese a esta, copie y guarde la dirección:

Imagen

Ahora en `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2` ingrese a `libnvvp`, copie y guarde la dirección:

Imagen

Ingrese a Windows y escriba "environment variables":

![Editar variables de ambiente](/images/edit_environment_variables.png "Editar variables de ambiente")

Haga clic en el icono y debiese aparecer la siguiente ventana:

![Edición variables de ambiente](/images/edit_var_pop_up.png "Edición variables de ambiente")

En las variables de usuario elija editar **Path**:

![Edición Path](/images/edit_path.png "Edición Path")

Haga clic en **New** y añada las rutas de `bin` y `libnvvp`

>```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\bin```
>
>```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\libnvvp```

Haga clic en **OK** y **¡¡REINICIE EL COMPUTADOR!!**.

### 6. Instalar TensorFlow en un ambiente virtual

----

Para nuestro caso particular utilizamos pyenv pero cualquier otra herramienta para crear ambientes de Python sirve:

>`pyenv install 3.8`
>
>`pyenv global 3.8`

Instalamos nuestra versión de TensorFlow y JupyterLab para trabajar con Jupyter notebook:
>`pip install tensorflow==2.10.0`
>
>`pip install jupyterlab ipykernel`

Generamos un Jupyter notebook y ejecutamos lo siguiente para verificar que TensorFlow reconozca correctamente la GPU:

```python
import tensorflow as tf
from tensorflow.python.client import device_lib


print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
device_lib.list_local_devices()
```

Si la instalación es correcta el output de esta celda de código debiese mostrar que reconoce al menos 1 GPU.

----

## Agradecimientos

----

A [@aladdinpersson](https://github.com/aladdinpersson) y a su minucioso video sobre como realizar la instalación el cual pese a estar desactualizado fue un valioso aporte en la realización de este recurso.

----

## Referencias

----

* <https://www.tensorflow.org/>
* <https://developer.nvidia.com/>
* <https://github.com/aladdinpersson/Machine-Learning-Collection>
* <https://www.youtube.com/watch?v=hHWkvEcDBO0&t=341s>
* <https://towardsdatascience.com/how-to-finally-install-tensorflow-gpu-on-windows-10-63527910f255>
* <https://towardsdatascience.com/how-to-host-jupyter-notebook-slides-on-github-d785f30e6e2>
* <https://pages.github.com/>
* <https://rise.readthedocs.io/en/stable/>
* <https://12ft.io/>

----

## Autores

----

* Domingo Benoit Cea

### Contacto

----
Para contactarme sobre esta guía por favor añadir en el asunto:

`[Guía Instalación TensorFlow con GPU]`

Con dirección al siguiente email:

> <domingo.benoit@sansano.usm.cl>

Fecha creación: 31 de mayo del 2023
