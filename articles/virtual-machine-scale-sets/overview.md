---
title: Azure Sanal Makine Ölçek Kümelerine Genel Bakış | Microsoft Docs
description: Azure sanal makine ölçek kümeleri ve uygulamalarınızın otomatik olarak nasıl ölçeklendirildiği hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 05/21/2018
ms.author: manayar
ms.openlocfilehash: 98c183d65a18c186b515df8ec02ba26b8c2c4143
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258299"
---
# <a name="what-are-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri nedir?
Azure sanal makine ölçek kümeleri sayesinde bir grup özdeş, yük dengeli sanal makineler oluşturup yönetebilirsiniz. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Ölçek kümeleri, uygulamalarınıza yönelik yüksek düzeyde kullanılabilirlik sağlar ve çok sayıda sanal makineyi merkezi olarak yönetmenize, yapılandırmanıza ve güncelleştirmenize imkan tanır. Sanal makine ölçek kümeleri ile, işlem, büyük veri ve kapsayıcı iş yükleri gibi alanlar için büyük ölçekli hizmetler derleyebilirsiniz.


## <a name="why-use-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri neden kullanılmalıdır?
Yedekleme ve gelişmiş performans sağlamak için uygulamalar genellikle birden çok örnek arasında dağıtılır. Müşteriler, uygulama örneklerinden birine istekleri dağıtan bir yük dengeleyici aracılığıyla uygulamanıza erişebilir. Bakım gerçekleştirmeniz veya bir uygulama örneğini güncelleştirmeniz gerekirse müşterileriniz başka bir kullanılabilir uygulama örneğine dağıtılmalıdır. Ek müşteri talebine yanıt vermek için, uygulamanızı çalıştıran uygulama örneği sayısını artırmanız gerekebilir.

Azure sanal makine ölçek kümeleri, birçok sanal makinede çalıştırılan uygulamalar için yönetim yeteneklerini, [kaynakların otomatik ölçeklendirmesini](virtual-machine-scale-sets-autoscale-overview.md) ve trafiğin yük dengelemesini sağlar. Ölçek kümeleri aşağıdaki temel avantajları sağlar:

- **Birden çok sanal makine oluşturma ve yönetme kolaylığı**
    - Uygulamanızı çalıştıran birçok sanal makineniz olduğunda, ortamınızda tutarlı bir yapılandırmanın tutulması önemlidir. Uygulamanızın güvenilir performansı için, sanal makine boyutu, disk yapılandırması ve uygulama yüklemeleri, tüm sanal makineler arasında eşleşmelidir.
    - Ölçek kümeleri ile tüm sanal makine örnekleri, aynı temel işletim sistemi görüntüsü ve yapılandırmasından oluşturulur. Bu yaklaşım, ek yapılandırma görevi veya ağ yönetimi olmadan yüzlerce sanal makineyi kolayca yönetmenize olanak sağlar.
    - Ölçek kümeleri, temel katman 4 trafik dağıtımı için [Azure yük dengeleyici](../load-balancer/load-balancer-overview.md) ve daha gelişmiş katman 7 trafik dağıtımı ve SSL sonlandırması için [Azure Application Gateway](../application-gateway/application-gateway-introduction.md) kullanımını destekler.

- **Yüksek kullanılabilirlik ve uygulama dayanıklılığı sağlar**
    - Ölçek kümeleri, uygulamanızın birden çok örneğini çalıştırmak için kullanılır. Bu sanal makine örneklerinden birinde sorun varsa müşteriler minimum kesintiyle diğer sanal makine örneğinden biri üzerinden uygulamanıza erişmeye devam eder.
    - Ek kullanılabilirlik için, [Kullanılabilirlik Alanları](../availability-zones/az-overview.md)’nı kullanarak tek bir veri merkezinde veya birden çok veri merkezinde bulunan bir ölçek kümesinde sanal makine örneklerini otomatik olarak dağıtabilirsiniz.

- **Kaynak talebi değiştikçe, uygulamanızın otomatik olarak ölçeklendirme yapmasını sağlar**
    - Uygulamanıza yönelik müşteri talebi gün veya hafta boyunca değişebilir. Uygulama talebi arttıkça ölçek kümeleri, müşteri talebini eşleştirmek için sanal makine örneği sayısını otomatik olarak artırabilir ve sonra talep azaldıkça sanal makine örneği sayısını azaltabilir.
    - Bir yandan otomatik ölçeklendirme, talep düşükken uygulamanızı çalıştıran gereksiz sanal makine örneği sayısını en aza indirirken, diğer yandan da talep arttıkça müşteriler kabul edilebilir performans düzeyi elde etmeye devam eder ve otomatik olarak ek sanal makine örnekleri eklenir. Bu yetenek gerektiğinde maliyetlerin düşürülmesine ve etkili şekilde Azure kaynakları oluşturulmasına yardımcı olur.

- **Büyük ölçekte çalışma**
    - Ölçek kümeleri, 1000 adede kadar sanal makine örneğini destekler. Oluşturun ve kendi özel VM görüntülerini karşıya yükleme, 600 sanal makine örneği sınırını olur.
    - Üretim iş yükleri ile en iyi performans için kullanmak [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md).


## <a name="differences-between-virtual-machines-and-scale-sets"></a>Sanal makineler ile ölçek kümeleri arasındaki farklar
Ölçek kümeleri, sanal makinelerden derlenir. Ölçek kümeleri ile, uygulamalarınızı çalıştırmak ve ölçeklendirmek için yönetim ve otomasyon katmanları sağlanır. Bunun yerine tek tek sanal makineleri el ile oluşturup yönetebilir veya benzer bir otomasyon düzeyi derlemek için mevcut araçları tümleştirebilirsiniz. Aşağıdaki tabloda, ölçek kümelerinin birden çok sanal makine örneğinin el ile yönetilmesine kıyasla avantajları açıklanmaktadır.

| Senaryo                           | Sanal makinelerin el ile gruplanması                                                                    | Sanal makine ölçek kümesi |
|------------------------------------|----------------------------------------------------------------------------------------|---------------------------|
| Ek sanal makine örnekleri ekleme        | El ile oluşturma, yapılandırma ve uyumluluk sağlama işlemi                             | Merkezi yapılandırmadan otomatik olarak oluşturma |
| Trafik dengeleme ve dağıtımı | El ile Azure yük dengeleyici veya Application Gateway ve yapılandırma işlemi      | Otomatik olarak Azure yük dengeleyici veya Application Gateway oluşturabilir ve bunlarla tümleştirilebilir |
| Yüksek düzeyde kullanılabilirlik ve yedekleme   | El ile Kullanılabilirlik Kümesi oluşturma veya Kullanılabilirlik Bölgelerinde sanal makineleri dağıtma ve izleme | Kullanılabilirlik Bölgelerinde veya Kullanılabilirlik Kümelerinde sanal makine örneklerinin otomatik dağıtımı |
| Sanal makinelerin ölçeklendirmesi                     | El ile izleme ve Azure Otomasyonu                                                 | Ana bilgisayar ölçümlerine, konuk ölçümlerine, Application Insights’a veya zamanlamaya dayalı otomatik ölçeklendirme |

Ölçek kümelerine yönelik ek bir maliyet yoktur. Yalnızca sanal makine örnekleri, yük dengeleyici veya Yönetilen Disk depolama alanı gibi temel işlem kaynakları için ödeme yaparsınız. Otomatik ölçeklendirme ve yedekleme gibi yönetim ve otomasyon özellikleri, sanal makineler kullanıldığında ek ücrete yol açmaz.


## <a name="next-steps"></a>Sonraki adımlar
Başlamak için, Azure portalında ilk sanal makine ölçek kümenizi oluşturun.

> [!div class="nextstepaction"]
> [Azure portalında ölçek kümesi oluşturma](quick-create-portal.md)
