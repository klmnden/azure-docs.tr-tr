---
title: Docker Machine ile azure'da Docker ana bilgisayarları oluşturma | Microsoft Docs
description: Azure'da docker konakları oluşturmak için Docker Machine kullanımını açıklar.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 98b2d76f4b8ba9ba8316f3a33fd2ff5ed5899833
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969534"
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker-Machine ile Azure’da Docker Ana Bilgisayarları Oluşturma
Çalışan [Docker](https://www.docker.com/) kapsayıcılar, docker daemon çalıştıran sanal makine bir konak gerektirir.
Bu konu nasıl kullanılacağını açıklar [docker-machine](https://docs.docker.com/machine/) Azure'da çalışan Docker daemon ile yapılandırılmış yeni Linux VM'ler oluşturmak için komutu. 

**Not:** 

* *Bu makale docker-machine sürüm 0.9.0-rc2 veya büyük bağlıdır.*
* *Windows kapsayıcıları docker-machine ile yakın gelecekte desteklenecektir.*

## <a name="create-vms-with-docker-machine"></a>Docker makinesi ile VM oluşturma
Docker ile azure'da Vm'leri barındıracak oluşturma `docker-machine create` komutunu kullanarak `azure` sürücü. 

Abonelik kimliğinizi Azure sürücüsü gerektirir Kullanabileceğiniz [Azure CLI](cli-install-nodejs.md) veya [Azure portalı](https://portal.azure.com) Azure aboneliğinizi alınacak. 

**Azure portalını kullanarak**

* Seçin **abonelikleri** sol gezinti bölmesi ve kopyalama abonelik kimliği.

**Azure CLI kullanma**

* Tür ```azure account list``` ve abonelik kimliğini kopyalayın.

Tür `docker-machine create --driver azure` seçenekleri ve varsayılan değerleri görmek için.
Ayrıca bkz [Docker Azure sürücüsü belgeleri](https://docs.docker.com/machine/drivers/azure/) daha fazla bilgi için. 

Aşağıdaki örnek, bağlı kullanır [varsayılan değerler](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), ancak isteğe bağlı olarak bu değerleri ayarlayın: 

* Azure dns genel IP ile ilişkili adı ve oluşturulan sertifikaları. Sanal makinenizi DNS adıdır. VM ardından güvenli bir şekilde durdurabilir, dinamik IP sürüm ve vm yeni bir IP ile yeniden başlatıldıktan sonra yeniden bağlanma olanağı sağlar. Ad ön eki UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com Bu bölge için benzersiz olmalıdır.
* Giden internet erişimi için VM üzerindeki 80 numaralı bağlantı noktasını açın
* daha hızlı premium depolama kullanmak için VM boyutu
* premium depolama vm disk için kullanılan

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Docker-machine ile docker konağı seçin
Bir giriş için ana bilgisayarınızda docker-machine oluşturduktan sonra varsayılan ana bilgisayarda docker komutlarını çalıştırırken ayarlayabilirsiniz.

## <a name="using-powershell"></a>PowerShell’i kullanma
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Bash kullanarak
```bash
eval $(docker-machine env MyDockerHost)
```

Belirtilen konak karşı docker komutları artık çalıştırabilirsiniz

```
docker ps
docker info
```

## <a name="run-a-container"></a>Bir kapsayıcı çalıştırın
Yapılandırılmış bir konakla konağınız düzgün yapılandırılmış olup olmadığını test etmek için basit bir web sunucusu artık çalıştırabilirsiniz.
Burada standart nginx görüntüsü kullanmak, 80 numaralı bağlantı noktasında dinleme yapması gerektiğini belirtin ve konak VM yeniden başlatılırsa, kapsayıcı kullanacağı de yeniden (`--restart=always`). 

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
Çalışan kapsayıcıları kullanarak incelemek `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Ve çalışmakta olan kapsayıcıyı görmek için şunu yazın `docker-machine ip <VM name>` tarayıcıya girmek için IP adresini bulmak için:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Çalışan ngnix kapsayıcı](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Özet
Docker-machine ile azure'da docker ana bilgisayarları için tek bir docker konağı doğrulamaları kolayca sağlayabilirsiniz.
Kapsayıcılar barındırma için üretim bkz [Azure Container Service](http://aka.ms/AzureContainerService)

Visual Studio ile .NET Core uygulamaları geliştirmek için bkz: [Visual Studio için Docker araçları](http://aka.ms/DockerToolsForVS)

