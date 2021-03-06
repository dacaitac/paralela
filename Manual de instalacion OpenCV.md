# Practica Blur


## Instalación OpenCV

### Instalar CMake
De todas las versiones que probé esta fue la primera que funcionó sin errores.
Hay que tener en cuenta que es necesario instalar la GUI, para después en modo gráfico agregar y eliminar las dependencias que se van a utilizar en el proyecto, como OpenMP y CUDA.


Hay que tener en cuenta los requerimientos que dicen en el sitio oficial 

[https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html](https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html)


[http://embedonix.com/articles/linux/installing-cmake-3-5-2-on-ubuntu/](http://embedonix.com/articles/linux/installing-cmake-3-5-2-on-ubuntu/)

### Ejecutar Script de instalación de OpenCV
Luego se descarga y ejecuta el siguiente script (dandole primero permisos de sudo chmod +x scInst.sh ), desde el lugar donde quiere que queden los archivos de instalacion de OpenCV
[https://github.com/dacaitac/Paralela/blob/master/scInst.sh](https://github.com/dacaitac/Paralela/blob/master/scInst.sh)
que clona la librería y alista los paquetes que se necesitan en CMake

Y ya para la instalación se sigue este otro tutorial
[https://towardsdatascience.com/how-to-install-opencv-and-extra-modules-from-source-using-cmake-and-then-set-it-up-in-your-pycharm-7e6ae25dbac5](https://towardsdatascience.com/how-to-install-opencv-and-extra-modules-from-source-using-cmake-and-then-set-it-up-in-your-pycharm-7e6ae25dbac5)

Cuando se esté haciendo la instalación es importante seleccionar WITH_OPENMP y deseleccionar todas las que dicen DNN


#### Hay que asegurarse de  que pkg-config quede instalado, porque se necesita para compilar
[https://people.freedesktop.org/~dbn/pkg-config-guide.html](https://people.freedesktop.org/~dbn/pkg-config-guide.html)



## Compilar

Por ahora estoy utilizando este comando para compilar
 
 g++ DisplayImage.cpp -o app -I/usr/local/include/opencv -I/usr/local/include -L/usr/local/lib -lopencv_ml -lopencv_objdetect -lopencv_stitching -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_imgcodecs -lopencv_video -lopencv_photo -lopencv_imgproc -lopencv_flann -lopencv_core

Si quedó todo bien instalado se debe poder con

g++ m.cpp -o app `pkg-config --cflags --libs opencv`

#### Asegurarse que la instalación haya quedado en /usr/local/include/opencv2 que es donde estan los include que se usan para compilar

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Ejecución del programa

Al iniciar con la ejecucion del programa, se debe utilizar el siguiente comando

./app  NombreDeLaImagen.ElFormatoQueGuste NombreDeLaImagen_Blur.ElMismoFormato #(Tamaño Del Kernel) #(Numero De Hilos)

### Funcionamiento del programa

El programa usa la libreria OpenCV con el fin de poder trabajar la imagen como si fuera una matriz. Así, utilizando un algoritmo de convolucion de matrices, se puede generar el efecto borroso. Inicialmente, la imagen es copiada en una matriz de numeros enteros, donde cada numero representa el valor de un pixel. Para poder repartir la carga de trabajo entre varios hilos, se separó la matriz por filas, y cada sección es operada por un hilo diferente. Despues de cada operacion, los valores obtenidos de la convolución, son guardados en una nueva matriz. Y esta a su vez es transferida a un nuevo archivo de imagen, con el efecto borroso, ya generado.
