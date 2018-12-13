---
title: İçin Azure SQL veritabanı sorgu performansı öngörüleri | Microsoft Docs
description: Sorgu performansı izleme için Azure SQL veritabanı çoğu CPU kullanan sorguları tanımlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: carlrab
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 0dd6430021eed5571a6590f411a68e747cd7b506
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274896"
---
# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL veritabanı sorgu performansı İçgörüleri
İlişkisel veritabanlarının performansını ayarlama ve yönetme önemli uzmanlık ve zaman yatırım gerektiren bir görevdir. Sorgu performansı İçgörüleri, veritabanı performans sorunlarını giderme aşağıdakileri sağlayarak daha az süre beklemesini sağlar:

* Veritabanları (DTU) kaynak tüketiminizi ayrıntılı Öngörüler. 
* Potansiyel olarak daha iyi performans için ayarlanmış CPU/süresi/yürütme sayısı, üst sorgulardan.
* Bir sorgunun ayrıntılarına detaya gitme olanağı, metin ve kaynak kullanımı geçmişini görüntüleyin. 
* Performans ayarı tarafından gerçekleştirilen eylemleri göster ek açıklamalar [SQL Azure veritabanı Danışmanı](sql-database-advisor.md)  



## <a name="prerequisites"></a>Önkoşullar
* Sorgu performansı İçgörüleri gerektirir [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) veritabanınız üzerinde etkindir. Query Store çalışmıyor, portal etkinleştirmek isteyip istemediğinizi sorar.

## <a name="permissions"></a>İzinler
Aşağıdaki [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) sorgu performansı İçgörüleri'ni kullanmak için gereken izinler: 

* **Okuyucu**, **sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** izinleri gereklidir üst görüntülemek için kaynak tüketen sorguları ve grafikleri. 
* **Sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** izinleri, sorgu metnini görüntülemek için gereklidir.

## <a name="using-query-performance-insight"></a>Sorgu performansı İçgörüleri'ni kullanma
Sorgu performansı İçgörüleri kolayca kullanılabilir:

* Açık [Azure portalında](https://portal.azure.com/) ve incelemek istediğiniz veritabanını bulun. 
  * Sol taraftaki menüde, destek ve sorun giderme adımı altında "Sorgu performansı İçgörüleri" seçin.
* İlk sekmesinde, en çok kaynak tüketen sorguları listesini gözden geçirin.
* Ayrıntılarını görüntülemek için tek bir sorgu seçin.
* Açık [SQL Azure veritabanı Danışmanı](sql-database-advisor.md) ve herhangi bir önerimiz kullanılabilir olup olmadığını denetleyin.
* Kaydırıcıları kullanın veya gözlemlenen aralığını değiştirmek için simgeler yakınlaştırma.
  
    ![Performans Panosu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Birkaç saat veri sorgu performansı öngörüleri sağlamak için SQL veritabanı için Query Store tarafından yakalanması gerekir. Veritabanında hiç etkinlik yok veya belirli bir zaman dönemi sırasında Query Store etkin değil, grafikler söz konusu zaman dilimiyle görüntülenirken boş olur. Çalışır durumda değilse, Query Store herhangi bir zamanda sağlayabilir.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>En çok CPU kullanan sorguları gözden geçirin
İçinde [portalı](http://portal.azure.com) aşağıdakileri yapın:

1. Bir SQL veritabanı'na gidin ve tıklatın **tüm ayarlar** > **destek + sorun giderme** > **sorgu performansı içgörüleri**. 
   
    ![Sorgu Performansı İçgörüleri][1]
   
    En sık kullanılan sorgular görünümü açılır ve en çok CPU kullanan sorguları listelenir.
2. Grafik ayrıntıları için geçici bir çözüm'e tıklayın.<br>Çubuklarının seçili zaman aralığı boyunca seçili sorgular tarafından tüketilen CPU % gösterirken genel DTU % veritabanı için en üst satırına gösterir (örneğin, **geçen hafta** seçilen her bir çubuğun bir günü temsil eder).
   
    ![en sık kullanılan sorgular][2]
   
    Alt kılavuz görünür sorgular için toplanan bilgileri temsil eder.
   
   * Sorgu kimliği - sorgu veritabanı içinde benzersiz tanımlayıcısı.
   * CPU başına gözlemlenebilir aralığı sırasında sorgu (toplama işlevini bağlıdır).
   * Sorgu başına süresi (toplama işleve bağlı olarak değişir).
   * Belirli bir sorgu için yürütmelerin toplam sayısı.
     
     Seçin veya eklemek veya grafikten onay kutularını kullanarak bunları dışlamak için bireysel sorguya temizleyin.
3. Verilerinizi eski hale gelirse, tıklatın **Yenile** düğmesi.
4. Kaydırıcıları ve yakınlaştırma düğmelerini Gözlem aralığını değiştirmek ve ani araştırmak için kullanabilirsiniz: ![ayarları](./media/sql-database-query-performance/zoom.png)
5. İsteğe bağlı olarak farklı bir görünüm istiyorsanız, seçebileceğiniz **özel** sekme ve ayarlayın:
   
   * Ölçüm (CPU, süre, yürütme sayısı)
   * Zaman aralığı (son 24 saat, geçen hafta, geçen ay). 
   * Sorgu sayısı.
   * Toplama işlevi.
     
     ![ayarlar](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Tekli sorgu ayrıntılarını görüntüleme
Sorgu Ayrıntıları görüntülemek için:

1. Herhangi bir sorgu listesinde en sık kullanılan sorgular,'a tıklayın.
   
    ![ayrıntılar](./media/sql-database-query-performance/details.png)
2. Ayrıntılar görünümü açılır ve sorguları CPU süresi/tüketim/yürütme sayısı zamanla ayrılmıştır.
3. Grafik ayrıntıları için geçici bir çözüm'e tıklayın.
   
   * Üst grafik ile genel veritabanı DTU % satır, ve çubukları seçili sorgu tarafından tüketilen CPU % olduğunu gösterir.
   * İkinci grafik seçilen sorgunun toplam süreyi gösterir.
   * Alt grafik seçilen sorgunun yürütmelerin toplam sayısı gösterilmektedir.
     
     ![Sorgu Ayrıntıları][3]
4. İsteğe bağlı olarak, kaydırıcıları kullanmak, yakınlaştırma düğmelerini veya tıklayın **ayarları** sorgu verilerin nasıl görüntüleneceğini özelleştirebilirsiniz ya da farklı bir zaman aralığı seçin.

## <a name="review-top-queries-per-duration"></a>En sık kullanılan sorgular süresi başına gözden geçirin
Sorgu performansı İçgörüleri son güncelleştirme, olası performans sorunlarını belirlemenize yardımcı olacak iki yeni ölçüm kullanıma sunduk: süresi ve yürütme sayısı.<br>

Uzun süre çalışan sorgulardan uzun kaynakları, diğer kullanıcıların engelleme ve ölçeklenebilirlik sınırlama büyük potansiyeline sahiptir. Bunlar ayrıca iyileştirme için iyi adaylar.<br>

Uzun süre çalışan sorguları belirlemek için:

1. Açık **özel** Seçilen veritabanı için sekmesinde sorgu performansı İçgörüleri
2. Değiştirme olmasını ölçümleri **süresi**
3. Sorgular ve Gözlem aralık sayısını seçin
4. Toplama işlevini seçin
   
   * **Sum** tüm gözlem zaman aralığı boyunca tüm sorgu yürütme süresini ekler.
   * **En fazla** sorguları tam gözlem aralıkta hangi yürütme süresi en fazla bulur.
   * **Ortalama** bulur ortalama yürütme süresi, tüm sorgu yürütme ve bu ortalamalar dışında üst göster. 
     
     ![Sorgu süresi][4]

## <a name="review-top-queries-per-execution-count"></a>En sık kullanılan sorgular yürütme sayısı başına gözden geçirin
Yüksek yürütmelerinin sayısı değil etkileyen veritabanının kendisi ve kaynak kullanımı düşük olabilir, ancak genel uygulama alabilirsiniz yavaş.

Bazı durumlarda, artırmak için çok yüksek yürütme sayısı açabilir gidiş dönüş ağ. Gidiş dönüş performansını önemli ölçüde etkiler. Ağ gecikme değiştirilebilir ve aşağı akış sunucusunu gecikme değildirler. 

Örneğin, çok sayıda veri odaklı Web siteleri, yoğun bir şekilde her kullanıcı isteği için veritabanına erişir. Olsa da yardımcı olur, artan ağ trafiğini havuzu ve veritabanı sunucusunda yük işlemeyi bağlantı performansını olumsuz yönde etkileyebilir.  Genel öneriler minimuma gidiş dönüş tutmaktır.

Sık tanımlamak için sorgular ("geveze") sorguları yürütüldü:

1. Açık **özel** Seçilen veritabanı için sekmesinde sorgu performansı İçgörüleri
2. Değiştirme olmasını ölçümleri **yürütme sayısı**
3. Sorgular ve Gözlem aralık sayısını seçin
   
    ![sorgu yürütme sayısı][5]

## <a name="understanding-performance-tuning-annotations"></a>Performans ayarlama ek açıklamaları anlama
Sorgu performansı İçgörüleri iş yükünüzü keşfedilirken dikey çizgi grafiğin üstünde simgelerle fark edebilirsiniz.<br>

Bu simgeler, ek açıklamalar, performansı etkileyen tarafından gerçekleştirilen eylemler temsil ettikleri [SQL Azure veritabanı Danışmanı](sql-database-advisor.md). Vurgulama ek açıklama tarafından eylem hakkında temel bilgileri alın:

![Sorgu ek açıklaması][6]

Advisor öneri uygulamak ya da daha fazla bilgi edinmek istiyorsanız, simgesine tıklayın. Bu eylem ayrıntılarını açar. Etkin bir öneri ise hemen komutunu kullanarak uygulayabilirsiniz.

![Sorgu ek açıklama Ayrıntıları][7]

### <a name="multiple-annotations"></a>Birden çok ek açıklama.
Yakınlaştırma düzeyi nedeniyle birbirine yakın olan ek açıklamaları birine daraltılmış, mümkündür. Bu özel bir simge ile temsil edilir, tıklayarak burada ek açıklamaları listesi gruplandırılmış açık yeni dikey pencerede gösterilir.
Sorgular ve performans ayarlama eylemleri ilişkilendirme, iş yükü daha iyi anlamak için yardımcı olabilir. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Query Store yapılandırma sorgu performansı İçgörüleri için en iyi duruma getirme
Sorgu performansı İçgörüleri kullanımını sırasında aşağıdaki Query Store iletileri karşılaşabilirsiniz:

* "Query Store düzgün şekilde bu veritabanında yapılandırılmadı. Daha fazla bilgi edinmek için buraya tıklayın."
* "Query Store düzgün şekilde bu veritabanında yapılandırılmadı. Ayarları değiştirmek için buraya tıklayın." 

Bu iletiler genellikle Query Store yeni veri toplamanın mümkün olmadığı durumlarda görüntülenir. 

İlk harfi Query Store salt okunur durumda ve parametreleri en uygun şekilde ayarlandığında gerçekleşir. Bu, Query Store boyutunu artırmayı veya Query Store temizleyerek düzeltebilirsiniz.

![qds düğmesi][8]

Query Store kapalı veya parametreleri en uygun şekilde ayarlanmadığı ikinci durumda olur. <br>Bekletme ve yakalama ilkeyi değiştirin ve aşağıdaki komutları yürüterek veya Portalı'ndan doğrudan Query Store'ı etkinleştirin:

![qds düğmesi][9]

### <a name="recommended-retention-and-capture-policy"></a>Önerilen saklama ve yakalama İlkesi
İki tür bekletme ilkesini vardır:

* Boyutu göre - OTOMATİĞE ayarla, otomatik olarak en büyük boyuta ulaşıldığında veri temizleyecek durumunda.
* Diğer bir deyişle, Query Store alanı yetersiz çalıştıracaksanız, sorgu bilgileri 30 günden eski siler 30 gün olarak ayarlayacağız varsayılan olarak zaman tabanlı

Yakalama ilke ayarlanabilir:

* **Tüm** -tüm sorgular yakalar.
* **Otomatik** -seyrek sorguları ve önemsiz derleme ve çalıştırma süresine sahip sorguları yok sayılır. Yürütme sayısı, derleme ve çalışma zamanı süresi için eşikler dahili olarak belirlenir. Varsayılan seçenek budur.
* **Hiçbiri** -Query Store yeni sorgular yakalamayı durdurur ancak, çalışma zamanı istatistikleri zaten yakalanan sorgular için yine de toplanır.

Tüm ilkeler, otomatik ve 30 gün için temizleme İlkesi ayarlama öneririz:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Query Store boyutunu artırın. Bu, bir veritabanına bağlanma ve sorgu verme tarafından gerçekleştirilebilir:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Query Store beklemek istemiyorsanız işaretini kaldırabilirsiniz ancak bu ayarları uygulama Query Store yeni sorgular, toplama sonunda hale getirir. 

> [!NOTE]
> Aşağıdaki sorgu yürütme Query Store tüm geçerli bilgileri silinmesine neden olur. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Özet
Sorgu performansı İçgörüleri sorgu İş yükünüzün etkisini ve veritabanı kaynak tüketimi için nasıl ilişkili olduğunu anlamanıza yardımcı olur. Bu özellik, üst kullanan sorguları hakkında bilgi edinin ve kolayca bir sorun haline gelmeden önce düzeltmek için gerekenleri belirlemek.

## <a name="next-steps"></a>Sonraki adımlar
SQL veritabanınızın performansını iyileştirme ile ilgili ek öneriler için tıklatın [önerileri](sql-database-advisor.md) üzerinde **sorgu performansı İçgörüleri** dikey penceresi.

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

