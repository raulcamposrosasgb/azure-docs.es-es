---
title: Implementación de un contenedor de Docker para ASP.NET en Azure Container Registry (ACR) | Microsoft Docs
description: Aprenda a usar Visual Studio Tools para Docker para implementar una aplicación web de ASP.NET Core en un registro de contenedor.
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: ''
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: mlearned
ms.openlocfilehash: 58df17b17de1d93683875b68dd7c6c087bc6d16d
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972315"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Implementación de un contenedor ASP.NET en un registro de contenedor con Visual Studio
## <a name="overview"></a>Información general
Docker es un motor de contenedor ligero, semejante de alguna manera a una máquina virtual, que puede utilizar para hospedar aplicaciones y servicios.
Este tutorial le guía a través del uso de Visual Studio para publicar su aplicación en contenedores en una instancia de [Azure Container Registry](https://azure.microsoft.com/services/container-registry).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) antes de empezar.

## <a name="prerequisites"></a>requisitos previos
Para completar este tutorial:

* Instalar la versión más reciente de [Visual Studio 2017](https://azure.microsoft.com/downloads/) con la carga de trabajo "ASP.NET y desarrollo web"
* Instalar [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Cree una aplicación web ASP.NET Core
Los siguientes pasos le guían en el proceso de creación de una aplicación ASP.NET Core básica que se usará en este tutorial.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Publique el contenedor en Azure Container Registry.
1. Haga clic con el botón derecho en el **Explorador de soluciones** y elija **Publicar**.
2. En el cuadro de diálogo de destino de publicación, seleccione la pestaña **Container Registry**.
3. Elija **New Azure Container Registry** (Nueva instancia de Azure Container Registry) y haga clic en **Publish** (Publicar).
4. Rellene los valores deseados en el **Create a new Azure Container Registry** (Crear una nueva instancia de Azure Container Registry).

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Prefijo de DNS** | Nombre único globalmente | Nombre que identifica de forma única el nuevo registro de contenedor. |
    | **Suscripción** | Elija una suscripción | La suscripción de Azure que se va a usar. |
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nombre del grupo de recursos en el que se va a crear el registro de contenedor. Elija **Nuevo** para crear un grupo de recursos nuevo.|
    | **[SKU](https://docs.microsoft.com/azure/container-registry/container-registry-skus)** | Estándar | Nivel de servicio del registro de contenedor  |
    | **Ubicación del registro** | Una ubicación cercana a usted | Elija una ubicación en una [región](https://azure.microsoft.com/regions/) cercana a usted o a otros servicios que usarán el registro de contenedor. |
    ![Cuadro de diálogo Crear Azure Container Registry de Visual Studio][0]
5. Haga clic en **Crear**

Ahora puede extraer el contenedor del registro a cualquier host capaz de ejecutar imágenes de Docker, por ejemplo [Azure Container Instances](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
