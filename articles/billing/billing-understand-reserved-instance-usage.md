---
title: Kullandıkça Öde aboneliğiniz için Azure ayrılmış örnek kullanımını anlamak | Microsoft Docs
description: Kullandıkça Öde aboneliğiniz için Azure ayrılmış VM örneğine nasıl uygulandığını anlamak için kullanımınızı okuma öğrenin.
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
ms.openlocfilehash: 7e303f3e5ce0e618d941be4190f6fadb40f2e09d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064054"
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak

Reservationıd gelen kullanarak Azure ayrılmış VM örneği kullanımını anlamak [ayırma sayfası](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ve kullanım dosyasından [Azure hesapları portal](https://account.azure.com).


>[!NOTE]
>Bu makalede EA müşteriler için geçerli değildir. EA müşteri olup olmadığını görmek [olduğunuz Kurumsal kayıt için ayrılmış örnek anlamak kullanımı.](billing-understand-reserved-instance-usage-ea.md) Bu makalede ayrıca ayrılmış örnek için tek bir abonelik uygulandığını varsayar. Ayrılmış örnek için birden fazla abonelik uyguladıysanız, ayrılmış örnek avantajı birden çok kullanım csv dosyalarını yayılabilir. 

İçin aşağıdaki bölümde, Doğu ABD bölgesi ve aşağıdaki tabloda, ayrılmış örnek bilgi görülüyor Standard_DS1_v2 Windows VM çalıştığını varsayın:

| Alan | Değer |
|---| :---: |
|Reservationıd |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Miktar |1|
|SKU | Standard_DS1_v2|
|Bölge | eastus |

## <a name="reserved-instance-application"></a>Ayrılmış örnek uygulama

Dağıtılan VM ayrılmış örnek öznitelikleri eşleştiğinden VM donanım bölümünü ele alınmıştır. Hangi Windows yazılım tarafından ayrılmış örnek kapsamında değil görmek için Git [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Csv deyimi bölümü
Bu bölümde, csv, ayrılmış örnek için toplam kullanım gösterir. "Ayırma-" ve verilerinizin aşağıdaki ekran görüntüsüne benzer içeren ölçer alt kategorisi alanın filtre uygulamak: ![filtrelenmiş ayrılmış örnek kullanım ayrıntılarını ve ücretleri ekran görüntüsü](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Ayırma temel VM satırı ayrılmış örneği tarafından kapsanan toplam süreyi içeriyor. Bu satırı $0,00 çünkü ayrılmış örnek kapsar. Ayırma Windows Svr (1 çekirdek) satır Windows yazılım maliyetini kapsar.

### <a name="daily-usage-section-of-csv"></a>Csv günlük kullanımı bölümü
Filtre hakkında ek bilgiler ve yazın, **ayırma kimliği**. Aşağıdaki ekran görüntüsünde, ayrılmış örnek ilgili alanları gösterir. 

![Günlük kullanım ayrıntılarını ve ücretleri ekran görüntüsü](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **Reservationıd** ek bilgilerini avantajı VM uygulamak için kullanılan ayrılmış örnek bir alandır.
2. ConsumptionMeter VM ölçer kimliğidir.
3. Ayırma tabanlı VM ölçer alt satır deyimi bölüm $0 maliyet satırını temsil eder. Bu VM çalıştıran maliyetini zaten tarafından ayrılmış örnek Ücretli.
4. Bu ayrılmış örnek için ölçüm kimliğidir. Bu ölçüm maliyetini $0'dır. Ayrılmış örnek için niteleyen VM maliyeti hesap için csv bu MeterId sahiptir. 
5. Standard_DS1_v2 olan bir vCPU VM ve VM Azure karma avantajı dağıtılır. Bu nedenle, bu ölçüm, Windows yazılım ekstra ücret ele alınmaktadır. Bkz: [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md) 1 çekirdek VM için D serisinin karşılık gelen ölçer bulmak için. Azure karma avantajı kullanılırsa, bu ekstra ücret uygulanmaz. 

## <a name="next-steps"></a>Sonraki adımlar
Ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure'da ayrılmış örnekler yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
