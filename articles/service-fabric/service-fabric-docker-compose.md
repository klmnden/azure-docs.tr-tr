---
title: "Azure Service Fabric Docker Compose dağıtım Önizleme"
description: "Azure Service Fabric Service Fabric kullanarak var olan kapsayıcıları düzenlemek kolay hale getirmek için Docker Compose'u biçimi kabul eder. Bu destek, şu anda önizlemede değil."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2017
ms.author: subramar
ms.openlocfilehash: b6275cee87455bf8a226a51a6b2093b67c3159d0
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>Docker Compose dağıtım desteği Azure Service Fabric (Önizleme)

Docker kullanan [docker-compose.yml](https://docs.docker.com/compose) çok kapsayıcı uygulamaları tanımlamak için dosya. Müşteriler için Azure Service Fabric varolan kapsayıcı uygulamaları düzenlemek için Docker aşina kolaylaştırmak için biz Docker Compose dağıtım Önizleme desteği yerel olarak platform eklediniz. Service Fabric sürüm 3 ve sonraki kabul edebileceği `docker-compose.yml` dosyaları. 

Bu destek önizlemede olmadığından, yalnızca bir alt kümesini oluşturma yönergeleri desteklenir. Örneğin, uygulama yükseltmeler desteklenmez. Ancak, istediğiniz zaman kaldırma ve bunları yükseltme yerine uygulamaları dağıtma.

Bu önizleme özelliğini kullanmak için 5.7 veya büyük karşılık gelen SDK ile birlikte Azure portalı üzerinden Service Fabric çalışma zamanı sürümü ile kümenizi oluşturun. 

> [!NOTE]
> Bu özellik Önizleme aşamasındadır ve üretimde desteklenmiyor.
> Aşağıdaki örneklerde, çalışma zamanı sürüm 6.0 ve SDK'sı sürüm 2.8 temel alır.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Service Fabric Docker Compose bir dosyada dağıtma

Aşağıdaki komutlar bir Service Fabric uygulaması oluşturma (adlı `fabric:/TestContainerApp`), hangi izleyebilir ve başka bir Service Fabric uygulaması gibi yönetebilirsiniz. Sistem durumu sorgularının sayısı için belirtilen uygulama adı kullanabilirsiniz.
Service Fabric "DeploymentName" Oluştur dağıtım tanımlayıcı olarak tanır.

### <a name="use-powershell"></a>PowerShell kullanma

Service Fabric oluşturan bir dağıtım, PowerShell içinde aşağıdaki komutu çalıştırarak bir docker-compose.yml dosyası oluşturun:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`ve `RegistryPassword` kapsayıcı kayıt defteri kullanıcı adı ve parola bakın. Dağıtımını tamamladıktan sonra aşağıdaki komutu kullanarak durumunu denetleyebilirsiniz:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

PowerShell aracılığıyla oluşturma dağıtımını silmek için aşağıdaki komutu kullanın:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

PowerShell aracılığıyla oluşturma dağıtım yükseltmeyi başlatmak için aşağıdaki komutu kullanın:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

Yükseltme kabul edildikten sonra yükseltme işlemi ilerleme durumu aşağıdaki komutu kullanarak izlenebilir:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -Deployment TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) kullanın

Alternatif olarak, aşağıdaki Service Fabric CLI komutu kullanabilirsiniz:

```azurecli
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Dağıtım oluşturduktan sonra aşağıdaki komutu kullanarak durumunu denetleyebilirsiniz:

```azurecli
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Oluşturma dağıtımını silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Bir oluşturma dağıtımı yükseltme işlemini başlatmak için aşağıdaki komutu kullanın:

```azurecli
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Yükseltme kabul edildikten sonra yükseltme işlemi ilerleme durumu aşağıdaki komutu kullanarak izlenebilir:

```azurecli
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Desteklenen oluşturma yönergeleri

Bu önizleme bir alt kümesini aşağıdaki temelleri dahil olmak üzere Oluştur sürüm 3 biçiminden yapılandırma seçeneklerini destekler:

* Hizmetleri > dağıtmak > çoğaltmaları
* Hizmetleri > dağıtmak > yerleştirme > kısıtlamaları
* Hizmetleri > dağıtmak > kaynak > sınırları
    * cpu-paylaşımları
    * -bellek
    * -bellek-değiştirme
* Hizmetleri > komutları
* Hizmetleri > ortamı
* Hizmetleri > bağlantı noktaları
* Hizmetleri > görüntüsü
* Hizmetleri > yalıtımını (yalnızca Windows)
* Hizmetleri > günlüğü > sürücüsü
* Hizmetleri > günlüğü > sürücü > Seçenekleri
* Birim & dağıtmak > birim

Bölümünde açıklandığı gibi kaynak sınırları, zorlama küme ayarlamak [Service Fabric kaynak İdaresi](service-fabric-resource-governance.md). Tüm diğer Docker Compose yönergeleri Bu önizleme için desteklenmez.

## <a name="servicednsname-computation"></a>ServiceDnsName computation

Bir oluşturma dosyasında belirttiğiniz hizmet adını bir tam etki alanı adı ise (diğer bir deyişle, bir nokta [.] içerdiği), Service Fabric tarafından kayıtlı DNS adı `<ServiceName>` (nokta dahil). Aksi durumda, her yol kesimi uygulama ad hizmeti DNS adı, üst düzey etki alanı etiketi olma ilk yol kesimi ile etki alanı etiketi olur.

Örneğin, belirtilen uygulama adı ise `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` kayıtlı DNS adı olacaktır.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>Service Fabric uygulama modeli (tür tanımı) dağıtımı (örnek tanımı) oluşturan

Docker-compose.yml dosyası kapsayıcılar kendi özellikler ve yapılandırmalar da dahil olmak üzere, dağıtılabilir bir dizi açıklar.
Örneğin, dosyayı ortam değişkenleri ve bağlantı noktası içerebilir. Docker-compose.yml dosyası kısıtlamalarından, kaynak sınırları ve DNS adları gibi dağıtım parametreleri de belirtebilirsiniz.

[Service Fabric uygulama modeli](service-fabric-application-model.md) kullandığı hizmet türlerini ve aynı türde pek çok uygulama örnekleri bulunan uygulama türleri. Örneğin, müşteri başına bir uygulama örneği olabilir. Bu tür tabanlı modeli, çalışma zamanı ile kayıtlı aynı uygulama türünün birden fazla sürümünü destekler.

Örneğin, bir müşterinin AppTypeA 1.0 türüyle örneği bir uygulama olabilir ve Müşteri B aynı türü ve sürümü örneği başka bir uygulama olabilir. Uygulama bildirimleri uygulama türlerini tanımlayın ve uygulama oluşturduğunuzda uygulama adı ve dağıtım parametrelerini belirtin.

Bu model esneklik sunar ancak biz de türleri bildirim dosyasından örtük nerede daha basit ve örnek tabanlı dağıtım modelini destekleyen planlıyorsanız. Bu modelde, her uygulamanın kendi bağımsız bildirimi alır. Biz bu çalışmaların örneği tabanlı dağıtım biçimi olduğu docker-compose.yml için destek ekleyerek önizlemede.

## <a name="next-steps"></a>Sonraki adımlar

* Üzerinde okuma [Service Fabric uygulama modeli](service-fabric-application-model.md)
* [Service Fabric CLI kullanmaya başlama](service-fabric-cli.md)
