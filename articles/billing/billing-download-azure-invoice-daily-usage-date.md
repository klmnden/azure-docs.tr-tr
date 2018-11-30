---
title: Azure faturanızı ve günlük kullanım verilerinizi indirme | Microsoft Docs
description: İndirme veya Azure fatura faturanızı ve günlük kullanım verilerinizi görüntülemek açıklar.
keywords: Fatura, fatura indirme, azure fatura, azure kullanımı
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2018
ms.author: cwatson
ms.openlocfilehash: 80721fc82a54c62c982298cb8eabb999caaf1dfb
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583117"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>İndirme veya Azure fatura ve günlük kullanım verilerini görüntüleme

Çoğu abonelikler için fatura indirebilirsiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) veya e-posta ile gönderilen sahip. Bir Kurumsal Anlaşma (EA müşterisinin) ile bir Azure müşterisi iseniz, kuruluşunuzun fatura karşıdan yükleyemiyor. Kayıt için fatura almaya ayarlanmış yapabilmek için fatura gönderilir.

Kullanım bir EA müşterisinin olarak karşıdan yüklemek isterseniz, kullanılabilir, [Azure portalında](https://portal.azure.com/) > **maliyet Yönetimi + faturalandırma** > **kullanım ve Ücret**. Diğer abonelikler için Git [Azure hesap Merkezi](https://account.azure.com/Subscriptions).

Yalnızca bazı rollerde fatura hesap yöneticisi veya kuruluş yöneticisi gibi faturayı ve kullanım bilgilerini alma iznine sahip. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-or-view-your-invoice"></a>İndirme veya faturanızı görüntüleyin

 Bir EA müşterisiyseniz, kuruluşunuzun fatura karşıdan yükleyemiyor. Kayıt için fatura almaya ayarlanmış yapabilmek için fatura gönderilir. Diğer abonelikler için faturanızı e-posta ile alın veya Azure portalından indirin.

### <a name="get-your-invoice-in-email-pdf"></a>Faturanızı (.pdf) e-posta ile alın
Kabul et ve Azure'ı almak için ek alıcılar yapılandırma faturayı e. Bu özellik Destek teklifleri, Kurumsal Anlaşma ya da Open ile Azure gibi bazı abonelikler için kullanılamıyor olabilir.

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip olduğunuz her abonelik için kabul et. Tıklayın **faturalar** ardından **my faturayı e-posta**. 

    ![Katılım akışı gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Tıklayın **katılım** ve koşullarını kabul edin.

    ![Katılım akışı adım 2 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Anlaşmayı kabul ettiğiniz sonra ek alıcılar yapılandırabilirsiniz. E-posta adresi, artık bir alıcı kaldırıldığında depolanır. Fikrinizi değiştirirseniz, bunları yeniden eklemeniz gerekir.

    ![Katılım akışı adım 3 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Adımları tamamladıktan sonra bir e-posta almazsanız, e-posta adresinizi doğru olduğundan emin olun [iletişim tercihleri profilinize](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>E-postada faturanızı bulaşmasından çıkma
Faturanızı e-posta almak istemiyorsanız tıklayın **iyileştirilmiş e-postayla faturaların**. Bu seçenek, faturalar içinde e-posta almak için herhangi bir e-posta adresi kaldırır. Geri kullanmayı seçerseniz, alıcıların yeniden yapılandırmanız gerekir.

 ![Vazgeçme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="download-invoice-from-azure-portal-pdf"></a>Azure portalından (.pdf) karşıdan yükleme faturası

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında [faturalar erişimi olan bir kullanıcı](billing-manage-access.md).

2. Seçin **faturalar**. 

    ![Faturalandırma ve kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Tıklayın **faturayı indir** PDF faturanızı bir kopyasını görüntülemek için. Bu derse **kullanılamıyor**, bkz: [neden olmayan görüyorum fatura son fatura dönemi için?](#noinvoice)

    ![Her fatura dönemi için faturalandırma dönemleri, indirme seçeneğini ve Toplam ücret gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Fatura dönemi tıklayarak günlük kullanımınızla da görüntüleyebilirsiniz. 

Faturanızı hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md). Maliyetleri yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

### <a name="noinvoice"></a> Son fatura dönemi için fatura neden göremiyorum?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Aboneliğinizde henüz aşmadığınız aylık kredi miktarı bulunabilir veya Ücretsiz Deneme aboneliğiniz olabilir. Fatura yalnızca yapmanız gereken ödemeler olduğunda düzenlenir.

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Fatura henüz düzenlenmemiştir. Fatura döneminin sonuna kadar bekleyin.

- Hesap Yöneticisi değilseniz eski faturaları görüntüleyemezsiniz.

## <a name="download-usage"></a>Kullanımı indir

 Çoğu abonelikler için günlük kullanım dosyanızda Bul [Azure hesap Merkezi](https://account.azure.com/Subscriptions). Kullanım bir EA müşterisinin olarak karşıdan yüklemek isterseniz, kullanılabilir, [Azure portalında](https://portal.azure.com/) > **maliyet Yönetimi + faturalandırma** > **kullanım ve Ücret**. 

### <a name="download-usage-from-the-account-center-csv"></a>Hesap Merkezi'nden (.csv) kullanımı indir

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

### <a name="download-usage-for-ea-customers"></a>EA müşterileri kullanımı indir

Görüntülemek ve kullanım verileri bir EA müşterisinin olarak indirmek için Kurumsal yönetici veya hesap sahibi veya departman yönetici görünümü ücretleri ilkesi etkin olması gerekir.

1. [Azure Portal]( http://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seçin **kullanım ve Ücret**.
1. İndirmek istediğiniz ayında seçin **indirme**.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
