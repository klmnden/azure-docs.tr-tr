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
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: b78fb7d697f8a72b3c2f99c4509ea6ac5c5e5566
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60616475"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>İndirme veya Azure fatura ve günlük kullanım verilerini görüntüleme

Çoğu abonelikler için fatura indirebilirsiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) veya e-posta ile gönderilen sahip. Bir Kurumsal Anlaşma (EA müşterisinin) ile bir Azure müşterisi iseniz, kuruluşunuzun fatura karşıdan yükleyemiyor. Kayıt için fatura almaya ayarlanmış yapabilmek için fatura gönderilir.

Bir Kurumsal Sözleşme müşterisi olduğunuz veya varsa bir [Microsoft Müşteri sözleşmesi](#check-access-to-a-microsoft-customer-agreement), kullanımı indirebileceğiniz [Azure portalında](https://portal.azure.com/). Diğer abonelikler için Git [Azure hesap Merkezi](https://account.azure.com/Subscriptions) kullanım indirilemedi.

Yalnızca bazı rollerde fatura hesap yöneticisi veya kuruluş yöneticisi gibi faturayı ve kullanım bilgilerini alma iznine sahip. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

Microsoft Müşteri sözleşmesi varsa, bir faturalandırma profili sahibi, katkıda bulunan, okuyucu, veya faturalama yöneticisi, fatura ve kullanım bilgilerini görüntülemek için. Microsoft Müşteri sözleşmesi için fatura rolleri hakkında daha fazla bilgi edinmek için bkz. [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Azure faturalarınızda (.pdf) indirin

Çoğu abonelikler için Azure portalından faturanızı indirebilirsiniz. Microsoft Müşteri sözleşmesi varsa, bkz faturalar için fatura profili.

### <a name="download-invoices-for-an-individual-subscription"></a>Tek bir abonelik için fatura indir

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında [faturalar erişimi olan bir kullanıcı](billing-manage-access.md).

2. **Faturalar**'ı seçin.

    ![Faturalandırma ve kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Tıklayın **faturayı indir** PDF faturanızı bir kopyasını görüntülemek için. Bu derse **kullanılamıyor**, bkz: [neden olmayan görüyorum fatura son fatura dönemi için?](#noinvoice)

    ![Her fatura dönemi için faturalandırma dönemleri, indirme seçeneğini ve Toplam ücret gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Fatura dönemi tıklayarak günlük kullanımınızla da görüntüleyebilirsiniz.

Faturanızı hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için fatura indir

Faturalar her biri için oluşturulur [profili faturalama](billing-mca-overview.md#understand-billing-profiles) Microsoft Müşteri sözleşmesi. Bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama yöneticisi, Azure portalından faturaları indirmesine izin gerekir.

1. Arama **maliyet Yönetimi + faturalandırma**.
2. Faturalandırma profili seçin.
3. **Faturalar**'ı seçin.
4. Fatura kılavuzunda, indirmek istediğiniz fatura satırını bulur.
5. Üç noktaya tıklayın (`...`) satırın sonunda.
6. İndirme bağlam menüsünde seçin **fatura**.

Son fatura dönemi için fatura görmüyorsanız bkz **ek bilgi**. <!-- Fix this -->
### <a name="noinvoice"></a> Son fatura dönemi için fatura neden göremiyorum?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Fatura henüz düzenlenmemiştir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntülemek için izniniz yok. Microsoft Müşteri sözleşmesi varsa, faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama Yöneticisi. Hesap Yöneticisi değilseniz, diğer abonelikler için eski faturalar göremeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

- Ücretsiz deneme sürümü ya da bir aylık kredi miktarını aşan yaramadı aboneliğinizle varsa, Microsoft Müşteri sözleşmesi yoksa fatura elde etmezsiniz.

## <a name="get-your-invoice-in-email-pdf"></a>Faturanızı (.pdf) e-posta ile alın

Kabul et ve Azure'ı almak için ek alıcılar yapılandırma faturayı e. Bu özellik Destek teklifleri, Kurumsal Anlaşma ya da Open ile Azure gibi bazı abonelikler için kullanılamıyor olabilir. Bir Microsoft Customer sözleşme yaptıysanız, e-posta ile faturalandırma profilinizi faturalar Get bakın.

### <a name="get-your-subscriptions-invoices-in-email"></a>Aboneliğinizin faturaların e-posta ile alın

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip olduğunuz her abonelik için kabul et. Tıklayın **faturalar** ardından **my faturayı e-posta**.

    ![Katılım akışı gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Tıklayın **katılım** ve koşullarını kabul edin.

    ![Katılım akışı adım 2 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Anlaşmayı kabul ettiğiniz sonra ek alıcılar yapılandırabilirsiniz. E-posta adresi, artık bir alıcı kaldırıldığında depolanır. Fikrinizi değiştirirseniz, bunları yeniden eklemeniz gerekir.

    ![Katılım akışı adım 3 gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Adımları tamamladıktan sonra bir e-posta almazsanız, e-posta adresinizi doğru olduğundan emin olun [iletişim tercihleri profilinize](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>E-posta aboneliğinizin faturalar alma dışında iyileştirilmiş

Yukarıdaki ve tıklayarak adımları izleyerek e-posta ile faturanızı alma dışında iyileştirilmiş **iyileştirilmiş e-postayla faturaların**. Bu seçenek, faturaların e-posta ile gönderilmesi için girilmiş olan e-posta adreslerini kaldırır. Geri katılırsanız alıcıların yeniden yapılandırabilirsiniz.

 ![Vazgeçme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri sözleşmesi faturalarınızda e-posta ile alın

Microsoft Müşteri sözleşmesi varsa, bir e-postada faturanızı almak için de seçebilirsiniz. Tüm faturalandırma profili sahipleri, Katkıda Bulunanlar, okuyucular ve fatura yöneticilerinin e-posta ile fatura alırsınız. Okuyucu, e-posta fatura tercih güncelleştirilemiyor.

1. Arama **maliyet Yönetimi + faturalandırma**.
1. Faturalandırma profili seçin.
1. Altında **ayarları**seçin **özellikleri**.
1. Altında **e-posta fatura**seçin **güncelleştirme e-posta fatura tercih**.
1. Seçin **katılım**.
1. Tıklayın **güncelleştirme**.

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>E-posta ile faturalandırma profili faturalarınızda alma dışında iyileştirilmiş

Yukarıdaki ve tıklayarak adımları izleyerek e-posta ile faturanızı alma dışında iyileştirilmiş **çıkma**. Tüm sahipleri, Katkıda Bulunanlar, okuyucular ve fatura yöneticileri kabul faturayı e-posta ile çok alma dışında. Okuyucu, e-posta fatura tercihini değiştiremezsiniz.

## <a name="download-usage"></a>Kullanımı indirme

 Çoğu abonelikler için günlük kullanım dosyanızda Bul [Azure hesap Merkezi](https://account.azure.com/Subscriptions). EA müşterisiyseniz veya bir Microsoft Müşteri Sözleşmemiz var, kullanımı indirebilirsiniz [Azure portalında](https://portal.azure.com/). <!-- TO DO: update PayG experience to Ibiza once it ships-->

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

Günlük kullanımınız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>EA müşterileri kullanımı indir

Etkin ilke görünümü departmanı yönetici ücretleri veya görüntülemek ve kullanım verileri bir EA müşterisinin olarak indirmek için hesap sahibi, bir kuruluş yöneticisi olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama *maliyet Yönetimi + faturalandırma*.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seçin **kullanım ve Ücret**.
1. İndirmek istediğiniz ayında seçin **indirme**.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmenizi kullanımı indir

Görüntülemek ve faturalandırma profili için kullanım verilerini indirmek için bir faturalandırma profili sahibi, katkıda bulunan, okuyucu, veya faturalama Yöneticisi.

#### <a name="download-usage-for-billed-charges"></a>Faturalandırılan ücretler kullanımı indir

1. Arama **maliyet Yönetimi + faturalandırma**.
2. Faturalandırma profili seçin.
3. **Faturalar**'ı seçin.
4. Fatura kılavuzunda fatura indirmek istediğiniz kullanım için karşılık gelen satırı bulur.
5. Üç noktaya tıklayın (`...`) satırın sonunda.
6. İndirme bağlam menüsünde seçin **Azure kullanım ve Ücret**.

#### <a name="download-usage-for-open-charges"></a>Açık ücretler kullanımı indir

Ayrıca, ay başından bu yana kullanım ücretleri henüz faturalandırılmış yok anlamına gelen geçerli fatura dönemi için indirebilirsiniz.

1. Arama **maliyet Yönetimi + faturalandırma**.
2. Faturalandırma profili seçin.
3. İçinde **genel bakış** dikey penceresinde tıklayın **indirme Azure kullanım ve Ücret**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Fatura ve ücretleri hakkında daha fazla bilgi için bkz:

- [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md)
- [Azure faturanızla ilgili koşulları anlama](billing-understand-your-invoice.md)
- [Şirket koşulları anlama, Microsoft Azure ayrıntılı kullanım](billing-understand-your-usage.md)
- [Kuruluşunuzun Azure fiyatlandırmayı görüntüleyin](billing-ea-pricing.md)

Microsoft Müşteri sözleşmesi varsa, bkz:

- [Faturadaki fatura profiliniz için ücretlerini anlama](billing-mca-understand-your-bill.md)
- [Fatura profiliniz için fatura ilgili koşulları anlama](billing-mca-understand-your-invoice.md)
- [Azure kullanım ve Ücret dosya fatura profiliniz için anlama](billing-mca-understand-your-usage.md)
- [Görüntüleyin ve fatura profiliniz için vergi belgelerini indirin](billing-mca-download-tax-document.md)
- [Kuruluşunuzun Azure fiyatlandırmayı görüntüleyin](billing-ea-pricing.md)
