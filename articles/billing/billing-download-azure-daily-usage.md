---
title: Görüntüle ve indir Azure kullanımı ve ücretleri
description: Azure günlük kullanımı ve ücretleri görüntüleme veya indirme işlemini açıklamaktadır.
keywords: Kullanım faturalandırma, kullanım ücretlerini, kullanım, kullanımını görüntüleyin, azure fatura, azure kullanımı indir
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 7d2d7be562eaaa7dd21e63735f5697ffe5a62f8a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491440"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Azure kullanım ve Ücret görüntülemenize ve indirmenize

Bir Kurumsal Sözleşme müşterisi olduğunuz veya varsa bir [Microsoft Müşteri sözleşmesi](#check-your-access-to-a-microsoft-customer-agreement), Azure kullanımı ve ücretleri indirmek [Azure portalında](https://portal.azure.com/). Diğer abonelikler için Git [Azure hesap Merkezi](https://account.azure.com/Subscriptions) kullanım indirilemedi.

Yalnızca bazı rollerde Hesap Yöneticisi veya kuruluş yöneticisi gibi Azure kullanım bilgilerini alma iznine sahip. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

Varsa bir [Microsoft Müşteri sözleşmesi](#check-your-access-to-a-microsoft-customer-agreement), bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama yöneticisi, Azure kullanımı ve ücretleri görüntülemek için. Microsoft Müşteri sözleşmesi için fatura rolleri hakkında daha fazla bilgi edinmek için bkz. [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

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

Günlük kullanımınız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

## <a name="download-usage-for-ea-customers"></a>EA müşterileri kullanımı indir

Etkin ilke görünümü departmanı yönetici ücretleri veya görüntülemek ve kullanım verileri bir EA müşterisinin olarak indirmek için hesap sahibi, bir kuruluş yöneticisi olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama *maliyet Yönetimi + faturalandırma*.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seçin **kullanım ve Ücret**.
1. İndirmek istediğiniz ayında seçin **indirme**.

## <a name="download-usage-for-your-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmenizi kullanımı indir

Microsoft Müşteri sözleşmesi varsa, Azure kullanımı ve ücretleri fatura profiliniz için indirebilirsiniz. Bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya faturalama yöneticisi Azure kullanımı ve ücretleri CSV olarak indirmek için gerekir.

### <a name="download-usage-for-billed-charges"></a>Faturalandırılan ücretler kullanımı indir

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Arama *maliyet Yönetimi + faturalandırma*.
3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
4. **Faturalar**'ı seçin.
5. Fatura kılavuzunda fatura indirmek istediğiniz kullanım için karşılık gelen satırı bulur.
6. Üç nokta simgesine tıklayın (`...`) satırın sonunda.

    ![Satır sonundaki üç noktayı gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)

7. İndirme bağlam menüsünde seçin **Azure kullanım ve Ücret**.

     ![Azure kullanım ve Ücret seçili gösteren ekran görüntüsü](./media/billing-download-azure-usage/contextmenu-usage.png)

### <a name="download-usage-for-pending-charges"></a>Kullanım ücretleri bekleyen indirin

Geçerli fatura dönemi için ayın başından bu yana kullanımı da indirebilirsiniz. Henüz faturalandırılmış değil Bu kullanım ücretlerine uygulanır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Arama *maliyet Yönetimi + faturalandırma*.
3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
4. İçinde **genel bakış** alanında, indirme bağlantıları ay başından bu yana ücretleri altında bulabilirsiniz.
5. Seçin **Azure kullanım ve Ücret**.

    ![Genel Bakış'tan indirmeyi gösteren ekran görüntüsü](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi erişiminizi kontrol edin
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturayı ve kullanım ücretlerinizin hakkında daha fazla bilgi için bkz:

- [Şirket koşulları anlama, Microsoft Azure ayrıntılı kullanım](billing-understand-your-usage.md)
- [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md)
- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Görüntüleyin ve indirin, kuruluşunuzun Azure fiyatlandırması](billing-ea-pricing.md)

Microsoft Müşteri sözleşmesi varsa, bkz:

- [Microsoft Müşteri sözleşmesi Azure ilgili koşulları anlama ayrıntılı kullanım](billing-mca-understand-your-usage.md)
- [Microsoft Müşteri sözleşmesi faturanızla ilgili ücretlerini anlama](billing-mca-understand-your-bill.md)
- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Görüntüleyin ve Microsoft Müşteri sözleşmenizi vergi belgelerini indirin](billing-mca-download-tax-document.md)
- [Görüntüleyin ve indirin, kuruluşunuzun Azure fiyatlandırması](billing-ea-pricing.md)
