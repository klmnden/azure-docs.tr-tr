---
title: PostgreSQL için Azure veritabanı'nda iş sürekliliğine genel bakış
description: PostgreSQL için Azure veritabanı'nda iş sürekliliğine genel bakış.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: b1d566ac571ddd2b2be3aff160f669e277887209
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61456283"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda iş sürekliliğine genel bakış

Bu genel bakışta, PostgreSQL için Azure veritabanı iş sürekliliği ve olağanüstü durum kurtarma sağlayan özellikleri açıklar. Veri kaybına neden veya veritabanı ve uygulama kullanılamaz hale gelmesine neden olaylardan kurtarmak için seçenekler hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkileyen, bir Azure bölgesinde kesinti yaşandığında veya uygulamanız zaman bakıma gerek duyacağını ne yapılacağını öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikleri

PostgreSQL için Azure veritabanı, otomatik yedeklemeler ve kullanıcıların coğrafi geri yükleme başlatmak için iş sürekliliği özellikleri sunar. Tahmini kurtarma süresi (ERT) ve olası veri kaybı her farklı özelliklere sahiptir. Bu seçenekleri kavradıktan sonra bunlar arasında seçin ve bunları birlikte farklı senaryolar için kullanın. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır.-Bu, Kurtarma süresi hedefi (RTO) önce kabul edilebilen maksimum süre anlamanız gerekir. Ayrıca en son veri miktarını anlamanıza gerek güncelleştirmelerinin (zaman aralığı) uygulama edilebilecek kesintiden sonra kurtarılırken - Bu, kurtarma noktası hedefi (RPO).

Aşağıdaki tabloda kullanılabilir özellikleri için ERT ve RPO değerleri karşılaştırılmaktadır:

| **Özelliği** | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası |
| Coğrafi çoğaltmalı yedeklerden coğrafi geri yükleme | Desteklenmiyor | ERT < 12 sa.<br/>RPO < 1 saat | ERT < 12 sa.<br/>RPO < 1 saat |

> [!IMPORTANT]
> Silinen sunucuları **olamaz** geri yüklenemiyor. Sunucu silerseniz sunucusuna ait tüm veritabanlarını da silinir ve kurtarılamaz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Bir kullanıcı veya uygulama hatasından sonra bir sunucusunu kurtarma

Hizmetin yedeklemeler çeşitli kesintilerden bir sunucuya kurtarmak için kullanabilirsiniz. Bir kullanıcı yanlışlıkla veri silebilir, istemeden önemli bir tabloyu bırakın veya hatta bir veritabanının tamamını bırakabilir. Bir uygulama yanlışlıkla bir uygulama, hata nedeniyle hatalı verilerle iyi verilerin üzerine ve benzeri.

Bir nokta,-zaman-zamanında bilinen iyi bir noktaya sunucunuza bir kopyasını oluşturmak için geri yükleme gerçekleştirebilirsiniz. Bu noktada, sunucunuz için yapılandırdığınız yedekleme Bekletme dönemi içinde olmalıdır. Verileri yeni sunucuya geri yüklendikten sonra özgün sunucunun yeni geri yüklenen sunucu ile değiştirin veya gerekli verileri özgün sunucuya geri yüklenen sunucudan kopyalayabilirsiniz.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Bir Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti oluştuğunda, yalnızca birkaç dakika sürebilecek, ancak son saat için iş kesintisi neden olur.

Sunucunuz, veri merkezi kesintisi sona erdiğinde tekrar çevrimiçi duruma gelmesini bekleyin bir seçenektir. Bu zaman, örneğin bir geliştirme ortamı belirli bir süre için çevrimdışı olması kabul edilebildiği uygulamalar için çalışır. Veri merkezinde bir kesinti varsa sunucunuza bir süredir ihtiyacınız yoksa bu seçenek yalnızca çalışır böylece, kesinti ne kadar sürebilecek, bilmezsiniz.

Diğer seçenek, coğrafi olarak yedekli yedeklemeler kullanarak sunucuya geri yükleyen PostgreSQL'ın coğrafi geri yükleme özelliği için Azure veritabanı'nı kullanmaktır. Sunucunuz barındırılan bölgeyi çevrimdışı olsa bile bu yedeklemeler erişilebilir olur. Bu yedeklemeleri geri yüklemek için başka bir bölgede ve sunucunuzun çevrimiçi duruma getirin.

> [!IMPORTANT]
> Sunucu yedekleme coğrafi olarak yedekli depolama ile sağladıysanız coğrafi geri yükleme yalnızca mümkündür. Mevcut bir sunucu için coğrafi olarak yedekli yedeklemeleri yerel olarak yedekli geçiş yapmak istiyorsanız, mevcut sunucunuzun pg_dump kullanarak bir döküm almak ve coğrafi olarak yedekli yedeklemelerde yapılandırılmış yeni oluşturulan geri gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [otomatik PostgreSQL için Azure veritabanı'nda yedeklemeler](concepts-backup.md). 
- Kullanarak geri yüklemeyi öğreneceksiniz [Azure portalında](howto-restore-server-portal.md) veya [Azure CLI'yı](howto-restore-server-cli.md).
- Hakkında bilgi edinin [PostgreSQL için Azure veritabanı'nda çoğaltmaları okuma](concepts-read-replicas.md).