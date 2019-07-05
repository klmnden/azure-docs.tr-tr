---
title: Microsoft Müşteri anlaşmanın faturasında - Azure ücretlerini anlama
description: Okuma ve faturanızla ilgili ücretlerini anlama hakkında bilgi edinin.
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: ee250589133abb1944ff17e39dc650cbae4279c6
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490672"
---
# <a name="understand-charges-on-your-microsoft-customer-agreement-invoice"></a>Microsoft Müşteri sözleşmesi faturanızla ilgili ücretlerini anlama

Tek tek işlemleri analiz ederek ücret, faturada anlayabilirsiniz. Microsoft Müşteri sözleşmesi için fatura hesabında bir fatura her ay faturalandırma her profil için oluşturulur. Faturanın önceki aydan itibaren tüm ücretleri içerir. Azure Portalı'nda faturalarınızı görüntüleyebilirsiniz. Daha fazla bilgi için [bir Microsoft Müşteri sözleşmesi faturaları indirmesine](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Azure portalında bir fatura için işlemleri görüntüle

1. [Azure Portal](https://www.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

    ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seçin **tüm işlemleri** sayfanın sol tarafındaki. Erişiminizi bağlı olarak, bir faturalama hesabı, faturalandırma profili veya bir fatura bölümü seçin ve ardından olabilir **tüm işlemleri**.

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
    |Faturalandırma profili     | Bu faturalandırma profilin faturada işlem gösterilir |

5. Fatura Kimliği hareketlerinde fatura filtrelemek arayın.

### <a name="view-transactions-by-invoice-sections"></a>Fatura bölümleri işlemleri görüntüle

Fatura bölümler bir fatura profilin fatura maliyetlerini düzenlemenize yardımcı olur. Daha fazla bilgi için [fatura bölümü anlamanız](billing-mca-overview.md#invoice-sections). Fatura oluşturulduğunda, faturalandırma profili içindeki tüm bölümler için ücret faturada gösterilmektedir.

Aşağıdaki görüntüde bir örnek fatura hesap bölüm fatura bölümü için ücret gösterir.

![Fatura bölüm bilgileri tarafından ayrıntılarını gösteren örnek resim](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Bir fatura bölümü ücretleri tespit ettik, işlem giderleri anlamak için Azure portalında görüntüleyebilirsiniz.

1. Tüm işlemler sayfasında hareketlerinde fatura görüntülemek için Azure portalında gidin. Daha fazla bilgi için [Azure portalında görüntüleme işlemleri için fatura](#view-transactions-for-an-invoice-in-the-azure-portal).

2. İşlemleri görüntülemek için fatura bölüm adına göre filtreleyin.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Sonraki faturanızı tahmin etmek için ücretleri gözden geçirin

Microsoft Müşteri sözleşmesi için fatura hesabında ücretleri tahmini ve bunlar faturalandırılır kadar beklemeye. Bekleyen ücretleri bir sonraki faturanızı tahmin etmek için Azure portalında görüntüleyebilirsiniz. Bekleyen tahmini ücretleri ve vergiler dahil değildir. Bekleyen ücretlerden sonraki faturanızla ilgili gerçek ücretleri değişir.

### <a name="view-summary-of-pending-charges"></a>Bekleyen ücretleri özeti görüntüle

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **özeti** ekranın üst kısmında sekmesinden.

5. Ücretler bölümü, ayın başından bu yana ve son aya ait giderler görüntüler.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

Ayın başından bu yana ücretleri geçerli ay için bekleyen ücretlerdir ve fatura ayında oluşturulduğunda faturalandırılır. Son aya ait fatura hala değil oluşturulur sonra geçen ayın ücretlerdir ayrıca bekliyor ve sonraki faturanızı görünür.

### <a name="view-pending-transactions"></a>Bekleyen işlem görüntüleyin

Ücretleri tespit ederken, giderleri katkıda tek tek işlemleri analiz ederek ücretleri anlayabilirsiniz. Bu noktada, kullanım ücretleri tüm işlem sayfada görüntülenmiyor. Bekleyen kullanım ücretleri Azure abonelikler sayfasında görüntüleyebilirsiniz. Daha fazla bilgi için [kullanım ücretleri bekleyen görüntüleme](#view-pending-usage-charges)

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **tüm işlemleri** sayfanın sol tarafındaki.

5. Arama *bekleyen*. Kullanım **Timespan** ücretleri geçerli veya geçen ay görüntülemek için filtre.

   ![Bekleyen işlemlerin listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Bekleyen kullanım ücretleri görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama *maliyet Yönetimi + faturalandırma*.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** sonra faturalandırma profili seçin.

4. Seçin **tüm abonelikleri** bir sayfanın sol tarafındaki.

5. Azure abonelik sayfasında, geçerli ve son aya ait ücretler her abonelik için fatura profilinde görüntüler. Ayın başından bu yana ücretleri geçerli ay için bekleyen ücretlerdir ve fatura ayında oluşturulduğunda faturalandırılır. Son aya ait fatura hala değil oluşturulur sonra geçen ayın ücretlerdir ayrıca bekleniyor.

    ![Faturalandırma profili Azure abonelikleri listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Azure Kullanım ücretlerinizi analiz edin

Kullanım tabanlı ücretlerinizin analiz etmek için Azure kullanımı ve ücretleri CSV dosyasını kullanın. Fatura veya dosyası indirebilirsiniz ücretleri bekliyor. Daha fazla bilgi için [Azure faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Fatura bölümünde ayrıntılı kullanımını görüntüleyin

Kullanım ücretleri, fatura bölümler için mutabık kılınacak Azure kullanım ve Ücret dosya filtreleyebilirsiniz.

Aşağıdaki adımlarda hesap bölüm fatura bölümü için işlem ücretleri mutabık kılma yoluyla yol:

![Fatura bölüm bilgileri tarafından ayrıntılarını gösteren örnek resim](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | Fatura PDF | Azure kullanım ve Ücret CSV |
 | --- | --- |
 |Hesap bölüm |invoiceSectionName |
 |Kullanım ücretleri - planı Microsoft Azure |productOrderName |
 |İşlem |serviceFamily |

1. Filtre **invoiceSectionName** CSV dosyasındaki sütun **hesap bölüm**.
2. Filtre **productOrderName** CSV dosyasındaki sütun **Microsoft Azure planlama**.
3. Filtre **serviceFamily** CSV dosyasındaki sütun **Microsoft.Compute**.

![Kullanımını gösterir ve kullanım ücretlerine ilişkin fatura bölümü tarafından filtre dosyası ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)


### <a name="view-detailed-usage-by-subscription"></a>Aboneliğe göre kullanım ayrıntılarını görüntüleyin

Kullanım ücretleri Abonelikleriniz için mutabık kılınacak Azure kullanım ve Ücret CSV dosyası filtreleyebilirsiniz. Bir fatura profilinde tüm abonelikleri görmek için bkz: [kullanım ücretleri bekleyen görüntüleme](#view-pending-usage-charges).

Abonelik ücretleri tespit ederken, Azure kullanımı ve ücretleri CSV dosyası ücretleri çözümlemek için kullanın.

Aşağıdaki resimde, Azure portalında Aboneliklerin listesini gösterir.

![Faturalandırma profili Azure abonelikleri listesini gösteren ekran görüntüsü](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filtre **subscriptionName** Azure kullanım ve Ücret CSV dosyası sütununda **WA_Subscription** WA_Subscription için ayrıntılı kullanım ücretlerini görüntülemek için.

![Kullanımını gösterir ve aboneliğe göre filtre dosyası ücretleri ekran görüntüsü](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Faturanızın ödeme

Faturanızın ödeme yönergeleri fatura alt kısmında gösterilir. [Nasıl ödeme yapabileceğinizi öğrenin](billing-mca-understand-your-invoice.md#how-to-pay).

Zaten faturanızın ödeme yaptıysanız, Azure portalında faturaları sayfasında ödeme durumunu kontrol edebilirsiniz.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı kullanım ve fatura hakkında daha fazla bilgi için bkz:

- [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md)
- [Microsoft Müşteri sözleşmesi faturanızla ilgili koşulları anlama](billing-mca-understand-your-invoice.md)
- [Microsoft Müşteri sözleşmesi kullanımınızı CSV ilgili koşulları anlama](billing-mca-understand-your-usage.md)
