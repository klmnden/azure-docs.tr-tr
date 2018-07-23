---
title: MySQL için Azure veritabanı fiyatlandırma katmanları
description: Bu makalede fiyatlandırma katmanları için MySQL için Azure veritabanı açıklanır.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 07/11/2018
ms.openlocfilehash: 70891e4c1425badb43dac66ada9c0b3a43b8fb0d
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173682"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Fiyatlandırma katmanları MySQL için Azure veritabanı

Üç farklı fiyatlandırma katmanlarından birini MySQL için Azure veritabanı oluşturabilirsiniz: temel, genel amaçlı ve bellek için iyileştirilmiş. Fiyatlandırma katmanları tarafından sağlanabilen sanal çekirdek ve bellek sanal çekirdek başına verileri depolamak için kullanılan depolama teknolojisi olarak işlem miktarını ayrılır. MySQL sunucu düzeyinde tüm kaynaklar sağlanır. Bir sunucu, bir veya birden çok veritabanına sahip olabilir.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5. nesil | Gen 4, 5. nesil | 5. Nesil |
| Sanal çekirdekler | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Sanal çekirdek başına bellek | 2 GB | 5 GB | 10 GB |
| Depolama boyutu | 5 GB ila 1 TB | 5 GB ile 4 TB | 5 GB ile 4 TB |
| Depolama türü | Azure standart depolama | Azure Premium Depolama | Azure Premium Depolama |
| Veritabanı yedeği saklama dönemi | 7 ila 35 gün | 7 ila 35 gün | 7 ila 35 gün |

Fiyatlandırma katmanını seçmek için bir başlangıç noktası olarak aşağıdaki tabloyu kullanın.

| Fiyatlandırma katmanı | Hedef iş yükleri |
|:-------------|:-----------------|
| Temel | Hafif işlem ve g/ç performansı gerektiren iş yükleri. Geliştirme veya test için kullanılan sunucular verilebilir veya küçük ölçekli az sıklıkta kullanılan uygulamalar. |
| Genel Amaçlı | Dengeli işlem ve bellek sıra ölçeklenebilir g/ç aktarım hızı gerektiren çoğu işletme iş yükü. Web ve mobil uygulamalar ve diğer Kurumsal uygulamaları barındırma sunucuları örneklerindendir.|
| Bellek için İyileştirilmiş | Daha hızlı işleme ve daha yüksek Eş zamanlılık için bellek içi performans gerektiren yüksek performanslı veritabanı iş yükleri. Gerçek zamanlı veri ve yüksek performanslı işlem veya Analiz uygulamaları işlemek için sunucular verilebilir.|

Bir sunucu oluşturduktan sonra sanal çekirdek, donanım oluşturma ve fiyatlandırma Katmanı (temel gelen ve giden hariç) yukarı veya aşağı saniyeler içinde değiştirilebilir. Depolama'yı kurma ve uygulamanızda kesinti yaşamadan veritabanınızın yedekleme bekletme süresi yukarı veya aşağı miktarı da bağımsız olarak ayarlayabilirsiniz. Bir sunucu oluşturulduktan sonra yedekleme depolama türünü değiştiremezsiniz. Daha fazla bilgi için [ölçeklendirme kaynakları](#scale-resources) bölümü.

## <a name="compute-generations-and-vcores"></a>İşlem Nesilleri ve sanal çekirdekler

İşlem kaynakları, temel alınan donanım mantıksal CPU'yu temsil eden sanal çekirdekler sağlanır. Şu anda iki işlem Nesilleri, Gen 4 ve 5. nesil seçebilirsiniz. Gen 4 mantıksal CPU'lar Intel E5-2673 v3 dayalı (Haswell) 2,4 GHz işlemcileri. 5 mantıksal CPU'lar Intel E5-2673 v4 nesil (Broadwell) 2.3 GHz işlemcileri. Gen 4 ve 5. nesil ("X" kullanılabilir gösterir) aşağıdaki bölgelerde kullanılabilir.

| **Azure bölgesi** | **4. nesil** | **5. nesil** |
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
| Fransa Orta |  | X |
| Birleşik Krallık Batı |  | X |
| Birleşik Krallık Güney |  | X |
| Doğu Asya | X | X |
| Güneydoğu Asya | X | X |
| Avustralya Doğu |  | X |
| Avustralya Orta |  | X |
| Avustralya Orta 2 |  | X |
| Avustralya Güneydoğu |  | X |
| Orta Hindistan | X | X |
| Batı Hindistan | X | X |
| Güney Hindistan |  | X |
| Japonya Doğu | X | X |
| Japonya Batı | X | X |
| Kore Orta |  | X |
| Kore Güney |  | X |

## <a name="storage"></a>Depolama

MySQL için Azure veritabanı'na kullanılabilir depolama kapasitesi miktarını sağladığınız depolama alanıdır. Depolama, veritabanı dosyalarını, geçici dosyaları, işlem günlükleri ve MySQL sunucusu için kullanılan günlükleri. Toplam miktarı sağladığınız depolama g/ç kapasite sunucunuza da tanımlar.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Azure standart depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 5 GB ila 1 TB | 5 GB ile 4 TB | 5 GB ile 4 TB |
| Depolama artırma boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>En az 100 IOPS<br/>En çok 6000 IOPS | 3 IOPS/GB<br/>En az 100 IOPS<br/>En çok 6000 IOPS |

Ek depolama kapasitesi, sırasında ve sunucunun oluşturulduktan sonra ekleyebilirsiniz. Temel katmanı, bir IOPS garanti sağlamaz. Genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanları, IOPS sağlanan depolama boyutu 3:1 oranını ölçeklendirin.

G/ç tüketiminiz Azure portalında veya Azure CLI komutlarını kullanarak izleyebilirsiniz. İlgili ölçümleri izlemek için [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzdesi](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Depolama sınırı ulaşma

Boş depolama alanı miktarı az 5 GB veya sağlanan depolama alanı, %5 ulaştığında sunucu salt okunur olarak işaretlenmiş, küçüktür. Örneğin, 100 GB depolama alanı sağlamış ve gerçek kullanımı gider salt okunur 95 GB, sunucunun işaretlenir. 5 GB depolama alanını sağladıysanız, boş depolama alanı 250 MB'tan az ulaştığında alternatif olarak, sunucunun salt okunur işaretlenir.  

Hizmet sunucusunu salt okunur hale getirmek çalışır, ancak tüm yeni yazma işlemi talepleri engellenir ve mevcut etkin işlemler yürütülmeye devam eder. Sunucu salt okunur ayarlandığında, tüm sonraki yazma işlemleri ve işlem başarısız kaydeder. Okuma sorguları kesintisiz olarak çalışmaya devam eder. Sağlanan depolama artırdıktan sonra sunucu yeniden yazma işlemleri kabul etmeye hazır olacaktır.

Salt okunur duruma girmesini önlemek için sunucu depolama alanınızın eşiği yaklaşırken bildiren bir uyarı ayarlamanızı öneririz. Daha fazla bilgi için şirket belgelerine bakın. [bir alarm ayarlama](howto-alert-on-metric.md).

## <a name="backup"></a>Backup

Hizmet, sunucunuzun yedeklemeleri otomatik olarak alır. Yedeklemeler için en düşük bekletme süresi yedi gündür. 35 güne kadar bir bekletme süresi ayarlayabilirsiniz. Bekletme sunucu yaşam süresi boyunca herhangi bir noktada ayarlanabilir. Yerel olarak yedekli ve coğrafi olarak yedekli yedeklemeler arasında seçim yapabilirsiniz. Coğrafi olarak yedekli yedeklemeler de depolanır [coğrafi eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) sunucunuzun oluşturulduğu bölgenin. Bu yedeklilik, olağanüstü bir koruma düzeyi sağlar. Ayrıca sunucunuzun hizmet ile coğrafi olarak yedekli yedeklemeleri kullanılabilir olduğu diğer herhangi bir Azure bölgesine geri yükleme olanağı elde edin. Sunucu oluşturulduktan sonra iki yedekleme depolama seçenekleri arasında değiştirmek mümkün değildir.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuz oluşturduktan sonra bağımsız olarak değiştirebileceğiniz sanal çekirdekler, donanım oluşturma, fiyatlandırma katmanını (temel gelen ve giden hariç), depolama ve yedekleme bekletme süresi miktarı. Bir sunucu oluşturulduktan sonra yedekleme depolama türünü değiştiremezsiniz. Sanal çekirdek sayısı, yukarı veya aşağı ölçeklendirilebilir. Yedekleme bekletme süresi yukarı veya aşağı 7'den 35 gün öncesine ölçeklendirilebilir. Depolama boyutu yalnızca artırılabilir. Kaynakları ölçeklendirme portal veya Azure CLI aracılığıyla yapılabilir. Azure CLI kullanarak bir ölçeklendirme ilişkin bir örnek için bkz [İzleyici ve ölçek Azure CLI kullanarak MySQL sunucusu için Azure veritabanı](scripts/sample-scale-server.md).

Sanal çekirdek sayısı değiştirdiğinizde, donanım oluşturma veya fiyatlandırma katmanı, özgün sunucunun bir kopyasını yeni işlem ayırma ile oluşturulur. Yeni Sunucu çalışır duruma geldikten sonra bağlantıları yeni sunucuya etkinleştirildi. Sistem, yeni sunucuya geçer şu sırasında yeni bağlantı kurulabilir ve kesinleşmemiş tüm işlemler geri alınır. Bu pencere değişir, ancak çoğu durumda, bir dakikadan az olacaktır.

Depolamayı ölçeklendirme ve yedekleme Bekletme dönemi değiştirmeyi true çevrimiçi işlemlerdir. Kapalı kalma süresi ve uygulamanızı etkilenmez. Sağlanan depolama alanı boyutu ile IOPS ölçeklendirme gibi depolama alanının ölçeğini tarafından kullanılabilir IOPS sunucunuza artırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için bkz: hizmeti [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/). İstediğiniz yapılandırmanın maliyetini görmek için [Azure portalında](https://portal.azure.com/#create/Microsoft.MySQLServer) aylık maliyeti gösterilir **fiyatlandırma katmanı** sekmesinde belirlediğiniz seçeneklere bağlı. Azure aboneliğiniz yoksa, bir tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı'nı kullanabilirsiniz. Üzerinde [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, select **öğeleri ekleme**, genişletin **veritabanları** kategori seçip **MySQLiçinAzureveritabanı**seçenekleri özelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [portalda bir MySQL sunucusu oluşturma](howto-create-manage-server-portal.md).
- Bilgi edinmek için nasıl [izleme ve Azure CLI kullanarak MySQL için Azure veritabanı ölçeklendirme](scripts/sample-scale-server.md).
- Hakkında bilgi edinin [hizmet sınırlamaları](concepts-limits.md).
