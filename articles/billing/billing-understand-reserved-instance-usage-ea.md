---
title: Kuruluş için Azure ayrılmış örnek kullanımını anlamak | Microsoft Docs
description: Ayrılmış örnek uygulaması, işletme kaydı için anlamak için kullanımınızı okuma öğrenin.
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
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: cf79926e6497c50156f2a0191997ca06bc605c16
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="understand--reserved-instance-usage-for-your-enterprise-enrollment"></a>İşletme kaydı için ayrılmış örnek kullanım anlama
Ayrılmış örnek kullanımını gelen Reservationıd kullanarak anlayın [ayırma sayfası](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) ve kullanım dosyasından [EA portal.](https://ea.azure.com) Kullanım Özeti bölümünde ayırma kullanım de görebilirsiniz [EA portal.](https://ea.azure.com)

>[!NOTE]
>Kullandıkça Öde fatura bağlamda ayırma satın aldıysanız, bkz: [anlamak ayrılmış örnek kullanım Kullandıkça Öde aboneliğiniz için.](billing-understand-reserved-instance-usage.md)

İçin aşağıdaki bölümde, Doğu ABD bölgesi ve aşağıdaki tabloda, ayırma bilgileri görülüyor Standard_D1_v2 Windows VM çalıştığını varsayın:

| Alan | Değer |
|---| --- |
|Reservationıd |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Miktar |1|
|SKU | Standard_D1|
|Bölge | eastus |

## <a name="reservation-application"></a>Ayırma uygulama

Dağıtılan VM ayırma öznitelikleri eşleştiğinden VM donanım bölümünü ele alınmıştır. Hangi Windows yazılım tarafından ayrılmış örnek kapsamında değil görmek için Azure ayrılmış VM örnekleri yazılım maliyetlerinin gidin, Git [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md)


### <a name="reservation-usage-in-csv"></a>Csv ayırma kullanımı
EA kullanım csv EA Portalı'ndan yükleyebilirsiniz. Hakkında ek bilgiler indirilen csv dosyasında filtre ve yazın kimliğinizi ayırma Aşağıdaki ekran görüntüsü ayırmaya ilgili alanları gösterir:

![Ayrılmış örnek için EA csv](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. Ek bilgi alanında Reservationıd avantajı VM uygulamak için kullanılan ayırma temsil eder.
2. ConsumptionMeter MeterId VM için ' dir.
3. VM çalıştıran maliyetini ayırma tarafından zaten Ücretli beri $0 maliyeti ile ReservationMeter budur. 
4. Standard_D1 olan bir vCPU VM ve VM Azure karma avantajı dağıtılır. Bu nedenle, bu ölçüm, Windows yazılım ekstra ücret ele alınmaktadır. Bkz: [Azure ayırma VM örnekleri Windows yazılım maliyetleri.](billing-reserved-instance-windows-software-costs.md) 1 çekirdek VM için D serisinin karşılık gelen ölçer bulmak için. Azure karma avantajı kullanılırsa, bu ekstra ücret uygulanmaz.

### <a name="reservation-usage-in-usage-summary-page-in-ea-portal"></a>EA Portalı'ndaki Kullanım Özeti sayfasında ayırma kullanımı

Ayrılmış kullanım ayrıca görüntülenir Kullanım Özeti EA portalı bölümünde örneği: ![EA Kullanım Özeti](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Ayrılmış örnek tarafından kapsanan olarak VM donanım bileşeni için ücretlendirilirsiniz değil. 
2. Azure karma avantajı kullanılmaz gibi Windows yazılım için sizden ücret kesilir. 

## <a name="next-steps"></a>Sonraki adımlar
Ayrılmış sanal makine örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [Ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış sanal makine örneklerini yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış sanal makine örnekleri sahip sanal makinelerde paradan tasarruf](billing-save-compute-costs-reservations.md)
- [Ayrılmış sanal makine örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.