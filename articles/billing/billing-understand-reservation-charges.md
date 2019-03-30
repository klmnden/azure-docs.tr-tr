---
title: Azure SQL veritabanları için ayırmaları indirimi anlama | Microsoft Docs
description: Ayırma indirimi çalışan Azure SQL veritabanlarını nasıl uygulanacağını öğrenin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: banders
ms.openlocfilehash: aa4fc43efab8c168fd5351ec60def7a3d0eefada
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649450"
---
# <a name="understand-how-a-reservation-discount-is-applied-to-azure-sql-databases"></a>Ayırma indirimi Azure SQL veritabanına nasıl uygulanacağını anlama

Ayırma indirimi, SQL veritabanları için öznitelikler ve ayırma miktarı ile eşleşen, otomatik olarak bir Azure SQL veritabanı'nın ayrılmış kapasite satın sonra uygulanır. Rezervasyon SQL veritabanınızın işlem maliyetlerini kapsar. Yazılım, depolama ve ağ için ücretlendirilirsiniz normal fiyatlar. SQL veritabanınız için lisanslama maliyetlerini karşılamak [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

Ayrılmış sanal makine örnekleri için bkz. [anlamak Azure ayrılmış VM örnekleri indirim](billing-understand-vm-reservation-charges.md).

## <a name="reservation-discount-applied-to-sql-databases"></a>SQL veritabanları için uygulanan ayırma indirimi

 SQL veritabanları saatlik olarak çalıştırmak için SQL veritabanı ayrılmış kapasite indirim uygulanır. Satın aldığınız ayırma çalışan SQL veritabanı tarafından yayılan işlem kullanımı eşleştirilir. Saatin tamamı boyunca çalışmayan SQL Veritabanları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer SQL veritabanlarına otomatik olarak uygulanır. İndirim, SQL veritabanları için aynı anda çalışan uygulayabilirsiniz. SQL tam bir saat olarak çalışan ayırma öznitelikleri eşleşen veritabanları yoksa, ayırma indirimini bu saat için tüm avantajlarını elde etmezsiniz.

Aşağıdaki örnekler nasıl sayısı bağlı olarak SQL veritabanı ayrılmış kapasite indirim uygulanır çekirdek satın aldığınız ve bunların ne zaman çalıştırdığınızdan.

- Senaryo 1: Bir 8 çekirdek SQL veritabanı için SQL veritabanı ayrılmış kapasite satın. Bir 16 çekirdek ayırma özniteliklerini geri kalanı ile eşleşen bir SQL veritabanı çalıştırın. Kullandıkça Öde fiyatı 8 çekirdek SQL veritabanı işlem kullanımı için ücret ödersiniz. Ayırma indirimi 8 çekirdek SQL veritabanı işlem kullanımı bir saatlik sahip olursunuz.

Bu örneklerin geri kalanında, satın aldığınız SQL veritabanı ayrılmış kapasite 16 çekirdek bir SQL veritabanı olduğunu varsayar ve ayırma öznitelikleri geri kalanını çalışan SQL veritabanları aynı.

- Senaryo 2: 8 çekirdek ile her bir saat için iki SQL veritabanı çalıştırın. 16 çekirdek ayırma indirimi, SQL veritabanlarının kullanım için her iki 8 çekirdek işlem uygulanır.
- Senaryo 3: Bir çalıştırma 16 çekirdek SQL veritabanı 13'te 1:30 pm için. 1:30 pm 2 başka bir 16 çekirdek SQL veritabanı çalıştırın. Her ikisi de, ayırma indirimini tarafından ele alınmaktadır.
- Senaryo 4: Bir çalıştırma 16 çekirdek SQL veritabanı 13'te 1:45 pm için. 1:30 pm 2 başka bir 16 çekirdek SQL veritabanı çalıştırın. 15 dakikalık çakışma Kullandıkça Öde fiyatı Ücret ödersiniz. Ayırma indirimi işlem kullanımı saat geri kalanı için uygulanır.

Anlamak ve kullanım raporları faturalama Azure Ayırmalarınızın uygulamayı görüntülemek için bkz: [anlamak Azure ayırma kullanım](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için ayırma kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
