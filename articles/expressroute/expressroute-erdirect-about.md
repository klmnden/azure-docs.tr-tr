---
title: ExpressRoute doğrudan - Azure hakkında | Microsoft Docs
description: Bu sayfa ExpressRoute doğrudan genel bir bakış sağlar.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: e598cc03a1b7b4999719152540866c7168130e03
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807471"
---
# <a name="about-expressroute-direct"></a>ExpressRoute Direct hakkında

ExpressRoute doğrudan, Microsoft'un küresel ağı dünya genelindeki stratejik dağıtılmış eşleme konumlarda doğrudan bağlanma özelliği sağlar. ExpressRoute doğrudan çift 100 GB/sn veya uygun ölçekte etkin/etkin bağlantı destekleyen 10 GB/sn bağlantı sağlar.

ExpressRoute doğrudan sağlayan önemli özellikler dahil ancak bunlarla sınırlı değildir:

* Depolama ve Cosmos DB gibi hizmetler için Büyük Veri Alımı özelliği
* Fiziksel yalıtım gibi özel ve ayrı bağlantı gerektirir ve düzenlenen sektörler için: Banka, devlet ve perakende
* Bağlantı hattı dağıtımının iş birimine dayalı detaylı denetimi

## <a name="onboard-to-expressroute-direct"></a>ExpressRoute doğrudan ekleme

ExpressRoute doğrudan kullanmadan önce aboneliğinizi kaydetmelisiniz. Kaydetmek için bir e-posta Gönder <ExpressRouteDirect@microsoft.com> abonelik Kimliğinizi, aşağıdaki ayrıntılar dahil olmak üzere:

* Senaryo ile gerçekleştirmek için aradığınız **ExpressRoute doğrudan**
* Konum bkz - [iş ortakları ve eşleme konumları](expressroute-locations-providers.md) tüm konumlara tam listesi için
* Uygulama zaman çizelgesi
* Diğer sorular

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute hizmet sağlayıcısı ve ExpressRoute doğrudan kullanma

| **ExpressRoute kullanarak bir hizmet sağlayıcısı** | **ExpressRoute doğrudan** | 
| --- | --- |
| Hızlı ekleme ve mevcut altyapısıyla bağlantısını etkinleştirmek için hizmet sağlayıcıları kullanır. | 100 GB/sn/10 GB/sn altyapı ve tam yönetim tüm katmanların gerektirir
| Sağlayıcı Ethernet ve MPLS gibi yüzlerce ile tümleşir | Düzenlenen sektör ve büyük veri alımı için doğrudan/adanmış kapasite |
| 50 MB/sn devreler SKU'lardan 10 GB/sn | Müşteri, doğrudan aşağıdaki bağlantı hattı SKU'larında 100 GB/sn ExpressRoute bir birleşimini seçebilirsiniz: <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> Müşteri, doğrudan aşağıdaki SKU'larında 10 Gbps ExpressRoute bağlantı hattı bir birleşimini seçebilirsiniz:<ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>
| Tek bir kiracı için en iyi duruma getirilmiş | Tek kiracılı/bulut hizmeti sağlayıcıları için en iyi duruma getirilmiş / birden çok iş birimleri

## <a name="expressroute-direct-circuits"></a>Doğrudan ExpressRoute bağlantı hatları

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  

Her eşleme konumunda, Microsoft'un genel ağ erişimi olan ve varsayılan olarak herhangi bir bölgede coğrafi bölge erişebilir ve tüm küresel bölgeye sahip premium devresi erişebilir.  

Çoğu senaryoda işlevselliğini çalışması için bir ExpressRoute hizmet sağlayıcısı'nı kullanan bağlantı hatlarına eşdeğerdir. Daha fazla ayrıntı düzeyi ve ExpressRoute doğrudan kullanarak sunulan yeni özellikleri desteklemek için doğrudan ExpressRoute bağlantı hatları üzerinde mevcut bazı temel özellikleri vardır.

## <a name="circuit-skus"></a>Bağlantı hattı SKU'ları

ExpressRoute doğrudan Azure depolama ve diğer büyük veri hizmetlerle büyük veri alma senaryolarını destekler. ExpressRoute bağlantı hatları üzerinde 100 GB/sn ExpressRoute doğrudan şimdi de destek **40 GB/sn** ve **100 GB/sn** bağlantı hattı SKU'ları. Fiziksel bağlantı noktası çiftleri **100 veya 10 GB/sn** yalnızca ve birden çok sanal bağlantı hattına sahip olabilir. Bağlantı hattı boyutları:

| **ExpressRoute doğrudan 100 GB/sn** | **10 Gbps ExpressRoute doğrudan** | 
| --- | --- |
| **Bant genişliği abone**: 200 Gbps | **Bant genişliği abone**: 20 Gbps |
| <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> | <ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>

## <a name="technical-requirements"></a>Teknik Gereksinimler

* Microsoft Enterprise Edge (MSEE) yönlendirici arabirimleri:
    * İkili 10 ya da 100 Gigabit Ethernet bağlantı noktası yalnızca yönlendirici çifti arasında
    * Tek modu LR Fiber bağlantısı
    * IPv4 ve IPv6
    * IP MTU 1500 bayt

* Anahtar/yönlendirici Katman 2/Katman 3 bağlantısı:
    * 1 802.1Q (Dot1Q) etiketi veya iki etiketi (QinQ) 802.1Q desteklemelidir kapsülleme etiketi
    * Ethernet türü 0x8100 =
    * Microsoft tarafından - belirtilen bir VLAN kimliği temel dış VLAN etiket (STAG) eklemelisiniz *yalnızca QinQ uygulanabilir*
    * Çoklu BGP oturumları (VLAN) bağlantı noktası ve cihaz başına desteklemelidir
    * IPv4 ve IPv6 bağlantısı. *IPv6 için hiçbir ek alt arabirimi oluşturulur. IPv6 adresi için var olan alt arabirimi eklenecek*. 
    * İsteğe bağlı: [Çift yönlü iletme algılama (BFD)](https://docs.microsoft.com/azure/expressroute/expressroute-bfd) desteği, varsayılan olarak tüm özel eşlemeler ExpressRoute bağlantı hatları üzerinde yapılandırıldığı

## <a name="vlan-tagging"></a>VLAN etiketleme

ExpressRoute doğrudan QinQ hem Dot1Q VLAN etiketleme destekler.

* **QinQ VLAN etiketleme** üzerinde yalıtılmış Yönlendirme etki alanları için sağlayan bir ExpressRoute bağlantı hattı temelinde. Azure, dinamik olarak bağlantı hattı oluşturma sırasında bir S etiketi ayırır ve değiştirilemez. Her eşleme (özel ve Microsoft) devreye benzersiz bir C-Tag VLAN yararlanacaktır. C-Tag devreler ExpressRoute doğrudan bağlantı noktalarında arasında benzersiz olması gerekli değildir.

* **Dot1Q VLAN etiketleme** tek bir VLAN etiketli için sağlayan bir ExpressRoute doğrudan bağlantı noktası çifti temelinde. Tüm devreler ve ExpressRoute doğrudan bağlantı noktası çifti üzerinde eşlemeler arasında bir eşleme kullanılan C-Tag benzersiz olması gerekir.

## <a name="workflow"></a>İş akışı

[![İş akışı](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>SLA

ExpressRoute doğrudan aynı kurumsal sınıf SLA ile küresel Microsoft ağına aktif/aktif yedekli bağlantılar sağlar. ExpressRoute altyapı gereksizdir ve küresel Microsoft ağına bağlantısı yedekli ve çeşitli ve uygun şekilde ölçeklenen müşteri gereksinimlerine sahip. 

## <a name="next-steps"></a>Sonraki adımlar

[ExpressRoute doğrudan yapılandırın](expressroute-howto-erdirect.md)
