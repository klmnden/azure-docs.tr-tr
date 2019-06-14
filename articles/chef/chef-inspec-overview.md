---
title: Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın
description: Azure dağıtımlarınızı sorunları algılamak için Inspec kullanmayı öğrenin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirme, inspec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 03/19/2019
ms.topic: article
ms.openlocfilehash: bdfa30b48c79a8910d503bb9e54a42c30e5adba6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60629807"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın

[İnSpec](https://www.chef.io/inspec/) yazılım mühendisleri, işlemler ve güvenlik mühendisi arasında paylaşılabilir güvenlik ve uyumluluk kurallarını tanımlamak için Chef'ın açık kaynak bir dildir. Kolay okuma ve yazma kolay InSpec kod içinde ifade belirttiğiniz istenen duruma gerçek durumuyla altyapınızın karşılaştırarak Inspec çalışır. Inspec ihlalleri algılar ve bir rapor biçiminde bulguları gösterir, ancak düzeltme denetiminde koyar.

Inspec, kaynakların ve sanal makineler, ağ yapılandırmaları, Azure Active Directory ayarları ve daha fazlası dahil olmak üzere, bir Abonelikteki kaynak gruplarının durumunu doğrulamak için kullanabilirsiniz.

Bu makalede, Azure'da güvenlik ve uyumluluk kolaylaştırmak için Inspec kullanmanın avantajları anlatılmaktadır.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Uyumluluk anlamak ve değerlendirmek kolay hale getirir

Elektronik tablolar veya Word belgelerini yazılmış uyumluluk belgeleri gereksinimleri yorumlamak için açık kalır. Inspec ile gereksinimlerinizi tutulan, yürütülebilir, okunabilir koda dönüştürün. Kod ne Temizle amacıyla somut testleri ile değiştiriliyor değerlendirilmesi ilgili konuşmaları değiştirir.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Filo genelindeki sorunları algılayın ve bunların düzeltme öncelik

Inspec aracısız algılama modu - ölçekte - Etkilenme düzeyinizi hızlı bir şekilde değerlendirmek için etkinleştirin. Puanlama etkisi/önem derecesi için yerleşik meta verileri, özel olarak hangi alanlarda odaklanmak için düzeltme üzerinde belirlenmesine yardımcı olur. Ayrıca, kuralları hızla yanıt olarak yeni güvenlik açıkları ve yönetmelikleri yazın ve hemen kullanıma.

## <a name="audit-azure-virtual-machines-with-policy-guest-configuration"></a>Azure sanal makineler Konuk yapılandırma ilke ile denetleme

Azure, doğrudan aracılığıyla Azure sanal makineleri denetlemek için Chef InSpec tanımları kullanımını destekleyen [Azure İlkesi Konuk Yapılandırması](/azure/governance/policy/concepts/guest-configuration). Konuk yapılandırma Linux sanal makinesi belirtilen Chef InSpec tanım ve Azure İlkesi aracılığıyla geri rapor uyumluluk için değerlendirilir. Bu denetimleri sonuçlarını da Azure İzleyici günlüklerine bildirilir; Uyarılar ve diğer Otomasyon senaryoları etkinleştiriliyor.

## <a name="satisfy-audits"></a>Denetimleri karşılamak

Inspec ile yalnızca aralıklarla belirlenmiş üç aylık veya yıllık gibi herhangi bir zamanda - soruları denetim yanıt verebilirsiniz. Sürekli olarak InSpec testleri çalıştırarak geçmişi ve tam uyumluluk duruşunu bilmek bir denetim döngüsü girmek yerine bir denetçi'nın bulgular tarafından Şaşırmış.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure Cloud Shell'de InSpec deneyin](https://shell.azure.com)