---
title: Kuruluş için Azure ayrılmış örnek kullanımını anlamak | Microsoft Docs
description: İşletme kaydı için Azure ayrılmış VM örneğine nasıl uygulandığını anlamak için kullanımınızı okuma öğrenin.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: d6e8b2544f919abeb7fde0e37fc12bd29f0171ef
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064134"
---
# <a name="understand-azure-reserved-instance-usage-for-your-enterprise-enrollment"></a>İşletme kaydı için Azure ayrılmış örnek kullanım anlama
Ayrılmış örnek kullanımını kullanarak anlayın **Reservationıd** gelen [ayırmaları sayfa](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ve kullanım dosyasından [EA portal](https://ea.azure.com). Ayrılmış örnek kullanım Kullanım Özeti bölümünde de görebilirsiniz [EA portal](https://ea.azure.com).

>[!NOTE]
>Kullandıkça Öde fatura bağlamda ayrılmış örnek satın aldıysanız, bkz: [anlayın Kullandıkça Öde aboneliğiniz için örnek kullanım ayrılmış.](billing-understand-reserved-instance-usage.md)

İçin aşağıdaki bölümde, Doğu ABD bölgesi ve aşağıdaki tabloda, ayrılmış örnek bilgi görülüyor Standard_D1_v2 Windows VM çalıştığını varsayın:

| Alan | Değer |
|---| --- |
|Reservationıd |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Miktar |1|
|SKU | Standard_D1|
|Bölge | eastus |

## <a name="reserved-instance-application"></a>Ayrılmış örnek uygulama

Dağıtılan VM ayrılmış örnek öznitelikleri eşleştiğinden VM donanım bölümünü ele alınmıştır. Hangi Windows yazılım tarafından ayrılmış örnek kapsamında değil görmek için Azure ayrılmış VM örnekleri yazılım maliyetlerinin gidin, Git [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md)


### <a name="reserved-instance-usage-in-csv"></a>Ayrılmış örnek kullanım csv
EA kullanım csv EA Portalı'ndan yükleyebilirsiniz. Hakkında ek bilgiler indirilen csv dosyasında filtre ve yazın, **Reservationıd**. Aşağıdaki ekran görüntüsü, ayrılmış örnek ilgili alanları gösterir:

![Azure ayrılmış örnek için Kurumsal Anlaşma (Kurumsal Sözleşme) csv](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **Reservationıd** ek bilgilerini avantajı VM uygulamak için kullanılan ayrılmış örnek alanını temsil eder.
2. ConsumptionMeter MeterId VM için ' dir.
3. VM çalıştıran maliyetini zaten tarafından ayrılmış örnek Ücretli beri $0 maliyeti ile ayırma ölçer budur. 
4. Standard_D1 olan bir vCPU VM ve VM Azure karma avantajı dağıtılır. Bu nedenle, bu ölçüm, Windows yazılım ekstra ücret ele alınmaktadır. Bkz: [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md) 1 çekirdek VM için D serisinin karşılık gelen ölçer bulmak için. Azure karma avantajı kullanılırsa, bu ekstra ücret uygulanmaz.

### <a name="reserved-instance-usage-in-usage-summary-page-in-ea-portal"></a>Ayrılmış örnek kullanım EA Portalı'ndaki Özet sayfasında kullanımı

Ayrılmış örnek kullanım ayrıca görüntülenir Kullanım Özeti EA portalı bölümünde: ![Kurumsal Anlaşma (Kurumsal Sözleşme) Kullanım Özeti](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Ayrılmış örnek tarafından kapsanan olarak VM donanım bileşeni için ücretlendirilirsiniz değil. 
2. Azure karma avantajı kullanılmaz gibi Windows yazılım için sizden ücret kesilir. 

## <a name="next-steps"></a>Sonraki adımlar
Azure ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure'da ayrılmış örnekler yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.