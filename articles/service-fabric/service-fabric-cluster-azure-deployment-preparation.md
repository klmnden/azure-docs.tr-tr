---
title: Bir Azure Service Fabric Küme dağıtımı planlama | Microsoft Docs
description: Planlama ve bir üretim için Azure Service Fabric Küme dağıtımı için hazırlama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: aljo
ms.openlocfilehash: 0f3a9010805ec1a18490f6f530f60d7a3c763398
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663247"
---
# <a name="plan-and-prepare-for-a-cluster-deployment"></a>Planlama ve bir küme dağıtımı için hazırlama

Planlama ve hazırlama üretim Küme dağıtımı için çok önemlidir.  Dikkate alınması gereken birçok faktör vardır.  Bu makalede, Küme dağıtımı için hazırlama adımları boyunca size yol gösterir.

## <a name="read-the-best-practices-information"></a>En iyi bilgileri okuyun
Azure Service Fabric uygulama ve kümelerin başarılı bir şekilde yönetmek için üretim ortamınızın güvenilirliği iyileştirmek için almanızı öneririz, işlem vardır.  Daha fazla bilgi için okuma [Service Fabric uygulama ve küme en iyi uygulamalar](service-fabric-best-practices-overview.md).

## <a name="select-the-os-for-the-cluster"></a>Küme için işletim sistemini seçin
Service Fabric Service Fabric kümeleri herhangi bir VM veya Windows Server veya Linux çalıştıran bilgisayarlar üzerinde oluşturulmasını sağlar.  Kümenizi dağıtmadan önce işletim sistemi seçmeniz gerekir:  Windows veya Linux.  Kümedeki her düğüm (sanal makine) aynı işletim sistemini çalıştıran, Windows ve Linux Vm'leri aynı kümede karıştırılamaz.

## <a name="capacity-planning"></a>Kapasite planlaması
Herhangi bir üretim dağıtımı için kapasite planlaması önemli bir adımdır. Bu süreç kapsamında dikkat etmeniz gerekenler şunlardır:

* Düğüm türleri kümenizi için başlangıç sayısı 
* Her düğüm türünün (boyut, birincil örnek sayısı, internet'e yönelik, sayısı VM'ler gibi) özellikleri
* Kümenin güvenilirlik ve dayanıklılık özellikleri

### <a name="select-the-initial-number-of-node-types"></a>Düğüm türleri ilk sayısını seçin
Öncelikle, oluşturmakta olduğunuz küme için kullanılacak neler olduğunu şekil gerekir. Ne tür uygulamaların bu kümesi dağıtmayı planlıyor musunuz? Uygulamanızı birden çok hizmetlere sahip olmadığı ve herhangi biri genel veya internet'e yönelik olmam gerekir mi? Farklı altyapı gereksinimleri gibi daha büyük RAM veya daha yüksek CPU çevrimleri (uygulamanızı ayarlama yapan) hizmetlerinizi var mı? Service Fabric kümesi birden fazla düğüm türü oluşabilir: bir veya daha fazla birincil olmayan düğüm türleri ve birincil düğüm türü. Her düğüm türü, bir sanal makine ölçek kümesine eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. [Düğüm özellikleri ve yerleştirme kısıtlamaları] [ placementconstraints] belirli hizmetlere belirli düğüm türleri sınırlamak için ayarlanabilir.  Daha fazla bilgi için okuma [kümeniz için gerekli olan başlatmak için düğüm türü sayısı](service-fabric-cluster-capacity.md#the-number-of-node-types-your-cluster-needs-to-start-out-with).

### <a name="select-node-properties-for-each-node-type"></a>Her düğüm türü için düğüm özellikleri seçin
Düğüm türleri ilişkili bir ölçek kümesindeki sanal makine SKU'su, sayısı ve sanal makinelerin özelliklerini tanımlayın.

Vm'leri her düğüm türü için en az boyutu tarafından belirlenir [dayanıklılık katmanı] [ durability] düğüm türü seçin.

Birincil düğüm türü için en düşük VM sayısı tarafından belirlenir [güvenilirlik katmanını] [ reliability] belirleyin.

En düşük öneriler için bkz: [birincil düğüm türleri](service-fabric-cluster-capacity.md#primary-node-type---capacity-guidance), [birincil olmayan düğüm türleri üzerinde durum bilgisi olan iş yükleri](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateful-workloads), ve [birincil olmayan düğüm türleri üzerinde durum bilgisiz iş yükleri](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateless-workloads). 

Bu düğüm türü çalıştırmak istediğiniz uygulama/hizmetleri çoğaltmalarının sayısı en düşük düğüm sayısından daha fazla bağlı olmalıdır.  [Kapasite planlaması için Service Fabric uygulamaları](service-fabric-capacity-planning.md) uygulamalarınızı çalıştırmak için ihtiyacınız olan kaynakları tahmin etmenize yardımcı olur. Her zaman, bu küme ölçeği artırma veya uygulama iş yükünü değiştirmek için ayarlamak için daha sonra aşağı. 

### <a name="select-the-durability-and-reliability-levels-for-the-cluster"></a>Küme için dayanıklılık ve güvenilirlik düzeylerini seçin
Dayanıklılık katmanı, sanal makinelerinizin temel Azure altyapısıyla sahip ayrıcalıkları sisteme belirtmek için kullanılır. Birincil düğüm türü, sistem hizmetleri ve durum bilgisi olan hizmetlerinizin çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istek (örneğin, bir VM yeniden başlatma, VM yeniden görüntü oluşturma ya da VM geçişi) duraklatmak Service Fabric bu ayrıcalığı sağlar. Birincil olmayan düğüm türleri bu ayrıcalık, durum bilgisi olan hizmetler için çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istekler (örneğin, VM yeniden başlatma, VM yeniden görüntü oluşturma ve VM geçişi) duraklatmak Service Fabric verir.  Farklı düzeylerde ve önerileri kullanın ve ne zaman görmek için hangi düzeyde avantajları için [kümenin dayanıklılık özelliklerini][durability].

Güvenilirlik katmanı çoğaltmaları birincil düğüm türünde bu kümede çalıştırmak istediğiniz sistem hizmetlerinin sayısını ayarlamak için kullanılır. Daha fazla sayıda yineleme, daha güvenilir, kümenizin sistem hizmetleri şunlardır.  Farklı düzeylerde ve önerileri kullanın ve ne zaman görmek için hangi düzeyde avantajları için [güvenilirliği kümenin][reliability]. 

## <a name="enable-reverse-proxy-andor-dns"></a>Ters proxy ve/veya DNS etkinleştir
Kümedeki düğümler aynı yerel ağda olduğundan Hizmetleri birbirine kümesi içinde genel olarak bağlanan diğer hizmetleri uç noktalarına doğrudan erişebilirsiniz. Hizmetler arasında bağlantı daha kolay hale getirmek için Service Fabric ek hizmetleri sağlar: A [DNS hizmeti](service-fabric-dnsservice.md) ve [ters proxy hizmeti](service-fabric-reverseproxy.md).  Bir küme dağıtılırken, iki hizmet de etkinleştirilebilir.

Birçok hizmet, bu yana özellikle kapsayıcı Hizmetleri, var olan bir URL adı olabilir, bunları çözmek mümkün olan standart DNS kullanarak Protokolü (adlandırma hizmeti Protokolü yerine) kullanışlı özellikle uygulama "kaldırma ve kaydırma" senaryolarında. DNS hizmeti yaptığı tam olarak budur. DNS adlarını bir hizmet adına eşlemenize ve bu nedenle bitiş IP adreslerini sağlar.

Ters proxy (HTTPS dahil) bir HTTP uç noktalarını kullanıma Hizmetleri kümedeki ele alır. Ters proxy, belirli bir URI biçimi sağlayarak diğer hizmetleri çağırma büyük ölçüde kolaylaştırır.  Ters proxy ayrıca çözümleme işleme, bağlama ve birbiriyle iletişim kurmak için bir hizmet için gerekli adımları yeniden deneyin.

## <a name="prepare-for-disaster-recovery"></a>Olağanüstü Durum Kurtarmaya Hazırlanma
Yüksek kullanılabilirlik sunmak için kritik bir parçası Hizmetleri tüm farklı türde hataları hayatta kalamaz sağlamaktır. Bu planlanmamış hataları için özellikle önemlidir ve denetiminiz dışında. [Olağanüstü durum kurtarmasına hazırlanma](service-fabric-disaster-recovery.md) felaketler modellenir ve doğru bir şekilde yönetilen olabilir bazı yaygın hata modları açıklanmaktadır. Ayrıca, risk azaltma işlemleri ve yine de olağanüstü bir durum oluştuysa gerçekleştirilecek eylemler anlatılmaktadır.

## <a name="production-readiness-checklist"></a>Üretim hazırlığı denetim listesi
Uygulama ve küme üretim trafiği almaya hazır mı? Kümenizi üretim ortamına dağıtmadan önce çalışmasını [üretim hazırlık denetim](service-fabric-production-readiness-checklist.md). Uygulama ve küme bu denetim listesindeki öğeleri aracılığıyla sorunsuz çalışmasını tutun. Üretime geçmeden önce iade için tüm bu öğeleri kesinlikle öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* [Windows çalıştıran bir Service Fabric kümesi oluşturma](service-fabric-best-practices-overview.md)
* [Linux çalıştıran bir Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

[placementconstraints]: service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints
[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster