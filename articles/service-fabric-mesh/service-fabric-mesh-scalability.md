---
title: Azure Service fabric'in ölçeklenebilirlik Örgü uygulamalar | Microsoft Docs
description: Azure Service Fabric Mesh Hizmetleri'nde ölçeklendirme hakkında bilgi edinin.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 1688cac35ea9de43bac529a4994bd4ea55eb0ab7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811097"
---
# <a name="scaling-service-fabric-mesh-applications"></a>Service Fabric Mesh uygulamaları ölçeklendirme

Service Fabric Mesh’e uygulamaları dağıtmanın ana avantajlarından biri, hizmetlerinizin ölçeğini kolayca artırabilmeniz veya azaltabilmenizdir. Hizmetlerinizdeki değişen yük miktarlarını işlemek ve kullanılabilirliği artırmak için bu kullanılmalıdır. Ayrıca, hizmetleri veya veya kurulumu otomatik ölçeklendirme ilkeleri elle ölçeklendirebilirsiniz.

## <a name="manual-scaling-instances"></a>El ile ölçeklendirme örnekleri

Uygulama kaynağı için dağıtım şablonunda her hizmet, hizmetin dağıtılmasını istediğiniz sayıyı ayarlamak için kullanılabilen bir *replicaCount* özelliğine sahiptir. Bir uygulama, birden çok hizmetten oluşabilir; her hizmet, birlikte dağıtılan ve yönetilen benzersiz bir *replicaCount* sayısına sahiptir. Hizmet çoğaltmaları sayısını ölçeklendirmek için, dağıtım şablonunda ölçeklendirmek istediğiniz her bir hizmet için *replicaCount* değerini veya parametreler dosyasını değiştirin. Ardından uygulamayı yükseltin.

Hizmetleri örnekleri el ile ölçeklendirme örnekleri için bkz: [el ile hizmetlerinizi veya dışa ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md).

## <a name="autoscaling-service-instances"></a>Otomatik ölçeklendirme hizmeti örnekleri
Otomatik ölçeklendirme (yatay ölçeklendirme), hizmet örneği sayısını dinamik olarak ölçeklendirmeniz için ek bir Service fabric özelliktir. Otomatik ölçeklendirme, büyük esneklik sağlar ve sağlama veya CPU veya bellek kullanım oranlarına dayalı hizmet örnekleri kaldırılmasını sağlar.  Otomatik ölçeklendirme, hizmet örnekleri iş yükünüz için doğru sayıda çalıştırma ve maliyet iyileştirmenize olanak sağlar.

Otomatik ölçeklendirme İlkesi hizmet kaynak dosyasındaki hizmet başına tanımlanır. Her bir ölçeklendirme İlkesi iki bölümden oluşur:

- Hizmetini ölçeklendirdiğinizde açıklayan bir ölçeklendirme tetikleyici gerçekleştirilir. Zaman hizmeti ölçeklendirecek belirlemek üç faktör vardır. *Alt yükleme eşiği* hizmet içinde ne zaman ölçeklendirileceği belirleyen bir değer. Ortalama yük tüm örneklerinin bölüm bu değerden düşükse, hizmet olarak ayarlanacaktır. *Üst yükleme eşiği* çıkış hizmet ne zaman ölçeklendirileceği belirleyen bir değer. Ortalama yük tüm örneklerinin bölüm bu değerden daha yüksek ise, hizmetin kullanıma ayarlanacaktır. *Ölçeklendirme aralığı* belirler tetikleyici ne sıklıkla (saniye) denetlenir. Ölçeklendirme gerekliyse tetikleyici iade edildikten sonra mekanizması uygulanır. Ölçeklendirme gerekmiyorsa, hiçbir işlem yapılmadı. Ölçeklendirme aralığı sona ermeden önce her iki durumda da, tetikleyici yeniden denetlenmez.

- Tetiklendiğinde nasıl ölçekleme gerçekleştirileceğini açıklayan bir ölçeklendirme mekanizması. *Ölçeği artırma* kaç tane eklenecek veya mekanizması tetiklendiğinde kaldırıldı belirler. *En fazla örnek sayısı* ölçeklendirmeye yönelik üst sınır tanımlar. Örnek sayısı bu sınırı ulaşırsa, ardından hizmet out bağımsız olarak yük ölçeklenmez. *En az örnek sayısı* ölçeklendirme için alt sınırı tanımlar. Örneklerinin bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez.

Hizmetiniz için bir otomatik ölçeklendirme İlkesi hakkında bilgi edinmek için okumaya devam [otomatik ölçeklendirme Hizmetleri](service-fabric-mesh-howto-auto-scale-services.md).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama modeli hakkında daha fazla bilgi için bkz: [Service Fabric kaynakları](service-fabric-mesh-service-fabric-resources.md)
