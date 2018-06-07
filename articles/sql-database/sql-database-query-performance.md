---
title: Azure SQL veritabanı için sorgu performansı öngörüleri | Microsoft Docs
description: Sorgu performansı izleme CPU tüketimi sorguların çoğu bir Azure SQL veritabanı için tanımlar.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: f608beb0834b1c838b082e92340ebf9b650d8b3f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648743"
---
# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL veritabanı sorgu performansı öngörüleri
Yönetme ve ilişkisel veritabanlarının performansını ayarlama önemli uzmanlık ve zaman yatırımı gerektiren bir görevdir. Sorgu performansı öngörüleri, aşağıdakileri sağlayarak veritabanı performans sorunlarını giderme daha az süre beklemesini sağlar:

* Veritabanları (DTU) kaynak tüketimi daha derin bir anlayış. 
* Potansiyel olarak performansı için ayarlanan süre/CPU/yürütme sayısına göre üst sorgular.
* Bir sorgu ayrıntıları detaya yeteneği, metin ve kaynak kullanımı geçmişini görüntüleyin. 
* Performans tarafından gerçekleştirilen eylemler Göster ek açıklamaları ayarlama [SQL Azure veritabanı Danışmanı](sql-database-advisor.md)  



## <a name="prerequisites"></a>Önkoşullar
* Sorgu performansı öngörüleri gerektirir [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) veritabanınızda etkindir. Query Store çalışmıyorsa, portal etkinleştirmek ister.

## <a name="permissions"></a>İzinler
Aşağıdaki [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) sorgu performansı öngörüleri kullanmak için gereken izinler: 

* **Okuyucu**, **sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** izinleri gereklidir üst görüntülemek için kaynak tüketen sorguları ve grafikleri. 
* **Sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** sorgu metnini görüntülemek için gereken izinler.

## <a name="using-query-performance-insight"></a>Sorgu performansı öngörüleri kullanma
Sorgu performansı öngörüleri kullanımı kolay:

* Açık [Azure portal](https://portal.azure.com/) ve incelemek istediğiniz Bul veritabanı. 
  * Destek ve sorun giderme, sol taraftaki menüden "Sorgu performansı öngörüleri" seçin.
* İlk sekmesinde üst kaynak tüketen sorguları listesini gözden geçirin.
* Ayrıntılarını görüntülemek için bir sorguyu seçin.
* Açık [SQL Azure veritabanı Danışmanı](sql-database-advisor.md) ve herhangi bir önerimiz kullanılabilir olup olmadığını denetleyin.
* Kaydırma çubuklarını kullanın veya gözlemlenen aralığını değiştirmek için simgeler Yakınlaştır.
  
    ![Performans Panosu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Birkaç saat veri sorgu performansı öngörüleri sağlamak SQL veritabanı için Query Store tarafından yakalanan gerekir. Veritabanı hiçbir etkinlik varsa veya sorgu deposu belirli bir zaman süresi boyunca etkin olmayan grafikler döneme görüntülenirken boş olur. Çalışır durumda değilse herhangi bir anda Query Store sağlayabilir.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Kaynak tüketen sorguları üst CPU gözden geçirin
İçinde [portal](http://portal.azure.com) aşağıdakileri yapın:

1. Bir SQL veritabanına bulun ve tıklatın **tüm ayarları** > **destek + sorun giderme** > **sorgu performansı öngörüleri**. 
   
    ![Sorgu Performansı İçgörüleri][1]
   
    En sık kullanılan sorguların görünümünü açar ve üst CPU tüketim sorguları listelenir.
2. Ayrıntılar için grafik etrafında'ı tıklatın.<br>Çubukları seçilen zaman aralığı boyunca seçili sorgular tarafından tüketilen CPU % gösterirken üst çizgi veritabanı için genel DTU % gösterir (örneğin, **geçen hafta** seçili her çubuk bir gününü temsil eder).
   
    ![üst sorguları][2]
   
    Alt kılavuz görünür sorgular için toplanan bilgileri temsil eder.
   
   * Sorgu kimliği - sorgu veritabanı içinde benzersiz tanıtıcısı.
   * CPU (toplama işlevini bağlıdır) sorgu observable aralığı sırasında başına.
   * Sorgu başına süre (toplama işlevini bağlıdır).
   * Belirli bir sorgu için yürütmeleri toplam sayısı.
     
     Seçin veya dahil etmek veya onay kutularını kullanarak grafikten hariç tek tek sorguları temizleyin.
3. Verilerinizi eski hale gelirse tıklatın **yenileme** düğmesi.
4. Kaydırma çubuklarını kullanın ve yakınlaştırma gözlem aralığı değiştirin ve ani araştırmak için düğmeler: ![ayarları](./media/sql-database-query-performance/zoom.png)
5. İsteğe bağlı olarak, farklı bir görünüm istiyorsanız, seçebileceğiniz **özel** sekmesinde ve ayarlayın:
   
   * Ölçüm (CPU süresi, yürütme sayısı)
   * Zaman aralığı (son 24 saat, geçen hafta geçen ay). 
   * Sorgu sayısı.
   * Toplama işlevi.
     
     ![ayarlar](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Sorguyu ayrıntılarını görüntüleme
Sorgu Ayrıntıları görüntülemek için:

1. Herhangi bir sorgu listesinde en sık kullanılan sorguların'ı tıklatın.
   
    ![ayrıntılar](./media/sql-database-query-performance/details.png)
2. Ayrıntılar görünümünü açar ve zaman içinde sorguları CPU süresi/tüketim/yürütme sayısı ayrılmıştır.
3. Ayrıntılar için grafik etrafında'ı tıklatın.
   
   * Seçili sorgu tarafından tüketilen CPU % çubukları olan ve genel veritabanı DTU % satırıyla üst grafik gösterir.
   * İkinci grafik toplam süre seçilen sorgunun gösterir.
   * Alt grafik seçili sorgu tarafından yürütmeleri toplam sayısını gösterir.
     
     ![Sorgu Ayrıntıları][3]
4. İsteğe bağlı olarak, kaydırma çubuklarını kullanın, Yakınlaştırma düğmeleri veya tıklatın **ayarları** sorgu verinin nasıl görüntüleneceğini özelleştirme ya da farklı bir zaman aralığı seçin.

## <a name="review-top-queries-per-duration"></a>Üst sorguları süre her gözden geçirin
Olası performans sorunlarını belirlemenize yardımcı olabilecek iki yeni ölçümleri gösterdiğimizi sorgu performansı öngörüleri son güncelleştirme: süresi ve yürütme sayısı.<br>

Uzun süre çalışan sorgular uzun kaynakları kilitleme, diğer kullanıcıları engelleyen ve ölçeklenebilirlik sınırlamak için en büyük potansiyeline sahiptir. Bunlar ayrıca en iyi duruma getirme için en iyi bir aday değildir.<br>

Uzun süre çalışan sorgular tanımlamak için:

1. Açık **özel** sorgu performansı öngörüleri bir sekmede Seçilen veritabanı için
2. Değiştirme olmasını ölçümleri **süresi**
3. Sorgular ve Gözlem aralığı sayısını seçin
4. Toplama işlevi seçin
   
   * **Sum** tüm gözlem aralığı sırasında tüm sorgu yürütme süresini ekler.
   * **Max** tüm gözlem aralığında hangi yürütme süresi en fazla sorguları bulur.
   * **Ortalama** bulur ortalama yürütme süresi, tüm sorgu yürütmeleri ve bu ortalamalar dışında üstü göster. 
     
     ![Sorgu süresi][4]

## <a name="review-top-queries-per-execution-count"></a>Üst sorguları yürütme sayısı her gözden geçirin
Çok sayıda yürütmeleri değil etkileyen veritabanının kendisi ve kaynak kullanımı düşük olabilir, ancak genel uygulama alabilirsiniz yavaş.

Bazı durumlarda, artırmak için çok yüksek yürütme sayısı açabilir gidiş dönüş ağ. Gidiş dönüş performansını önemli ölçüde etkiler. Ağ gecikmesi tabidir ve aşağı akış sunucusu gecikme oldukları. 

Örneğin, birçok veri tabanlı Web sitesi yoğun her kullanıcının istek için veritabanına erişir. Yardımcı olur, artan ağ trafiğini havuzu ve veritabanı sunucusunda yük işlemeyi bağlantı performansını olumsuz etkileyebilir yapılırken.  Gidiş dönüş mutlak minimum değerde tutmak için genel öneriler değil.

Sık tanımlamak için sorgular ("chatty") sorguları yürütülen:

1. Açık **özel** sorgu performansı öngörüleri bir sekmede Seçilen veritabanı için
2. Değiştirme olmasını ölçümleri **yürütme sayısı**
3. Sorgular ve Gözlem aralığı sayısını seçin
   
    ![sorgu yürütme sayısı][5]

## <a name="understanding-performance-tuning-annotations"></a>Performans ayarlama ek açıklamaları anlama
Sorgu performansı öngörüleri, iş yükü keşfetme çalışırken, dikey çizgi grafiği üstünde simgelerle fark edebilirsiniz.<br>

Bu simgeleri ek açıklamaları; yine de uygun istiyor musunuz? tarafından gerçekleştirilen eylemler etkileyen performans temsil [SQL Azure veritabanı Danışmanı](sql-database-advisor.md). Vurgulama ek açıklama tarafından eylem hakkındaki temel bilgileri alın:

![Sorgu ek açıklaması][6]

Daha fazla bilgi edinmek veya Danışmanı öneriyi uygulamak istiyorsanız, simgeyi tıklatın. Eylem ayrıntılarını açar. Etkin bir öneri ise hemen komutunu kullanarak uygulayabilirsiniz.

![Sorgu ek açıklama Ayrıntıları][7]

### <a name="multiple-annotations"></a>Birden çok ek açıklama.
Yakınlaştırma düzeyi nedeniyle, diğer yakın olan ek açıklamaları birine daraltılmış olduğunu, mümkündür. Bu özel simgesi ile temsil edilir, tıklatarak nerede ek açıklamaları listesi gruplandırılmış açık yeni dikey penceresi gösterilir.
Sorgular ve performans Eylemler ayarlama ilişkilendirme, iş yükü daha iyi anlamak için yardımcı olabilir. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Sorgu performansı öngörüleri için Query Store yapılandırma en iyi duruma getirme
Aşağıdaki sorgu deposu iletileri kullanımınız sorgu performansı öngörüleri sırasında karşılaşabileceğiniz:

* "Query Store düzgün bu veritabanında yapılandırılmamış. Daha fazla bilgi edinmek için burayı tıklatın."
* "Query Store düzgün bu veritabanında yapılandırılmamış. Ayarları değiştirmek için burayı tıklatın." 

Query Store yeni veri toplamanın mümkün değilse, bu iletiler genellikle görünür. 

Query Store salt okunur durumda ve en iyi şekilde parametrelerinin ilk durumda olur. Bu sorgu deposunun boyutunu artırmayı veya sorgu deposunun temizlenmesi düzeltebilirsiniz.

![qds düğmesi][8]

Query Store kapalı olduğu veya parametreler en iyi şekilde oluşturulmamış ikinci durumda olur. <br>Bekletme ve yakalama ilkesini değiştirmek ve aşağıdaki komutları çalıştırarak veya doğrudan portalından Query Store etkinleştirin:

![qds düğmesi][9]

### <a name="recommended-retention-and-capture-policy"></a>Önerilen bekletme ve yakalama İlkesi
Bekletme ilkeleri iki tür vardır:

* Bağlı - boyutu AUTO olarak ayarlanırsa, otomatik olarak en büyük boyutuna ulaşıldığında veri temizleyecek durumunda.
* Bunun anlamı, Query Store alana çalıştıracaksanız, sorgu bilgileri 30 günden eski silecek 30 gün için ayarlarız varsayılan olarak zamana dayalı

Yakalama ilke kümesine:

* **Tüm** -tüm sorgular yakalar.
* **Otomatik** -sık sorgular ve önemsiz derleme ve çalıştırma süresi sorgularıyla yok sayılır. Eşikleri yürütme sayısı, derleme ve çalışma zamanı süresi için dahili olarak belirlenir. Varsayılan seçenek budur.
* **Hiçbiri** -Query Store yeni sorgular yakalamayı durdurur ancak, çalışma zamanı istatistikleri zaten yakalanan sorgular için hala toplanır.

Tüm ilkeleri otomatik olarak ve 30 gün için temiz ilke olarak ayarlanması önerilir:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Query Store boyutunu artırın. Bu, bir veritabanına bağlanma ve sorgu verme tarafından gerçekleştirilebilir:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Beklemek istemiyorsanız, Query Store temizleyebilirsiniz ancak bu ayarları uygulamak yeni sorgular toplama sorgu deposu sonunda hale getirir. 

> [!NOTE]
> Aşağıdaki sorgu yürütme ilişkin sorgu Deposu'nda tüm geçerli bilgileri siler. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Özet
Sorgu performansı öngörüleri sorgu iş yükü etkisini ve veritabanı kaynak tüketimi ilişkilendirilme şekli anlamanıza yardımcı olur. Bu özellik, kaynak tüketen sorguları üst hakkında bilgi edinin ve kolay bir sorun haline gelmeden önce düzeltmek için gerekenleri belirleyin.

## <a name="next-steps"></a>Sonraki adımlar
SQL veritabanınızın performansını iyileştirme konusunda ek öneriler için tıklatın [önerileri](sql-database-advisor.md) üzerinde **sorgu performansı öngörüleri** dikey.

![Performans Danışmanı](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

