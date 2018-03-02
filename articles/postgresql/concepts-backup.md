---
title: "Yedekleme ve geri yükleme Azure veritabanındaki PostgreSQL için"
description: "Otomatik yedekleme ve Azure veritabanınız PostgreSQL sunucu için geri yükleme hakkında bilgi edinin."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0f7ec38d2c271ebaa15e681a71eb32be7151921f
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="backup-and-restore-in-azure-database-for-postgresql"></a>Yedekleme ve geri yükleme Azure veritabanındaki PostgreSQL için

Azure veritabanı PostgreSQL için otomatik olarak sunucu yedeklemelerini oluşturur ve bunları yapılandırılan kullanıcı yerel olarak yedekli veya coğrafi olarak yedekli depolama alanında depolar. Yedeklemeleri sunucunuz bir nokta zaman için geri yüklemek için kullanılabilir. Verilerinizin yanlışlıkla Bozulması veya silinmesi korunmasına olduğundan yedekleme ve geri yükleme tüm iş sürekliliği stratejinizin önemli bir parçası değil.

## <a name="backups"></a>Yedeklemeler

Azure veritabanı PostgreSQL için tam, fark ve işlem günlüğü yedeklemeleri alır. Bu yedeklemeler bir sunucuya yapılandırılan yedekleme saklama süresi içinde tüm noktası zaman için geri yüklemenize olanak sağlar. Varsayılan yedekleme bekletme süresi yedi gündür. İsteğe bağlı olarak yukarı 35 gün olarak yapılandırabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

### <a name="backup-frequency"></a>Yedekleme sıklığı

Genellikle, tam yedekleme haftalık olarak ortaya yedekleri günde iki kez oluşur ve işlem günlüğü yedeklemeleri beş dakikada bir gerçekleşir. Bir sunucu hemen oluşturulduktan sonra ilk tam yedeklemede zamanlanır. İlk yedekleme büyük geri yüklenen sunucuda daha uzun sürebilir. Yeni bir sunucu geri yüklenebilir zaman içinde erken bir nokta ilk tam yedekleme tam olduğu zamanı gelmiştir.

### <a name="backup-redundancy-options"></a>Yedekleme artıklık seçenekleri

Azure veritabanı PostgreSQL için genel amaçlı ve bellek için iyileştirilmiş katmanlarında yedekleme yerel olarak yedekli veya coğrafi olarak yedekli depolama arasında seçim yapma esnekliği sağlar. Yedeklemeler, coğrafi olarak yedekli yedekleme deposunda depolanır, bunlar yalnızca, sunucunuzun barındırılan, ancak aynı zamanda çoğaltılır bölge içinde depolanmaz bir [eşleştirilmiş veri merkezi](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Bu, daha iyi koruma ve sunucunuz farklı bir bölgede bir olağanüstü durumda geri yükleme yeteneği sağlar. Temel katman yalnızca yerel olarak yedekli yedekleme depolama sunar.

> [!IMPORTANT]
> Yerel olarak yapılandırma artık ya da coğrafi olarak yedekli depolama, yedekleme sırasında sunucusu yalnızca izin için oluşturun. Sunucu sağlandıktan sonra yedekleme depolama artıklığı seçeneği değiştirilemiyor.

### <a name="backup-storage-cost"></a>Yedekleme depolama maliyeti

Azure veritabanı PostgreSQL için % 100 sağlanan sunucu deponuzda hiçbir ek ücret ödemeden yedekleme depolama sağlar. Genellikle, bu yedekleme bir yedi gün bekletme için uygundur. Kullanılan tüm ek yedekleme depolama GB ayında ücretlendirilir.

Örneğin, 250 GB Kapasiteli bir sunucuyla sağladıysanız, ek ücret ödemeden 250 GB yedekleme depolama sahip. 250 GB aşan depolama ücretlendirilir.

## <a name="restore"></a>Geri Yükleme

PostgreSQL için Azure veritabanı'nda bir geri yükleme gerçekleştirilmeden yeni bir sunucu özgün sunucunun yedeklemelerden oluşturur.

Geri yükleme iki tür vardır:

- **Zaman içinde nokta geri yükleme** ya da yedekleme artıklığı seçeneği ile kullanılabilir ve özgün sunucunuz ile aynı bölgede yeni bir sunucu oluşturur.
- **Coğrafi geri yükleme** kullanılabilir yalnızca coğrafi olarak yedekli depolama sunucunuz yapılandırılmış ve sunucunuz farklı bir bölgeye geri yüklemenize olanak sağlar.

Tahmini süre, Kurtarma veritabanı boyutları, işlem günlüğü boyutu, ağ bant genişliğini ve aynı anda aynı bölgede kurtarma veritabanı toplam sayısı dahil olmak üzere birçok faktöre bağlıdır. Kurtarma süresi genellikle değerinden 12 saattir.

> [!IMPORTANT]
> Sunucu silerseniz, sunucuya ait tüm veritabanlarının da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Yedekleme artıklık seçeneğinizi bağımsız, yedekleme saklama dönemi içinde zamanında herhangi bir noktaya geri yükleme gerçekleştirebilirsiniz. Yeni bir sunucu, aynı Azure bölgesinde özgün sunucusu olarak oluşturulur. Özgün sunucunun yapılandırması fiyatlandırma katmanı ile oluşturulan, oluşturma, vCores, depolama boyutu, yedekleme saklama dönemi ve yedekleme artıklığı seçeneği sayısı işlem.

Zaman içinde nokta geri yükleme, birden çok durumda yararlıdır. Örneğin, bir kullanıcı yanlışlıkla verileri sildiğinde, önemli tablo veya veritabanı yok sayar veya uygulamanın yanlışlıkla iyi verilerin bir uygulama hatası nedeniyle hatalı verilerle üzerine yazar.

Son beş dakika içinde zaman içinde bir noktaya geri yüklemeden önce gerçekleştirilecek bir sonraki işlem günlüğü yedeklemesi için beklemeniz gerekebilir.

### <a name="geo-restore"></a>Coğrafi Geri Yükleme

Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, bir sunucu hizmeti kullanılabilir olduğu başka bir Azure bölgesine geri yükleyebilirsiniz. Sunucunuz, sunucunun barındırıldığı bölgede bir olay nedeniyle kullanılamaz duruma geldiğinde coğrafi geri yükleme varsayılan kurtarma seçeneğidir. Büyük ölçekli olay durumunda olmadığından, veritabanı uygulamanızın bir bölge sonuçları, bir sunucu coğrafi olarak yedekli yedeklemelerden başka bir bölgede bulunan bir sunucuya geri yükleyebilirsiniz. Bir yedeklemenin ne zaman ve ne zaman farklı bir bölgeye çoğaltılır arasında bir gecikme olur. Bu gecikme en çok bir saat, bu nedenle, olağanüstü bir durum oluşursa, olabilir yukarı bir saat veri kaybı olabilir.

### <a name="perform-post-restore-tasks"></a>Gerçekleştirmek geri yükleme sonrası görevler

Bir geri yükleme ya da kurtarma mekanizması öğesinden sonra kullanıcılarınızın almak için aşağıdaki görevleri gerçekleştirmeniz gerekir ve uygulamalar geri hazır ve çalışır:

- Özgün sunucunun değiştirmek için yeni sunucu istediyseniz, istemcileri ve yeni sunucuya istemci uygulamaları yeniden yönlendirme
- Uygun sunucu düzeyinde güvenlik duvarı kuralları bağlanacak kullanıcılar için yerinde olduğundan emin olun
- Uygun oturum açma ve veritabanı düzeyi izinleri yerinde olduğundan emin olun
- Uyarıları uygun şekilde yapılandırma

## <a name="next-steps"></a>Sonraki adımlar

- İş sürekliliği hakkında daha fazla bilgi için bkz: [iş sürekliliğine genel bakış](concepts-business-continuity.md).
- Azure portalını kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı Azure portalını kullanarak zaman içinde bir noktaya geri](howto-restore-server-portal.md).
- Azure CLI kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı CLI kullanarak zaman içinde bir noktaya geri](howto-restore-server-cli.md).