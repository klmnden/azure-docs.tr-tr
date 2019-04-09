---
title: Microsoft Azure faturanızı görüntüleyip | Microsoft Docs
description: Microsoft Azure faturanızı görüntüleyip açıklar
keywords: Fatura, fatura indirme, azure fatura, azure kullanımı
services: billing
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: f71fe9b02765e0fc8fd5f3b7abbd54c87b08132f
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264072"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızı görüntüleme ve indirme

Çoğu abonelikler için fatura indirebilirsiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) veya e-posta ile gönderilen sahip. Bir Kurumsal Anlaşma (EA müşterisinin) ile bir Azure müşterisi iseniz, kuruluşunuzun fatura karşıdan yükleyemiyor. Kayıt için fatura almaya ayarlanmış yapabilmek için fatura gönderilir.

Yalnızca belirli rolleri gibi kuruluş yöneticisi ve Hesap Yöneticisi, faturaları görüntülemek için izne sahip. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

Varsa bir [Microsoft Müşteri sözleşmesi](#check-your-access-to-a-microsoft-customer-agreement), bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama yöneticisi faturalarınızda almak için. Microsoft Müşteri sözleşmesi için fatura rolleri hakkında daha fazla bilgi edinmek için bkz. [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="download-your-azure-invoices-pdf"></a>Azure faturalarınızda (.pdf) indirin

Çoğu abonelikler için Azure portalından faturanızı indirebilirsiniz. Microsoft Müşteri sözleşmesi varsa, bkz faturalar için fatura profili.

### <a name="download-invoices-for-an-individual-subscription"></a>Tek bir abonelik için fatura indir

1. Aboneliğinizden seçin [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında [faturalar erişimi olan bir kullanıcı](billing-manage-access.md).

2. Seçin **faturalar**.

    ![Faturalandırma ve kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Tıklayın **faturayı indir** PDF faturanızı bir kopyasını görüntülemek için. Bu derse **kullanılamıyor**, bkz: [neden olmayan görüyorum fatura son fatura dönemi için?](#noinvoice)

    ![Her fatura dönemi için faturalandırma dönemleri, indirme seçeneğini ve Toplam ücret gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Fatura dönemi tıklayarak günlük kullanımınızla da görüntüleyebilirsiniz.

Faturanızı hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için fatura indir

Faturalar her biri için oluşturulur [profili faturalama](billing-mca-overview.md#understand-billing-profiles) Microsoft Müşteri sözleşmesi. Bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama yöneticisi, Azure portalından faturaları indirmesine izin gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
1. Seçin **faturalar**.
1. Fatura kılavuzunda, indirmek istediğiniz fatura satırını bulur.
1. Üç noktaya tıklayın (`...`) satırın sonunda.
    ![Satır sonundaki üç noktayı gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. İndirme bağlam menüsünde seçin **fatura**.

    ![Bağlam menüsünü gösteren ekran görüntüsü](./media/billing-download-azure-invoice/contextmenu.png)

Son fatura dönemi için fatura görmüyorsanız bkz [neden olmayan görüyorum fatura son fatura dönemi için?](#noinvoice)

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

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
1. Altında **ayarları**seçin **özellikleri**.
1. Altında **e-posta fatura**seçin **güncelleştirme e-posta fatura tercih**.

    ![E-posta fatura özellikleri gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-email.png)

1. Seçin **katılım**.
1. Tıklayın **güncelleştirme**.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri sözleşmesi faturalarınızda e-posta alma dışında iyileştirilmiş

Yukarıdaki ve tıklayarak adımları izleyerek e-posta ile faturanızı alma dışında iyileştirilmiş **çıkma**. Tüm sahipleri, Katkıda Bulunanlar, okuyucular ve fatura yöneticileri kabul faturayı e-posta ile çok alma dışında. Okuyucu, e-posta fatura tercihini değiştiremezsiniz.

### <a name="noinvoice"></a> Son fatura dönemi için fatura neden göremiyorum?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Fatura henüz düzenlenmemiştir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntülemek için izniniz yok. Microsoft Müşteri sözleşmesi varsa, faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama Yöneticisi. Hesap Yöneticisi değilseniz, diğer abonelikler için eski faturalar göremeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

- Ücretsiz deneme sürümü ya da bir aylık kredi miktarını aşan yaramadı aboneliğinizle varsa, Microsoft Müşteri sözleşmesi yoksa fatura elde etmezsiniz.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi erişiminizi kontrol edin
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Fatura ve ücretleri hakkında daha fazla bilgi için bkz:

- [Microsoft Azure kullanımınızı ve ücretleri görüntüleme ve indirme](billing-download-azure-daily-usage.md)
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
