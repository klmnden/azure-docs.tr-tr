---
title: Azure altyapınıza uyumluluk Otomasyon için InSpec kullanın
description: Azure dağıtımlarınızda sorunlarını algılayacak şekilde InSpec kullanmayı öğrenin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirme, inspce
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4193b7fdb3932cbffa2b56b5d7eee6f3b573bd99
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Azure altyapınıza uyumluluk Otomasyon için InSpec kullanın
[İnSpec](https://www.chef.io/inspec/) test ve uygulamalarını ve altyapısını denetim için bir ücretsiz ve açık kaynaklı çerçevedir. Kolay okuma ve yazma kolay InSpec kodda express istenen durumu gerçek durumuyla sisteminizin karşılaştırarak InSpec çalışır. InSpec ihlalleri algılar ve bir rapor biçiminde bulgularını görüntüler, ancak düzeltme denetiminde koyar. Azure'da çalışan sanal makinelerinizi durumunu doğrulamak için InSpec kullanabilirsiniz. InSpec, tarama ve kaynaklar ve kaynak grupları bir abonelik içinde durumunu doğrulamak için de kullanabilirsiniz.

Bu makalede, Azure üzerinde güvenlik ve uyumluluk kolaylaştırmak için InSpec kullanmanın avantajları açıklanmıştır.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Uyumluluk anlamak ve değerlendirmek kolay hale getirir
InSpec ile gereksinimlerinizi sürümlü, yürütülebilir, okunabilir koda dönüştürür. Bu, burada tanımlayın ve özel durumları gerektiği gibi özelleştirin birleştirilebilir profillerine testlerinizi düzenlemenizi sağlar.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Donanma genelindeki sorunları algılamak ve bunların düzeltme öncelik
Aracısız InSpec algılamak modu - ölçekte - Etkilenme düzeyinizi hızlı bir şekilde değerlendirmek olanak sağlar. Etkisi/önem Puanlama için yerleşik meta verileri, hangi alanlarda odaklanmak üzerinde için düzeltme belirlenmesine yardımcı olur.

## <a name="inspect-machines-data-and-new-saas-apis"></a>Makineler, veriler ve yeni SaaS API'leri inceleyin.
InSpec bulut API Uyumluluk özelliklerini kaba ve hassas onaylar bulut uyumluluk ve Bu raporda hakkında sürekli yapmanıza olanak tanır.

## <a name="satisfy-audits"></a>Denetimleri karşılamak
InSpec ile yalnızca aralıklarla önceden belirlenmiş üç aylık veya yıllık gibi herhangi bir zamanda - soruları denetim yanıt verebilir. İnSpec bir denetçi'nın bulgular tarafından Şaşkın yerine tam uyumluluk duruş bilerek bir denetim döngüsü girmenize olanak sağlar.

## <a name="reduce-ambiguity-and-miscommunication-regarding-rules"></a>Belirsizlik ve kuralları ile ilgili miscommunication azaltın
Yapılandırmalar belgeleri bırakın ve işlemler için yorumlama açın. Yürütülebilir kod ne Temizle amacıyla somut testleri lehinde uygunluk hakkında görüşmeleri kaldırır.

## <a name="keep-up-with-rapidly-changing-threat-and-compliance-landscapes"></a>Tehdit ve uyumluluk Windows'un hızla değişen ile takip edin
İnSpec, yazma ve aynı gün algılama kodu yayımlama ve hızlı yanıt olarak yeni düzenlemeleri yeni kurallar yazma olanak sağlar. Bu tehditler veya mevzuatı değişiklikler artık acil durumlar eşit anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)