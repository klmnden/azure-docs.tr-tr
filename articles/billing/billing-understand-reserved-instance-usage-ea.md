---
title: Kuruluş için Azure ayırmaları kullanımını anlama | Microsoft Docs
description: Kurumsal kayıt için Azure ayırma nasıl uygulanacağını anlamak için kullanım okumayı öğrenin.
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
ms.date: 03/13/2019
ms.author: banders
ms.openlocfilehash: daa7f6a116578fa8d1f2b5bf825a6f4cd48f7f64
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648308"
---
# <a name="understand-azure-reservation-usage-for-your-enterprise-enrollment"></a>Kurumsal kayıt için Azure ayırma kullanımını anlama

Kullanım **Reservationıd** gelen [ayırmaları sayfa](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ve kullanım dosyasından [EA portal](https://ea.azure.com) ayırma kullanımınızı değerlendirilecek. Kullanımı Özet bölümünde ayırma kullanım atabilirsiniz [EA portal](https://ea.azure.com).

Bir Kullandıkça Öde faturalandırma bağlamında rezervasyon satın aldıysanız, bkz. [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlayın.](billing-understand-reserved-instance-usage.md)

## <a name="usage-for-reserved-virtual-machines-instances"></a>Kullanım için ayrılmış sanal makine örnekleri

İçin aşağıdaki bölümlerde, Doğu ABD bölgesinde ve aşağıdaki tabloda, ayırma bilgileri görünür işler için standart_d1_v2 Windows VM çalıştığını varsayalım:

| Alan | Değer |
|---| --- |
|Reservationıd |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Miktar |1|
|SKU | Standard_D1|
|Bölge | eastus |

Dağıtılan VM ayırma öznitelikleri ile eşleştiği için sanal makinenin donanım bölümü ele alınmıştır. Hangi Windows yazılım rezervasyon kapsamında değildir görmek için bkz [Azure ayrılmış VM örnekleri Windows yazılım maliyetleri](billing-reserved-instance-windows-software-costs.md).

### <a name="usage-in-csv-file-for-reserved-vm-instances"></a>Ayrılmış VM örnekleri için CSV dosyası kullanımı

Enterprise Portal'da Kurumsal kullanım CSV dosyası indirebilirsiniz. CSV dosyasında filtre **ek bilgi** ve yazın, **Reservationıd**. Aşağıdaki ekran görüntüsünde ayırmaya ilgili alanları gösterir:

![Azure ayırma için Kurumsal Anlaşma (EA) csv](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **Reservationıd** içinde **ek bilgi** alanını temsil eden sanal Makineye uygulanan ayırma.
2. **ConsumptionMeter** VM için ölçüm kimliğidir.
3. **Ölçüm kimliği** 0 ABD Doları maliyetle ayırma ölçer. Çalışan VM maliyeti, ayrılmış VM örneği tarafından ödenir.
4. Olan bir vCPU işler için standart_d1 VM ve VM Azure hibrit avantajı dağıtılır. Bu nedenle bu ölçüm, Windows yazılım başka bir ücret kapsar. 1 çekirdek VM için D serisi karşılık gelen bir ölçüm bulmak için bkz: [Azure ayrılmış VM örnekleri Windows yazılım maliyetleri](billing-reserved-instance-windows-software-costs.md).  Azure hibrit avantajı varsa, bu ek ücret uygulanmaz.

## <a name="usage-for-sql-database--cosmos-db-reserved-capacity-reservations"></a>Kapasite kullanımı SQL veritabanı ve Cosmos DB için ayrılmış

Aşağıdaki bölümlerde, kullanım raporu açıklamak için örnek olarak Azure SQL veritabanı kullanın. Azure Cosmos DB için de kullanımını almak için aynı adımları kullanabilirsiniz.

Bir SQL veritabanı Gen 4 Aşağıdaki tabloda, ayırma bilgileri görünür, Doğu ABD bölgesinde çalıştığını varsayalım:

| Alan | Değer |
|---| --- |
|Reservationıd |8244e673-83e9-45ad-b54b-3f5295d37cae|
|Miktar |2|
|Ürün| SQL veritabanı 4. nesil (2 Çekirdek)|
|Bölge | eastus |

### <a name="usage-in-csv-file"></a>CSV dosyası kullanımı

Filtre **ek bilgi** ve yazın, **rezervasyon kimliği**ve gerekli **ölçüm kategorisi** -Azure SQL veritabanı veya Azure Cosmos DB. Aşağıdaki ekran görüntüsünde ayırmaya ilgili alanları gösterir.

![SQL veritabanı için Kurumsal Anlaşma (EA) csv ayrılmış kapasite](./media/billing-understand-reserved-instance-usage-ea/billing-ea-sql-db-reserved-capacity-csv.png)

1. **Reservationıd** içinde **ek bilgi** SQL veritabanı kaynağı uygulanan ayırma bir alandır.
2. **ConsumptionMeter** SQL veritabanı kaynak için ölçüm kimliği.
3. **Ölçüm kimliği** 0 ABD Doları maliyetle ayırma ölçer. Ayırma için uygun bir SQL veritabanı kaynak bu ölçüm kimliği CSV dosyasında gösterir.

## <a name="usage-summary-page-in-enterprise-portal"></a>Enterprise Portal'da kullanım Özet sayfası

Azure ayırma kullanımınızı da Enterprise portal'ın kullanımı Özeti bölümü gösterilir: ![Kurumsal Anlaşma (EA) Kullanım Özeti](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Rezervasyon anlatılan sanal makinenin donanım bileşeni için ücretlendirilmezsiniz. Bir SQL veritabanı ayırma için gördüğünüz bir çizgiyle **hizmet adı** kapasite kullanımı Azure SQL veritabanı ayrılmış.
2. Bu örnekte, sanal makine ile kullanılan Windows yazılım için ücret ödersiniz için Azure hibrit teklifi yok.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).


## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)

