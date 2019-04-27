---
title: Azure SQL veritabanı sorgu performansı İçgörüleri | Microsoft Docs
description: Sorgu performansı izleme, bir Azure SQL veritabanı için çoğu CPU kullanan sorguları tanımlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 5d892005881436dec89c0d0d010f7f02e7bdebf9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60585373"
---
# <a name="query-performance-insight-for-azure-sql-database"></a>Azure SQL veritabanı sorgu performansı İçgörüleri

İlişkisel veritabanlarının performansını ayarlama ve yönetme, uzmanlık ve saat sürer. Sorgu performansı İçgörüleri, Azure SQL veritabanı akıllı performans ürün serisi bir parçasıdır. Veritabanı performans sorunlarını giderme sağlayarak daha az zaman ayırmanıza yardımcı olur:

* Veritabanları (DTU) kaynak tüketiminizi ayrıntılı Öngörüler.
* Üst veritabanı sorguları CPU süresi ve yürütme sayısı (olası performans geliştirmeleri için aday ayarlama) ile ilgili ayrıntılar.
* Sorgu metni ve kaynak kullanımı geçmişini görüntülemek için bir sorgu ayrıntılarına detaya gitme olanağı.
* Performans önerilerini göster ek açıklamalar [SQL veritabanı Danışmanı](sql-database-advisor.md).

![Sorgu Performansı İçgörüleri](./media/sql-database-query-performance/opening-title.png)

> [!TIP]
> Azure SQL veritabanı ile temel performansı izleme için sorgu performansı İçgörüleri öneririz. Bu makalede yayımlanmış ürün sınırlamaları unutmayın. Veritabanı performansını izleme Gelişmiş için öneririz [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md). Bu, otomatik performans sorunlarını gidermek için yerleşik zekaya sahiptir. En yaygın veritabanı performans sorunlarından bazıları otomatik olarak ayarlamak için önerilir [otomatik ayarlama](sql-database-automatic-tuning.md).

## <a name="prerequisites"></a>Önkoşullar

Sorgu performansı İçgörüleri gerektirir [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) veritabanınız üzerinde etkindir. Varsayılan olarak tüm Azure SQL veritabanları için otomatik olarak etkinleştirilir. Query Store çalışmıyor, Azure portalını etkinleştirmek isteyip istemediğinizi sorar.

> [!NOTE]
> "Query Store düzgün şekilde bu veritabanında yapılandırılmamış" iletisini portalda görünür değilse [Query Store yapılandırma en iyi duruma getirme](#optimize-the-query-store-configuration-for-query-performance-insight).
>

## <a name="permissions"></a>İzinler

Aşağıdakilere ihtiyacınız [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) sorgu performansı İçgörüleri kullanma izni:

* **Okuyucu**, **sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** izinleri gereklidir üst kaynak tüketen sorguları ve grafikleri görüntülemek için.
* **Sahibi**, **katkıda bulunan**, **SQL DB Katılımcısı**, veya **SQL Server Katılımcısı** izinleri, sorgu metnini görüntülemek için gereklidir.

## <a name="use-query-performance-insight"></a>Sorgu Performansı İçgörüleri’ni kullanma

Sorgu performansı İçgörüleri kolayca kullanılabilir:

1. Açık [Azure portalında](https://portal.azure.com/) ve incelemek istediğiniz veritabanını bulun.
2. Sol taraftaki menüsünden açmak **akıllı performans** > **sorgu performansı İçgörüleri**.
  
   ![Sorgu performansı İçgörüleri menüsünde](./media/sql-database-query-performance/tile.png)

3. İlk sekmesinde, en çok kaynak tüketen sorguları listesini gözden geçirin.
4. Ayrıntılarını görüntülemek için tek bir sorgu seçin.
5. Açık **akıllı performans** > **performans önerileri** ve performans önerisi kullanılabilir olup olmadığını denetleyin. Yerleşik performans önerileri hakkında daha fazla bilgi için bkz. [SQL veritabanı Danışmanı](sql-database-advisor.md).
6. Kaydırıcıları kullanın veya gözlemlenen aralığını değiştirmek için simgeler yakınlaştırma.

   ![Performans Panosu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> SQL veritabanı'nın sorgu performansı İçgörüleri bilgileri işlemek, birkaç saatlik veri yakalamak Query Store gerekir. Sorgu performansı İçgörüleri, zaman aralığı görüntülediğinde grafikleri veritabanı hiçbir etkinlik varsa veya Query Store belirli bir süre boyunca etkin değilse boş olur. Çalışır durumda değilse, Query Store istediğiniz zaman etkinleştirebilirsiniz. Daha fazla bilgi için [en iyi uygulamalar ile Query Store](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store).

## <a name="review-top-cpu-consuming-queries"></a>En çok CPU kullanan sorguları gözden geçirin

İlk kez açtığınızda varsayılan olarak, sorgu performansı İçgörüleri CPU kullanan ilk beş sorgu gösterir.

1. Seçin veya eklemek veya grafikten onay kutularını kullanarak bunları dışlamak için bireysel sorguya temizleyin.

    En üst satırına veritabanına ilişkin genel DTU yüzdesini gösterir. Çubuklar seçili sorgularının seçili zaman aralığı boyunca tüketilen CPU yüzdesini gösterir. Örneğin, varsa **geçen hafta** olduğu belirlenirse, tek bir günde her çubuk temsil eder.

    ![en sık kullanılan sorgular](./media/sql-database-query-performance/top-queries.png)

   > [!IMPORTANT]
   > Gösterilen DTU satır maksimum tüketim değerine bir saatlik dönem içinde toplanır. Sorgu yürütme istatistikleri ile yalnızca üst düzey bir karşılaştırma için tasarlanmıştır. Bazı durumlarda, yürütülen sorgular kıyasla DTU kullanımı çok yüksek görünebilir, ancak bu durum geçerli olmayabilir.
   >
   > Örneğin, bir sorgu DTU % 100 yalnızca birkaç dakika maxed, sorgu performansı İçgörüleri DTU satırında tüm saat tüketiminin % 100 (en yüksek toplu değer sonucu) olarak gösterilir.
   >
   > Daha hassas karşılaştırması için (en fazla bir dakika) bir özel DTU kullanımı grafiği oluşturmayı göz önünde bulundurun:
   >
   > 1. Azure portalında **Azure SQL veritabanı** > **izleme**.
   > 2. **Ölçümler**’i seçin.
   > 3. Seçin **+ Ekle grafik**.
   > 4. DTU yüzdesi grafiği seçin.
   > 5. Ayrıca, seçin **son 24 saat** sol menüsünde ve bir dakika olarak değiştirin.
   >
   > Özel DTU grafik, sorgu yürütme grafiği ile karşılaştırmak için ince bir ayrıntı düzeyi ile kullanın.

   Alt kılavuz görünür sorgular için toplu bilgiler gösterir:

   * Sorgu kimliği, veritabanında bir sorgu için benzersiz bir tanımlayıcıdır.
   * CPU üzerinde toplama işlevi bağlıdır gözlemlenebilir bir aralık boyunca sorgu başına.
   * Her toplama işlevini de bağımlı sorgu süresi.
   * Belirli bir sorgu için yürütmelerin toplam sayısı.

2. Verilerinizi eski hale gelirse seçin **Yenile** düğmesi.

3. Kaydırıcıları kullanın ve Yakınlaştırma düğmeleri Gözlem aralığını değiştirmek ve tüketim ani araştırmak için:

   ![Kaydırıcıları ve yakınlaştırma düğmelerini aralığını değiştirmek için](./media/sql-database-query-performance/zoom.png)

4. İsteğe bağlı olarak seçebileceğiniz **özel** görünümünü özelleştirmek için sekmesinde:

   * Ölçüm (CPU, süre, yürütme sayısı).
   * Zaman aralığı (son 24 saat, geçen hafta veya geçen ay).
   * Sorgu sayısı.
   * Toplama işlevi.
  
   ![Özel sekme](./media/sql-database-query-performance/custom-tab.png)
  
5. Seçin **gidin >** özelleştirilmiş görünümde görmek için düğme.

   > [!IMPORTANT]
   > Sorgu performansı İçgörüleri, ilk seçiminize bağlı olarak 5-20 kullanan sorguları görüntülemek için sınırlıdır. Veritabanınızı üstünde gösterilen olanları ötesinde birçok başka sorgular çalıştırabilir ve bu sorgular grafikte dahil edilmez.
   >
   > Üstünde gösterilen olanları ötesinde daha küçük sorgulara çok sayıda sık çalıştırmak ve çoğu DTU kullanan bir veritabanı iş yükü türü mevcut. Bu sorgular, performans grafiğinde görünmez.
   >
   > Toplam tüketim gözlemlenen süre içinde diğer üst tüketen sorguları'dan küçük olsa da örneğin, bir sorgu DTU önemli miktarda bir süredir kullanmışsa. Böyle bir durumda, bu sorgu, kaynak kullanımı grafiği görünmez.
   >
   > Top sorgusu yürütme sorgu performansı İçgörüleri sınırlamaları ötesinde anlamak ihtiyacınız varsa kullanmayı [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md) izleme ve sorun giderme Gelişmiş veritabanı performans için.
   >

## <a name="view-individual-query-details"></a>Tekli sorgu ayrıntılarını görüntüleme

Sorgu Ayrıntıları görüntülemek için:

1. Herhangi bir sorgu listesinde en sık kullanılan sorgular, seçin.

    ![En sık kullanılan sorgular listesi](./media/sql-database-query-performance/details.png)

   Ayrıntılı bir görünüm açılır. CPU tüketimi, süresi ve yürütme sayısı zamanla gösterilir.

2. Ayrıntılar için hesap özelliklerini seçin.

   * Üst grafik bir satır ile genel veritabanı DTU yüzdesi gösterilmektedir. Seçili sorguyu tüketilen CPU yüzdesi çubukları var.
   * İkinci grafik, seçilen sorgunun toplam süre gösterir.
   * Alt grafik seçilen sorgunun yürütmelerin toplam sayısı gösterilmektedir.

   ![Sorgu ayrıntıları](./media/sql-database-query-performance/query-details.png)

3. İsteğe bağlı olarak, kaydırıcıları kullanmak, yakınlaştırma düğmelerini kullanın veya seçin **ayarları** sorgu verilerin nasıl görüntüleneceğini özelleştirebilirsiniz ya da farklı bir zaman aralığı seçin.

   > [!IMPORTANT]
   > Sorgu performansı İçgörüleri DDL sorgular yakalamaz. Bazı durumlarda, tüm geçici sorgular yakalanmayabilir.
   >

## <a name="review-top-queries-per-duration"></a>En sık kullanılan sorgular süresi başına gözden geçirin

Sorgu performansı İçgörüleri iki ölçümlerde olası performans sorunlarını bulmanıza yardımcı olabilir: süresi ve yürütme sayısı.

Uzun süre çalışan sorgulardan uzun kaynakları, diğer kullanıcıların engelleme ve ölçeklenebilirlik sınırlama büyük potansiyeline sahiptir. Bunlar ayrıca iyileştirme için iyi adaylar hedeflenmiştir.

Uzun süre çalışan sorguları belirlemek için:

1. Açık **özel** sorgu performansı İçgörüleri seçili veritabanı için sekmesinde.
2. Ölçümleri değiştirmek **süresi**.
3. Sorgular ve Gözlem aralık sayısını seçin.
4. Toplama işlevi seçin:

   * **Sum** tüm sorgu yürütme süresini için tüm izleme aralığı ekler.
   * **En fazla** bulur, hangi cinsinden yürütme süresi tüm izleme aralığı için en yüksek olan sorgular.
   * **Ortalama** tüm sorgu yürütme ortalama yürütme süresi'ni bulur ve bu ortalamalar için üst olanları gösterir.

   ![Sorgu süresi](./media/sql-database-query-performance/top-duration.png)

5. Seçin **gidin >** özelleştirilmiş görünümde görmek için düğme.

   > [!IMPORTANT]
   > Sorgu görünümü ayarlama DTU satır güncelleştirmez. DTU satır, en fazla tüketim değeri her zaman aralığını gösterir.
   >
   > Veritabanı DTU tüketimi daha fazla ayrıntı (en fazla bir dakika) anlamak için Azure portalında özel bir grafik oluşturma göz önünde bulundurun:
   >
   > 1. Seçin **Azure SQL veritabanı** > **izleme**.
   > 2. **Ölçümler**’i seçin.
   > 3. Seçin **+ Ekle grafik**.
   > 4. DTU yüzdesi grafiği seçin.
   > 5. Ayrıca, seçin **son 24 saat** sol menüsünde ve bir dakika olarak değiştirin.
   >
   > Sorgu performans grafiği ile karşılaştırmak için özel DTU grafik kullanmanızı öneririz.
   >

## <a name="review-top-queries-per-execution-count"></a>En sık kullanılan sorgular yürütme sayısı başına gözden geçirin

Yürütme yüksek sayıda veritabanı etkileyen değil ve kaynak kullanımının düşük olduğu olsa bile get yavaşlatabilir veritabanı kullanan bir kullanıcı uygulama.

Bazı durumlarda, yüksek yürütme sayısı fazla uyguladığından ağ neden olabilir. Gidiş dönüş performansını etkiler. Bunlar, ağ gecikme değiştirilebilir ve aşağı akış sunucusunu gecikme hedeflenmiştir.

Örneğin, birçok veri tabanlı Web sitesi, yoğun bir şekilde her kullanıcı isteği için veritabanına erişir. Bağlantı havuzu yardımcı olmakla birlikte, daha yüksek ağ trafiği ve veritabanı sunucusu üzerindeki işlem yükü performansını yavaşlatabilir. Genel olarak, gidiş dönüş için en az tutun.

Yürütülen ("geveze") sorgular sık tanımlamak için:

1. Açık **özel** sorgu performansı İçgörüleri seçili veritabanı için sekmesinde.
2. Ölçümleri değiştirmek **yürütme sayısı**.
3. Sorgular ve Gözlem aralık sayısını seçin.
4. Seçin **gidin >** özelleştirilmiş görünümde görmek için düğme.

   ![sorgu yürütme sayısı](./media/sql-database-query-performance/top-execution.png)

## <a name="understand-performance-tuning-annotations"></a>Performans ek açıklamaları ayarlama anlama

Sorgu performansı İçgörüleri iş yükünüzü keşfedilirken bir dikey çizgi grafiğin üstünde simgelerle fark edebilirsiniz.

Bu simgeler, ek açıklamalar. Performans önerilerini gösterdikleri [SQL veritabanı Danışmanı](sql-database-advisor.md). Bir ek açıklama üzerine gelerek, performans önerileri hakkında özet bilgi alabilirsiniz.

   ![Sorgu ek açıklaması](./media/sql-database-query-performance/annotation.png)

Daha fazla bilgi edinin veya Danışman'ın öneri uygulamak istiyorsanız, önerilen eylemi ayrıntılarını açmak için simgeyi seçin. Portaldan bir etkin öneri varsa hemen uygulayabilirsiniz.

   ![Sorgu ek açıklama Ayrıntıları](./media/sql-database-query-performance/annotation-details.png)

Yakınlaştırma düzeyi nedeniyle bazı durumlarda, ek açıklamalar yakın olması birbirleriyle tek bir ek açıklama daraltılır mümkündür. Sorgu performansı İçgörüleri, bu grubu ek açıklama simgesi temsil eder. Grup ek açıklama simgesini seçerek ek açıklamalar listeleyen yeni bir dikey pencere açılır.

İlişkilendirilmesini sorgular ve performans ayarlama işlemlerini iş yükünüzü daha iyi anlamanıza yardımcı olabilir.

## <a name="optimize-the-query-store-configuration-for-query-performance-insight"></a>Query Store yapılandırma sorgu performansı İçgörüleri için en iyi duruma getirme

Sorgu performansı İçgörüleri'ni kullanırken, aşağıdaki Query Store hata iletileri görebilirsiniz:

* "Query Store düzgün şekilde bu veritabanında yapılandırılmadı. Daha fazla bilgi edinmek için buraya tıklayın."
* "Query Store düzgün şekilde bu veritabanında yapılandırılmadı. Ayarları değiştirmek için buraya tıklayın."

Query Store yeni veri topladığınızda bu iletiler genellikle görünür.

Query Store salt okunur durumdadır ve parametreleri en uygun şekilde ayarlandığında ilk harfi olur. Bu, veri deposunun boyutunu artırmayı ya da Query Store temizleme düzeltebilirsiniz. (Query Store işaretini kaldırırsanız, daha önce toplanan tüm telemetri kaybolur.)

   ![Query Store ayrıntıları](./media/sql-database-query-performance/qds-off.png)

Query Store etkin değil ya da parametreleri en uygun şekilde ayarlanmadı ikinci koşul olur. Bekletme ve yakalama ilke değişikliğini ve ayrıca Query Store, tablodan sağlanan aşağıdaki komutları çalıştırarak etkinleştirin [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya Azure portalında.

### <a name="recommended-retention-and-capture-policy"></a>Önerilen saklama ve yakalama İlkesi

İki tür bekletme ilkesini vardır:

* **Bağlı boyutu**: Bu ilke ayarı etkinse, **otomatik**, otomatik olarak en büyük boyut ulaşıldığında veri temizler.
* **Saat esası**: Varsayılan olarak, bu ilke, 30 gün olarak ayarlanır. Query Store alanı yetersiz çalıştırıyorsa, sorgu bilgileri 30 günden eski siler.

Yakalama ilkesi ayarlayabilirsiniz:

* **Tüm**: Query Store, tüm sorguları yakalar.
* **Otomatik**: Query Store seyrek sorguları ve önemsiz derleme ve çalıştırma süresine sahip sorguları yok sayar. Yürütme sayısı, eşik derleme süresi ve çalışma zamanı süresi dahili olarak belirlenir. Varsayılan seçenek budur.
* **Hiçbiri**: Yeni sorgular yakalama Query Store durdurur ancak önceden yakalanan sorgular için çalışma zamanı istatistikleri yine de toplanır.

Tüm ilkeler ayarlama öneririz **otomatik** ve aşağıdaki komutları yürüterek 30 gün için temizleme ilkesini [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya Azure portalında. (Değiştir `YourDB` veritabanı adıyla.)

```sql
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

Query Store bir veritabanına bağlanarak boyutunu [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) Azure portalı ve aşağıdaki sorguyu çalıştıran veya. (Değiştir `YourDB` veritabanı adıyla.)

```T-SQL
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

Bu ayarları uygulama, Query Store yeni sorgular için telemetri toplama sonunda bir hale getirir. Query Store, hemen işlevsel olması gerekiyorsa, Query Store SSMS veya Azure portalı aşağıdaki sorguyu çalıştırarak temizlemek isteğe bağlı olarak seçebilirsiniz. (Değiştir `YourDB` veritabanı adıyla.)

> [!NOTE]
> Aşağıdaki sorgu çalıştıran, Query Store, izlenen tüm daha önce toplanan telemetri silinmesine neden olur.

```SQL
    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;
```

## <a name="summary"></a>Özet

Sorgu performansı İçgörüleri sorgu iş yükü etkisini ve veritabanı kaynak tüketimi için nasıl ilişkili olduğunu anlamanıza yardımcı olur. Bu özellik ile veritabanınızda üst tüketen sorguları hakkında bilgi edinebilir ve bir sorun haline gelmeden önce en iyi duruma getirme sorguları bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Veritabanı performans önerileri için seçin [önerileri](sql-database-advisor.md) sorgu performansı İçgörüleri Gezinti dikey penceresinde.

    ![Öneriler sekmesinde](./media/sql-database-query-performance/ia.png)

* Etkinleştirmeyi düşünün [otomatik ayarlama](sql-database-automatic-tuning.md) ortak veritabanı performans sorunları için.
* Bilgi nasıl [Intelligent Insights](sql-database-intelligent-insights.md) otomatik olarak veritabanı performans sorunlarını gidermenize yardımcı olabilir.
* Kullanmayı [Azure SQL Analytics]( ../azure-monitor/insights/azure-sql.md) Gelişmiş performans, yerleşik zeka ile SQL veritabanları, elastik havuzlar ve yönetilen örnekler büyük filosundan izleme.
