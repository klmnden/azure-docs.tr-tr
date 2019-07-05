---
title: Gen2'den Gen3, Azure SQL veritabanı için ağ geçidi geçişi bildirimi | Microsoft Docs
description: Makale, Azure SQL veritabanı ağ geçidi IP adresi geçişi hakkında kullanıcılara bildirim sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
manager: craigg
ms.date: 07/01/2019
ms.openlocfilehash: 5894579c62c524394c7fea044b96885f7c8e8204
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538386"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Yeni ağ geçitleri için Azure SQL veritabanı trafiği geçişi

Azure altyapı arttıkça, Microsoft Donanım en olası müşteri deneyimi sağlıyoruz emin olmak için düzenli aralıklarla yenileyin. Önümüzdeki aylarda, biz ağ geçitleri üzerinde daha yeni donanım Nesilleri yerleşik eklemeyi planlıyorsanız ve eski donanım bazı bölgelerde temel alan ağ geçitleri yetkisini alma.  

Müşteriler, e-posta yoluyla ve her bölgede kullanılabilir ağ geçitleri için herhangi bir değişiklik öncesinde de Azure portalında bildirilir. En güncel bilgileri içinde tutulan [Azure SQL veritabanı ağ geçidi IP adreslerini](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) tablo.

## <a name="impact-of-this-change"></a>Bu değişikliğin etkisini

Ağ geçidi kullanımdan kaldırma, ilk yuvarlak şu bölgelerde 1 Eylül 2019 için zamanlandı:

- Batı ABD
- Batı Avrupa
- East US
- Orta ABD
- Güneydoğu Asya
- Orta Güney ABD
- Kuzey Avrupa
- Orta Kuzey ABD
- Japonya Batı
- Japonya Doğu
- Doğu ABD 2
- Doğu Asya

Kullanımdan IP adresi trafiği kabul etmesini durdurur ve yeni bağlantı girişimlerini bölgede ağ geçitleri birine yönlendirilir.

Burada bu değişikliğin etkisini göremezsiniz:

- Kendi bağlantı İlkesi herhangi bir etki görmez olarak yeniden yönlendirme kullanan müşteriler.
- SQL veritabanı bağlantısı gelen içinde Azure ve hizmet etiketleri kullanarak etkilenmiş olmaz.
- SQL Server için JDBC sürücüsü'nın desteklenen sürümleri kullanılarak yapılan bağlantı, herhangi bir etkisi görürsünüz. Desteklenen JDBC sürümleri için bkz: [SQL Server için Microsoft JDBC sürücüsü indirme](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Etkilenen yapmanız gerekenler yapın

Öneririz, IP adreslerine giden trafik için izin [Azure SQL veritabanı ağ geçidi IP adreslerini](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) bölgesinde TCP 1433 numaralı bağlantı noktası ve bağlantı noktası aralığı 11000-11999 güvenlik duvarı cihazınızdaki. Bağlantı noktası aralıkları hakkında daha fazla bilgi için bkz. [bağlantı İlkesi](sql-database-connectivity-architecture.md#connection-policy).

Bağlantıları sürüm 4.0 Microsoft JDBC sürücüsü kullanarak uygulamalar tarafından yapılan sertifika doğrulama başarısız olabilir. Microsoft JDBC alt sürümleri üzerinde ortak ad (CN) sertifikanın konu alanında kullanır. Risk azaltma hostNameInCertificate özelliği ayarlamak sağlandığından emin olmaktır *. database.windows.net. HostNameInCertificate özelliğinin nasıl ayarlandığını hakkında daha fazla bilgi için bkz. [SSL şifrelemesi ile bağlanma](/sql/connect/jdbc/connecting-with-ssl-encryption).

Yukarıdaki azaltma işe yaramazsa, aşağıdaki URL'yi kullanarak SQL veritabanı için bir destek isteği dosya: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinin [Azure SQL bağlantı mimarisi](sql-database-connectivity-architecture.md)