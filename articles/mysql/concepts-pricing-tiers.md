---
title: Azure veritabanı için MySQL için fiyatlandırma katmanları
description: Bu makalede, Azure veritabanı için MySQL için fiyatlandırma katmanlarına açıklanmaktadır.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: c1597f16dda8544908bbefaf39e75e667d38b22c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316477"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure veritabanı fiyatlandırma katmanlarına MySQL için

MySQL sunucusu için bir Azure veritabanı üç farklı fiyatlandırma katmanlarına birinde oluşturabilirsiniz: Basic, genel amaçlı ve bellek için iyileştirilmiş. Fiyatlandırma katmanlarına sağlanabilir vCores, vCore başına bellek ve verileri depolamak için kullanılan depolama teknolojisi işlem miktarını tarafından ayrılır. Tüm kaynaklar MySQL sunucu düzeyinde sağlanır. Bir sunucu bir veya daha çok veritabanları sağlayabilirsiniz.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5 Gen | Gen 4, 5 Gen | 5. Nesil |
| Sanal çekirdekler | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| VCore başına bellek | 2 GB | 5 GB | 10 GB |
| Depolama boyutu | 1 TB ' 5 GB | 5 GB ila 4 TB | 5 GB ila 4 TB |
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Veritabanı yedekleme bekletme süresi | 7 için 35 gün | 7 için 35 gün | 7 için 35 gün |

Bir fiyatlandırma katmanı seçmek için bir başlangıç noktası olarak aşağıdaki tabloyu kullanın.

| Fiyatlandırma katmanı | Hedef iş yükleri |
|:-------------|:-----------------|
| Temel | Açık işlem ve g/ç performansı gerektiren iş yükleri. Geliştirme veya test için kullanılan sunucuları örnekler veya küçük ölçekli uygulamalar kullanılmayan. |
| Genel Amaçlı | Dengeli işlem ve bellek ölçeklenebilir g/ç işleme ile gerektirir, çoğu kurumsal iş yükleri. Örnek web ve mobil uygulamaları ve diğer Kurumsal uygulamaları barındıran sunucuları içerir.|
| Bellek için İyileştirilmiş | Daha hızlı işlem yapma ve daha yüksek eşzamanlılık için bellek içi performans gerektiren yüksek performanslı veritabanı iş yükleri. Örnekler gerçek zamanlı veri ve yüksek performanslı işlem veya analitik uygulamaları işlemek için sunucuları içerir.|

Bir sunucu oluşturduktan sonra sayısı vCores, donanım oluşturma ve fiyatlandırma Katmanı (temel gelen ve giden hariç) yukarı veya aşağı saniye içinde değiştirilebilir. Depolama alanı ve yedekleme bekletme süresi yukarı veya aşağı uygulama kapalı kalma süresi ile miktarı da bağımsız olarak ayarlayabilirsiniz. Bir sunucu oluşturulduktan sonra yedekleme depolama türünü değiştiremezsiniz. Daha fazla bilgi için bkz: [ölçeklendirme kaynakları](#scale-resources) bölümü.

## <a name="compute-generations-and-vcores"></a>İşlem nesli ve vCores

İşlem kaynakları mantıksal CPU temel alınan donanım temsil eden vCores sağlanır. Şu anda iki işlem nesil, Gen 4 ve Gen 5 seçebilirsiniz. 4 mantıksal CPU üzerinde Intel E5-2673 v3 dayalı gen (Haswell) 2.4 GHz işlemci. 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı gen (Broadwell) 2.3 GHz işlemci. Gen 4 ve Gen 5 ("X" kullanılabilir gösterir) aşağıdaki bölgelerde kullanılabilir. 

| **Azure bölgesi** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| Orta ABD | X |  |
| Doğu ABD | X | X |
| Doğu ABD 2 | X | X |
| Orta Kuzey ABD | X | X |
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
| Doğu Asya | X | X |
| Güneydoğu Asya | X | X |
| Avustralya Doğu |  | X |
| Avustralya Güneydoğu |  | X |
| Orta Hindistan | X | X |
| Batı Hindistan | X | X |
| Güney Hindistan |  | X |
| Japonya Doğu | X | X |
| Japonya Batı | X | X |
| Kore Orta |  | X |
| Kore Güney |  | X |

## <a name="storage"></a>Depolama

Sağlamanız depolama MySQL sunucusu için Azure veritabanınıza kullanılabilir depolama kapasitesi miktarıdır. Depolama veritabanı dosyaları, geçici dosyaları, işlem günlüklerinin ve MySQL sunucusu için kullanılan günlükleri. Depolama sağlamanız toplam miktarı da sunucunuza kullanılabilir g/ç kapasitesi tanımlar.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Standart Azure depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 1 TB ' 5 GB | 5 GB ila 4 TB | 5 GB ila 4 TB |
| Depolama artırım boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOP/GB<br/>En az 100 IOPS<br/>Maksimum 7500 IOPS | 3 IOP/GB<br/>En az 100 IOPS<br/>Maksimum 7500 IOPS |

Ek depolama kapasitesi sırasında ve sunucu oluşturulduktan sonra ekleyebilirsiniz. Temel katman bir IOPS garanti sağlamaz. Genel amaçlı ve fiyatlandırma katmanlarına Bellek için iyileştirilmiş, 3:1 oranında sağlanan depolama boyutu ile IOPS ölçeklendirin.

Azure portalında veya Azure CLI komutları kullanarak, g/ç tüketim izleyebilirsiniz. İzlemek için ilgili Metrik [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzde](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Depolama sınırına ulaşması

Boş depolama alanı miktarını 5 GB ya da %5 sağlanan depolama değerinden ulaştığında sunucu salt okunur olarak işaretlenmiş, küçüktür. Örneğin, 100 GB depolama alanı sağlamış ve gerçek kullanımını gider salt okunur 95 GB, sunucu olarak işaretlendi. 5 GB depolama alanı sağladıysanız, boş depolama 250 MB'tan az ulaştığında alternatif olarak, sunucunun salt okunur işaretlenir.  

Hizmet sunucu salt okunur yapma girişiminde olsa da, tüm yeni yazma işlemi talepleri engellenir ve varolan etkin işlemlerin yürütülmeye devam eder. Sunucu salt okunur ayarlandığında, tüm sonraki yazma işlemleri ve işlem başarısız kaydeder. Okuma sorguları kesintisiz olarak çalışmaya devam eder. Sağlanan depolama artırdıktan sonra sunucu yazma işlemleri yeniden kabul etmeye hazır olacaktır.

## <a name="backup"></a>Backup

Hizmeti sunucunuzun yedeklemeleri otomatik olarak alır. Yedeklemeler için en düşük saklama süresi yedi gündür. 35 güne kadar tutma süresine ayarlayabilirsiniz. Bekletme sunucu ömrü boyunca herhangi bir noktada ayarlanabilir. Yerel olarak yedekli ve coğrafi olarak yedekli yedeklemeler arasında seçim yapabilirsiniz. Coğrafi olarak yedekli yedeklemeler de depolanır [coğrafi eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) sunucunuz oluşturulduğu bölgenin. Bu artıklık durumunda olağanüstü bir koruma düzeyi sağlar. Ayrıca sunucunuzun hizmet coğrafi olarak yedekli yedeklemelerde kullanılabilir olduğu diğer herhangi bir Azure bölgesine geri yükleme özelliği de kazanır. Sunucu oluşturulduktan sonra iki yedekleme depolama seçenekleri arasında değiştirmek mümkün değil.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuz oluşturduktan sonra bağımsız olarak değiştirebileceğiniz vCores, donanım oluşturma, fiyatlandırma Katmanı (temel gelen ve giden hariç), depolama ve yedekleme Bekletme dönemi miktarı. Bir sunucu oluşturulduktan sonra yedekleme depolama türünü değiştiremezsiniz. VCores sayısı yukarı veya aşağı genişletilebilir. Yedekleme Bekletme dönemi yukarı veya aşağı 7'den 35 gün ölçeklendirilebilir. Depolama boyutu yalnızca artırılabilir. Kaynaklarını ölçeklendirme portalı veya Azure CLI yoluyla gerçekleştirilebilir. Azure CLI kullanarak ölçeklendirme ilişkin bir örnek için bkz: [İzleyici ve ölçek Azure CLI kullanarak MySQL sunucusu için bir Azure veritabanı](scripts/sample-scale-server.md).

VCores sayısı değiştirdiğinizde, donanım oluşturma ya da fiyatlandırma katmanı, özgün sunucu kopyası ile yeni işlem ayırma oluşturulur. Yeni Sunucu çalışır durumda sonra bağlantıları yeni sunucuya geçiş. Sistem yeni sunucuya zaman geçer şu sırasında yeni bağlantı kuruldu ve tüm kaydedilmemiş işlemleri geri alınacak. Bu pencere değişir, ancak çoğu durumda, bir dakikadan az olur.

Depolama ölçekleme ve yedekleme Bekletme dönemi değiştirmeyi true çevrimiçi işlemlerdir. Kapalı kalma süresi yoktur ve uygulamanızı etkilenmez. Sağlanan depolama boyutu ile IOPS ölçeği gibi depolama alanı ölçeklendirme tarafından kullanılabilir IOPS sunucunuza artırabilir.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için bkz: hizmeti [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/). İstediğiniz yapılandırma maliyetini görmek için [Azure portal](https://portal.azure.com/#create/Microsoft.MySQLServer) aylık maliyeti gösterir **fiyatlandırma katmanı** sekmesi sayısına seçenekleri. Bir Azure aboneliğiniz yoksa, tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı kullanabilirsiniz. Üzerinde [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, select **Öğe Ekle**, genişletin **veritabanları** kategorisi ve seçin **Azure veritabanı için MySQL**seçenekleri özelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Portalı'nda MySQL sunucusu oluşturun](howto-create-manage-server-portal.md).
- Bilgi edinmek için nasıl [izlemek ve Azure CLI kullanarak bir Azure veritabanı MySQL sunucusu için ölçeklendirme](scripts/sample-scale-server.md).
- Hakkında bilgi edinin [service sınırlamalar](concepts-limits.md).
