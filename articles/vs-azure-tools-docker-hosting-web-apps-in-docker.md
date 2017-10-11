---
title: "Bir ASP.NET Core Linux Docker kapsayıcısı uzak bir Docker konağına dağıtın | Microsoft Docs"
description: "Bir Azure Docker ana Linux VM'de çalışan bir Docker kapsayıcısı ASP.NET Core web uygulama dağıtmak için Docker için Visual Studio Araçları kullanmayı öğrenin"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Bir ASP.NET kapsayıcısı uzak bir Docker konağına dağıtın
## <a name="overview"></a>Genel Bakış
Docker ana uygulamalar ve hizmetler için kullanabileceğiniz bir sanal makineye, bazı şekillerde benzer bir basit kapsayıcı alt yapısıdır.
Bu öğretici, kullanarak kılavuzluk [Docker için Visual Studio Araçları](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) Docker konağına PowerShell kullanarak Azure üzerinde ASP.NET Core uygulama dağıtmak için uzantı.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakiler bu öğreticiyi tamamlamak için gereklidir:

* Bir Azure Docker ana VM açıklandığı gibi oluşturmak [docker makine Azure ile kullanma](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* En son sürümünü yüklemek [Visual Studio](https://www.visualstudio.com/downloads/)
* Karşıdan [Microsoft ASP.NET Core 1.0 SDK'sı](https://go.microsoft.com/fwlink/?LinkID=809122)
* Yükleme [Windows için Docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Bir ASP.NET Core web uygulaması oluşturma
Aşağıdaki adımlar Bu öğreticide kullanılan temel bir ASP.NET Core uygulama oluşturmada size yol.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker desteği ekleme
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. DockerTask.ps1 PowerShell komut dosyası kullan
1. Projenizi kök dizininin bir PowerShell komut istemini açın. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Uzak doğrulama konak çalışıyor. Durumunu görmelisiniz çalışan = 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Uygulamasını kullanarak yapı yapı parametresi
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Uygulama Çalıştırma kullanarak Çalıştır parametresi
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Docker işlemi tamamlandıktan sonra aşağıdakine benzer sonuçlar görmeniz gerekir:
   
   ![Uygulamanızı görüntülemek][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
