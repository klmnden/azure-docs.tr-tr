---
title: "Docker makine ile azure'da Docker konakları oluştur | Microsoft Docs"
description: "Docker Azure'da docker ana bilgisayarları oluşturmak için makine kullanımını açıklar."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker-Machine ile Azure’da Docker Ana Bilgisayarları Oluşturma
Çalışan [Docker](https://www.docker.com/) kapsayıcıları VM docker arka plan programı çalıştıran bir konak gerektirir.
Bu konuda nasıl kullanılacağını açıklar [docker makine](https://docs.docker.com/machine/) yeni Linux VM'ler, Azure'da çalışan Docker daemon ile yapılandırılmış oluşturmak için komutu. 

**Not:** 

* *Bu makalede docker makine sürüm 0.9.0-rc2 ya da büyük bağlıdır*
* *Windows kapsayıcıları docker-makine üzerinden yakın gelecekte desteklenecektir*

## <a name="create-vms-with-docker-machine"></a>Docker makineyle VM'ler oluşturma
Docker ana VM'ler ile Azure oluşturmak `docker-machine create` komutu kullanılarak `azure` sürücü. 

Azure sürücüsü, abonelik kimliği gereklidir. Kullanabileceğiniz [Azure CLI](cli-install-nodejs.md) veya [Azure Portal](https://portal.azure.com) Azure aboneliğinizi alınamadı. 

**Azure Portalı'nı kullanarak**

* Seçin **abonelikleri** sol gezinti sayfasında ve kopyalama abonelik kimliği.

**Azure CLI kullanma**

* Tür ```azure account list``` ve abonelik kimliğini kopyalayın.

Tür `docker-machine create --driver azure` seçenekleri ve varsayılan değerleri görmek için.
Ayrıca bkz [Docker Azure sürücü belgelerine](https://docs.docker.com/machine/drivers/azure/) daha fazla bilgi için. 

Aşağıdaki örnek bağlı kullanır [varsayılan değerlerin](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), ancak isteğe bağlı olarak bu değerleri ayarlayın: 

* Azure dns için oluşturulan sertifikaları ve genel IP ile ilişkili adı. Sanal makineniz DNS adıdır. VM ardından güvenli bir şekilde Durdur, dinamik IP bırakın ve vm ile yeni bir IP yeniden başlatıldıktan sonra yeniden bağlanmayı olanağı sunar. Adı ön eki Bu bölge UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com benzersiz olması gerekir.
* VM giden internet erişimi için bağlantı noktası 80'i açın
* daha hızlı premium depolama alanını VM boyutu
* premium depolama vm disk için kullanılır

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Docker docker makineyle seçin
Docker-makine konağınız için bir giriş olduktan sonra docker komutlarını çalıştırırken varsayılan ana bilgisayar ayarlayabilirsiniz.

## <a name="using-powershell"></a>PowerShell’i kullanma
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Bash kullanma
```bash
eval $(docker-machine env MyDockerHost)
```

Belirtilen konak karşı şimdi docker komutları çalıştırabilirsiniz

```
docker ps
docker info
```

## <a name="run-a-container"></a>Bir kapsayıcı çalıştırın
Yapılandırılmış olan bir konak ana bilgisayarınız doğru yapılandırılmış olup olmadığını sınamak için basit bir web sunucusu artık çalıştırabilirsiniz.
Burada bir standart nginx yansıması kullanın, bağlantı noktası 80 üzerinde dinleme yapması gerektiğini belirtin ve VM konak yeniden başlatılırsa, kapsayıcı olur de yeniden (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Çıktı aşağıdaki gibi görünmelidir:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Test kapsayıcısı
Kullanarak çalışan kapsayıcılar inceleyin `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Ve çalışan kapsayıcı görmek için şunu yazın `docker-machine ip <VM name>` tarayıcıya girmek için IP adresini bulmak için:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Çalışan ngnix kapsayıcı](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Özet
Docker-makineyle Azure docker ana bilgisayarlar için ayrı ayrı docker ana doğrulamaları kolayca sağlayabilirsiniz.
Üretim için kapsayıcı görevi, barındırma bkz [Azure kapsayıcı hizmeti](http://aka.ms/AzureContainerService)

Visual Studio ile .NET Core uygulamaları geliştirmek için bkz: [Visual Studio için Docker araçları](http://aka.ms/DockerToolsForVS)

