---
title: Definición de un nuevo tipo de dispositivo en Azure IoT Central | Microsoft Docs
description: Este tutorial le muestra, como desarrollador, cómo definir un nuevo tipo de dispositivo en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y la configuración del tipo.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: a2601f55bbc7e99321689afdafcab3135b94bd5b
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702551"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo para definir un nuevo tipo de dispositivo en la aplicación de Microsoft Azure IoT Central. Una plantilla de dispositivo define la telemetría, el estado, las propiedades y la configuración del tipo de dispositivo.

Para poder probar la aplicación antes de conectar un dispositivo real, Azure IoT Central genera un dispositivo simulado a partir de la plantilla de dispositivo en el momento de la creación.

En este tutorial, creará una plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado). Un dispositivo acondicionador de aire conectado:

* Envía datos de telemetría, como la temperatura y la humedad.
* Indica su estado, como activado o desactivado.
* Tiene propiedades, como su versión de firmware y el número de serie.
* Tiene una configuración, como la temperatura de destino y la velocidad del ventilador.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una nueva plantilla de dispositivo
> * Agregar telemetría al dispositivo
> * Ver datos de telemetría simulados
> * Definir la medida de eventos
> * Ver eventos simulados
> * Definir la medida de estado
> * Ver el estado simulado
> * Utilizar las propiedades del dispositivo
> * Utilizar la configuración del dispositivo
> * Usar comandos

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Si realizó la guía de inicio rápido [Creación de una aplicación de Azure IoT Central](quick-deploy-iot-central.md), puede volver a usar la aplicación que creó en dicha guía. En caso contrario, complete los pasos siguientes para crear una aplicación de Azure IoT Central vacía:

1. Vaya a la página [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de Azure IoT Central.

2. Escriba la dirección de correo electrónico y la contraseña que usa para acceder a la suscripción de Azure:

   ![Incorporación de la cuenta de la organización](./media/tutorial-define-device-type/sign-in.png)

3. Para empezar a crear una nueva aplicación de Azure IoT Central, elija **New Application** (Nueva aplicación):

    ![Página Application Manager (Administrador de aplicaciones) de Azure IoT Central](./media/tutorial-define-device-type/iotcentralhome.png)

4. Para crear una nueva aplicación de Azure IoT Central:

    * Elija un nombre de aplicación descriptivo, como **Contoso Air Conditioners** (Acondicionadores de aire Contoso). Azure IoT Central genera un prefijo de dirección URL único. Puede cambiar este prefijo de dirección URL por algo más fácil de recordar.
    
    * Elija un directorio de Azure Active Directory y una suscripción de Azure que va a utilizar. Para más información acerca de los directorios y las suscripciones, consulte [Creación de una aplicación de Azure IoT Central](howto-create-application.md).
    
    * Use un grupo de recursos existente o cree un nuevo grupo de recursos con el nombre de su elección. Por ejemplo, **contoso-rg**.
    
    * Elija la región geográfica más cercana a usted.
    
    * Elija la plantilla de aplicación **Custom Application** (Aplicación personalizada).
    
    * Elija el plan de pago **Free 30 Day Trial Application** (Evaluación gratuita durante 30 días de la aplicación).
    
    * Seleccione **Create**.

    ![Página de creación de una aplicación de Azure IoT Central](./media/tutorial-define-device-type/iotcentralcreate.png)

Para más información, consulte el [Procedimiento de creación de una aplicación de Azure IoT Central](howto-create-application.md).

## <a name="create-a-new-custom-device-template"></a>Creación de una nueva plantilla de dispositivo personalizado

Como desarrollador, puede crear y editar las plantillas de dispositivo de la aplicación. Cuando se crea una plantilla de dispositivo, Azure IoT Central genera un dispositivo simulado a partir de la plantilla. El dispositivo simulado genera datos de telemetría que le permiten probar el comportamiento de la aplicación antes de conectar un dispositivo físico.

Para agregar una nueva plantilla de dispositivo a la aplicación, debe ir a la página **Application Builder** (Generador de aplicaciones). Para ello, elija **Application Builder** en el menú de navegación izquierdo.

![Página Application Builder](./media/tutorial-define-device-type/builderhome.png)

## <a name="add-a-device-and-define-telemetry"></a>Incorporación de un dispositivo y definición de la telemetría

Los pasos siguientes muestran cómo crear una nueva plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado) para los dispositivos que envían datos de telemetría de temperatura a la aplicación:

1. En la página **Application Builder** (Generador de aplicaciones), elija **Create Device Template** (Crear plantilla de dispositivo):

    ![Página Application Builder, Crear plantilla de dispositivo](./media/tutorial-define-device-type/builderhomedevices.png)

2. En la página **Device Templates** (Plantillas de dispositivo), elija **Custom** (Personalizada). Una plantilla de dispositivo **Personalizada** le permite definir todas las características y comportamientos de su acondicionador de aire conectado:

    ![Dispositivos](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. En la página **New Device Template** (Nueva plantilla de dispositivo), escriba **Connected Air Conditioner** como el nombre del dispositivo y, a continuación, elija **Create** (Crear). También puede cargar una imagen del dispositivo que estará visible para los operadores en el explorador de dispositivos:

    ![Dispositivo personalizado](./media/tutorial-define-device-type/createcustomdevice.png)

4. En la plantilla de dispositivo **Connected Air Conditioner**, asegúrese de que se encuentra en la página **Measurements** (Medidas), donde se define la telemetría. Cada plantilla de dispositivo que define tiene páginas independientes para que pueda:

    * Especificar las medidas, como la telemetría, los eventos y el estado, enviadas por el dispositivo.
    
    * Definir la configuración utilizada para controlar el dispositivo.
    
    * Definir las propiedades utilizadas para registrar información sobre el dispositivo.
    
    * Definir las reglas asociadas con el dispositivo.
    
    * Personalizar el panel del dispositivo para los operadores.

    ![Medidas del acondicionador de aire](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > Para cambiar el nombre del dispositivo o de la plantilla de dispositivo, haga clic en el texto en la parte superior de la página.

5. Para agregar la medida de telemetría de temperatura, elija **New Measurement** (Nueva medida). A continuación, elija **Telemetry** (Telemetría) como el tipo de medida:

    ![Medidas del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Cada tipo de telemetría que define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

    * Opciones de visualización.

    * Detalles de telemetría.

    * Parámetros de simulación.

    Para configurar la telemetría **Temperature** (Temperatura), use la información de la tabla siguiente:

    | Configuración              | Valor         |
    | -------------------- | -----------   |
    | Display Name (Nombre para mostrar)         | Temperatura   |
    | Nombre del campo           | temperatura   |
    | Unidades                | F             |
    | Min                  | 60            |
    | max                  | 110           |
    | Posiciones decimales       | 0             |

    También puede elegir un color para la presentación de la telemetría. Para guardar la definición de telemetría, elija **Save** (Guardar):

    ![Configurar la simulación de temperatura](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Después de unos instantes, la página **Measurements** (Medidas) muestra un gráfico de la telemetría de temperatura del dispositivo simulado acondicionador de aire conectado. Use los controles para administrar la visibilidad, la agregación o para editar la definición de telemetría:

    ![Ver la simulación de temperatura](./media/tutorial-define-device-type/viewsimulation.png)

8. También puede personalizar el gráfico mediante los controles **Line** (Línea), **Stacked** (Apilado) y **Edit Time Range** (Editar intervalo de tiempo):

    ![Personalizar el gráfico](./media/tutorial-define-device-type/customizechart.png)

## <a name="define-event-measurement"></a>Definir la medida de eventos

Puede usar eventos para definir datos puntuales en el tiempo que el dispositivo envía para indicar algo significativo, como un error o un error de componente. Al igual que las medidas de telemetría, Azure IoT Central puede simular eventos de dispositivo para poder probar el comportamiento de la aplicación antes de conectar un dispositivo físico. Puede definir medidas de eventos para el tipo de dispositivo en la vista **Measurements** (Medidas).

1. Para agregar la medida de eventos **Fan Motor Error** (Error del motor del ventilador), elija **New Measurement** (Nueva medida). A continuación, elija **Event** (Evento) como el tipo de medida:

    ![Medidas del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/eventnew.png)

2. Cada tipo de evento que define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

   * Nombre para mostrar.

   * Nombre del campo.

   * Gravedad.

    Para configurar el evento **Fan Motor Error** (Error del motor del ventilador), use la información de la tabla siguiente:

    | Configuración              | Valor             |
    | -------------------- | -----------       |
    | Display Name (Nombre para mostrar)         | Fan Motor Error   |
    | Nombre del campo           | fanmotorerr       |
    | Gravedad             | Error             |

    Para guardar la definición del evento, elija **Save** (Guardar):

    ![Configurar la medida de eventos](./media/tutorial-define-device-type/eventconfiguration.png)

3. Después de unos instantes, la página **Measurements** (Medidas) muestra un gráfico de los eventos generados aleatoriamente del dispositivo simulado acondicionador de aire conectado. Use los controles para administrar la visibilidad o para editar la definición del evento:

    ![Simulación de la visualización del evento](./media/tutorial-define-device-type/eventview.png)

1. Para ver detalles adicionales sobre el evento, haga clic en el evento en el gráfico:

    ![Ver detalles del evento](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-state-measurement"></a>Definir la medida de estado

Puede usar State (Estado) para definir y visualizar el estado del dispositivo o su componente durante un período de tiempo. Al igual que las medidas de telemetría, Azure IoT Central puede simular el estado del dispositivo para poder probar el comportamiento de la aplicación antes de conectar un dispositivo físico. Puede definir medidas de estado para el tipo de dispositivo en la vista **Measurements** (Medidas).

1. Para agregar la medida **Fan Mode** (Modo del ventilador), elija **New Measurement** (Nueva medida). A continuación, elija **State** (Estado) como el tipo de medida:

    ![Medidas de estado del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/statenew.png)

2. Cada tipo de estado que define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

   * Nombre para mostrar.

   * Nombre del campo.

   * Valores con etiquetas de presentación opcionales.

   * Color para cada valor.

    Para configurar el estado **Fan Mode** (Modo del ventilador), use la información de la tabla siguiente:

    | Configuración              | Valor             |
    | -------------------- | -----------       |
    | Display Name (Nombre para mostrar)         | Fan Mode          |
    | Nombre del campo           | fanmode           |
    | Valor                | 1                 |
    | Etiqueta para mostrar        | Operating         |
    | Valor                | 0                 |
    | Etiqueta para mostrar        | Stopped           |

    Para guardar la definición del estado, elija **Save** (Guardar):

    ![Configurar la medida de estado](./media/tutorial-define-device-type/stateconfiguration.png)

3. Después de unos instantes, la página **Measurements** (Medidas) muestra un gráfico de los estados generados aleatoriamente del dispositivo simulado acondicionador de aire conectado. Use los controles para administrar la visibilidad o para editar la definición del estado:

    ![Ver simulación del estado](./media/tutorial-define-device-type/stateview.png)

4. En caso de que haya demasiados puntos de datos enviados por el dispositivo en un intervalo pequeño, la medida de estado se muestra con un objeto visual diferente, tal y como se muestra a continuación. Si hace clic en el gráfico, se muestran todos los puntos de datos dentro de ese período de tiempo en un orden cronológico. También puede reducir el intervalo de tiempo para ver la medida trazada en el gráfico.

    ![Ver detalles del estado](./media/tutorial-define-device-type/stateviewdetail.png)

## <a name="settings-properties-and-commands"></a>Configuración, propiedades y comandos

La configuración, propiedades y propiedades del dispositivo, y los comandos son los diferentes valores definidos en una plantilla de dispositivo que se asocian a cada dispositivo individual:

* La _configuración_ se usa para enviar datos de configuración a un dispositivo desde la aplicación. Por ejemplo, un operador podría utilizar una configuración para cambiar el intervalo de telemetría del dispositivo de dos segundos a cinco segundos. Cuando un operador cambia un valor, la configuración se marca como pendiente en la interfaz de usuario hasta que el dispositivo confirma que ha accionado el cambio de configuración.

* Las _propiedades_ se usan para registrar información sobre el dispositivo en la aplicación. Por ejemplo, puede utilizar propiedades para registrar el número de serie del dispositivo o el número de teléfono del fabricante del dispositivo. Las propiedades se almacenan en la aplicación y no se sincronizan con el dispositivo. Un operador puede asignar valores a las propiedades.

* Las _propiedades del dispositivo_ se usan para permitir a un dispositivo enviar valores de propiedad a la aplicación. Solo el dispositivo puede actualizar estas propiedades. Para un operador, las propiedades del dispositivo son de solo lectura.

* Use _comandos_ para administrar de forma remota su dispositivo desde la aplicación. Puede ejecutar directamente los comandos en el dispositivo desde la nube para controlar los dispositivos. Por ejemplo, un operador puede ejecutar comandos como el de reinicio para reiniciar al instante el dispositivo.

## <a name="use-settings"></a>Usar la configuración

La *configuración* se usa para permitir a un operador enviar datos de configuración a un dispositivo. En esta sección, agregará una configuración para la plantilla de dispositivo **Connected Air Conditioner** que permite a un operador establecer la temperatura de destino del acondicionador de aire conectado.

1. Vaya la página **Settings** (Configuración) de la plantilla de dispositivo **Connected Air Conditioner**:

    ![Prepararse para agregar una configuración](./media/tutorial-define-device-type/deviceaddsetting.png)

    Puede crear una configuración de tipos diferentes, como números o texto.

2. Elija **Number** (Número) para agregar un valor de configuración numérico para el dispositivo.

3. Para configurar la configuración **Set Temperature** (Establecer temperatura), use la información de la tabla siguiente:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Display Name (Nombre para mostrar)         | Set Temperature |
    | Nombre del campo           | setTemperature  |
    | Unidad de medida  | F               |
    | Posiciones decimales       | 1               |
    | Valor mínimo        | 20              |
    | Valor máximo        | 200             |
    | Valor inicial        | 80              |
    | DESCRIPCIÓN          | Establezca la temperatura de destino del acondicionador de aire |

    Después, elija **Save** (Guardar):

    ![Configurar la opción Set Temperature (Establecer temperatura)](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Cuando el dispositivo confirma el cambio de configuración, el estado de la configuración se cambia a **synced** (sincronizado).

4. Para personalizar el diseño de la página **Settings** (Configuración), mueva los iconos de configuración y cambie su tamaño:

    ![Personalizar el diseño de la configuración](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties--device-properties"></a>Usar propiedades y propiedades del dispositivo

Las *propiedades* se usan para almacenar información sobre el dispositivo en la aplicación. En esta sección se agregan propiedades de la nube a la plantilla de dispositivo **Connected Air Conditioner** para almacenar la ubicación del dispositivo y la fecha de la última revisión. Tenga en cuenta que ambas son propiedades del dispositivo modificables. También hay propiedades de solo lectura notificadas por el dispositivo que no se pueden cambiar, como el número de serie del dispositivo y la versión de firmware.
 
1. Vaya la página **Properties** (Propiedades) de la plantilla de dispositivo **Connected Air Conditioner**:

    ![Prepararse para agregar una propiedad](./media/tutorial-define-device-type/deviceaddproperty.png)

    Puede crear propiedades de dispositivo de tipos diferentes, como números o texto. Para agregar una propiedad de ubicación a la plantilla de dispositivo, elija **Location**.

2. Para configurar la propiedad de ubicación, use la información de la tabla siguiente:

    | Campo                | Valor                |
    | -------------------- | -------------------- |
    | Display Name (Nombre para mostrar)         | Ubicación             |
    | Nombre del campo           | location             |
    | Valor inicial        | Seattle, WA          |
    | DESCRIPCIÓN          | Ubicación del dispositivo      |

    Deje los otros campos con sus valores predeterminados.

    ![Configurar las propiedades del dispositivo](./media/tutorial-define-device-type/configureproperties.png)

    Elija **Guardar**.

3. Para agregar una propiedad de fecha de la última revisión a la plantilla de dispositivo, elija **Date**.

4. Para configurar la propiedad de fecha de la última revisión, use la información de la tabla siguiente:

    | Campo                | Valor                   |
    | -------------------- | ----------------------- |
    | Display Name (Nombre para mostrar)         | Fecha de la última revisión       |
    | Nombre del campo           | serviceDate             |
    | Valor inicial        | 1/1/2018                |
    | DESCRIPCIÓN          | Última revisión           |

    ![Configurar las propiedades del dispositivo](./media/tutorial-define-device-type/configureproperties2.png)

    Elija **Guardar**.

5. Para personalizar el diseño de la página **Properties** (Propiedades), mueva los iconos de propiedades y cambie su tamaño:

    ![Personalizar el diseño de las propiedades](./media/tutorial-define-device-type/propertieslayout.png)


## <a name="use-commands"></a>Usar comandos

Los _comandos_ se usan para permitir que un operador ejecute comandos directamente en el dispositivo. En esta sección, agregará un comando a su plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado) que permite que un operador difunda determinado mensaje de eco en la pantalla del acondicionador de aire conectado (esto funciona con código de ejemplo MxChip).

1. Vaya la página **Commands** (Comandos) de la plantilla de dispositivo **Connected Air Conditioner**:

    ![Prepararse para agregar una configuración](media/tutorial-define-device-type/commandsecho.png)

    Puede crear comandos de distintos tipos según sus requisitos. 

1. Haga clic en **New Command** (Nuevo comando) para agregar un comando al dispositivo.

1. Para configurar el nuevo comando, use la información de la tabla siguiente:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Display Name (Nombre para mostrar)         | Comando Echo    |
    | Nombre del campo           | echo            |
    | Tiempo de espera predeterminado      | 30              |
    | Tipo de presentación         | text            |
    | DESCRIPCIÓN          | Comando de dispositivo  |  

Puede agregar entradas adicionales al comando haciendo clic en **+** para las entradas.

2. Elija **Guardar**.

3. Para personalizar el diseño de la página **Commands** (Comandos), mueva los iconos de configuración y cambie su tamaño:

    ![Personalizar el diseño de la configuración](media/tutorial-define-device-type/commandstileresize.png)

## <a name="view-your-simulated-device"></a>Visualización del dispositivo simulado

Ahora que ha definido la plantilla de dispositivo **Connected Air Conditioner**, puede personalizar su **panel** para incluir las medidas, la configuración y las propiedades que ha definido. A continuación, puede obtener una vista previa del panel como un operador:

1. Vaya a la página **Dashboard** (Panel) de la plantilla de dispositivo **Connected Air Conditioner**:

    ![Paneles del acondicionador de aire conectado](./media/tutorial-define-device-type/aircondashboards.png)

2. Elija **Line Chart** (Gráfico de líneas) para agregar el componente en el **panel**:

    ![Componentes del panel](./media/tutorial-define-device-type/dashboardcomponents1.png)

3. Configure el componente **Line Chart** (Gráfico de líneas) según se indica en la tabla siguiente:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Temperatura |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas | temperature (elija **Visibility** [Visibilidad] junto a **temperature**) |

    ![Configuración del gráfico de línea](./media/tutorial-define-device-type/linechartsettings.png)

    A continuación, elija **Save** (Guardar).

4. Configure el componente **Event Chart** (Gráfico de eventos) según se indica en la tabla siguiente:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Eventos |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas | Fan Motor Error (elija **Visibility** [Visibilidad] junto a **Fan Motor Error**) |

    ![Configuración del gráfico de línea](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    A continuación, elija **Save** (Guardar).

5. Configure el componente **State Chart** (Gráfico de estado) según se indica en la tabla siguiente:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Fan Mode |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas | Fan Mode (elija **Visibility** [Visibilidad] junto a **Fan Mode**) |

    ![Configuración del gráfico de línea](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    A continuación, elija **Save** (Guardar).

6. Para agregar la configuración establecer temperatura al panel, elija **Settings and Properties** (Configuración y propiedades):

    ![Componentes del panel](./media/tutorial-define-device-type/dashboardcomponents4.png)

7. Configure el componente **Settings and Properties** (Configuración y propiedades) según se indica en la tabla siguiente:

    | Configuración                 | Valor         |
    | ----------------------- | ------------- |
    | Título                   | Set target temperature |
    | Configuración y propiedades | Set Temperature |

    ![Configuración de la propiedad de número de serie](./media/tutorial-define-device-type/propertysettings3.png)

    A continuación, elija **Save** (Guardar).

8. Para agregar el número de serie del dispositivo al panel, elija **Settings and Properties** (Configuración y propiedades):

    ![Componentes del panel](./media/tutorial-define-device-type/dashboardcomponents3.png)

9. Configure el componente **Settings and Properties** (Configuración y propiedades) según se indica en la tabla siguiente:

    | Configuración                 | Valor         |
    | ----------------------- | ------------- |
    | Título                   | Serial number |
    | Configuración y propiedades | Número de serie |

    ![Configuración de la propiedad de número de serie](./media/tutorial-define-device-type/propertysettings1.png)

    A continuación, elija **Save** (Guardar).

10. Para agregar la versión de firmware del dispositivo al panel, elija **Settings and Properties** (Configuración y propiedades):

    ![Componentes del panel](./media/tutorial-define-device-type/dashboardcomponents4.png)

11. Configure el componente **Settings and Properties** (Configuración y propiedades) según se indica en la tabla siguiente:

    | Configuración                 | Valor            |
    | ----------------------- | ---------------- |
    | Título                   | Firmware version |
    | Configuración y propiedades | Versión de firmware |

    ![Configuración de la propiedad de número de serie](./media/tutorial-define-device-type/propertysettings2.png)

    A continuación, elija **Save** (Guardar).

12. Para ver el panel como un operador, desactive **Design Mode** (Modo de diseño) en la parte superior derecha de la página.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Creación de una nueva plantilla de dispositivo
> * Agregar telemetría al dispositivo
> * Ver datos de telemetría simulados
> * Definir los eventos del dispositivo
> * Ver eventos simulados
> * Definir el estado
> * Ver el estado simulado
> * Utilizar las propiedades del dispositivo
> * Utilizar la configuración del dispositivo

Ahora que ha definido una plantilla de dispositivo en la aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos:

* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules.md)
* [Personalización de la vista del operador](tutorial-customize-operator.md)
