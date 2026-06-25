# 3. Instalación de controladores

> Para la instalación de controladores, puede omitir este paso inicialmente, ya que los controladores generalmente se instalan automáticamente cuando la placa de desarrollo se conecta a su computadora. Si la placa no se reconoce después de la conexión, consulte esta sección para instalar los controladores.

## 3.1 Sistema Windows

**Verificación del controlador**

1. Conecte la placa base a la computadora.

![](./media/1.jpg)

2. Abra el Administrador de dispositivos. Si aparece el mensaje **"Silicon Labs CP210x USB to UART Bridge (COMx)"**, esto demuestra que el controlador ha sido instalado. Por favor, omita la parte de **"Instalación de controladores"**.

![](./media/Animation.gif)

**Instalación manual del controlador**

1. Descarga del controlador

- Sistema Windows: [Controlador del sistema Windows](./Windows.7z)

2. Conecte la placa base a la computadora y abra el Administrador de dispositivos. Si hay un signo de exclamación amarillo frente al controlador en la imagen, esto demuestra que el controlador no está instalado. Por favor, descargue e instale el controlador manualmente.

![](./media/Animation-1750921346712-3.gif)

## 3.2 Sistema MAC

**1 Verificación del controlador**

Conecte la placa de desarrollo a la computadora. Según [Herramientas] ---> [Puerto], seleccione el puerto de la placa de desarrollo (Nota: Si no puede confirmar cuál es el puerto de la placa de desarrollo, conecte la placa base y tome fotografías para registrar todos los puertos. Luego desconecte la placa de desarrollo y vuelva a tomar fotografías para registrar todos los puertos. Compare para encontrar los puertos que desaparecieron. El puerto que desapareció es el puerto de la placa. Seleccione ese puerto). Si no puede reconocer el puerto, intente cambiar el puerto USB de la computadora o use un cable diferente para reconocer el puerto. Si aún no funciona, consulte los siguientes pasos para instalar el controlador.

![](./media/20250626154343.png)

**2 Instalación manual del controlador**

1. Descarga del controlador

​       Sistema Mac: [Controlador del sistema Mac](./Mac.7z)

2. Haga doble clic para descomprimir el paquete zip del controlador descargado

![](./media/image-20250417083615847-1749262759458-8.png)

![](./media/image-20250417083758947-1749262759458-7.png)

![](./media/image-20250417083918581-1749262759458-5.png)

3. Después de eso, continúe haciendo clic en **"Siguiente"** hasta que se complete la instalación

![](./media/7cca827fe946096f228797dadce10661.png)

En este punto, el puerto puede ser reconocido enchufando la placa nuevamente.

4. Luego vaya al Arduino IDE, haga clic en "Herramientas", seleccione la placa Arduino Uno y el puerto de la placa de desarrollo reconocido.

![](./media/2.png)

5. Haga clic en ![image-20250417085312966](./media/image-20250417085312966-1749262759459-18.png) para cargar el código y mostrar "Carga completada".

![](./media/3.png)