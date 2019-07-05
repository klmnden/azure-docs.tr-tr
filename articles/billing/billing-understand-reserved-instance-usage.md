---
title: Kullandıkça Öde tarifesine göre ile tek bir aboneliği Azure ayırma kullanımı
description: Azure ayırma için tek tek aboneliğinizi Kullandıkça Öde tarifesine göre nasıl uygulanacağını anlamak için kullanım okumayı öğrenin.
author: bandersmsft
manager: yashr
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 11f05c3de50f2f82173b6666d304887fbc2038cc
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490375"
---
# <a name="understand-azure-reservation-usage-for-your-individual-subscription-with-pay-as-you-go-rates-subscription"></a>Tek tek aboneliğinizin Kullandıkça Öde tarifesine göre aboneliğiyle Azure ayırma kullanımını anlama

Reservationıd gelen kullanın [ayırma sayfası](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ve kullanım dosyasından [Azure hesap portalını](https://account.azure.com) ayırma kullanımınızı değerlendirilecek.

Bir kurumsal anlaşma kapsamında olan bir Müşteriyseniz bkz [Kurumsal kayıt için ayırma kullanımını anlayın.](billing-understand-reserved-instance-usage-ea.md).

Bu makalede ayırma tek bir abonelik için geçerli olduğunu varsayar. Rezervasyon için birden fazla aboneliğiniz uygulanırsa, rezervasyon Avantajı'nızı birden çok kullanım CSV dosyalarını yayılabilir.

## <a name="usage-for-reserved-virtual-machine-instances"></a>Kullanım için ayrılmış sanal makine örnekleri

Aşağıdaki bölümlerde için Standard_DS1_v2 Windows VM, ayrılmış VM örneği bilgi aşağıdaki gibi görünür aşağıdaki tabloda, Doğu ABD bölgesinde çalıştığını varsayalım:

| Alan | Değer |
|---| :---: |
|Reservationıd |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Miktar |1|
|SKU | Standard_DS1_v2|
|Bölge | eastus |

Dağıtılan VM ayırma öznitelikleri ile eşleştiği için sanal makinenin donanım bölümü ele alınmıştır. Ayrılmış VM örneği tarafından hangi Windows yazılım kapsamında değildir görmek için bkz [Azure ayrılmış VM örnekleri Windows yazılım maliyetleri](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv-file-for-vms"></a>VM'ler için CSV dosyası bölümünü deyimi

CSV dosyanız bu bölümü, ayırma için toplam kullanım gösterir. Filtre uygulamak **ölçüm alt kategorisi** içeren alan **"Ayırma-"** . Aşağıdaki ekran görüntüsüne benzer bir şey görürsünüz:

![Filtrelenmiş ayırma kullanım ayrıntıları ve ücretleri ekran görüntüsü](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

**Ayırma-temel VM** ayırma tarafından kapsanan toplam süreyi satırı içeriyor. Bu satırı 0,00 ABD Doları çünkü ayırma kapsar. **Ayırma-Windows Svr (1 çekirdek)** satırı Windows yazılımının maliyetini kapsar.

### <a name="daily-usage-section-of-csv-file"></a>Günlük kullanım bölümüne CSV dosyası

Filtre **ek bilgi** ve yazın, **rezervasyon kimliği**. Aşağıdaki ekran görüntüsünde ayırmaya ilgili alanları gösterir.

![Günlük kullanım ayrıntıları ve ücretleri ekran görüntüsü](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **Reservationıd** içinde **ek bilgi** VM'ye uygulanan ayırma bir alandır.
2. **ConsumptionMeter** VM için ölçüm kimliğidir.
3. **Ayırma-temel VM** **ölçüm alt kategorisi** satır 0 ABD Doları maliyet deyimi bölümünde temsil eder. Bu VM çalıştırmanın maliyeti, rezervasyon zaten ödenir.
4. **Ölçüm kimliği** ayırma için ölçüm kimliği. Bu ölçüm maliyeti 0 TL'dir. Bu ölçüm kimliği için ayırma indirimi niteleyen herhangi bir VM için görünür.
5. Standard_DS1_v2 bir vCPU olan VM ve VM Azure karma avantajı olmadan dağıtılır. Bu nedenle, bu ölçüm, Windows yazılım başka bir ücret kapsar. D serisi 1 çekirdek VM karşılık gelen bir ölçüm bulmak için bkz: [Azure ayrılmış VM örnekleri Windows yazılım maliyetleri](billing-reserved-instance-windows-software-costs.md). Azure hibrit avantajı varsa, bu ek ücret uygulanmaz.

## <a name="usage-for-sql-database--cosmos-db-reservations"></a>SQL veritabanı ve Cosmos DB ayırmalar için kullanım

Aşağıdaki bölümlerde, kullanım raporu açıklamak için örnek olarak Azure SQL veritabanı kullanın. Azure Cosmos DB için de kullanımını almak için aynı adımları kullanabilirsiniz.

Bir SQL veritabanı Gen 4 Aşağıdaki tabloda, ayırma bilgileri görünür, Doğu ABD bölgesinde çalıştığını varsayalım:

| Alan | Değer |
|---| --- |
|Reservationıd |446ec809-423d-467c-8c5c-bbd5d22906b1|
|Miktar |2|
|Product| SQL veritabanı 4. nesil (2 Çekirdek)|
|Bölge | eastus |

### <a name="statement-section-of-csv-file"></a>CSV dosyasının deyimi bölümü

Filtre **ayrılmış örnek kullanımını** gerekli seçin ve ölçüm adı **ölçüm kategorisi** -Azure SQL veritabanı veya Azure Cosmos DB. Aşağıdaki ekran görüntüsüne benzer bir şey görürsünüz:

![SQL veritabanı ayrılmış kapasite için CSV dosyası](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-statements.png)

**Ayrılmış örnek kullanımını** çizgi bulunan Çekirdek saatleri ayırma tarafından kapsanan toplam sayısı. Rezervasyon maliyeti anlatıldığı gibi bu satırı 0 ABD Doları hızıdır.

### <a name="detail-section-of-csv-file"></a>CSV dosyasının ayrıntı bölümü

Filtre **ek bilgi** ve yazın, **rezervasyon kimliği**. Aşağıdaki ekran görüntüsünde, SQL veritabanı ayrılmış kapasite ayırma için ilgili alanları gösterir.

![SQL veritabanı ayrılmış kapasite için CSV dosyası](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-details.png)

1. **Reservationıd** içinde **ek bilgi** SQL veritabanı kaynağı uygulanan SQL veritabanı ayrılmış kapasite ayırma bir alandır.
2. **ConsumptionMeter** SQL veritabanı kaynak için ölçüm kimliği.
3. **Ölçüm kimliği** ayırma ölçer. Bu ölçüm maliyeti 0 TL'dir. Ayırma indirimini Bu ölçüm kimliği CSV dosyasında gösterir. uygun bir SQL veritabanı kaynaklar.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
