---
title: Fatura ve günlük kullanım verileri faturalama Azure karşıdan | Microsoft Docs
description: İndirme veya, Azure fatura faturayı ve günlük kullanım verilerini görüntülemek açıklar.
keywords: Fatura Fatura, fatura indirme, azure fatura, azure kullanımı
services: billing
documentationcenter: ''
author: genlin
manager: tonguyen
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 59ee992518b5e529f486415f962c6f26881875f1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607421"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>İndirme veya Azure fatura faturayı ve günlük kullanım verilerini görüntüleme
Faturanız dan indirebilirsiniz [Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) veya e-posta ile gönderilen sahip. Günlük kullanımı indirmek için Git [Azure hesap Merkezi](https://account.azure.com/Subscriptions). Yalnızca belirli rolleri fatura fatura ve kullanım bilgilerini, Hesap Yöneticisi gibi alma iznine sahip. Fatura bilgilere erişim sağlama hakkında daha fazla bilgi için bkz: [rollerini kullanarak faturalama Azure erişimi yönetme](billing-manage-access.md).

Bu makalede, Kurumsal Anlaşma (Kurumsal Sözleşme) müşteriler için geçerli değildir. EA müşteri değilseniz, faturalarınızı doğrudan kayıt Yöneticiler gönderilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="get-your-invoice-in-email-pdf"></a>E-postayla (.pdf) faturanızı Al
Kabul ve Azure almak için ek alıcılar yapılandırma fatura bir e-posta. Bu özellik için destek sunar, Kurumsal Anlaşma ya da Open ile Azure gibi belirli abonelikleri kullanılamayabilir.

1. Aboneliğinizden seçin [abonelikler sayfası](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip olduğunuz her abonelik için katılımı. Tıklatın **faturalar** sonra **my fatura e-posta**. 

    ![Katılımı akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Tıklatın **kabul** ve koşulları kabul edin.

    ![Katılımı akış adım 2 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Anlaşmayı kabul ettiğiniz sonra ek alıcılar yapılandırabilirsiniz. Bir alıcı kaldırıldığında, e-posta adresi artık depolanır. Fikrinizi değiştirirseniz, yeniden eklemeniz gerekir.

    ![Katılımı akış adım 3 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Bir e-posta adımları izledikten sonra alamazsanız e-posta adresinizi doğru olduğundan emin olun [iletişim tercihleri profilinizde](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>E-postayla faturanızı alma çıkma
Faturanız e-posta ile almak istemiyorsanız, e-postayla gönderilmiş faturalar çıkışı Opt'ı tıklatın. Bu e-postayla faturaları alacak şekilde ayarlanmış herhangi bir e-posta adresi kaldırır. İptal etmeyi seçerseniz, arkada alıcılar yeniden yapılandırmanız gerekir.

 ![Vazgeçme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

## <a name="download-invoice-from-azure-portal-pdf"></a>Azure Portalı'ndan (.pdf) fatura indirin

1. Aboneliğinizden seçin [abonelikler sayfası](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) olarak Azure Portalı'nda [faturalar erişimi olan bir kullanıcı](billing-manage-access.md).

2. Seçin **faturalar**. 

    ![Faturalama ve kullanım seçeneği gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Tıklatın **karşıdan fatura** PDF faturanızı kopyasını görüntülemek için. Bunu diyorsa **kullanılamaz**, bkz: [yok görmemin nedeni fatura son fatura dönemine ait?](#noinvoice)

    ![Fatura her dönem için fatura süreleri, yükleme seçeneği ve toplam ücretleri gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Günlük kullanımı fatura döneminde tıklayarak da görüntüleyebilirsiniz. 

Faturanız hakkında daha fazla bilgi için bkz: [faturanızı anlamak için Microsoft Azure](billing-understand-your-bill.md). Maliyetleri yönetme hakkında Yardım için bkz: [Azure faturalama ve maliyet yönetimi ile beklenmeyen maliyetleri önlemek](billing-getting-started.md).

## <a name="download-usage-from-the-account-center-csv"></a>Kullanım (.csv) hesap Merkezi'nden indirin

1. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.

2. Fatura ve kullanım bilgilerini istediğiniz aboneliği seçin.

3. Seçin **FATURALAMA GEÇMİŞİ**. 

    ![Fatura geçmişi seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Son altı fatura dönemi ve geçerli faturalanmamış dönemin, deyimleri görebilirsiniz. 

    ![Fatura dönemleri, fatura ve günlük kullanımı ve toplam ücretleri her fatura dönemi için indirme seçeneklerini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Seçin **geçerli bildirimini görüntüle** tahmin oluşturulma zamanında ücretlerinizi tahmini görmek için. Bu bilgiler yalnızca her gün güncelleştirilir ve tüm kullanımınızı içermeyebilir. Aylık faturanızı bu tahmin farklı olabilir.

    ![Geçerli bildirimini görüntüle seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Geçerli ücretler tahmin gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Seçin **kullanımı indir** günlük kullanım verileri bir CSV dosyası olarak karşıdan yüklemek için. Kullanılabilir iki sürümlerini görürseniz, sürüm 2 indirin.

    ![Kullanımı indir seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Yalnızca Hesap Yöneticisi Azure hesap merkezi erişebilir. Bir sahip gibi diğer faturalama yöneticileri kullanım bilgileri kullanarak elde edebilirsiniz [fatura API'leri](billing-usage-rate-card-overview.md).

Günlük kullanımı hakkında daha fazla bilgi için bkz: [faturanızı anlamak için Microsoft Azure](billing-understand-your-bill.md). Maliyetleri yönetme hakkında Yardım için bkz: [Azure faturalama ve maliyet yönetimi ile beklenmeyen maliyetleri önlemek](billing-getting-started.md).

## <a name="noinvoice"></a> Son fatura dönemine ait bir fatura neden göremiyorum?

Fatura görmüyorum çeşitli nedenleri olabilir:

- Aboneliğinizle aşan kaydetmedi aylık bir kredi tutarına sahip veya ücretsiz deneme sürümü vardır. Para borçlu faturaya yalnızca oluşturulur.

- 30 günden kısa bir süre için Azure abone günden değil.

- Fatura henüz oluşturulan değil. Fatura döneminin sonuna kadar bekleyin.

- Hesap Yöneticisi değilseniz, eski faturalar kullanabileceğiniz olmayabilir.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.

