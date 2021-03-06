---
title: Conexión a Azure Database for MySQL desde C++
description: En este tutorial rápido se proporciona un ejemplo de código de C++ que puede usar para conectarse a Azure Database for MySQL y consultar datos en este servicio.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: C++
ms.topic: quickstart
ms.date: 04/12/2018
ms.openlocfilehash: 0c017907378376c01e4a4a98190f73a9452b9a3d
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264959"
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>Azure Database for MySQL: uso de Connector/C++ para conectarse y consultar datos
En este tutorial rápido se muestra cómo conectarse a una instancia de Azure Database for MySQL mediante una aplicación de C++. Se indica cómo usar instrucciones SQL para consultar, insertar, actualizar y eliminar datos en la base de datos. En este tema se da por hecho que está familiarizado con el desarrollo mediante C++ y que nunca ha usado Azure Database for MySQL.

## <a name="prerequisites"></a>requisitos previos
En este tutorial rápido se usan como punto de partida los recursos creados en una de las guías siguientes:
- [Create an Azure Database for MySQL server using Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md) (Creación de un servidor de Azure Database for MySQL mediante Azure Portal)
- [Create an Azure Database for MySQL server using Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md) (Creación de un servidor de Azure Database for MySQL mediante la CLI de Azure)

Además, deberá:
- Instalar [.NET Framework](https://www.microsoft.com/net/download)
- Instalar [Visual Studio](https://www.visualstudio.com/downloads/)
- Instalación de [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/) 
- Instalación de [Boost](http://www.boost.org/)

## <a name="install-visual-studio-and-net"></a>Instalación de Visual Studio y .NET
En los pasos de esta sección se supone que está familiarizado con el desarrollo mediante .NET.

### <a name="windows"></a>**Windows**
- Instale Visual Studio 2017 Community, que es un IDE gratuito, ampliable, rico en contenido y visualmente atractivo para la creación de aplicaciones modernas para Android, iOS, Windows, así como aplicaciones web y de base de datos, y servicios en la nube. Puede instalar .NET Framework completo o solo .NET Core: los fragmentos de código de la guía de inicio rápido funcionan con cualquiera de las dos opciones. Si Visual Studio ya está instalado en el equipo, omita los dos pasos siguientes.
   1. Descargue el [instalador de Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
   2. Ejecute el instalador y siga las indicaciones para completar la instalación.

### <a name="configure-visual-studio"></a>**Configuración de Visual Studio**
1. Desde Visual Studio, en Proyecto -> Propiedades -> Enlazador -> General > Directorios de bibliotecas adicionales, agregue el directorio "\lib\opt" (por ejemplo, C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) del conector de C++.
2. Desde Visual Studio, en Proyecto -> Propiedades -> C/C++ -> General -> Directorios de inclusión adicionales:
   - Agregue el directorio "\include" del conector de C++ (por ejemplo, x86)\MySQL\MySQL Connector C++ 1.1.9\include\).
   - Agregue el directorio raíz de la biblioteca Boost (por ejemplo, C:\boost_1_64_0\).
3. Desde Visual Studio, en Proyecto -> Propiedades -> Enlazador -> Entrada > Dependencias adicionales, agregue **mysqlcppconn.lib** en el campo de texto.
4. Copie el archivo **mysqlcppconn.dll** desde la carpeta de la biblioteca del conector de C++ en el paso 3 al mismo directorio que la aplicación ejecutable o agréguelo a la variable de entorno para que la aplicación pueda encontrarlo.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión
Obtenga la información de conexión necesaria para conectarse a Azure Database for MySQL. Necesitará el nombre completo del servidor y las credenciales de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú izquierdo de Azure Portal, haga clic en **Todos los recursos** y, luego, busque el servidor que ha creado, por ejemplo, **mydemoserver**.
3. Haga clic en el nombre del servidor.
4. En el panel **Información general** del servidor, anote el **nombre del servidor** y el **nombre de inicio de sesión del administrador del servidor**. Si olvida la contraseña, puede restablecerla en este panel.
 ![Nombre del servidor de Azure Database for MySQL](./media/connect-cpp/1_server-overview-name-login.png)

## <a name="connect-create-table-and-insert-data"></a>Conexión, creación de una tabla e inserción de datos
Use el código siguiente para conectarse y cargar los datos mediante las instrucciones SQL **CREATE TABLE** e **INSERT INTO**. El código usa la clase sql::Driver con el método connect() para establecer una conexión con MySQL. A continuación, el código usa los métodos createStatement() y execute() para ejecutar los comandos de base de datos. 

Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

//for demonstration only. never save your password in the code!
const string server = "tcp://yourservername.mysql.database.azure.com:3306";
const string username = "username@servername";
const string password = "yourpassword";

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        con = driver->connect(server, username, password);
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to server. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    //please create database "quickstartdb" ahead of time
    con->setSchema("quickstartdb");

    stmt = con->createStatement();
    stmt->execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt->execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con->prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt->setString(1, "banana");
    pstmt->setInt(2, 150);
    pstmt->execute();
    cout << "One row inserted." << endl;

    pstmt->setString(1, "orange");
    pstmt->setInt(2, 154);
    pstmt->execute();
    cout << "One row inserted." << endl;

    pstmt->setString(1, "apple");
    pstmt->setInt(2, 100);
    pstmt->execute();
    cout << "One row inserted." << endl;

    delete pstmt;
    delete con;
    system("pause");
    return 0;
}
```

## <a name="read-data"></a>Lectura de datos

Use el código siguiente para conectarse y leer los datos mediante la instrucción SQL **SELECT**. El código usa la clase sql::Driver con el método connect() para establecer una conexión con MySQL. A continuación, el código usa los métodos prepareStatement() y executeQuery() para ejecutar los comandos de selección. Después, el código usa next() para avanzar a los registros de los resultados. Finalmente, el código usa getInt() and getString() para analizar los valores del registro.

Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

//for demonstration only. never save your password in the code!
const string server = "tcp://yourservername.mysql.database.azure.com:3306";
const string username = "username@servername";
const string password = "yourpassword";

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver->connect(server, username, password);
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to server. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    con->setSchema("quickstartdb");

    //select  
    pstmt = con->prepareStatement("SELECT * FROM inventory;");
    result = pstmt->executeQuery();

    while (result->next())
        printf("Reading from table=(%d, %s, %d)\n", result->getInt(1), result->getString(2).c_str(), result->getInt(3));

    delete result;
    delete pstmt;
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Actualización de datos
Use el código siguiente para conectarse y leer los datos mediante la instrucción SQL **UPDATE**. El código usa la clase sql::Driver con el método connect() para establecer una conexión con MySQL. A continuación, el código usa los métodos prepareStatement() y executeQuery() para ejecutar los comandos de actualización. 

Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

//for demonstration only. never save your password in the code!
const string server = "tcp://yourservername.mysql.database.azure.com:3306";
const string username = "username@servername";
const string password = "yourpassword";

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver->connect(server, username, password);
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to server. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
    
    con->setSchema("quickstartdb");

    //update
    pstmt = con->prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt->setInt(1, 200);
    pstmt->setString(2, "banana");
    pstmt->executeQuery();
    printf("Row updated\n");

    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Eliminación de datos
Use el código siguiente para conectarse y leer los datos mediante la instrucción SQL **DELETE**. El código usa la clase sql::Driver con el método connect() para establecer una conexión con MySQL. A continuación, el código usa los métodos prepareStatement() y executeQuery() para ejecutar los comandos de eliminación.

Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

//for demonstration only. never save your password in the code!
const string server = "tcp://yourservername.mysql.database.azure.com:3306";
const string username = "username@servername";
const string password = "yourpassword";

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver->connect(server, username, password);
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to server. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
    
    con->setSchema("quickstartdb");
        
    //delete
    pstmt = con->prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt->setString(1, "orange");
    result = pstmt->executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Migre su Base de datos MySQL a Azure Database for MySQL mediante el volcado y la restauración](concepts-migrate-dump-restore.md)
