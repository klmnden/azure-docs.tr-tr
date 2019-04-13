---
title: Azure Service Fabric Mesh CLI’yi ayarlama | Microsoft Docs
description: Azure Service Fabric Mesh CLI’yi ayarlamayı öğrenin.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: c716ae0a2bb30e7e8eb249a1d230097efc0d3795
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521025"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Service Fabric Mesh komut satırı arabirimi (CLI) dağıtmak ve yerel olarak ve Azure Service Fabric Mesh kaynakları yönetmek için gereklidir. 

Kullanılabilir CLI üç tür vardır ve bunlar aşağıdaki tabloda özetlenmiştir. 

| CLI Modülü | Hedef ortam |  Açıklama | 
|---|---|---|
| az mesh | Azure Service Fabric Mesh | Uygulamalarınızı dağıtma ve Azure Service Fabric Mesh ortama yönelik kaynakları yönetmenize olanak tanıyan birincil CLI. 
| sfctl | Yerel küme | Dağıtım ve Service Fabric yerel küme kaynaklarında test sağlayan Service Fabric CLI.  
| Maven CLI | Yerel kümeler ve Azure Service Fabric Mesh | Çevresinde bir sarmalayıcı `az mesh` ve `sfctl` Java geliştiriciler yerel hem de Azure geliştirme deneyimi için bilindik komut satırı deneyimi sağlar.  

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-azure-service-fabric-mesh-cli"></a>Azure Service Fabric kafes CLI yükleme
1. Azure CLI Sürüm 2.0.43 yüklemeniz gerekir ya da daha sonra. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI yükleme][azure-cli-install].

2. Aşağıdaki komutu kullanarak Azure Service Fabric CLI'sını Mesh uzantısı modülünü yükleyin. 

    ```azurecli-interactive
    az extension add --name mesh
    ```

3. Aşağıdaki komutu kullanarak mevcut bir Azure Service Fabric CLI'sını Mesh modülü güncelleştirin.

    ```azurecli-interactive
    az extension update --name mesh
    ```

## <a name="install-the-service-fabric-cli-sfctl"></a>Service Fabric CLI (sfctl) yükleme 

Yönergeleri takip edin [Service Fabric CLI'yı ayarlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli). **Sfctl** modülü, Service Fabric kümeleri yerel makinenizde karşı kaynak modeline bağlı uygulamaları dağıtımını için kullanılabilir. 

## <a name="install-the-maven-cli"></a>CLI Maven'i yükleyin 

Maven CLI makinenizde yüklü olması için aşağıdaki gereksinimleri kullanmak için: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* Azure ağ CLI (az kafes) - Azure Service Fabric Mesh hedefine 
* Yerel küme hedeflemek için SFCTL (sfctl)- 

Service Fabric için Maven CLI hala Önizleme aşamasındadır. 

Maven plugin Maven Java uygulamanızı kullanmak için aşağıdaki kod parçacığı pom.xml dosyanıza ekleyin:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
          <configuration>
            ...
          </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Okuma [Maven CLI başvurusu](service-fabric-mesh-reference-maven.md) bölümü ayrıntılı kullanımı hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

[Windows dağıtım ortamınızı](service-fabric-mesh-howto-setup-developer-environment-sdk.md) da ayarlayabilirsiniz.

[Sık sorulan soruların ve sorunların](service-fabric-mesh-faq.md) yanıtlarını bulun.

[azure-cli-install]: /cli/azure/install-azure-cli
