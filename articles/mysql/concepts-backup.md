---
title: Yedekleme ve MySQL için Azure veritabanı'nda geri yükleme
description: Otomatik yedeklemeler ve MySQL için Azure veritabanı sunucunuza geri yükleme hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 6fe5aea9b8fa87efdfa7cc57716cf548a52e076b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60532119"
---
# <a name="backup-and-restore-in-azure-database-for-mysql"></a>Yedekleme ve MySQL için Azure veritabanı'nda geri yükleme

MySQL için Azure veritabanı, otomatik olarak sunucu yedeklemelerini oluşturur ve bunları yapılandırılan kullanıcı yerel olarak yedekli veya coğrafi olarak yedekli depolama alanında depolar. Yedeklemeler, sunucunuz bir-belirli bir noktaya için geri yüklemek için kullanılabilir. Bunlar yanlışlıkla Bozulması veya silinmesi, verilerinizi korumak için yedekleme ve geri yükleme herhangi bir iş devamlılığı stratejinizin önemli bir parçası olan.

## <a name="backups"></a>Yedeklemeler

MySQL için Azure veritabanı, tam, değişiklik yedeklemelerinin ve işlem günlüğü yedeklemeleri alır. Bu yedeklemeler bir sunucuya yapılandırılan yedekleme saklama döneminizin tüm-belirli bir noktaya için geri yüklemenize olanak sağlar. Varsayılan yedekleme bekletme süresi yedi gündür. İsteğe bağlı olarak ayarlamak için 35 gün yapılandırabilirsiniz. Tüm yedeklemeler, AES 256 bit şifreleme kullanılarak şifrelenir.

### <a name="backup-frequency"></a>Yedekleme sıklığı

Genellikle tam yedekleme haftalık olarak ortaya farklı yedeklemeler günde iki kez oluşur ve işlem günlüğü yedeklemeleri beş dakikada bir gerçekleşir. Hemen bir sunucu oluşturulduktan sonra ilk tam yedeklemede zamanlanır. İlk yedeklemeyi büyük geri yüklenen sunucuda daha uzun sürebilir. En erken bir noktada yeni bir sunucuya geri yüklenebilir zamanlı ilk tam yedekleme tam olduğu zamandır.

### <a name="backup-redundancy-options"></a>Fazladan yedek seçenekleri

MySQL için Azure veritabanı genel amaçlı ve bellek için iyileştirilmiş katmanlarındaki yedekleme yerel olarak yedekli veya coğrafi olarak yedekli depolama arasında seçim yapma esnekliği sağlar. Yedeklemelerin yedekleme coğrafi olarak yedekli depolamada saklanan, bunlar yalnızca, sunucunuzun barındırılan, ancak de çoğaltılır bölge içinde depolanmaz bir [eşleştirilmiş veri merkezine](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Bu, daha iyi koruma ve olağanüstü bir durumda farklı bir bölgede sunucunuz geri yükleme olanağı sağlar. Temel katman yalnızca yerel olarak yedekli yedek depolama alanı sunar.

> [!IMPORTANT]
> Yerel olarak yapılandırma yedekli veya coğrafi olarak yedekli depolama, yedekleme sırasında sunucusu yalnızca sunulduğunu oluşturun. Sunucu oluşturulduktan sonra yedekleme depolama yedekliliği seçeneği değiştiremezsiniz.

### <a name="backup-storage-cost"></a>Yedekleme depolama maliyeti

MySQL için Azure veritabanı, en fazla % 100'ünü sağlanan sunucu depolama alanınızın hiçbir ek ücret ödemeden yedekleme depolama alanı olarak sağlar. Genellikle, bu, yedi gün yedekleme bekletme için uygundur. Tüm ek yedek depolama kullanılan GB-ay üzerinden ücretlendirilir.

Örneğin, bir sunucu ile 250 GB sağladıysa, ek ücret ödemeden 250 GB yedekleme alanı vardır. Depolama aşan 250 GB üzerinden ücretlendirilir.

## <a name="restore"></a>Geri Yükleme

MySQL için Azure veritabanı'nda bir geri yükleme işlemi yeni bir sunucu özgün sunucunun yedeklerden oluşturur.

Geri yükleme iki tür vardır:

- **Belirli bir noktaya geri yükleme** ya da fazladan yedek seçeneği ile kullanılabilir ve özgün sunucunuzla aynı bölgede yeni bir sunucu oluşturur.
- **Coğrafi geri yükleme** kullanılabilir yalnızca sunucunuz coğrafi olarak yedekli depolama için yapılandırılmış ve sunucunuz farklı bir bölgeye geri yüklemenize olanak sağlar.

Tahmini kurtarma süresi, veritabanı boyutu, işlem günlüğü boyutu, ağ bant genişliğini ve aynı anda aynı bölgede kurtarılan veri tabanı toplam sayısı dahil olmak üzere birçok faktöre bağlıdır. Kurtarma zamanı, genellikle daha az 12 saati geçmez.

> [!IMPORTANT]
> Silinen sunucuları **olamaz** geri yüklenemiyor. Sunucu silerseniz sunucusuna ait tüm veritabanlarını da silinir ve kurtarılamaz. Sunucu kaynaklarını korumak için dağıtım sonrasında, yanlışlıkla silme veya beklenmeyen değişiklikleri, yöneticiler yararlanabilir [yönetim kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources).

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Bağımsız, fazladan yedek seçenek olarak, zaman Yedekleme saklama döneminizin herhangi bir noktaya geri yükleme gerçekleştirebilirsiniz. Özgün sunucu ile aynı Azure bölgesinde yeni bir sunucu oluşturulur. Fiyatlandırma katmanı için özgün sunucunun yapılandırması ile oluşturulan, işlem nesli, sanal çekirdek, depolama boyutu, yedekleme bekletme süresi ve fazladan yedek seçenek sayısı.

Belirli bir noktaya geri yükleme, birden çok senaryoda kullanışlıdır. Bir kullanıcı yanlışlıkla veri sildiğinde, örneğin, bir önemli bir tabloya veya veritabanına, keser veya bir uygulama yanlışlıkla iyi bir uygulama, hata nedeniyle hatalı verilerle verinin üzerine yazar.

Son beş dakika içinde zaman içinde bir noktaya geri yüklemeden önce gerçekleştirilecek bir sonraki işlem günlüğü yedeklemesi için beklemeniz gerekebilir.

### <a name="geo-restore"></a>Coğrafi Geri Yükleme

Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, hizmetin kullanılabildiği başka bir Azure bölgesine bir sunucuya geri yükleyebilirsiniz. Sunucunuz sunucu barındırıldığı bölgedeki bir olay nedeniyle kullanılamaz olduğunda varsayılan kurtarma seçeneğini coğrafi geri yükleme olduğu. Büyük ölçekli olay kullanılamazlık veritabanı uygulamanızın bir bölge sonucu, bir sunucu coğrafi olarak yedekli yedeklemelerden başka bir bölgede bir sunucuya geri yükleyebilirsiniz. Yedekleme zaman alınır ve ne zaman farklı bir bölgeye çoğaltılır arasında bir gecikme olur. Bu gecikme, bir saat, bu nedenle, bir olağanüstü durum oluşursa, olabilir yukarı bir saatlik veri kaybı için en fazla olabilir.

Coğrafi geri yükleme sırasında işlem oluşturma, sanal çekirdek, yedekleme bekletme süresi ve fazladan yedek seçenekleri değiştirilebilir sunucu yapılandırmalarını içerir. Değişen fiyatlandırma katmanını (temel, genel amaçlı ve bellek için iyileştirilmiş) veya coğrafi geri yükleme sırasında depolama boyutu desteklenmiyor.

### <a name="perform-post-restore-tasks"></a>Gerçekleştirmek geri yükleme sonrası görevler

Sonra bu iki kurtarma sisteminden bir geri yükleme, kullanıcılarınızın almak için aşağıdaki görevleri gerçekleştirmeniz gerekir ve uygulamalar çalışır duruma geri:

- Yeni sunucunun özgün sunucunun değiştirilecek geliyorsa, istemcilerin ve istemci uygulamalarının yeni sunucuya yeniden yönlendirme
- Uygun sunucu düzeyinde güvenlik duvarı kuralları, kullanıcıların bağlanması yerinde olduğundan emin olun
- Uygun giriş bilgilerinin ve veritabanı düzeyi izinlerin mevcut olduğunu doğrulama
- Uyarıları uygun şekilde yapılandırma

## <a name="next-steps"></a>Sonraki adımlar

- İş sürekliliği hakkında daha fazla bilgi için bkz: [iş sürekliliğine genel bakış](concepts-business-continuity.md).
- Azure portalını kullanarak bir noktaya geri yüklemek için bkz: [veritabanını Azure portalını kullanarak bir noktaya geri yükleme](howto-restore-server-portal.md).
- Azure CLI kullanarak bir noktaya geri yüklemek için bkz: [veritabanı CLI kullanarak bir noktaya geri yükleme](howto-restore-server-cli.md).
