---
title: Azure ExpressRoute doğrudan hakkında | Microsoft Docs
description: Bu sayfa ExpressRoute doğrudan (Önizleme) genel bir bakış sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: c33bec76fe17336221c873778c2993d75fec81e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962241"
---
# <a name="about-expressroute-direct-preview"></a>ExpressRoute hakkında doğrudan (Önizleme)

ExpressRoute doğrudan müşteriler Microsoft'un küresel ağı dünya genelindeki stratejik dağıtılmış eşleme konumlarda doğrudan bağlanma özelliği sağlar. ExpressRoute doğrudan uygun ölçekte etkin/etkin bağlantı destekleyen çift 100Gbps bağlantı sağlar. 

ExpressRoute doğrudan sağlayan önemli özellikler dahil ancak bunlarla sınırlı değildir:

* Depolama ve Cosmos DB gibi hizmetleri çok büyük veri alımı 
* Fiziksel yalıtım düzenlemesi ve gerekli olan sektörler için adanmış ve yalıtılmış gibi bağlantı: bankacılık, kamu ve perakende 
* İş birimini temel alan bağlantı hattı dağıtımının ayrıntılı denetim

> [!IMPORTANT]
> ExpressRoute doğrudan şu anda Önizleme aşamasındadır.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enroll-in-the-preview"></a>Önizlemeye kaydolma

ExpressRoute doğrudan kullanabilmeniz için önce önizleme aboneliğinizde kaydolmanız gerekir. Kaydetmek için, <ExpressRouteDirect@microsoft.com> adresine abonelik kimliğinizi içeren bir e-posta gönderin. ExpressRoute doğrudan Kurumsal düzeyde bir özelliktir. Ek ayrıntıları belirtin:

* Senaryo ile gerçekleştirmek için aradığınız **ExpressRoute doğrudan**
* Konum bkz - [iş ortakları ve eşleme konumları](expressroute-locations-providers.md) tüm konumlara tam listesi için
* Uygulama zaman çizelgesi
* Hizmetleri ile ilgili her türlü sorularınız

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute hizmet sağlayıcısı ve ExpressRoute doğrudan kullanma

| **ExpressRoute kullanarak bir hizmet sağlayıcısı** | **ExpressRoute doğrudan** | 
| --- | --- | 
| Hızlı ekleme ve mevcut altyapısıyla bağlantısını etkinleştirmek için hizmet sağlayıcısı'nı kullanır | 100Gbps altyapı ve tam yönetim tüm katmanların gerektirir
| Sağlayıcı Ethernet ve MPLS gibi yüzlerce ile tümleşir | Düzenlenen sektör ve büyük veri alımı için doğrudan/adanmış kapasite | 
| 50 MB/sn 10Gbps devreler SKU'ları | 1 GB/sn devreler SKU'lardan 100Gbps için
| Tek bir kiracı için en iyi duruma getirilmiş | Tek kiracılı/bulut hizmeti sağlayıcıları için en iyi duruma getirilmiş / birden çok iş birimleri

## <a name="expressroute-direct-circuits"></a>Doğrudan ExpressRoute bağlantı hatları

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  

Her eşleme konumunda, Microsoft'un genel ağ erişimi olan ve varsayılan olarak herhangi bir bölgede coğrafi bölge erişebilir ve tüm küresel bölgeye sahip premium devresi erişebilir.  

Çoğu senaryoda işlevselliğini çalışması için bir ExpressRoute hizmet sağlayıcısı'nı kullanan bağlantı hatlarına eşdeğerdir. Daha fazla ayrıntı düzeyi ve ExpressRoute doğrudan kullanarak sunulan yeni özellikleri desteklemek için doğrudan ExpressRoute bağlantı hatları üzerinde mevcut bazı temel özellikleri vardır.

## <a name="circuit-skus"></a>Bağlantı hattı SKU'ları

ExpressRoute doğrudan Azure depolama ve diğer büyük veri hizmetlerle büyük veri alma senaryolarını destekler. ExpressRoute bağlantı hattına ExpressRoute doğrudan üzerinde şimdi de destek **40G** ve **100G** bağlantı hattı SKU'ları. 

## <a name="vlan-tagging"></a>VLAN etiketleme

ExpressRoute doğrudan QinQ hem Dot1Q VLAN etiketleme destekler.

* **QinQ VLAN etiketleme** üzerinde yalıtılmış Yönlendirme etki alanları için sağlayan bir ExpressRoute bağlantı hattı temelinde. Azure, dinamik olarak bağlantı hattı oluşturma sırasında bir S etiketi ayırır ve değiştirilemez. Her eşleme (özel ve Microsoft) devreye benzersiz bir C-Tag VLAN yararlanacaktır. C-Tag devreler ExpressRoute doğrudan bağlantı noktalarında arasında benzersiz olması gerekli değildir. 

* **Dot1Q VLAN etiketleme** tek bir VLAN etiketli için sağlayan bir ExpressRoute doğrudan bağlantı noktası çifti temelinde. Tüm devreler ve ExpressRoute doğrudan bağlantı noktası çifti üzerinde eşlemeler arasında bir eşleme kullanılan C-Tag benzersiz olması gerekir.

## <a name="workflow"></a>İş akışı

![iş akışı](./media/expressroute-erdirect-about/workflow1.png)

## <a name="sla"></a>SLA

ExpressRoute doğrudan aynı kurumsal sınıf SLA ile küresel Microsoft ağına aktif/aktif yedekli bağlantılar sağlar. ExpressRoute altyapı gereksizdir ve küresel Microsoft ağına bağlantısı yedekli ve çeşitli ve uygun şekilde ölçeklenen müşteri gereksinimlerine sahip. Önizleme süresince olacaktır SLA ve yalnızca üretim dışı iş yükleri için kabul edilmesi.

## <a name="next-steps"></a>Sonraki adımlar

[ExpressRoute doğrudan yapılandırın](expressroute-howto-erdirect.md)