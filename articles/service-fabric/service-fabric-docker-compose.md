---
title: Azure Service Fabric Docker Compose dağıtımı Önizleme
description: Azure Service Fabric, Service Fabric kullanarak mevcut kapsayıcıları düzenleyin daha kolay hale getirmek için Docker Compose biçimlerini kabul eder. Bu destek, şu anda Önizleme aşamasındadır.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: da86ed9a3e6979bd1dc05aef6ef70c7b8533a8c1
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661403"
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>(Önizleme) Azure Service fabric'te docker Compose dağıtımı desteği

Docker kullanan [docker-compose.yml](https://docs.docker.com/compose) çok kapsayıcılı uygulamalar tanımlamak için dosya. Müşteriler için Azure Service fabric'te mevcut kapsayıcı uygulamaları düzenlemek için Docker ile tanıdık kolaylaştırmak için Docker Compose dağıtımı için Önizleme desteği yerel olarak platform ekledik. Service Fabric sürümü 3 ve sonraki sürümlerinde kabul edebilir `docker-compose.yml` dosyaları. 

Bu destek, Önizleme aşamasında olduğundan, yalnızca bir alt kümesini Compose yönergelerinin desteklenir. Örneğin, uygulama yükseltme desteklenmez. Ancak, her zaman kaldırabileceğiniz ve yerine dilediğinizde uygulamaları dağıtın.

Bu önizleme özelliğini kullanmak için 5.7 veya büyük karşılık gelen SDK ile birlikte Azure portal aracılığıyla Service Fabric çalışma zamanı sürümü ile kümenizi oluşturun. 

> [!NOTE]
> Bu özellik Önizleme aşamasındadır ve üretim ortamında desteklenmez.
> Aşağıdaki örnekler, çalışma zamanı sürüm 6.0 ve SDK sürüm 2.8 temel alır.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Bir Service Fabric üzerinde Docker Compose dosyası dağıtma

Aşağıdaki komutlar bir Service Fabric uygulaması oluşturma (adlı `fabric:/TestContainerApp`), izleme ve yönetme gibi başka bir Service Fabric uygulama. Sistem durumu sorgularının sayısı için belirtilen uygulama adı kullanabilirsiniz.
Service Fabric Compose dağıtımı tanımlayıcı olarak "DeploymentName" tanır.

### <a name="use-powershell"></a>PowerShell kullanma

Bir Service Fabric Compose dağıtımı PowerShell'de aşağıdaki komutu çalıştırarak docker-compose.yml dosyasından oluşturun:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName` ve `RegistryPassword` kapsayıcı kayıt defteri kullanıcı adı ve parola bakın. Dağıtımını tamamladıktan sonra aşağıdaki komutu kullanarak durumu denetleyebilirsiniz:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

Compose dağıtımı PowerShell aracılığıyla silmek için aşağıdaki komutu kullanın:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

PowerShell aracılığıyla bir Compose dağıtımı yükseltme işlemini başlatmak için aşağıdaki komutu kullanın:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

Geri alma Compose dağıtımı PowerShell aracılığıyla yükseltme, aşağıdaki komutu kullanın:

```powershell
Start-ServiceFabricComposeDeploymentRollback -DeploymentName TestContainerApp
```

Yükseltme kabul edildikten sonra aşağıdaki komutu kullanarak yükseltme işlemi ilerleme durumu izlenebilir:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) kullanın

Alternatif olarak, aşağıdaki Service Fabric CLI komutunu kullanabilirsiniz:

```azurecli
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Dağıtım oluşturduktan sonra aşağıdaki komutu kullanarak durumu denetleyebilirsiniz:

```azurecli
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Compose dağıtımı silmek için aşağıdaki komutu kullanın:

```azurecli
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Compose dağıtımı yükseltme işlemini başlatmak için aşağıdaki komutu kullanın:

```azurecli
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Compose dağıtımı geri alma, yükseltme, aşağıdaki komutu kullanın:

```azurecli
sfctl compose upgrade-rollback --deployment-name TestContainerApp [ --timeout ]
```

Yükseltme kabul edildikten sonra aşağıdaki komutu kullanarak yükseltme işlemi ilerleme durumu izlenebilir:

```azurecli
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Desteklenen oluşturma yönergeleri

Bu önizlemede aşağıdaki temelleri dahil olmak üzere Compose sürüm 3 biçimi yapılandırma seçeneğinden kümesini destekler:

* Hizmetleri > dağıtma > çoğaltmalar
* Hizmetleri > dağıtma > yerleştirme > kısıtlamaları
* Hizmetleri > dağıtma > kaynak > sınırları
    * cpu paylaşımları
    * -bellek
    * -bellek-swap
* Hizmetleri > komutları
* Hizmetleri > ortam
* Hizmetleri > bağlantı noktaları
* Hizmetleri > Görüntü
* Hizmetleri > yalıtımını (yalnızca Windows)
* Hizmetleri > Günlük > sürücü
* Hizmetleri > Günlük > sürücüsü > Seçenekleri
* Toplu & dağıtma > birim

Kaynak sınırları zorunlu tutmak için kümesi açıklandığı ayarlama [Service Fabric kaynak İdaresi](service-fabric-resource-governance.md). Tüm diğer Docker Compose yönergeleri Bu önizleme için desteklenmez.

### <a name="ports-section"></a>Bağlantı noktaları bölümüne

Service Fabric hizmeti dinleyicisi tarafından kullanılan bağlantı noktaları bölümündeki http veya https protokolünü belirtin. Bu uç nokta Protokolü doğru isteklerini iletmek ters proxy izin vermek için adlandırma hizmeti ile yayımlanan şunları sağlar:
* Güvenli olmayan Service Fabric Compose Hizmetleri yönlendirmek belirtin **/http**. Örneğin,- **"80:80 / http"**.
* Güvenli Service Fabric Compose Hizmetleri yönlendirmek belirtin **/https**. Örneğin,- **"443:443 / https"**.

> [!NOTE]
> Service Fabric dinleyici URL'sini doğru kaydetmek için Service Fabric /http ve /https bağlantı noktaları bölümünde söz dizimi özeldir.  Docker compose dosyası sözdizimi programlı olarak doğrulandı, bir doğrulama hatasına neden.

## <a name="servicednsname-computation"></a>ServiceDnsName hesaplama

Compose dosyasında belirttiğiniz hizmet adını bir tam etki alanı adı ise (diğer bir deyişle, bir nokta [.] içerdiği), Service Fabric tarafından kayıtlı DNS adı `<ServiceName>` (nokta dahil olmak üzere). Aksi durumda, her uygulama adı, yol kesimi hizmet DNS adı ile en üst düzey etki alanı etiketini olma ilk yol kesimini bir etki alanı etiketi olur.

Örneğin, belirtilen uygulama adı ise `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` kayıtlı DNS adı olacaktır.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>Service Fabric uygulama modelini (tür tanımı) karşı yazma dağıtımı (örnek tanımı)

Docker-compose.yml dosyası, kapsayıcılar, özellikleri ve yapılandırmalar da dahil olmak üzere dağıtılabilir bir dizi açıklar.
Örneğin, dosya, ortam değişkenleri ve bağlantı noktalarını içerebilir. Docker-compose.yml dosyasında yerleştirme kısıtlamaları, kaynak sınırları ve DNS adları gibi dağıtım parametreleri de belirtebilirsiniz.

[Service Fabric uygulama modelini](service-fabric-application-model.md) kullandığı hizmet türleri ve uygulama türleri sahip olduğunuz aynı türde pek çok uygulama örnekleri. Örneğin, müşteri başına bir uygulama örneği olabilir. Bu tür tabanlı model çalışma zamanı ile kayıtlı aynı uygulama türünün birden çok sürümünü destekler.

Örneğin, bir müşteri örneği AppTypeA 1.0 türünü içeren bir uygulama olabilir ve Müşteri B aynı türü ve sürümü örneği başka bir uygulama olabilir. Uygulama bildirimleri uygulama türleri tanımlamak ve uygulama oluşturduğunuzda, uygulama adını ve dağıtım parametrelerini belirtin.

Bu model, üst düzeyde esneklik sunar ancak biz de burada türleri bildirim dosyanızdan örtüktür daha basit, örnek tabanlı bir dağıtım modeli desteklemeyi planlıyor musunuz. Bu modelde, her uygulama kendi bağımsız bildirimi alır. Bir dağıtım örneği tabanlı biçimi olan docker-compose.yml için destek ekleyerek Biz bu çalışmaların önizlemesini sunuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* Üzerinde okuma [Service Fabric uygulama modeli](service-fabric-application-model.md)
* [Service Fabric CLI kullanmaya başlama](service-fabric-cli.md)
