---
title: "Kullandıkça Öde aboneliğiniz için Azure ayrılmış örnek kullanımını anlamak | Microsoft Docs"
description: "Ayrılmış örnek anlayın uygulamaya Kullandıkça Öde aboneliğine kullanımınızı okuma öğrenin"
services: billing
documentationcenter: 
author: manish-shukla01
manager: manshuk
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: 29f153803d5eb74e2d287d97cf9436e81b2a3e20
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak

Ayrılmış örnek kullanımını gelen Reservationıd kullanarak anlayın [ayırma sayfası](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) ve kullanım dosyasından [Azure hesapları portal.](https://account.azure.com)


>[!NOTE]
>Bu makalede EA müşteriler için geçerli değildir. EA müşteri olup olmadığını görmek [olduğunuz Kurumsal kayıt için ayrılmış örnek anlamak kullanımı.](billing-understand-reserved-instance-usage-ea.md) Bu makale, ayrıca ayırma tek bir abonelik için geçerli olduğunu varsayar. Ayırma birden fazla aboneliğine uyguladıysanız ayırma avantajı birden çok kullanım csv dosyalarını yayılabilir. 

İçin aşağıdaki bölümde, Doğu ABD bölgesi ve aşağıdaki tabloda, ayırma bilgileri görülüyor Standard_DS1_v2 Windows VM çalıştığını varsayın:

| Alan | Değer |
|---| :---: |
|Reservationıd |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Miktar |1|
|SKU | Standard_DS1_v2|
|Bölge | eastus |

## <a name="reservation-application"></a>Ayırma uygulama

Dağıtılan VM ayırma öznitelikleri eşleştiğinden VM donanım bölümünü ele alınmıştır. Hangi Windows yazılım tarafından ayrılmış örnek kapsamında değil görmek için Git [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Csv deyimi bölümü
Bu bölümde, csv, ayırma için toplam kullanım gösterir. "Ayırma-" ve verilerinizin aşağıdaki ekran görüntüsüne benzer içeren ölçer alt kategori alanın filtre uygulamak: ![doğrudan deyimi ayırma](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Ayırma temel VM satırı Rezervasyon tarafından kapsanan toplam süreyi içeriyor. Bu satırı $0,00 çünkü ayrılmış örnek kapsar. Ayırma Windows Svr (1 çekirdek) satır Windows yazılım maliyetini kapsar.

### <a name="daily-usage-section-of-csv"></a>Csv günlük kullanımı bölümü
Hakkında ek bilgiler filtre ve kimliğinizi ayırma yazın. Aşağıdaki ekran görüntüsünde ayırmaya ilgili alanları gösterir. 

![Günlük kullanım ücretleri](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. Ek bilgi alanında Reservationıd avantajı VM uygulamak için kullanılan ayırma ' dir.
2. ConsumptionMeter VM ölçer kimliğidir.
3. Ayırma tabanlı VM ölçer alt kategori satır deyimi bölüm $0 maliyet satırını temsil eder. Bu VM çalıştıran maliyetini zaten rezervasyon Ücretli.
4. Bu ayırma ölçer kimliğidir. Bu ölçüm maliyetini $0'dır. Ayrılmış örnek için niteleyen VM maliyeti hesap için csv bu MeterId sahiptir. 
5. Standard_DS1_v2 olan bir vCPU VM ve VM Azure karma avantajı dağıtılır. Bu nedenle, bu ölçüm, Windows yazılım ekstra ücret ele alınmaktadır. Bkz: [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md) 1 çekirdek VM için D serisinin karşılık gelen ölçer bulmak için. Azure karma avantajı kullanılırsa, bu ekstra ücret uygulanmaz. 

## <a name="next-steps"></a>Sonraki adımlar
Ayrılmış sanal makine örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [Ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış sanal makine örneklerini yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış sanal makine örnekleri sahip sanal makinelerde paradan tasarruf](billing-save-compute-costs-reservations.md)
- [Ayrılmış sanal makine örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.