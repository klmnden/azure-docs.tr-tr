---
title: PostgreSQL - tek bir sunucu için Azure veritabanı fiyatlandırma katmanları
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı fiyatlandırma katmanları açıklanır.
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 5f60a2786a87f4bd9be1f4a9e2a7a222e097b2e1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448070"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>-Tek bir sunucu PostgreSQL için Azure veritabanı'nda fiyatlandırma katmanları

PostgreSQL sunucusu için Azure veritabanı üç farklı fiyatlandırma katmanında oluşturabilirsiniz: Temel, genel amaçlı ve bellek için iyileştirilmiş. Fiyatlandırma katmanları tarafından sağlanabilen sanal çekirdek ve bellek sanal çekirdek başına verileri depolamak için kullanılan depolama teknolojisi olarak işlem miktarını ayrılır. PostgreSQL sunucu düzeyinde tüm kaynaklar sağlanır. Bir sunucu, bir veya birden çok veritabanına sahip olabilir.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, 5. nesil | Gen 4, 5. nesil | 5\. Nesil |
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

İşlem kaynakları, temel alınan donanım mantıksal CPU'yu temsil eden sanal çekirdekler sağlanır. Çin Doğu 1, Çin Kuzey 1, US DoD Orta ve ABD DoD Doğu yazılımınız Intel E5-2673 v3 temel 4. nesil mantıksal CPU'lar (Haswell) 2,4 GHz işlemcileri. Diğer tüm bölgeler Intel E5-2673 v4 temel 5. nesil mantıksal CPU'lar yazılımınız (Broadwell) 2.3 GHz işlemcileri.

## <a name="storage"></a>Depolama

PostgreSQL sunucusu için Azure veritabanı'na kullanılabilir depolama kapasitesi miktarını sağladığınız depolama alanıdır. Depolama, veritabanı dosyalarını, geçici dosyaları, işlem günlükleri ve PostgreSQL sunucusu için kullanılan günlükleri. Toplam miktarı sağladığınız depolama g/ç kapasite sunucunuza da tanımlar.

|    | **Temel** | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Azure standart depolama | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 5 GB ila 1 TB | 5 GB ile 4 TB | 5 GB ile 4 TB |
| Depolama artırma boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>Min 100 IOPS<br/>En çok 6000 IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>En çok 6000 IOPS |

Sırasında ve sonrasında sunucu oluşturulması ek depolama kapasitesi eklemek ve sistem otomatik olarak iş yükünüz depolama tüketimine göre depolama büyümesine izin verebilirsiniz. Temel katmanı, bir IOPS garanti sağlamaz. Genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanları, IOPS sağlanan depolama boyutu 3:1 oranını ölçeklendirin.

G/ç tüketiminiz Azure portalında veya Azure CLI komutlarını kullanarak izleyebilirsiniz. İlgili ölçümleri izlemek için [depolama sınırı, depolama yüzdesi, kullanılan depolama alanı ve g/ç yüzdesi](concepts-monitoring.md).

### <a name="large-storage-preview"></a>Büyük depolama (Önizleme)

Biz depolama sınırları, genel amaçlı ve bellek için iyileştirilmiş Katmanlar artmaktadır. Sunucuları yeni oluşturulan bu katılımı önizlemesine 16 TB'a kadar depolama alanı sağlayabilir. IOPS kadar 20.000 IOPS 3:1 oranında ölçeklendirilir. Geçerli genel olarak kullanılabilir depolamaya sahip olarak, sunucu oluşturulduktan sonra ek depolama kapasitesi eklemenize ve sistem otomatik olarak iş yükünüz depolama tüketimine göre depolama büyümesine izin verebilirsiniz.

|              | **Genel amaçlı** | **Bellek için iyileştirilmiş** |
|:-------------|:--------------------|:---------------------|
| Depolama türü | Azure Premium Depolama | Azure Premium Depolama |
| Depolama boyutu | 16 TB'ye kadar 32 GB| 32-16 TB |
| Depolama artırma boyutu | 1 GB | 1 GB |
| IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>20.000 IOPS maks. | 3 IOPS/GB<br/>Min 100 IOPS<br/>20.000 IOPS maks. |

> [!IMPORTANT]
> Büyük depolama şu anda aşağıdaki bölgelerde genel önizlemeye sunulmuştur: Doğu ABD, Doğu ABD 2, Orta ABD, Batı ABD, Kuzey Avrupa, Batı Avrupa, UK Güney, UK Batı, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Avustralya Doğu, Avustralya Güney Doğu.
>
> Büyük depolama önizlemesi şu anda desteklemez:
>
> * Sanal ağ hizmet uç noktaları üzerinden gelen bağlantılar
> * Coğrafi olarak yedekli yedekleri
> * Okuma amaçlı çoğaltmalar

### <a name="reaching-the-storage-limit"></a>Depolama sınırı ulaşma

Sağlanan 100 GB depolama alanı miktarından daha azıyla çalışabilse sunucuları boş depolama alanı 512 MB veya sağlanan depolama boyutu %5 küçükse salt okunur olarak işaretlenir. Yalnızca boş depolama alanı 5 GB'tan daha az olduğunda birden çok sağlanan 100 GB depolama sunucularıyla salt okunur olarak işaretlenir.

Örneğin 110 GB depolama alanı sağladığınız ve gerçek kullanımı gider salt okunur 105 GB, sunucunun işaretlenir. 5 GB depolama alanını sağladıysanız, 512 MB boş depolama alanı ulaştığında alternatif olarak, sunucunun salt okunur işaretlenir.

Sunucu salt okunur ayarlandığında, tüm mevcut oturumlarının bağlantısı kesilir ve kaydedilmemiş işlemleri geri alınır. Herhangi bir sonraki yazma işlemleri ve işlem başarısız kaydeder. Tüm sonraki okuma sorguları kesintisiz olarak çalışır.  

Sunucunuz için sağlanan depolama alanı miktarını artırmak veya boş depolama alanı boşaltmak için okuma-yazma modunda ve bırakma verileri yeni bir oturum başlatın. Çalışan `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` yazma modunda okumak üzere geçerli oturumdaki ayarlar. Veri bozulmasını önlemek için sunucu yine de salt okunur durumda olduğunda herhangi bir yazma işlemi gerçekleştirmeyin.

Depolama alanında açmanızı öneririz otomatik büyütme veya sunucu depolama alanınızın eşiği yaklaşırken bildiren bir uyarı ayarlamak için bu nedenle, kaçınmak salt okunur duruma alma. Daha fazla bilgi için şirket belgelerine bakın. [bir alarm ayarlama](howto-alert-on-metric.md).

### <a name="storage-auto-grow"></a>Depolama otomatik büyütme

Depolama otomatik büyümesi halinde olan etkin depolama otomatik olarak iş yükünü etkilemeden büyür. Boş depolama alanı 1 GB veya 10 sağlanan depolama yüzdesi büyük olarak sağlanan 100 GB depolama alanı miktarından daha azıyla çalışabilse sunucular için 5 GB ile sağlanan depolama boyutu artar. Boş depolama alanı, sağlanan depolama boyutu %5 altında olduğunda 100 GB'den fazla sağlanan depolama alanı ile sunucular için sağlanan depolama boyutu %5 oranında artar. Yukarıda belirtildiği gibi maksimum depolama sınırları geçerlidir.

Örneğin, 1000 GB depolama alanı sağlamış ve gerçek kullanımı gider 950 GB, sunucu depolama boyutu 1050 GB'a artırdık. 10 GB depolama alanı sağladıysanız, 1 GB'tan az depolama ücretsiz alternatif olarak, depolama boyutu için 15 GB artış olduğunda.

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
