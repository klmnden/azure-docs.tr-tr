---
title: Azure faturanızı ve günlük kullanım verilerinizi indirme | Microsoft Docs
description: İndirme veya Azure fatura faturanızı ve günlük kullanım verilerinizi görüntülemek açıklar.
keywords: Fatura, fatura indirme, azure fatura, azure kullanımı
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
ms.author: cwatson
ms.openlocfilehash: f0cdfef50c07674a08766933f2f7edfc946462a4
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395024"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>İndirme veya Azure fatura ve günlük kullanım verilerini görüntüleme
Faturanızı dan indirebileceğiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) veya e-posta ile gönderilen sahip. Günlük kullanımınızla indirmek için Git [Azure hesap Merkezi](https://account.azure.com/Subscriptions). Yalnızca bazı rollerde fatura ve kullanım bilgilerini, Hesap Yöneticisi gibi alma iznine sahip. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

Bu makalede, Kurumsal Anlaşma (EA) müşterileri için geçerli değildir. Bir EA müşterisiyseniz faturalarınızda doğrudan kayıt yöneticileri için gönderilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

> [!div class="nextstepaction"]
> [Azure faturalama belgelerinin iyileştirilmesine yardımcı olun](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="get-your-invoice-in-email-pdf"></a>Faturanızı (.pdf) e-posta ile alın
Kabul et ve Azure'ı almak için ek alıcılar yapılandırma faturayı e. Bu özellik Destek teklifleri, Kurumsal Anlaşma ya da Open ile Azure gibi bazı abonelikler için kullanılamıyor olabilir.

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Kabul etme olduğunuz her bir abonelik için. Tıklayın **faturalar** ardından **my faturayı e-posta**. 

    ![Katılım akışı gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Tıklayın **katılım** ve koşullarını kabul edin.

    ![Katılım akışı adım 2 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Anlaşmayı kabul ettiğiniz sonra ek alıcılar yapılandırabilirsiniz. E-posta adresi, artık bir alıcı kaldırıldığında depolanır. Fikrinizi değiştirirseniz, bunları yeniden eklemeniz gerekir.

    ![Katılım akışı adım 3 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Adımları tamamladıktan sonra bir e-posta almazsanız, e-posta adresinizi doğru olduğundan emin olun [iletişim tercihleri profilinize](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>E-postada faturanızı bulaşmasından çıkma
Faturanızı e-posta almak istemiyorsanız, e-postayla fatura kullanıma Opt'e tıklayın. Bu, faturalar içinde e-posta almak için herhangi bir e-posta adresi kaldırır. Katılırsanız, arka planda, alıcıların yeniden yapılandırmanız gerekir.

 ![Vazgeçme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

## <a name="download-invoice-from-azure-portal-pdf"></a>Azure portalından (.pdf) karşıdan yükleme faturası

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında [faturalar erişimi olan bir kullanıcı](billing-manage-access.md).

2. Seçin **faturalar**. 

    ![Faturalandırma ve kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Tıklayın **faturayı indir** PDF faturanızı bir kopyasını görüntülemek için. Bu derse **kullanılamıyor**, bkz: [neden olmayan görüyorum fatura son fatura dönemi için?](#noinvoice)

    ![Her fatura dönemi için faturalandırma dönemleri, indirme seçeneğini ve Toplam ücret gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Fatura dönemi tıklayarak günlük kullanımınızla da görüntüleyebilirsiniz. 

Faturanızı hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md). Maliyetleri yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

## <a name="download-usage-from-the-account-center-csv"></a>Hesap Merkezi'nden (.csv) kullanımı indir

1. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/subscriptions) hesap yöneticisi olarak.

2. Faturayı ve kullanım bilgileri istediğiniz aboneliği seçin.

3. Seçin **fatura GEÇMİŞİ**. 

    ![Fatura geçmişi seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Son altı fatura dönemine ve geçerli faturalanmamış dönem için deyimleri görebilirsiniz. 

    ![Faturalandırma dönemleri, fatura ve günlük kullanım ve toplam ücretleri her fatura dönemi için indirme seçeneklerini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Tahminin oluşturulduğu zaman itibarıyla yapılacak bir ücret tahminini görmek için **Geçerli Bildirimi Görüntüle**'yi seçin. Bu bilgiler yalnızca günlük olarak güncellenir ve tüm kullanımları içermeyebilir. Aylık faturanız bu tahminden farklı olabilir.

    ![Geçerli bildirimi görüntüle seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Geçerli ücretler tahmini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Günlük kullanım verilerini CSV dosyası olarak indirmek için **Kullanımı İndir**'i seçin. İki sürüm varsa 2. sürümü indirin.

    ![Kullanımı indir seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Yalnızca Hesap Yöneticisi, Azure hesap Merkezi'ne erişebilirsiniz. Kullanım bilgileri kullanarak bir sahibi gibi diğer faturalama yöneticileri alabilirsiniz [faturalandırma API'lerini](billing-usage-rate-card-overview.md).

Günlük kullanımınız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md). Maliyetleri yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

## <a name="noinvoice"></a> Son fatura dönemi için fatura neden göremiyorum?

Fatura görmüyor çeşitli nedenleri olabilir:

- Bir aylık kredi miktarını aşan alamadık, aboneliğiniz ile sahip olduğunuz veya ücretsiz deneme sürümü vardır. Fatura para borçlu yalnızca üretilir.

- Azure'a abone günden itibaren 30 günden az olduğu.

- Fatura henüz oluşturulmadığından. Fatura döneminin sonuna kadar bekleyin.

- Hesap Yöneticisi değilseniz, eski faturalar için kullanılamıyor olabilir.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

