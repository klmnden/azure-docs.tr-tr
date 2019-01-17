---
title: Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın
description: Azure dağıtımlarınızı sorunları algılamak için Inspec kullanmayı öğrenin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirme, inspec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: a5de2ca04a193f97a2a65a043f744abb8e0ea758
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359230"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Otomasyon için Uyumluluk, Azure altyapısının Inspec kullanın
[İnSpec](https://www.chef.io/inspec/) yazılım mühendisleri, işlemler ve güvenlik mühendisi arasında paylaşılabilir güvenlik ve uyumluluk kurallarını tanımlamak için Chef'ın açık kaynak bir dildir. Kolay okuma ve yazma kolay InSpec kod içinde ifade belirttiğiniz istenen duruma gerçek durumuyla altyapınızın karşılaştırarak Inspec çalışır. Inspec ihlalleri algılar ve bir rapor biçiminde bulguları gösterir, ancak düzeltme denetiminde koyar.

Inspec kaynaklarını ve kaynak gruplarını içinde sanal makineler, ağ yapılandırmaları, Azure Active Directory ayarları ve daha fazlası dahil olmak üzere, bir abonelik durumunu doğrulamak için kullanabilirsiniz.

Bu makalede, Azure'da güvenlik ve uyumluluk kolaylaştırmak için Inspec kullanmanın avantajları anlatılmaktadır.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Uyumluluk anlamak ve değerlendirmek kolay hale getirir
Elektronik tablolar veya Word belgelerini yazılmış uyumluluk belgeleri bırakın gereksinimleri açık yorumlamak için. Inspec ile gereksinimlerinizi tutulan, yürütülebilir, okunabilir koda dönüştürün. Kod ne Temizle amacıyla somut testleri ile değiştiriliyor değerlendirilmesi ilgili konuşmaları değiştirir.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Filo genelindeki sorunları algılayın ve bunların düzeltme öncelik
Inspec aracısız algılama modu - ölçekte - Etkilenme düzeyinizi hızlı bir şekilde değerlendirmek için etkinleştirin. Puanlama etkisi/önem derecesi için yerleşik meta verileri, özel olarak hangi alanlarda odaklanmak için düzeltme üzerinde belirlenmesine yardımcı olur. Ayrıca, kuralları hızla yanıt olarak yeni güvenlik açıkları ve yönetmelikleri yazın ve hemen kullanıma.

## <a name="satisfy-audits"></a>Denetimleri karşılamak
Inspec ile yalnızca aralıklarla belirlenmiş üç aylık veya yıllık gibi herhangi bir zamanda - soruları denetim yanıt verebilirsiniz. Sürekli olarak InSpec testleri çalıştırarak geçmişi ve tam uyumluluk duruşunu bilmek bir denetim döngüsü girmek yerine bir denetçi'nın bulgular tarafından Şaşırmış.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cloud Shell'de InSpec deneyin [ ![Cloud Shell'i Başlat](https://shell.azure.com/images/launchcloudshell.png "Cloud Shell'i Başlat")](https://shell.azure.com)
