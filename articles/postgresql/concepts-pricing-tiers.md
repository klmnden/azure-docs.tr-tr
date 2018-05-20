---
title: Azure veritabanı PostgreSQL için için fiyatlandırma katmanları
description: Bu makalede, Azure veritabanı için PostgreSQL için fiyatlandırma katmanlarına açıklanmaktadır.
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: aa8d92e86a40841ca46ff39f72ebf0ee24d332f8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Azure veritabanı fiyatlandırma katmanlarına PostgreSQL için

Üç farklı fiyatlandırma katmanlarına birinde PostgreSQL sunucu için bir Azure veritabanı oluşturabilirsiniz: Basic, genel amaçlı ve bellek için iyileştirilmiş. Fiyatlandırma katmanlarına sağlanabilir vCores, vCore başına bellek ve verileri depolamak için kullanılan depolama teknolojisi işlem miktarını tarafından ayrılır. Tüm kaynaklar PostgreSQL sunucu düzeyinde sağlanır. Bir sunucu bir veya daha çok veritabanları sağlayabilirsiniz.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5 Gen | Gen 4, 5 Gen | 5. Nesil |
| Sanal çekirdekler | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| VCore başına bellek | Taban Çizgisi | 2 x Basic | Genel amaçlı x 2 |
| Depolama boyutu | 1 TB ' 5 GB | 5 GB ile 2 TB | 5 GB ile 2 TB |
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Veritabanı yedekleme bekletme süresi | 7 için 35 gün | 7 için 35 gün | 7 için 35 gün |

Bir fiyatlandırma katmanı seçmek için bir başlangıç noktası olarak aşağıdaki tabloyu kullanın.

| Fiyatlandırma katmanı | Hedef iş yükleri |
|:-------------|:-----------------|
| Temel | Açık işlem ve g/ç performansı gerektiren iş yükleri. Geliştirme veya test için kullanılan sunucuları örnekler veya küçük ölçekli uygulamalar kullanılmayan. |
| Genel Amaçlı | Dengeli işlem ve bellek ölçeklenebilir g/ç işleme ile gerektirir, çoğu kurumsal iş yükleri. Örnek web ve mobil uygulamaları ve diğer Kurumsal uygulamaları barındıran sunucuları içerir.|
| Bellek için İyileştirilmiş | Daha hızlı işlem yapma ve daha yüksek eşzamanlılık için bellek içi performans gerektiren yüksek performanslı veritabanı iş yükleri. Örnekler gerçek zamanlı veri ve yüksek performanslı işlem veya analitik uygulamaları işlemek için sunucuları içerir.|

Bir sunucu oluşturduktan sonra vCores sayısı yukarı veya aşağı (aynı fiyatlandırma katmanı içinde) saniye içinde değiştirilebilir. Depolama alanı ve yedekleme bekletme süresi yukarı veya aşağı uygulama kapalı kalma süresi ile miktarı da bağımsız olarak ayarlayabilirsiniz. Bir sunucu oluşturulduktan sonra fiyatlandırma katmanı veya yedekleme depolama türünü değiştiremezsiniz. Daha fazla bilgi için bkz: [ölçeklendirme kaynakları](#scale-resources) bölümü.


## <a name="compute-generations-vcores-and-memory"></a>İşlem nesli, vCores ve bellek

İşlem kaynakları mantıksal CPU temel alınan donanım temsil eden vCores sağlanır. Şu anda iki işlem nesil, Gen 4 ve Gen 5 seçebilirsiniz. 4 mantıksal CPU üzerinde Intel E5-2673 v3 dayalı gen (Haswell) 2.4 GHz işlemci. 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı gen (Broadwell) 2.3 GHz işlemci. Gen 4 ve Gen 5 ("X" kullanılabilir gösterir) aşağıdaki bölgelerde kullanılabilir. 

| **Azure bölgesi** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| Orta ABD | X |  |
| Doğu ABD | X | X |
| Doğu ABD 2 | X | X |
| Orta Kuzey ABD | X |  |
| Orta Güney ABD | X | X |
| Batı ABD | X | X |
| Batı ABD 2 |  | X |
| Orta Kanada | X | X |
| Doğu Kanada | X | X |
| Güney Brezilya | X | X |
| Kuzey Avrupa | X | X |
| Batı Avrupa |  | X |
| Birleşik Krallık Batı |  | X |
| Birleşik Krallık Güney |  | X |
| Doğu Asya | X |  |
| Güneydoğu Asya | X | X |
| Avustralya Doğu |  | X |
| Avustralya Güneydoğu |  | X |
| Orta Hindistan | X |  |
| Batı Hindistan | X |  |
| Güney Hindistan |  | X |
| Japonya Doğu | X | X |
| Japonya Batı | X | X |
| Kore Güney |  | X |

Fiyatlandırma katmanına bağlı olarak, belirli bir bellek miktarı her vCore sağlanır. Artırabilir ya da vCores sayısını azaltmak için sunucunuzu, bellek artırır veya orantılı olarak azaltır. Genel amaçlı katmanı çift temel katmana göre vCore başına bellek miktarını sağlar. Bellek için iyileştirilmiş katmanı çift genel amaçlı katmanına karşılaştırıldığında bellek miktarını sağlar.

## <a name="storage"></a>Depolama

Sağlamanız depolama PostgreSQL server Azure veritabanınıza kullanılabilir depolama kapasitesi miktarıdır. Depolama veritabanı dosyaları, geçici dosyaları, işlem günlüklerinin ve PostgreSQL sunucusu için kullanılan günlükleri. Depolama sağlamanız toplam miktarı da sunucunuza kullanılabilir g/ç kapasitesi tanımlar.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 1 TB ' 5 GB | 5 GB ile 2 TB | 5 GB ile 2 TB |
| Depolama artırım boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOP/GB<br/>En az 100 IOPS | 3 IOP/GB<br/>En az 100 IOPS |

Ek depolama kapasitesi sırasında ve sunucu oluşturulduktan sonra ekleyebilirsiniz. Temel katman bir IOPS garanti sağlamaz. Genel amaçlı ve fiyatlandırma katmanlarına Bellek için iyileştirilmiş, 3:1 oranında sağlanan depolama boyutu ile IOPS ölçeklendirin.

Azure portalında veya Azure CLI komutları kullanarak, g/ç tüketim izleyebilirsiniz. İzlemek için ilgili Metrik [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzde](concepts-monitoring.md).

### <a name="reaching-the-store-limit"></a>Depolama sınırına ulaşmasını

Boş depolama alanı miktarını 5 GB ya da %5 sağlanan depolama değerinden ulaştığında sunucu salt okunur olarak işaretlenmiş, küçüktür. Örneğin, 100 GB depolama alanı sağlamış ve gerçek kullanımını gider salt okunur 95 GB, sunucu olarak işaretlendi. 5 GB depolama alanı sağladıysanız, boş depolama 250 MB'tan az ulaştığında alternatif olarak, sunucunun salt okunur işaretlenir.  

Sunucu salt okunur ayarlandığında, tüm oturumlarına kesilir ve kaydedilmemiş işlemleri geri alınır. Herhangi bir sonraki yazma işlemleri ve işlem başarısız kaydeder. İzleyen tüm okuma sorgular kesintisiz olarak çalışır.  

Sağlanan depolama sunucunuza artırın veya boş depolama geri kazanmak için okuma-yazma modunda ve açılan verileri yeni bir oturum başlatabilirsiniz. Çalışan `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` yazma modunda okumak üzere geçerli oturumdaki ayarlar. Sunucu hala salt okunur durumda olduğunda veri bozulmasını önlemek için tüm yazma işlemlerini gerçekleştirme.

## <a name="backup"></a>Backup

Hizmeti sunucunuzun yedeklemeleri otomatik olarak alır. Yedeklemeler için en düşük saklama süresi yedi gündür. 35 güne kadar tutma süresine ayarlayabilirsiniz. Bekletme sunucu ömrü boyunca herhangi bir noktada ayarlanabilir. Yerel olarak yedekli ve coğrafi olarak yedekli yedeklemeler arasında seçim yapabilirsiniz. Coğrafi olarak yedekli yedeklemeler de depolanır [coğrafi eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) sunucunuz oluşturulduğu bölgenin. Bu artıklık durumunda olağanüstü bir koruma düzeyi sağlar. Ayrıca sunucunuzun hizmet coğrafi olarak yedekli yedeklemelerde kullanılabilir olduğu diğer herhangi bir Azure bölgesine geri yükleme özelliği de kazanır. Sunucu oluşturulduktan sonra iki yedekleme depolama seçenekleri arasında değiştirmek mümkün değil.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuz oluşturduktan sonra bağımsız olarak vCores, depolama alanı miktarı ve yedekleme bekletme süresini değiştirebilirsiniz. Bir sunucu oluşturulduktan sonra fiyatlandırma katmanı veya yedekleme depolama türünü değiştiremezsiniz. VCores sayısı yukarı veya aşağı içinde aynı fiyatlandırma katmanı genişletilebilir. Yedekleme Bekletme dönemi yukarı veya aşağı 7'den 35 gün ölçeklendirilebilir. Depolama boyutu yalnızca artırılabilir.  Kaynaklarını ölçeklendirme portalı veya Azure CLI yoluyla gerçekleştirilebilir. Azure CLI kullanarak ölçeklendirme ilişkin bir örnek için bkz: [İzleyici ve ölçek Azure CLI kullanarak PostgreSQL sunucu için bir Azure veritabanı](scripts/sample-scale-server-up-or-down.md).

VCores sayısı değiştirdiğinizde, özgün sunucu kopyası ile yeni işlem ayırma oluşturulur. Yeni Sunucu çalışır durumda sonra bağlantıları yeni sunucuya geçiş. Sistem yeni sunucuya zaman geçer şu sırasında yeni bağlantı kuruldu ve tüm kaydedilmemiş işlemleri geri alınacak. Bu pencere değişir, ancak çoğu durumda bir dakikadan az olur.

Depolama ölçekleme ve yedekleme Bekletme dönemi değiştirmeyi true çevrimiçi işlemlerdir. Kapalı kalma süresi yoktur ve uygulamanızı etkilenmez. Sağlanan depolama boyutu ile IOPS ölçeği gibi depolama alanı ölçeklendirme tarafından kullanılabilir IOPS sunucunuza artırabilir.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için bkz: hizmeti [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/PostgreSQL/). İstediğiniz yapılandırma maliyetini görmek için [Azure portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) aylık maliyeti gösterir **fiyatlandırma katmanı** sekmesi sayısına seçenekleri. Bir Azure aboneliğiniz yoksa, tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı kullanabilirsiniz. Üzerinde [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, select **Öğe Ekle**, genişletin **veritabanları** kategorisi ve seçin **Azure veritabanı PostgreSQLiçin** seçenekleri özelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [portalda PostgreSQL sunucu oluşturma](tutorial-design-database-using-azure-portal.md).
- Bilgi edinmek için nasıl [izlemek ve Azure CLI kullanarak bir Azure veritabanı PostgreSQL sunucu için ölçeklendirme](scripts/sample-scale-server-up-or-down.md).
- Hakkında bilgi edinin [service sınırlamalar](concepts-limits.md).
