---
title: Ayırma indirimleri Azure SQL veri ambarı'na nasıl geçerli | Microsoft Docs
description: Nasıl paradan yardımcı olmak için Azure SQL veri ambarı ayırma indirimleri geçerli öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 10e19377d31489cd19465fe6171ffb530bd58c28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60918435"
---
# <a name="how-reservation-discounts-apply-to-azure-sql-data-warehouse"></a>Ayırma indirimleri Azure SQL veri ambarı'na nasıl uygulanır?

Azure SQL veri ambarı ayrılmış kapasite satın sonra ayırma indirimini ilgili bölgede mevcut veri ambarları için otomatik olarak uygulanır. Ayırma indirimi, SQL veri ambarı cDWU ölçüm gösterilen kullanım için geçerlidir. Depolama ve ağ Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

## <a name="reservation-discount-application"></a>Ayırma indirimi

Ambar saat bazında çalışan SQL veri ambarı ayrılmış kapasite indirim uygulanır. Daha sonra bir saat için dağıtılan bir ambarı yoksa, ayrılmış kapasite için bu saat boşa harcanmış olur. Bunu üzeri aktarılmaz.

Satın almanızdan sonra zaman içinde herhangi bir noktada ambarları çalıştırarak gösterilen SQL veri ambarı kullanım için satın aldığınız ayırma eşleştirilir. Bazı ambarları kapatırsanız ayırma indirimleri otomatik olarak diğer eşleşen ambarlar için geçerlidir.

Tam bir saat için çalıştırma ambarlar için ayırma eşleşen diğer örneklerine o saat içinde otomatik olarak uygulanır.

## <a name="discount-examples"></a>Örnekler indirim

Aşağıdaki örnekler, SQL veri ambarı ayrılmış kapasite indirim, dağıtımları bağlı olarak uygulanması hakkında gösterir.

- **Örnek 1**: 5 100 cDWU ayrılmış kapasite birimleri satın. Sizin için bir saat DW1500c SQL veri ambarı örneği çalıştırın. Bu durumda, kullanım için 15 birimleri 100 cDWU kullanım yayılır. Ayırma indirimi, kullanılan 5 birimi için geçerlidir. Kullandıkça Öde tarifesine göre kullandığınız 100 cDWU kullanım için kalan 10 birim kullanarak ücretlendirilir.

- **Örnek 2**: 5 100 cDWU ayrılmış kapasite birimleri satın. Bir saat için iki DW100c SQL veri ambarı örneği çalıştırın. Bu durumda, iki kullanım olaylar 100 cDWU kullanımı 1 birim için gönderilir. Her iki kullanım olayları, ayrılmış kapasite indirim alın. Kalan 3 adet 100 cDWU ayrılmış kapasite boşa ve gelecekte kullanılmak üzere taşıyan yok.

- **Örnek 3**: 100 cDWU ayrılmış kapasitesinin 1 birim satın. İki DW100c SQL veri ambarı örneği çalıştırın. Her 30 dakika boyunca çalışır. Bu durumda, her iki kullanım olayları ayrılmış kapasite indirim alın. Kullanım kullanarak Kullandıkça Öde tarifesine göre ücretlendirilir.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

- Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Ayırma işlemleri görüntüle](billing-view-reservations.md)
- [İşlemleri ayırma ve kullanımı API aracılığıyla alın](billing-reservation-apis.md)
- [Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
