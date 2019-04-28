---
title: Görüntüleyin ve indirin, kuruluşunuzun Azure fiyatlandırması | Microsoft Docs
description: Görüntüleyin ve fiyatlandırma indirin veya kuruluşunuzun fiyatlandırma ile maliyetleri tahmin öğrenin.
services: ''
documentationcenter: ''
author: adpick
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: d4c64433089f14d845cea8be9adf0ef13675cfd1
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127642"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Görüntüleyin ve indirin, kuruluşunuzun Azure fiyatlandırması

Azure müşterileri bir Kurumsal Anlaşma'ile (EA) veya [Microsoft Müşteri sözleşmesi](#check-your-access-to-a-microsoft-customer-agreement) görüntüleyin ve fiyatlandırma, Azure portalından indirin. Microsoft Müşteri sözleşmesi varsa, Görünüm ve yükleme için Microsoft Müşteri sözleşmenizi fiyatlandırma bakın.

## <a name="view-and-download-ea-pricing"></a>Görüntülemenize ve indirmenize EA fiyatlandırması

Kuruluşunuz için Kurumsal yönetici tarafından belirlenen politikalara bağlı olarak, yalnızca belirli yönetim rolleri kuruluşunuzun EA fiyatlandırma bilgilerine erişim sağlar. Daha fazla bilgi için [azure'da yönetim rolleri Azure Kurumsal anlaşmasına anlamak](billing-understand-ea-roles.md).

1. Bir kuruluş yöneticisi olarak oturum açın [Azure portalında](https://portal.azure.com/).
1. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-ea-pricing/portal-cm-billing-search.png)

1. Faturalama hesap altında seçin **kullanım ve Ücret**.

   ![Kullanım ve faturalandırma altında ücretleri gösteren ekran görüntüsü](./media/billing-ea-pricing/ea-pricing-usage-charges-nav.png)

1. Seçin ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-ea-pricing/download-icon.png) **indirme** aya ait.
1. Altında **fiyat**seçin **csv indirme**.

   ![Fiyat listesini gösteren ekran görüntüsü csv düğmesi indirin](./media/billing-ea-pricing/download-ea-price-sheet.png)

## <a name="view-and-download-pricing-for-your-microsoft-customer-agreement"></a>Görüntüleyin ve Microsoft Müşteri sözleşmenizi fiyatlandırma indirin

Fatura Profil sahibi, katkıda bulunan, okuyucu veya fatura Yöneticisi görüntülemenizi ve indirmenizi fiyatlandırma olması gerekir. Microsoft Müşteri sözleşmesi için fatura rolleri hakkında daha fazla bilgi edinmek için bkz. [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="download-price-sheets-for-the-current-billing-period"></a>Geçerli fatura dönemi için fiyat listeleri indirin

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
1. İçinde **genel bakış** dikey penceresinde, indirme bağlantıları ay başından bu yana ücretleri altında bulabilirsiniz.
1. Seçin **Azure fiyat**.
![Genel Bakış'tan indirmeyi gösteren ekran görüntüsü](./media/billing-ea-pricing/open-pricing.png)

### <a name="download-price-sheets-for-billed-charges"></a>Fiyat listeleri faturalandırılan ücretler indirin

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Faturalandırma profili seçin. Erişiminizi bağlı olarak, bir faturalama hesabı seçmeniz gerekebilir.
1. **Faturalar**'ı seçin.
1. Fatura kılavuzunda fatura indirmek istediğiniz fiyat listesine karşılık gelen satırı bulur.
1. Üç noktaya tıklayın (`...`) satırın sonunda.
![Seçili nokta gösteren ekran görüntüsü](./media/billing-ea-pricing/billingprofile-invoicegrid.png)

1. Seçili fatura'ndaki hizmet fiyatları görmek istiyorsanız, seçin **fatura fiyat**.
1. Belirli bir fatura dönemi için tüm Azure Hizmetleri için fiyatları görmek isteyip istemediğinizi seçin **Azure fiyat**.

![Fiyat listeleri ile bağlam menüsünü gösteren ekran görüntüsü](./media/billing-ea-pricing/contextmenu-pricesheet.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Azure fiyatlandırma hesaplayıcısı ile kullanımı tahmini maliyetinizi hesaplayın

Azure fiyatlandırma hesaplayıcısı ile maliyetleri tahmin etme, kuruluşunuzun fiyatlandırma de kullanabilirsiniz.

1. Git [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator).
1. Sağ üst kısımdaki seçin **oturum**.
1. Altında **programlar ve teklif** > **lisanslama programı**seçin **Kurumsal Anlaşma (EA)**.
1. Altında **programlar ve teklif** > **seçili sözleşmesi**seçin **hiçbiri seçilmedi**.

    ![Fiyat listesini gösteren ekran görüntüsü csv düğmesi indirin](./media/billing-ea-pricing/ea-pricing-calculator-estimate.png)

1. Kuruluş seçin.
1. **Uygula**’yı seçin.
1. Arayın ve ürünleri tahmininize ekleyin.
1. Tahmini fiyatlar kuruluş için seçtiğiniz fiyatlandırma temel alır.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi erişiminizi kontrol edin
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir EA müşterisiyseniz bakın:

- [Azure için Kurumsal Anlaşma fatura bilgilerinizi erişimi yönetme](billing-manage-access.md)
- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
- [Kurumsal Anlaşma müşterileri için faturanızı anlayın](billing-understand-your-bill-ea.md)

Microsoft Müşteri sözleşmesi varsa, bkz:

- [Bir Microsoft Müşteri sözleşmesi, fiyat koşullarını anlama](billing-mca-understand-pricesheet.md)
- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
- [Görüntüleyin ve fatura profiliniz için vergi belgelerini indirin](billing-mca-download-tax-document.md)
- [Faturadaki fatura profil ücretlerini anlama](billing-mca-understand-your-bill.md)
