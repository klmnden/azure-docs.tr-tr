---
title: "Azure'da yüksek kullanılabilirlik bağlantı noktalarına genel bakış | Microsoft Docs"
description: "Yüksek oranda kullanılabilir bağlantı noktaları yük bir iç yük dengeleyici Dengeleme hakkında bilgi edinin."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: 46e284d1636988390f3533d93bfd07399f45dc92
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktalarına genel bakış

Azure yük dengeleyici standart bir iç yük dengeleyici kullanırken tüm bağlantı noktalarındaki Bakiye TCP ve UDP akışları aynı anda yükleme yardımcı olur. 

>[!NOTE]
> Yüksek kullanılabilirlik (HA) bağlantı noktalarını özellik yük dengeleyici standardı ile kullanılabilir ve şu anda önizlemede değil. Önizleme sırasında Özellik kullanılabilirliği ve güvenilirliği genel kullanılabilirlik sürümde özellikleri olarak aynı düzeyde sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Yük Dengeleyici standart kaynaklarla HA bağlantı noktalarını kullanacak biçimde yük dengeleyici Standard Önizleme için kaydolun. Yönergeleri izleyin yük dengeleyici için kaydolma [Standard Önizleme](https://aka.ms/lbpreview#preview-sign-up) de.

Bir HA bağlantı noktası kuralı bir bir Yük Dengeleme kuralı, bir iç yük dengeleyici standart yapılandırılmış çeşididir. Bir iç yük dengeleyici Standard tüm bağlantı noktalarında gelen tüm TCP ve UDP akışları yükünü dengelemek için tek bir kural sağlayarak, yük dengeleyici kullanımınız basitleştirebilirsiniz. Yük Dengeleme karar akış yapılır. Bu aşağıdaki beş bölütlü bağlantısında dayanır: kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokol.

HA bağlantı noktalarını özelliği ile yüksek kullanılabilirlik ve ağ sanal Gereçleri (NVA) içinde sanal ağlar için ölçek gibi kritik senaryolarda yardımcı olur. Çok sayıda bağlantı noktaları yük dengeli olması gerektiğinde de yardımcı olabilir. 

Ön uç ve arka uç bağlantı noktaları kümesine HA bağlantı noktalarını özelliği yapılandırılır **0**ve protokol **tüm**. İç yük dengeleyici kaynak bağlantı noktası numarası bağımsız olarak tüm TCP ve UDP akışlar sonra dengeler.

## <a name="why-use-ha-ports"></a>HA bağlantı noktalarını neden kullanılır?

### <a name="nva"></a>Ağ sanal Gereçleri

NVAs Azure İş yükünüzün birden çok tür güvenlik tehditlerine karşı güvenliğini sağlamak için kullanabilirsiniz. Bu senaryolarda NVAs kullanıldığında, güvenilir ve yüksek oranda kullanılabilir olması gerekir ve bunlar için isteğe bağlı ölçeğini gerekir.

Yalnızca NVA örnekleri Azure iç yük dengeleyici arka uç havuzuna ekleme ve bir HA bağlantı noktalarını yük dengeleyici kuralı yapılandırarak bu hedefleri elde edebilirsiniz.

HA bağlantı noktaları gibi çeşitli avantajları NVA HA senaryoları için sağlar:
- Örnek sistem durumu araştırmalarının ile sağlıklı örneklerine hızlı yük devretme
- Genişleme için daha yüksek performansla  *n* -etkin örnekleri
- *N*-etkin ve Etkin-pasif senaryoları
- Uygulamaları izlemek için Apache ZooKeeper düğümleri gibi karmaşık çözümleri gereksinimini

Aşağıdaki diyagramda bir hub ve bağlı sanal ağ dağıtımı gösterir. Bağlı bileşen zorlamalı tünel hub sanal ağ ve güvenilir alanı çıkmadan önce NVA aracılığıyla kendi trafiği. Bir iç yük dengeleyici standart bir HA bağlantı noktası yapılandırması ile arkasında NVAs var. Tüm trafiği işlenen ve buna uygun olarak iletilir.

![NVAs HA modunda dağıtılmış olan hub ve bağlı sanal ağ diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> NVAs kullanıyorsanız, ilgili sağlayıcı ile en iyi HA bağlantı noktalarını kullanacak biçimde nasıl ve hangi senaryolar desteklenir onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Yük Dengeleme çok sayıda bağlantı noktaları

HA bağlantı noktalarını bağlantı noktalarının çok sayıda Yük Dengeleme gerektiren uygulamalar için de kullanabilirsiniz. Bir iç kullanarak bu senaryoları basitleştirebilirsiniz [yük dengeleyici standart](https://aka.ms/lbpreview) HA bağlantı noktasına sahip. Tek bir Yük Dengeleme kuralı birden çok ayrı Yük Dengeleme kuralları, her bağlantı noktası için bir tane değiştirir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktalarını özelliği kullanılabilir [aynı bölgeleri yük dengeleyici standart olarak](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Önizleme kaydolma

Yük Dengeleyici standart HA bağlantı noktalarını özelliği önizlemede katılmak için yük dengeleyici aboneliğinizin kaydetmek [Standard Önizleme](https://aka.ms/lbpreview#preview-sign-up). Azure CLI 2.0 veya PowerShell kullanarak kaydedebilirsiniz.

>[!NOTE]
>Kayıt bir saate kadar sürebilir.

## <a name="limitations"></a>Sınırlamalar

HA bağlantı noktalarını özellik için özel durumlar ve desteklenen yapılandırmalar şunlardır:

- Bir tek doğrudan sunucu yük dengeleyici kuralı HA bağlantı noktasıyla dönüşü (DSR - Azure kayan IP) tek bir ön uç IP yapılandırmasına sahip olabilir veya tek DSR dışı yük dengeleyici kuralı HA bağlantı noktasına sahip olabilir. Her ikisi de olamaz.
- Tek bir ağ arabirimi IP yapılandırması yalnızca bir olmayan-yük dengeleyici kuralı HA bağlantı noktasıyla DSR olabilir. Herhangi bir kuralın bu ipconfig için yapılandıramazsınız.
- Tüm ilgili ön uç IP yapılandırmalarını benzersiz olması koşuluyla yük dengeleyici kuralları HA bağlantı noktaları, bir veya daha fazla DSR bir tek bir ağ arabirimi IP yapılandırmasına sahip olabilir.
- Tüm Yük Dengeleme kuralları HA bağlantı noktalarını (yalnızca DSR) kullanıyorsanız, iki (veya daha fazla) yük dengeleyici kuralları aynı arka uç havuzuna işaret eden birlikte bulunabilir. Aynı tüm kurallar olmayan olduğunda true olur-HA (DSR ve DSR olmayan) bağlantı noktaları. Ancak, HA bağlantı noktaları ve HA olmayan bağlantı noktası kuralları bileşimini ise, iki tür Yük Dengeleme kuralları birlikte bulunamaz.
- HA bağlantı noktalarını özellik IPv6 için kullanılamıyor.
- Akış simetrisi NVA senaryoları için yalnızca tek bir NIC ile desteklenir. Açıklamaya bakın ve için diyagram [sanal gereçler ağ](#nva). 



## <a name="next-steps"></a>Sonraki adımlar

- [HA bağlantı noktalarını bir iç yük dengeleyici standart yapılandırma](load-balancer-configure-ha-ports.md)
- [Yük Dengeleyici Standard önizleme hakkında bilgi edinin](https://aka.ms/lbpreview)

