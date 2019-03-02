---
title: Microsoft Müşteri anlaşmanın faturasında - Azure ücretlerini anlama | Microsoft Docs
description: Okuma ve faturanızla ilgili ücretlerini anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 3c5f5778d68b8df4d4b3132ff96bedb9d5da24ee
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249372"
---
# <a name="understand-the-charges-on-your-microsoft-customer-agreements-invoice"></a>Microsoft Müşteri anlaşmanın faturasında ücretlerini anlama

Ücret, faturada faturasında ayrı işlemler analiz ederek mutabakat yapabilirsiniz.

Microsoft Müşteri sözleşmesi için fatura hesabında bir fatura her ay faturalandırma her profil için oluşturulur. Faturanın önceki aydan itibaren tüm ücretleri içerir. Azure Portalı'nda faturalarınızı görüntüleyebilirsiniz. Daha fazla bilgi için [görüntüleyin ve indirme, Microsoft Azure fatura](billing-download-azure-invoice.md).

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Azure portalında bir fatura için işlemleri görüntüle

1. [Azure Portal](https://www.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

    ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seçin **tüm işlemleri** ekranın sol tarafındaki. Bir faturalama hesabı ya da fatura profili seçmek için sahip olabileceğiniz erişiminizi bağlı olarak ardından seçin **tüm işlemleri**.

4. Tüm işlemler sayfasında aşağıdaki bilgileri görüntüler:

    ![Faturalandırılan işlem listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billed-transactions-list.png)

    |Sütun  |Tanım  |
    |---------|---------|
    |Tarih     | İşlem tarihi  |
    |Fatura Kimliği     | İşlem faturalandırılır fatura tanımlayıcısı. Bir destek isteği gönderirseniz kimliği destek talebinizi hızlandırmak için Azure destek ekibi ile paylaşabilirsiniz. |
    |İşlem türü     |  Satın alma, iptal et ve kullanım ücretleri gibi işlem türü  |
    |Ürün ailesi     | Sanal makineler için işlem veya Azure SQL veritabanı için veritabanı gibi ürün kategorisi|
    |Ürün SKU'su     | Ürününüzü örneğini tanımlayan benzersiz bir kod |
    |Miktar     |  İşlem miktarını      |
    |Fatura bölümü     | Profilin fatura ödeme, bu bölümde işlem gösterilir |
    |Faturalama profili     | Bu faturalandırma profilin faturada işlem gösterilir |

5. Fatura Kimliği hareketlerinde fatura filtrelemek için arama yapın.

### <a name="view-transactions-by-invoice-sections"></a>Fatura bölümleri işlemleri görüntüle

Fatura bölümleri fatura profilin fatura maliyetlerini düzenlemenize olanak tanır. Daha fazla bilgi için [anlayın fatura bölüm](billing-mca-overview.md#understand-invoice-sections). Fatura oluşturulduğunda, faturalandırma profili içindeki tüm bölümler için ücret faturada gösterilir.

Aşağıdaki görüntüde bir örnek faturada BT departmanı fatura bölüm ücretleri gösterir.

![Fatura bölüm bilgileri tarafından ayrıntılarını gösteren örnek resim](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Bir fatura bölümü ücretleri belirledikten sonra işlem ücretleri mutabakat sağlamak için Azure portalında görüntüleyebilirsiniz.

1. Tüm işlemler sayfasında hareketlerinde fatura görüntülemek için Azure portalında gidin. Daha fazla bilgi için [Azure portalında görüntüleme işlemleri için fatura](#view-transactions-for-an-invoice-in-the-azure-portal).

2. Fatura bölümü için işlemleri görüntülemek için fatura bölüm adına göre filtreleyin.

## <a name="understand-pending-charges-to-estimate-your-next-invoice"></a>Sonraki faturanızı tahmin etmek için ücretleri anlama

Microsoft Müşteri sözleşmesi için fatura hesabındaki ücret faturalandırılır kadar bunlar tahmin ve bekleyen kabul. Kesin ve kabul bunlar faturada gösterdiğinizde faturalandırılır. Faturalandırma profili ücretleri, bir sonraki fatura tahmini ücretleri bekleyen görüntüleyebilirsiniz.

### <a name="view-charges-summary"></a>Ücretleri özetini görüntüle

1. [Azure Portal](https://www.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **özeti** ekranın üst kısmında sekmesinden.

5. Ücretler bölümü, ayın başından bu yana ve son aya ait giderler görüntüler.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

Ayın başından bu yana ücretleri geçerli ay için bekleyen ücretlerdir ve fatura ayında oluşturulduğunda faturalandırılır. Son aya ait fatura hala değil oluşturulur sonra geçen ayın ücretlerdir ayrıca bekleniyor.

### <a name="view-pending-transactions"></a>Bekleyen işlem görüntüleyin

Ücretleri tanımladıktan sonra tüm işlemler sayfasında ücretleri katkıda tek tek işlemleri görüntüleyebilirsiniz. Bu noktada, kullanım ücretleri tüm işlem sayfasında görüntülenmez. Bekleyen kullanım ücretleri Azure abonelikler sayfasında görüntüleyebilirsiniz. Daha fazla bilgi için [kullanım ücretleri bekleyen görüntüleme](#view-pending-usage-charges)

1. [Azure Portal](https://www.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **tüm işlemleri** ekranın sol tarafındaki.

5. Arama **bekleyen**. Kullanım **Timespan** ücretleri geçerli veya geçen ay görüntülemek için filtre.

   ![Bekleyen işlemlerin listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Bekleyen kullanım ücretleri görüntüleme

1. [Azure Portal](https://www.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **tüm abonelikleri** ekranın sol tarafındaki.

5. Azure abonelik sayfasında, geçerli ve son aya ait ücretler her abonelik için fatura profilinde görüntüler. Ayın başından bu yana ücretleri geçerli ay için bekleyen ücretlerdir ve fatura ayında oluşturulduğunda faturalandırılır. Son aya ait fatura hala değil oluşturulur sonra geçen ayın ücretlerdir ayrıca bekleniyor.

    ![Faturalandırma profili Azure abonelikleri listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

Azure kullanım ve Ücret dosyasını kullanarak ayrıntılı kullanım ücretlerini görüntüleyebilirsiniz. Daha fazla bilgi için sonraki bölüme bakın.

## <a name="analyze-your-azure-usage-charges"></a>Azure Kullanım ücretlerinizi analiz edin

Temel Kullanım ücretlerinizi analiz etmek için Azure kullanımı ve ücretleri csv dosyasını kullanın. Fatura veya için dosyayı indirebilirsiniz ücretleri bekliyor. Daha fazla bilgi için [Azure faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Fatura bölümünde ayrıntılı kullanımını görüntüleyin

Kullanım ücretleri, fatura bölümler için mutabık kılınacak Azure kullanım ve Ücret dosya filtreleyebilirsiniz.

Aşağıdaki adımlarda hesap bölüm fatura bölümü için işlem ücretleri mutabık kılma yoluyla yol:

![Fatura bölüm bilgileri tarafından ayrıntılarını gösteren örnek resim](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | Fatura PDF | Azure kullanım ve Ücret CSV |
 | --- | --- |
 |BT departmanı |invoiceSectionName |
 |Kullanım ücretleri - Azure standart |productOrderName |
 |Depolama |serviceFamily |

1. Filtre **invoiceSectionName** CSV dosyasındaki sütun **hesap bölüm**.
2. Filtre **productOrderName** CSV dosyasındaki sütun **Microsoft Azure planlama**.
3. Filtre **serviceFamily** CSV dosyasındaki sütun **Microsoft.Compute**.

![Kullanımını gösterir ve kullanım ücretlerine ilişkin fatura bölümü tarafından filtre dosyası ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)

<!--Todo Add screenshot of usage file -->

### <a name="view-detailed-usage-by-subscription"></a>Aboneliğe göre kullanım ayrıntılarını görüntüleyin

Kullanım ücretleri Abonelikleriniz için mutabık kılınacak Azure kullanım ve Ücret csv dosyası filtreleyebilirsiniz. Bir fatura profilinde tüm abonelikleri görmek için bkz: [kullanım ücretleri bekleyen görüntüleme](#view-pending-usage-charges).

Abonelik ücretleri tanımladıktan sonra Azure kullanım ve Ücret csv dosyası ücretleri çözümlemek için kullanın.

Aşağıdaki ekran görüntüsünde, Azure portalında Aboneliklerin listesini görüntüler.

![Faturalandırma profili Azure abonelikleri listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filtre **subscriptionName** Azure kullanım ve Ücret CSV dosyası sütununda **WA_Subscription** WA_Subscription için ayrıntılı kullanım ücretlerini görüntülemek için.

![Kullanımını gösterir ve aboneliğe göre filtre dosyası ücretleri ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Faturanızın ödeme

Faturanızın ödeme yönergeleri fatura alt kısmında gösterilir. [Nasıl ödeme yapabileceğinizi öğrenin](billing-mca-understand-your-invoice.md#how-to-pay).

Zaten faturanızın ödeme yaptıysanız, Azure portalında faturaları sayfasında ödeme durumunu kontrol edebilirsiniz. 

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı kullanım ve fatura hakkında daha fazla bilgi için bkz:

- [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md)
- [Microsoft Müşteri sözleşmesi faturanızla ilgili koşulları anlama](billing-mca-understand-your-invoice.md)
- [Microsoft Müşteri sözleşmesi kullanımınızı CSV ilgili koşulları anlama](billing-mca-understand-your-usage.md)
