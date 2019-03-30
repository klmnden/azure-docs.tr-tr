---
title: Varolan bir yürütülebilir dosya, Azure Service Fabric'e dağıtma | Microsoft Docs
description: Bir Service Fabric kümesine dağıtılabilmesi olarak bir konuk yürütülebilir dosyası, mevcut bir uygulama paketleme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/15/2018
ms.author: aljo
ms.openlocfilehash: b7efeb1b4d83f6a6b372f73a7c0a5ca9bffdc052
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670651"
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Varolan bir yürütülebilir dosya Service Fabric'e dağıtma
Azure Service Fabric'te kod, Node.js, Java veya C++ gibi herhangi bir türde bir hizmet olarak çalıştırabilirsiniz. Service Fabric Konuk yürütülebilir dosyaları Hizmetleri bu türlere başvurur.

Konuk yürütülebilir dosyaları tarafından Service Fabric durum bilgisi olmayan hizmetler gibi kabul edilir. Sonuç olarak, bunlar kullanılabilirlik ve diğer ölçümlere göre bir kümedeki düğümlere yerleştirilir. Bu makalede, paketleme ve Konuk yürütülebilir dosyası, Visual Studio veya bir komut satırı yardımcı programını kullanarak bir Service Fabric kümesine dağıtma açıklar.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Service Fabric içinde yürütülebilir Konuk çalıştıran avantajları
Bir Service Fabric kümesi içinde yürütülebilir Konuk çalıştırmanın çeşitli avantajları vardır:

* Yüksek kullanılabilirlik. Service Fabric'te çalışan uygulamalar'ın, yüksek oranda kullanılabilir hale getirilir. Service Fabric uygulaması örneğini çalıştırıyorsanız sağlar.
* Sistem durumu izleme. Service Fabric sistem durumu izleme, bir uygulama çalıştıran ve bir hata varsa tanılama bilgileri sağlayan algılar.   
* Uygulama yaşam döngüsü yönetimi. Yükseltme sırasında bildirilen kötü sistem durumu olayı varsa yükseltme kesinti yaşamadan sağlamanın yanı sıra, Service Fabric önceki sürümüne otomatik geri alma sağlar.    
* Yoğunluğu. Bir kümedeki her uygulamanın kendi donanımda çalışan ihtiyacını ortadan kaldırır, birden çok uygulamayı çalıştırabilirsiniz.
* Bulunabilirliği: REST kullanarak kümedeki diğer hizmetleri bulmak için Service Fabric adlandırma hizmetine çağrı yapabilir. 

## <a name="samples"></a>Örnekler
* [Paketleme ve dağıtma Konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Uygulama ve hizmet bildirim dosyaları'na genel bakış
Konuk tarafından yürütülebilir bir dağıtımı bir parçası olarak, Service Fabric paketleme ve dağıtım modelini açıklandığı anlamak kullanışlıdır [uygulama modeli](service-fabric-application-model.md). Service Fabric paketleme modelini iki XML dosyaları kullanır: uygulama ve hizmet bildirimleri. ApplicationManifest.xml ve ServiceManifest.xml dosyaları için şema tanımı Service Fabric SDK'sı ile yüklenen *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Uygulama bildirimi** uygulama bildirimini uygulamayı tanımlamak için kullanılır. Onu oluşturan hizmetler listelenir ve örnek sayısı gibi nasıl bir veya daha fazla hizmet tanımlamak için kullanılan diğer parametreleri dağıtılmalıdır.

  Service Fabric'te uygulama dağıtım ve yükseltme birimidir. Bir uygulamanın olası arızaları ve olası geri alma işlemleri yönetildiği tek bir birim olarak yükseltilebilir. Service Fabric, yükseltme işlemi ya da olduğunu garanti eder başarılı ya da yükseltme başarısız olursa uygulama bilinmeyen ya da kararsız bir durum oluşturmaz.
* **Hizmet bildirimi** hizmet bildirimi bir hizmet bileşenlerinin açıklar. Bu hizmet ve kod ve yapılandırma türünü ve adını gibi verileri içerir. Hizmet bildirimi dağıtıldıktan sonra hizmeti yapılandırmak için kullanılan bazı ek parametreler de içerir.

## <a name="application-package-file-structure"></a>Uygulama paketi dosya yapısı
Service Fabric uygulama dağıtmak için uygulamayı bir önceden tanımlanmış dizin yapısına uygun olmalıdır. Bu yapının bir örnek verilmiştir.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

ApplicationPackageRoot uygulamayı tanımlayan ApplicationManifest.xml dosyasını içerir. Uygulamasına dahil edilen her hizmet için bir alt hizmeti gerektiren tüm yapıtlar içerecek şekilde kullanılır. Bu alt dizinleri ServiceManifest.xml ve bu normalde, aşağıdaki şunlardır:

* *Kod*. Bu dizin hizmet kodunu içerir.
* *Config*. Bu dizin, hizmete özgü yapılandırma ayarlarını almak için çalışma zamanında erişebilir, bir Settings.xml dosyasının (ve gerekirse diğer dosyaları) içerir.
* *Veri*. Bu hizmet gerekebilir ek yerel verileri depolamak için ek bir dizindir. Veriler, yalnızca geçici verileri depolamak için kullanılmalıdır. Service Fabric kopyalayın veya hizmetinin (örneğin, yük devretme sırasında) alınmasını gerekiyorsa değişiklikler için veri dizini çoğaltmak desteklemez.

> [!NOTE]
> Oluşturmanız gerekmez `config` ve `data` ihtiyacınız yoksa dizinleri.
>
>

## <a name="next-steps"></a>Sonraki adımlar
İlgili bilgi ve görevler için aşağıdaki makalelere bakın.
* [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio kullanarak ilk Konuk yürütülebilir uygulamanızı oluşturma](quickstart-guest-app.md)
* [Paketleme ve dağıtma Konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), yayın öncesi paketleme aracı bağlantı
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)

