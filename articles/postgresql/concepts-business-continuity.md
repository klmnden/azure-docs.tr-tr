---
title: "İş sürekliliği PostgreSQL için Azure veritabanıyla genel bakış"
description: "İş sürekliliği PostgreSQL için Azure veritabanıyla genel bakış."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 1b981b650d75556f4521aaf0f089443bb88d064a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql"></a>İş sürekliliği PostgreSQL için Azure veritabanıyla genel bakış

Bu genel bakışta Azure veritabanı PostgreSQL için iş devamlılığı ve olağanüstü durum kurtarma için sağlayan özellikleri açıklanmaktadır. Veri kaybına neden ya da veritabanı ve uygulamanızın kullanılamaz hale gelmesine neden kesintiye uğratan olaylarından kurtarma seçenekleri hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğü etkiler, bir Azure bölgesinin bir kesinti veya bakım uygulamanızın gerektirdiği olmadığında yapmanız gerekenler hakkında bilgi edinin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

Azure veritabanı PostgreSQL için otomatik yedeklemeler ve kullanıcılara coğrafi geri yükleme başlatmasını dahil iş sürekliliği özellikleri sağlar. Tahmini kurtarma süresi (Ekle) ve olası veri kaybı her farklı özelliklere sahip. Bu seçenekler anladığınızda, bunlar arasında seçin ve bunları farklı senaryolar için birlikte kullanın. İş sürekliliği planınızın geliştirdikçe kesintiye uğratan olayından sonra uygulama tamamen kurtarır - bu kurtarma süresi hedefi (RTO) önce maksimum kabul edilebilir süreyi anlamanız gerekir. Ayrıca en fazla son veri miktarı anlamak ihtiyacınız uygulama güncelleştirmeleri (zaman aralığı) tolerans kesintiye uğratan olayından sonra kurtarırken kaybetme - bu kurtarma noktası hedefi (RPO).

Aşağıdaki tabloda Ekle ve RPO için kullanılabilen özellikleri karşılaştırılır:

| **Özelliği** | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Saklama dönemi içinde herhangi bir geri yükleme noktası | Saklama dönemi içinde herhangi bir geri yükleme noktası | Saklama dönemi içinde herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılmış yedeklerden coğrafi geri yükleme | Desteklenmiyor | Ekle < 12 h<br/>RPO < 1 h | Ekle < 12 h<br/>RPO < 1 h |

> [!IMPORTANT]
> Sunucu silerseniz, sunucuya ait tüm veritabanlarının da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Bir sunucuyu kurtardıktan sonra bir kullanıcı veya uygulama hatası

Hizmetin yedeklemeler, bir sunucu çeşitli kesintiye uğratan olaylarından kurtarmak için kullanabilirsiniz. Bir kullanıcı yanlışlıkla bazı verileri silmek, yanlışlıkla önemli bir tablo bırakma veya bile tüm veritabanını bırakın. Bir uygulama yanlışlıkla iyi veri bir uygulama hatası nedeniyle hatalı verilerle üzerine ve benzeri.

Bir noktayı-içinde--zamanında bilinen iyi bir noktaya sunucunuza bir kopyasını oluşturmak için geri yükleme gerçekleştirebilirsiniz. Bu noktaya sunucunuz için yapılandırdığınız yedekleme saklama dönemi içinde olmalıdır. Verileri yeni bir sunucuya geri yüklendikten sonra özgün sunucunun yeni geri yüklenen sunucuyla değiştirebilir veya gerekli verileri geri yüklenen sunucudan özgün sunucuya kopyalayın.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Bir Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti oluştuğunda, yalnızca birkaç dakika son, ancak saat boyunca en son iş kesintiye neden olur.

Veri Merkezi kesintisinden üzerinden olduğunda tekrar çevrimiçi duruma sunucunuz için bir seçenek beklenir. Bu, belirli bir süre, örneğin bir geliştirme ortamı için sunucu çevrimdışı olmasını destekleyebilir uygulamalar için çalışır. Veri merkezi bir kesinti olduğunda, böylece bir süredir sunucunuz gerekmiyorsa, bu seçenek yalnızca çalışır, ne kadar süreyle kesinti son, bilmezsiniz.

Diğer seçeneği, coğrafi olarak yedekli yedeklemeler kullanarak sunucuya yükler PostgreSQL'ın coğrafi geri yükleme özelliği için Azure veritabanı kullanmaktır. Sunucunuz içinde barındırılan bölge çevrimdışı olduğunda bile bu yedeklemeler erişilebilir olur. Başka bir bölgeye bu yedeklerden geri yükleme ve sunucunuzu yeniden çevrimiçi duruma getirin.

> [!IMPORTANT]
> Coğrafi olarak yedekli yedekleme depolama sunucusuyla sağlanan, coğrafi geri yükleme yalnızca mümkündür.

## <a name="next-steps"></a>Sonraki adımlar
- Otomatik yedekleme hakkında daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanı yedeklemeleri](concepts-backup.md). 
- Azure portalını kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı Azure portalını kullanarak zaman içinde bir noktaya geri](howto-restore-server-portal.md).
- Azure CLI kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı CLI kullanarak zaman içinde bir noktaya geri](howto-restore-server-cli.md).