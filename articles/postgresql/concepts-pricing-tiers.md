---
title: PostgreSQL - tek bir sunucu için Azure veritabanı fiyatlandırma katmanları
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı fiyatlandırma katmanları açıklanır.
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: e2580a57f943ad8da16cfbaeda2ee35d0f4bb691
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073191"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>-Tek bir sunucu PostgreSQL için Azure veritabanı'nda fiyatlandırma katmanları

PostgreSQL sunucusu için Azure veritabanı üç farklı fiyatlandırma katmanında oluşturabilirsiniz: Temel, genel amaçlı ve bellek için iyileştirilmiş. Fiyatlandırma katmanları tarafından sağlanabilen sanal çekirdek ve bellek sanal çekirdek başına verileri depolamak için kullanılan depolama teknolojisi olarak işlem miktarını ayrılır. PostgreSQL sunucu düzeyinde tüm kaynaklar sağlanır. Bir sunucu, bir veya birden çok veritabanına sahip olabilir.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5. nesil | Gen 4, 5. nesil | 5. Nesil |
| Sanal çekirdekler | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
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
| Orta ABD |  | X |
| Doğu ABD |  | X |
| Doğu ABD 2 |  | X |
| Orta Kuzey ABD |  | X |
| Orta Güney ABD |  | X |
| Batı ABD |  | X |
| Batı ABD 2 |  | X |
| Güney Brezilya |  | X |
| Orta Kanada |  | X |
| Doğu Kanada |  | X |
| Kuzey Avrupa |  | X |
| Batı Avrupa |  | X |
| Fransa Orta |  | X |
| Birleşik Krallık Güney |  | X |
| Birleşik Krallık Batı |  | X |
| Doğu Asya |  | X |
| Güneydoğu Asya |  | X |
| Avustralya Doğu |  | X |
| Avustralya Orta |  | X |
| Avustralya Orta 2 |  | X |
| Avustralya Güneydoğu |  | X |
| Orta Hindistan |  | X |
| Güney Hindistan |  | X |
| Batı Hindistan |  | X |
| Japonya Doğu |  | X |
| Japonya Batı |  | X |
| Kore Orta |  | X |
| Kore Güney |  | X |
| Çin Doğu 1 | X |  |
| Çin Doğu 2 |  | X |
| Çin Kuzey 1 | X |  |
| Çin Kuzey 2 |  | X |
| Almanya Orta |  | X |
| US DoD Orta  | X |  |
| US DoD Doğu  | X |  |
| ABD Devleti Arizona |  | X |
| ABD Devleti Texas |  | X |
| ABD Devleti Virginia |  | X |

## <a name="storage"></a>Depolama

PostgreSQL sunucusu için Azure veritabanı'na kullanılabilir depolama kapasitesi miktarını sağladığınız depolama alanıdır. Depolama, veritabanı dosyalarını, geçici dosyaları, işlem günlükleri ve PostgreSQL sunucusu için kullanılan günlükleri. Toplam miktarı sağladığınız depolama g/ç kapasite sunucunuza da tanımlar.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Azure standart depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 5 GB ila 1 TB | 5 GB ile 4 TB | 5 GB ile 4 TB |
| Depolama artırma boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>Min 100 IOPS<br/>En çok 6000 IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>En çok 6000 IOPS |

Ek depolama kapasitesi, sırasında ve sunucunun oluşturulduktan sonra ekleyebilirsiniz. Temel katmanı, bir IOPS garanti sağlamaz. Genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanları, IOPS sağlanan depolama boyutu 3:1 oranını ölçeklendirin.

G/ç tüketiminiz Azure portalında veya Azure CLI komutlarını kullanarak izleyebilirsiniz. İlgili ölçümleri izlemek için [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzdesi](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Depolama sınırı ulaşma

Boş depolama alanı miktarı 5 GB veya sağlanan depolama alanının %5'i (hangisi daha düşükse) olduğunda sunucu salt okunur olarak işaretlenir. Örneğin, 100 GB depolama alanı sağlamış ve gerçek kullanımı gider salt okunur 95 GB, sunucunun işaretlenir. Alternatif olarak, 5 GB depolama alanı sağladıysanız boş depolama alanı 250 MB seviyesinin altına düştüğünde sunucu salt okunur olarak işaretlenir.  

Sunucu salt okunur ayarlandığında, tüm mevcut oturumlarının bağlantısı kesilir ve kaydedilmemiş işlemleri geri alınır. Herhangi bir sonraki yazma işlemleri ve işlem başarısız kaydeder. Tüm sonraki okuma sorguları kesintisiz olarak çalışır.  

Sunucunuz için sağlanan depolama alanı miktarını artırmak veya boş depolama alanı boşaltmak için okuma-yazma modunda ve bırakma verileri yeni bir oturum başlatın. Çalışan `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` yazma modunda okumak üzere geçerli oturumdaki ayarlar. Veri bozulmasını önlemek için sunucu yine de salt okunur durumda olduğunda herhangi bir yazma işlemi gerçekleştirmeyin.

Salt okunur duruma girmesini önlemek için sunucu depolama alanınızın eşiği yaklaşırken bildiren bir uyarı ayarlamanızı öneririz. Daha fazla bilgi için şirket belgelerine bakın. [bir alarm ayarlama](howto-alert-on-metric.md).

## <a name="backup"></a>Backup

Hizmet, sunucunuzun yedeklemeleri otomatik olarak alır. Yedeklemeler için en düşük bekletme süresi yedi gündür. 35 güne kadar bir bekletme süresi ayarlayabilirsiniz. Bekletme sunucu yaşam süresi boyunca herhangi bir noktada ayarlanabilir. Yerel olarak yedekli ve coğrafi olarak yedekli yedeklemeler arasında seçim yapabilirsiniz. Coğrafi olarak yedekli yedeklemeler de depolanır [coğrafi eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) sunucunuzun oluşturulduğu bölgenin. Bu yedeklilik, olağanüstü bir koruma düzeyi sağlar. Ayrıca sunucunuzun hizmet ile coğrafi olarak yedekli yedeklemeleri kullanılabilir olduğu diğer herhangi bir Azure bölgesine geri yükleme olanağı elde edin. Sunucu oluşturulduktan sonra iki yedekleme depolama seçenekleri arasında değiştirmek mümkün değildir.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuz oluşturduktan sonra bağımsız olarak değiştirebileceğiniz sanal çekirdekler, donanım oluşturma, fiyatlandırma katmanını (temel gelen ve giden hariç), depolama ve yedekleme bekletme süresi miktarı. Bir sunucu oluşturulduktan sonra yedekleme depolama türünü değiştiremezsiniz. Sanal çekirdek sayısı, yukarı veya aşağı ölçeklendirilebilir. Yedekleme bekletme süresi yukarı veya aşağı 7'den 35 gün öncesine ölçeklendirilebilir. Depolama boyutu yalnızca artırılabilir. Kaynakları ölçeklendirme portal veya Azure CLI aracılığıyla yapılabilir. Azure CLI kullanarak bir ölçeklendirme ilişkin bir örnek için bkz [İzleyici ve ölçek Azure CLI kullanarak PostgreSQL sunucusu için Azure veritabanı](scripts/sample-scale-server-up-or-down.md).

Sanal çekirdek sayısı değiştirdiğinizde, donanım oluşturma veya fiyatlandırma katmanı, özgün sunucunun bir kopyasını yeni işlem ayırma ile oluşturulur. Yeni Sunucu çalışır duruma geldikten sonra bağlantıları yeni sunucuya etkinleştirildi. Sistem, yeni sunucuya geçer şu sırasında yeni bağlantı kurulabilir ve kesinleşmemiş tüm işlemler geri alınır. Bu pencere değişir, ancak çoğu durumda, bir dakikadan az olacaktır.

Depolamayı ölçeklendirme ve yedekleme Bekletme dönemi değiştirmeyi true çevrimiçi işlemlerdir. Kapalı kalma süresi ve uygulamanızı etkilenmez. Sağlanan depolama alanı boyutu ile IOPS ölçeklendirme gibi depolama alanının ölçeğini tarafından kullanılabilir IOPS sunucunuza artırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için bkz: hizmeti [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/PostgreSQL/). İstediğiniz yapılandırmanın maliyetini görmek için [Azure portalında](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) aylık maliyeti gösterilir **fiyatlandırma katmanı** sekmesinde belirlediğiniz seçeneklere bağlı. Azure aboneliğiniz yoksa, bir tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı'nı kullanabilirsiniz. Üzerinde [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, select **öğeleri ekleme**, genişletin **veritabanları** kategori seçip **PostgreSQLiçinAzureveritabanı** seçenekleri özelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [portalda bir PostgreSQL sunucusu oluşturma](tutorial-design-database-using-azure-portal.md).
- Hakkında bilgi edinin [hizmet sınırları](concepts-limits.md). 
- Bilgi edinmek için nasıl [okuma Çoğaltmalarla ölçeğini genişletme](howto-read-replicas-portal.md).
