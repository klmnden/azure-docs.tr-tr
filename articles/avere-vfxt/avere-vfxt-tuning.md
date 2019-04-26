---
title: Avere vFXT küme ayarlama - Azure
description: Azure için Avere vFXT performansını iyileştirmek için özel ayarlar genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f5e780dcab20befe19ca34020908eee93c290516
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409182"
---
# <a name="cluster-tuning"></a>Küme ayarlama


Çoğu vFXT kümede özelleştirilmiş performans ayarları yararlı olabilir. Bu ayarlar, belirli bir iş akışı, veri kümesi ve araçları ile en iyi şekilde çalışması için küme yardımcı olur. 

Avere Denetim Masası'ndan kullanılabilir olmayan özellikleri yapılandırma genellikle içerdiğinden bu özelleştirme, bir destek temsilcisine yapılmalıdır.

Bu bölümde, bazı özel ayarı yapılabilir açıklanmaktadır.

<!-- 
[ xxx keep or not? \/ research this xxx ]

> [!TIP]
> The VDBench utility can be helpful in generating I/O workloads to test a vFXT cluster. Read [Measuring vFXT Performance](vdbench.md) to learn more.

-->

## <a name="general-optimizations"></a>Genel iyileştirmeler

Bu değişiklikler, veri kümesi kalitelerini veya iş akışı stili göre önerilen.

* İş yükü yazma yoğunluklu ise, varsayılan % 20'den yazma önbelleğinin boyutunu artırın. 
* Veri kümesi çok sayıda küçük dosyaları gerektiriyorsa, küme önbelleğin dosya sayısı sınırı artırın. 
* İşi kopyalama veya iki depoları arasında veri taşıma içeriyorsa, veri taşıma için kullanılan iş parçacığı sayısını ayarlayın: 
  * Hızını artırmak için kullanılan paralel iş parçacığı sayısını artırabilir.
  * Arka uç depolama birimini aşırı yüklenerek, kullanılan paralel iş parçacığı sayısını azaltmak gerekebilir.
* Küme NFSv4 ACL'leri kullanan bir çekirdek dosyalayıcı için verileri önbelleğe alır, belirli istemcileri için dosya yetkilendirme kolaylaştırmak için önbelleğe erişim modu etkinleştirin.

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>NAS bulut ya da bulut ağ geçidi en iyi duruma getirme

(Burada vFXT kümesi bir bulut kapsayıcısı NAS stili erişim sağlar) cloud NAS veya ağ geçidi senaryosunda vFXT küme ve bulut depolama arasındaki daha yüksek veri hızlarını yararlanmak için temsilcinize gibi daha fazla bilgi için bu ayarları değiştirme önerebilir agresif bir biçimde veri depolama birimine önbellekten gönderin:

* Küme ve depolama kapsayıcısını arasında TCP bağlantısı sayısını artırın
* Küme arasındaki iletişim için geri KALAN zaman aşımı değerini azaltın ve hemen başarılı olmayan yeniden denemek için depolama daha çabuk Yazar  
* Her arka uç yazma böylece segment aktarımları bir 8 MB öbek veri yerine 1 MB kesim boyutunu artırın

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>Bulut Patlaması veya karma WAN en iyi duruma getirme

Bu değişiklikler senaryonuz ya da karma depolama WAN iyileştirme senaryosu (vFXT küme bulut arasında tümleştirme sağlar ve şirket içinde donanım depolama) Patlaması bulutta yararlı olabilir:

* Küme çekirdeği dosyalayıcı arasında izin verilen TCP bağlantı sayısını artırın
* (Bu ayar şirket uzak dosyalayıcı veya Bulut çekirdek olandan farklı bir Azure bölgesinde kullanılabilir.) uzaktan çekirdek dosyalayıcı WAN iyileştirme ayarını etkinleştirin
* TCP yuva arabellek boyutunu (iş yükü ve performans gereksinimlerini) bağlı olarak artırın
* Nedenle önbelleğe alınan dosyaları (bağlı olarak iş yükü ve performans gereksinimlerini) azaltmak "İleri her zaman" ayarını etkinleştirin

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Yardım, Avere vFXT Azure'a yönelik en iyi duruma getirme

Açıklanan yordamı kullanın [sisteminizle Yardım Al](avere-vfxt-open-ticket.md) için bu iyileştirmeleri hakkında destek personeline başvurun. 