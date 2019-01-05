---
title: Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın
description: Azure dağıtımlarınızı sorunları algılamak için Inspec kullanmayı öğrenin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirin, inspce
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 9256a4daf6564761553b495e559805a46e4eae32
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050736"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın
[İnSpec](https://www.chef.io/inspec/) test etmek ve uygulamalarınızın ve altyapınızın denetim için bir ücretsiz ve açık kaynak çerçevesidir. Kolay okuma ve yazma kolay InSpec kod içinde ifade belirttiğiniz istenen duruma gerçek durumuyla sisteminizin karşılaştırarak Inspec çalışır. Inspec ihlalleri algılar ve bir rapor biçiminde bulguları gösterir, ancak düzeltme denetiminde koyar. Azure'da çalışan sanal makinelerinizin durumunu doğrulamak için Inspec kullanabilirsiniz. Inspec taramak ve kaynaklarını ve kaynak gruplarını bir abonelik içinde durumunu doğrulamak için de kullanabilirsiniz.

Bu makalede, Azure'da güvenlik ve uyumluluk kolaylaştırmak için Inspec kullanmanın avantajları anlatılmaktadır.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Uyumluluk anlamak ve değerlendirmek kolay hale getirir
Inspec ile gereksinimlerinizi tutulan, yürütülebilir, okunabilir koda dönüştürün. Bu, testlerinizi tanımlayın ve gerektiği şekilde özel durumları özelleştirdiğiniz yerdir birleştirilebilir profilleri düzenlemenize olanak sağlar.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Filo genelindeki sorunları algılayın ve bunların düzeltme öncelik
Aracısız Inspec algılar mod - ölçekte - Etkilenme düzeyinizi hızlı bir şekilde değerlendirmek için etkinleştirin. Puanlama etkisi/önem derecesi için yerleşik meta verileri, özel olarak hangi alanlarda odaklanmak için düzeltme üzerinde belirlenmesine yardımcı olur.

## <a name="inspect-machines-data-and-new-saas-apis"></a>Makineler, veriler ve yeni SaaS API'lerine inceleyin
InSpec bulut API Uyumluluk özellikleri kaba ve ayrıntılı bir onayları bulut uyumluluk ve bu raporu hakkında sürekli olarak yapmanıza olanak tanır.

## <a name="satisfy-audits"></a>Denetimleri karşılamak
Inspec ile yalnızca aralıklarla belirlenmiş üç aylık veya yıllık gibi herhangi bir zamanda - soruları denetim yanıt verebilirsiniz. İnSpec bir denetçi'nın bulgular tarafından Şaşırmış yerine tam uyumluluk duruşunu bilmek bir denetim döngüsü girmenizi sağlar.

## <a name="reduce-ambiguity-and-miscommunication-regarding-rules"></a>Belirsizlik ve kuralları ile ilgili miscommunication azaltın
Yapılandırmalar belgeleri bırakın ve yorumlamak için işlemler açın. Görüşmeler hakkında ne Temizle amacıyla somut testleri ile değiştiriliyor değerlendirilmesi yürütülebilir kod kaldırır.

## <a name="keep-up-with-rapidly-changing-threat-and-compliance-landscapes"></a>Tehdit ve uyumluluk ortamlarını hızla değişen ile takip edin
İnSpec, yazma, aynı gün algılama kodu yayımlayın ve hızlı yanıt olarak yeni düzenlemeler yeni kurallar yazma sağlar. Bu, tehditleri ve yönetmelikleri değişiklikler artık acil durumlar eşit anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)