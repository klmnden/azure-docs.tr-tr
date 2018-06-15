---
title: Azure Service Fabric varolan yürütülebilir bir dağıtma | Microsoft Docs
description: Bir Service Fabric kümesi dağıtılabilmesi amacıyla yürütülebilir, konuk olarak var olan bir uygulama paketleme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/15/2018
ms.author: mfussell
ms.openlocfilehash: cdaf3dae12c2c9da1f6bcbebbff560b98e62bade
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212849"
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Varolan yürütülebilir bir Service Fabric dağıtma
Azure Service Fabric kod, Node.js, Java ya da C++ gibi herhangi bir türde bir hizmet olarak çalıştırabilirsiniz. Service Fabric bu tür hizmet Konuk yürütülebilir başvurur.

Konuk yürütülebilir dosyalar gibi durum bilgisi olmayan hizmetler Service Fabric tarafından kabul edilir. Sonuç olarak, bunlar kullanılabilirlik ve diğer ölçümleri temelinde, bir kümedeki düğümlere yerleştirilir. Bu makalede, paket ve Visual Studio veya komut satırı yardımcı programını kullanarak bir Service Fabric kümesi için yürütülebilir bir konuk dağıtma açıklar.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Service Fabric yürütülebilir Konuk çalıştıran avantajları
Bir Service Fabric kümesi yürütülebilir Konuk çalıştırmak için çeşitli avantajları vardır:

* Yüksek kullanılabilirlik. Service Fabric çalışan uygulamaları yüksek oranda kullanılabilir hale getirilir. Service Fabric uygulaması örneğini çalıştırmasını sağlar.
* Sistem durumu izleme. Service Fabric sistem durumu izleme uygulama çalışıp çalışmadığını ve bir hata olduğunda tanılama bilgileri sağlayan olmadığını algılar.   
* Uygulama yaşam döngüsü yönetimi. Yükseltme sırasında bildirilen hatalı sistem durumu olayı ise yükseltmeler kapalı kalma süresi ile sağlamanın yanı sıra, Service Fabric önceki sürümüne otomatik geri alma işlemi sağlar.    
* Yoğunluğu. Kendi donanım üzerinde çalıştırmak her bir uygulama için ihtiyacını ortadan kaldıran bir kümede birden çok uygulama çalıştırabilirsiniz.
* Bulunabilirliği: REST kullanarak kümedeki diğer hizmetler bulmak için Service Fabric adlandırma hizmeti çağırabilirsiniz. 

## <a name="samples"></a>Örnekler
* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Uygulama ve hizmet bildirim dosyaları'na genel bakış
Bir konuk yürütülebilir dağıtma bir parçası olarak, Service Fabric paketleme ve dağıtım modeli açıklandığı gibi anlamak kullanışlıdır [uygulama modeli](service-fabric-application-model.md). Service Fabric paketleme model üzerinde iki XML dosyasını kullanır: uygulama ve hizmet bildirimleri. ApplicationManifest.xml ve ServiceManifest.xml dosyaları için şema tanımı Service Fabric SDK ile yüklenen *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Uygulama bildirimini** uygulama bildirimi uygulamanın açıklamak için kullanılır. Onu oluşturan hizmetleri listeler ve örnek sayısı gibi nasıl bir veya daha fazla hizmet tanımlamak için kullanılan diğer parametreleri dağıtılmalıdır.

  Service Fabric bir uygulama dağıtımı ve yükseltmesi birimidir. Bir uygulama olası hataları ve olası düzeyine yönetildiği tek bir birim olarak yükseltilebilir. Service Fabric garanti yükseltme işlemi şunlardan biri olduğundan başarılı ya da yükseltme başarısız olursa, uygulama bilinmeyen veya kararsız bir durumda bırakmaz.
* **Hizmet bildirimi** hizmet bildirimi bir hizmet bileşenlerini açıklar. Ad ve tür hizmetini ve kod ve yapılandırma gibi verileri içerir. Hizmet bildirimi dağıtıldıktan sonra hizmeti yapılandırmak için kullanılan bazı ek parametreler de içerir.

## <a name="application-package-file-structure"></a>Uygulama paketi dosya yapısı
Service Fabric bir uygulamayı dağıtmak için uygulama önceden tanımlanmış dizin yapısına uygun olmalıdır. Aşağıda, yapıyı bir örnektir.

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

ApplicationPackageRoot uygulama tanımlar ApplicationManifest.xml dosyası içerir. Uygulamasına dahil edilen her hizmet için bir alt dizin hizmeti gerektiren tüm yapıları kapsamak için kullanılır. Bu alt dizinlere ServiceManifest.xml ve genellikle aşağıdaki şunlardır:

* *Kod*. Bu dizin hizmeti kodunu içerir.
* *Config*. Bu dizin hizmeti belirli bir yapılandırma ayarlarını almak için çalışma zamanında erişebilmesini Settings.xml dosya (ve gerekirse diğer dosyaları) içerir.
* *Veri*. Bu, hizmet gerekebilir ek yerel verileri depolamak için ek bir dizindir. Verileri yalnızca kısa ömürlü verileri depolamak için kullanılmalıdır. Service Fabric kopyalamayın veya hizmet (örneğin, yük devretme sırasında) yeniden konumlandırılmasını gerekiyorsa, değişiklikleri veri dizinine çoğaltır.

> [!NOTE]
> Oluşturmanız gerekmez `config` ve `data` ihtiyaç duymuyorsanız dizinleri.
>
>

## <a name="next-steps"></a>Sonraki adımlar
İlgili bilgi ve görevler için aşağıdaki makalelere bakın.
* [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio kullanarak ilk Konuk yürütülebilir uygulamanızı oluşturma](quickstart-guest-app.md)
* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), yayın öncesi paketleme Aracı'nın bağlantı
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)

