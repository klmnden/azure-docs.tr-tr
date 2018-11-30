---
title: Kuruluş için Azure faturanızı anlama | Microsoft Docs
description: Okuma ve bir Kurumsal Anlaşma Azure müşterileriyle kullanımınızı ve faturanızı anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: adpick
manager: dougeby
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: cwatson
ms.openlocfilehash: b724fc7a887550b4115a988149b4b7a6c95de830
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584477"
---
# <a name="understand-your-bill-for-azure-customers-with-an-enterprise-agreement"></a>Bir kurumsal anlaşma kapsamında olan Azure müşterileri için faturanızı anlayın

Kurumsal Sözleşme, azure müşterilerine kuruluşun kredi aşan veya kredi tarafından kapsamında olmayan hizmetlerini bir fatura alırsınız. 

Kuruluşunuzun kredi parasal taahhüdünüz içerir. Parasal taahhüt, kuruluşunuzun Azure Hizmetleri kullanımı için önceden ödenen miktarıdır. Microsoft hesabı yöneticinizle veya satıcınızla başvurarak Kurumsal sözleşmenize parasal taahhüt fonlarını ekleyebilirsiniz.  

## <a name="when-credit-exceeded-or-doesnt-apply"></a>Ne zaman iade aşıldı veya geçerli değildir

Aşağıdaki durumlarda bir veya daha fazla fatura alın:

- **Hizmet fazla kullanımı**: kuruluşunuzun kullanım ücretleri kredi bakiyeniz aşıyor.
- **Ücretleri ayrı olarak faturalandırılır**: kullanılan kuruluşunuz tarafından kredi kapsamında olmayan hizmetleri. Aşağıdaki hizmetleri için kredi bakiyeniz bağımsız olarak faturalanan:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop 
    - Kayıtlı kullanıcı
    - Openlogic
    - Uzaktan erişim hakları XenApp Essentials kayıtlı kullanıcı
    - Ubuntu Advantage
    - Visual Studio Enterprise (aylık)
    - Visual Studio Enterprise (Yıllık)
    - Visual Studio Professional (aylık)
    - Visual Studio Professional (Yıllık)
- **Market ücretlerini**: Azure Marketi satın alma ve kullanım kuruluşunuzun kredi tarafından kapsanmaz ve ayrı olarak faturalandırılır. Kuruluş Yöneticisi enable ve disable Market alışverişleri kuruluşlarında Enterprise Portal'da özelliğine sahiptir. 

Son ücretleri olduğunda, hizmet fazla kullanım ve fatura dönemi boyunca ayrı olarak faturalandırılan ücretler için ücretleri her iki tür içeren bir fatura alırsınız. Her zaman, Pazar ücretleri ayrı olarak faturalandırılır.

## <a name="review-charges"></a>Gözden geçirme ücretleri

Gözden geçirin ve ücret, faturada doğrulamak için bir kuruluş yöneticisi olması gerekir. Daha fazla bilgi için [azure'da yönetim rolleri Azure Kurumsal anlaşmasına anlamak](billing-understand-ea-roles.md). Kuruluşunuz için kuruluş yöneticisi olan bilmiyorsanız [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="review-service-overage-invoice"></a>Fazla kullanım hizmeti fatura, gözden geçirme

Enterprise portal, toplam kullanım miktarınızdan karşılaştırma **raporları** > **Kullanım Özeti** hizmet fazla kullanım faturanızı ile. Servis fazla kullanım faturasına kuruluşunuzun kredi aşan kullanım ve/veya iade tarafından kapsamında olmayan hizmetlerini içerir. Tutarları **Kullanım Özeti** vergiler dahil değildir. 

1. Oturum [kurumsal portal](https://ea.azure.com).
1. Seçin **raporları**.
1. Sağ üst köşesindeki sekmesi üzerinde görünümünden geçiş **M** için **C** ve fatura döneminde eşleştirin.
 
   ![M gösteren ekran görüntüsü + C seçeneği kullanım özeti.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. **Toplam kullanım** tutarı eşleşmelidir **bugünkü Genişletilmiş** , servis fazla kullanım faturasına. Hüküm ve fatura ve üzerinde gösterilen açıklamaları aşağıdaki tabloda listelenmektedir **Kullanım Özeti** Enterprise Portal'da:

   |Fatura dönemi|Kullanım Özeti terimi|Açıklama|
   |---|---|---|
   |Toplam miktarı genişletilmiş|Toplam Kullanım|Kredi uygulanmadan önce belirli bir dönem için toplam öncesi vergi kullanım ücreti.|
   |Taahhüt kullanımı|Taahhüt kullanımı|Belirli bir dönemde uygulanan alacak.|
   |Toplam satış|Toplam fazla kullanım|Kredi miktarını aşan toplam kullanım ücreti. Bu miktar, vergi dahil değildir.|
   |Vergi tutarı|Uygulanamaz|Belirli bir süre için toplam satış tutarı uygulanan vergiler.|
   |Toplam tutar|Uygulanamaz|Miktarı kredi uygulanır ve vergi eklendikten sonra Fatura için son.|
1. Ücretlerinizle ilgili daha fazla bilgi için Git **kullanımı indir** > **Gelişmiş rapor indirme**. Bu rapor, vergileri veya ayırmaları ücretleri veya Market ücretlerini içermez.

      ![Gelişmiş rapor gösteren ekran görüntüsü indirme kullanımı sekmesinde indirin.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-advanced.png)

### <a name="review-marketplace-invoice"></a>Market faturasında gözden geçirme

Toplam açık, Azure Market'ten karşılaştırma **raporları** > **Kullanım Özeti** Enterprise Portal'da Market faturanızı ile. Market faturasında, yalnızca Azure Marketi satın alma ve kullanım içindir. Tutarları **Kullanım Özeti** vergiler dahil değildir. 

1. Oturum [kurumsal portal](https://ea.azure.com).
1. Seçin **raporları**.
1. Sağ üst köşesindeki sekmesi üzerinde görünümünden geçiş **M** için **C** ve fatura döneminde eşleştirin.

     ![M gösteren ekran görüntüsü + C seçeneği kullanım özeti.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. **Azure Marketi** toplam eşleşmelidir **toplam satış** Market faturanızla ilgili.
1. Kullanım tabanlı ücretlerinizle ilgili daha fazla bilgi için Git **kullanımı indir**. Altında **Market ücretlerini**seçin **indirme**. Bu rapor, vergiler dahil değil veya tek seferlik satın alımları göster.

     ![Market ücretlerini seçeneğinde indirin gösteren ekran görüntüsü.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-marketplace.png)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
