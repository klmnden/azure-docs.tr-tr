---
title: "MySQL için Azure veritabanında fiyatlandırma katmanları"
description: "Bu makalede Azure veritabanındaki fiyatlandırma katmanları için MySQL açıklanmaktadır."
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6bd24da05c337a902ce0e4a2b9acf22a809eb653
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure veritabanı fiyatlandırma katmanlarına MySQL için

MySQL sunucusu için bir Azure veritabanı üç farklı fiyatlandırma katmanlarına - temel, genel amaçlı ve bellek için iyileştirilmiş oluşturulabilir. Fiyatlandırma katmanlarına sağlanabilir vCores, vCore başına bellek ve verileri depolamak için kullanılan depolama teknolojisi işlem miktarını tarafından ayrılır. Tüm kaynaklar MySQL sunucu düzeyinde sağlanır. Bir sunucu bir veya daha çok veritabanları sağlayabilirsiniz.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5 Gen | Gen 4, 5 Gen | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16, 32 |
| VCore başına bellek | 1x | 2 x Basic | Genel amaçlı x 2 |
| Depolama Boyutu | 1 TB ' 5 GB | 1 TB ' 5 GB | 1 TB ' 5 GB |
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Veritabanı yedekleme bekletme süresi | 7 - 35 gün | 7 - 35 gün | 7 - 35 gün |

Aşağıdaki tabloda bir fiyatlandırma katmanı seçme özelliği için bir başlangıç noktası olarak kullanılabilir:

| Fiyatlandırma katmanı | Hedef iş yükleri |
|:-------------|:-----------------|
| Temel | Açık işlem ve g/ç performansı gerektiren iş yükleri. Geliştirme veya test için kullanılan sunucuları örnekler veya küçük ölçekli uygulamalar kullanılmayan. |
| Genel Amaçlı | Dengeli işlem ve bellek ölçeklenebilir g/ç işleme ile gerektiren çoğu kurumsal iş yükleri. Örnek Web ve mobil uygulamaları ve diğer Kurumsal uygulamaları barındırmak üzere sunucu verilebilir.|
| Bellek için İyileştirilmiş | Daha hızlı işlem yapma ve daha yüksek eşzamanlılık için bellek içi performans gerektiren yüksek performanslı veritabanı iş yükleri. Gerçek zamanlı veri ve yüksek performanslı işlem veya analitik uygulamaları işlemek için sunucu örnekler.|

Bir sunucu oluşturduktan sonra vCores sayısı yukarı veya aşağı saniye içinde değiştirilebilir. Depolama alanı ve yedekleme bekletme süresi yukarı veya aşağı uygulama kapalı kalma süresi ile miktarını bağımsız olarak da ayarlayabilirsiniz. Daha fazla ayrıntı için aşağıdaki ölçeklendirme bölümüne bakın.

## <a name="compute-generations-vcores-and-memory"></a>İşlem nesli, vCores ve bellek

İşlem kaynakları, temel alınan donanım mantıksal CPU temsil eden vCores sağlanır. Şu anda iki işlem nesil, Gen 4 ve Gen 5 aralarından seçim yapabileceğiniz sunulur. 4 mantıksal CPU üzerinde Intel E5-2673 v3 dayalı gen (Haswell) 2.4 GHz işlemci. 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı gen (Broadwell) 2.3 GHz işlemci.

Fiyatlandırma katmanına bağlı olarak, belirli bir bellek miktarı her vCore sağlanır. Artırabilir ya da vCores sayısını azaltmak için sunucunuzu, bellek artırır veya orantılı olarak azaltır. Genel amaçlı katmanı çift temel katmana göre vCore başına bellek miktarını sağlar. Bellek için iyileştirilmiş katmanı çift genel amaçlı katmanına karşılaştırıldığında bellek miktarını sağlar.

## <a name="storage"></a>Depolama

Sağlamanız depolama MySQL sunucusu için Azure veritabanınıza kullanılabilir depolama kapasitesi miktarıdır. Depolama veritabanı dosyaları, geçici dosyaları, işlem günlüklerinin ve MySQL sunucusu için kullanılan günlükleri. Depolama sağlamanız toplam miktarı da sunucunuza kullanılabilir g/ç kapasitesi tanımlar:

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama Boyutu | 1 TB ' 5 GB | 1 TB ' 5 GB | 1 TB ' 5 GB |
| Depolama artırım boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>Min 100 IOPS | 3 IOPS/GB<br/>Min 100 IOPS |

Ek depolama kapasitesi sırasında ve sonrasında sunucusunun oluşturulması eklenebilir. Temel katman bir IOPS garanti sağlamaz. Genel amaçlı ve fiyatlandırma katmanlarına Bellek için iyileştirilmiş, 3:1 oranında sağlanan depolama boyutu ile IOPS ölçeklendirin.

Azure portalında veya Azure CLI komutları kullanarak, g/ç tüketim izleyebilirsiniz. İzlemek için ilgili Metrik [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzde](concepts-monitoring.md).

## <a name="backup"></a>Backup

Hizmeti sunucunuzun yedeklemeleri otomatik olarak alır. Yedeklemeler için en düşük saklama süresi yedi gündür. 35 güne kadar tutma süresine ayarlayabilirsiniz. Bekletme sunucu ömrü boyunca herhangi bir noktada ayarlanabilir. Yerel olarak yedekli ve coğrafi olarak yedekli yedeklemeler arasında seçim yapabilirsiniz. Coğrafi olarak yedekli yedeklemeler de depolanır [coğrafi eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) bölge içinde sunucunuz oluşturulur. Bir olağanüstü durum durumunda bir koruma düzeyi sağlar. Ayrıca sunucunuzun hizmet coğrafi olarak yedekli yedeklemelerde kullanılabilir olduğu diğer herhangi bir Azure bölgesine geri yükleme özelliği de kazanır. Bu sunucunun oluşturulduktan sonra iki yedekleme depolama seçenekleri arasında değiştirmek mümkün olmadığını unutmayın.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuz oluşturduktan sonra depolama ve yedekleme Bekletme dönemi miktarını vCores bağımsız olarak değiştirebilirsiniz. Bir sunucu oluşturulduktan sonra fiyatlandırma katmanı veya yedekleme depolama türünü değiştiremezsiniz. vCores ve yedekleme Bekletme dönemi yukarı veya aşağı genişletilebilir. Depolama boyutu yalnızca artırılabilir. Kaynaklarını ölçeklendirme portalı veya Azure CLI yoluyla gerçekleştirilebilir. CLI kullanarak ölçeklendirmeye yönelik bir örnek bulunabilir [burada](scripts/sample-scale-server.md).

VCores sayısını değiştirme kopyasını özgün sunucunun yeni işlem ayırma ile oluşturulur. Yeni Sunucu çalışır hale geldikten sonra bağlantıları yeni sunucuya geçiş. Sistem yeni sunucuya zaman geçer kısa süre sırasında yeni bağlantı kuruldu ve tüm kaydedilmemiş işlemleri geri alınır. Bu pencere değişir, ancak çoğu durumda bir dakikadan az olur.

Depolama ölçekleme ve yedekleme Bekletme dönemi değiştirmeyi true çevrimiçi işlemlerdir. Kapalı kalma süresi veya uygulamanızın üzerinde etkisi yoktur. Sağlanan depolama boyutu ile IOPS ölçeği gibi depolama alanı ölçeklendirme tarafından kullanılabilir IOPS sunucunuza artırabilir.

## <a name="pricing"></a>Fiyatlandırma

Lütfen hizmet gözden [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/) için en güncel fiyatlandırma bilgileri. Hangi, istenen yapılandırma maliyetleri görmek için [Azure portal](https://portal.azure.com/#create/Microsoft.MySQLServer) aylık maliyeti gösterir **fiyatlandırma katmanı** sekmesi sayısına seçtiğiniz seçenekleri. Bir Azure aboneliğiniz yoksa, tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı kullanabilirsiniz. Ziyaret [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, ardından **Öğe Ekle**, genişletin **veritabanları** kategorisi ve seçin **Azure veritabanı için MySQL** seçenekleri özelleştirmek için.

## <a name="next-steps"></a>Sonraki Adımlar

- Bilgi edinmek için nasıl [Portalı'nda MySQL sunucusu oluşturun](howto-create-manage-server-portal.md)
- Bilgi edinmek için nasıl [izlemek ve Azure CLI kullanarak MySQL sunucusu için bir Azure veritabanı ölçeklendirme](scripts/sample-scale-server.md)
- Hakkında bilgi edinin [hizmet sınırlamaları](concepts-limits.md)
