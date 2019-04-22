---
title: Visual Studio için Azure Data Lake araçları kullanarak veri dengesizliği sorunları çözün
description: Visual Studio için Azure Data Lake araçları kullanarak veri dengesizliği sorunları için sorun giderme olası çözümleri.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/16/2016
ms.openlocfilehash: af55c161944447f2e6e2245fbb920803779984ca
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496206"
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Visual Studio için Azure Data Lake araçları kullanarak veri dengesizliği sorunları çözün

## <a name="what-is-data-skew"></a>Eğriltme veri nedir?

Kısaca ifade etmek gerekirse veri dengesizliği bir aşırı gösterilen değerdir. Vergi döndürür, her ABD devlet için bir examiner denetlemek için 50 vergi examiners atamadığınızı düşünün. Popülasyonu yok küçük olduğu için Wyoming examiner az yapmak için vardır. California'da, ancak examiner durumun büyük doldurma nedeniyle çok meşgul tutulur.
    ![Veri dengesizliği sorun örneği](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

Senaryomuzdaki ise veriler eşit olmayan şekilde tüm vergi examiners arasında bazı examiners diğerlerinden daha fazla çalışma gerekir yani dağıtılır. Kendi işinde vergi examiner örnek gibi durumlar sık karşılaşırsınız. Daha fazla teknik terimlerle den eşlerine daha da fazla veriye bir köşe alır, projenin tamamı sonunda diğerlerinden daha fazla ve bu iş köşe yaptığı bir durum yavaşlatır. Kötüsü, işi başarısız olabilir, köşe, örneğin, gerekli olabileceğinden 5 saatlik çalışma zamanı ile ilgili bir sınırlama ve 6 GB bellek ile ilgili bir sınırlama.

## <a name="resolving-data-skew-problems"></a>Veri dengesizliği sorunları çözme

Visual Studio için Azure Data Lake araçları, işinizi veri dengesizliği ile ilgili bir sorun olup olmadığını belirlemenize yardımcı olabilir. Bir sorun varsa, bu bölümdeki çözümleri deneyerek çözebilirsiniz.

## <a name="solution-1-improve-table-partitioning"></a>Çözüm 1: Tablo bölümleme geliştirin

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a>1. seçenek: Asimetrik anahtar değeri önceden Filtrele

İş mantığınızı etkilemez, yüksek frekanslı değerleri önceden filtre uygulayabilirsiniz. Örneğin, 000-000-000 GUID sütunda çok fazla varsa, bu değeri toplamak istemeyebilirsiniz. Önce toplam, yazabileceğiniz "WHERE GUID! ="000-000-000"" yüksek frekanslı değeri filtrelemek için.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>2. seçenek: Farklı bir dağıtım ve bölüm anahtarı seçin

Önceki örnekte, yalnızca ülke karşısında denetim vergi iş yükü denetlemek istiyorsanız, anahtar olarak kimlik numarasını seçerek veri dağıtım artırabilir. Farklı bir bölüm veya dağıtım anahtarı çekme bazen verileri daha eşit dağıtabilirsiniz, ancak bu seçenek iş mantığınızı etkilemediğinden emin emin olmanız gerekir. Örneğin, her durum için vergi toplamı hesaplamak için belirtmek istediğiniz _durumu_ bölüm anahtarı olarak. Bu sorunla karşılaşmaya devam ederseniz seçeneği 3'ü kullanarak deneyin.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Seçenek 3: Daha fazla bölüm veya dağıtım anahtarları Ekle

Yalnızca kullanmak yerine _durumu_ bir bölüm anahtarı olarak bölümleme için birden fazla anahtar kullanabilirsiniz. Örneğin, eklemeyi düşünün _posta kodu_ veri bölüm boyutları azaltmaya ve verileri daha eşit dağıtmaya yönelik ek bir bölüm anahtarı.

### <a name="option-4-use-round-robin-distribution"></a>Seçenek 4: Hepsini bir kez deneme dağıtım kullanın

Bölüm ve dağıtım için uygun bir anahtar bulamazsanız, hepsini bir kez deneme dağıtım kullanmayı deneyebilirsiniz. Hepsini bir kez deneme dağıtım, tüm satırları eşit olarak değerlendirir ve rastgele bunlara karşılık gelen demetlerin içine yerleştirir. Eşit olarak dağıtılmış verileri, ancak yerel konumu bilgileri, bazı işlemler için işlem performansı da azaltabilir bir dezavantajı kaybeder. Ayrıca, veri dengesizliği sorun toplama dengesiz anahtarı için yine de yapıyorsanız açık kalır. Hepsini bir kez deneme dağıtımı hakkında daha fazla bilgi için U-SQL tablo dağıtımlar bölümüne bakın. [CREATE TABLE (U-SQL): Şema ile tablo oluşturma](/u-sql/ddl/tables/create/managed/create-table-u-sql-creating-a-table-with-schema#dis_sch).

## <a name="solution-2-improve-the-query-plan"></a>Çözüm 2: Sorgu planı geliştirin

### <a name="option-1-use-the-create-statistics-statement"></a>1. seçenek: CREATE STATISTICS deyimini kullanın

U-SQL tablolarında CREATE STATISTICS deyim sağlar. Bu ifade sorgu iyileştiricisi tablo içinde saklanan verileri gibi özellikleri değer dağılımı hakkında daha fazla bilgi sağlar. Sorguların çoğu, sorgu iyileştiricisi zaten gerekli istatistikleri yüksek kaliteli sorgu planı oluşturur. Bazen, CREATE STATISTICS ile ek istatistikleri oluşturmak veya sorgu tasarımını değiştirerek sorgu performansını artırmak gerekebilir. Daha fazla bilgi için [CREATE STATISTICS (U-SQL)](/u-sql/ddl/statistics/create-statistics) sayfası.

Kod örneği:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>İstatistik bilgilerini otomatik olarak güncelleştirilmez. İstatistikleri tekrar oluşturmak zorunda kalmadan bir tablodaki verileri güncelleştirirseniz, sorgu performansı reddedebilir.

### <a name="option-2-use-skewfactor"></a>2. seçenek: SQL'da kullanın

Her durum için vergi toplamak istiyorsanız, GROUP BY durumu, veri dengesizliği kaçınmak olmayan bir yaklaşım kullanmanız gerekir. Ancak, sorgu iyileştiricisi yürütme planı sizin için hazırlanabilmeniz adına, veri dengesizliği anahtarları tanımlamak için bir veri ipucuyla sağlayabilirsiniz.

Genellikle, parametre 0,5 ve 1, ölçüde eğriltme ve 1 anlamı ağır eğriltme anlamı 0,5 olarak ayarlayabilirsiniz. İpucu için geçerli deyim ve tüm aşağı akış deyimleri yürütme-planı iyileştirme etkilediğinden, olası tuşlarla ilgili toplama dengesiz önce ipucu eklemeyi unutmayın.

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Kod örneği:

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a>Seçenek 3: ROWCOUNT kullanın  
Birleştirilmiş satır kümesi küçük olduğunu biliyorsanız SQL'da yanı sıra belirli dengesiz anahtarı birleşim durumlarda, iyileştirici birleştirme önce U-SQL deyiminde ROWCOUNT ipucu ekleyerek söyleyebilirsiniz. Bu şekilde iyileştirici performansını artırmak için bir yayın birleştirme stratejisi seçebilirsiniz. Satır sayısı veri dengesizliği sorunu çözmez, ancak bazı ek Yardım sağlayabilir unutmayın.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Kod örneği:

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a>3. çözüm: Birleştirici ve kullanıcı tanımlı Azaltıcı geliştirin

Bazen bir kullanıcı tanımlı işleç karmaşık bir işlem mantığı ile birlikte dağıtılacak yazabilirsiniz ve bazı durumlarda bir veri dengesizliği sorun iyi-yazılan Azaltıcı ve birleştirici azaltabileceğini.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>1. seçenek: Mümkünse bir özyinelemeli Azaltıcı kullanın

Varsayılan olarak, bir kullanıcı tanımlı Azaltıcı anahtarı için iş miktarını azaltmaya anlamına tek bir köşe dağıtılır özyinelemeli olmayan modda çalışır. Ancak verilerinizin dengesiz, büyük veri kümelerini tek bir köşe işlenmesi ve uzun bir süredir çalıştırın.

Performansı artırmak için Azaltıcı özyinelemeli modunda çalışacak şekilde tanımlamak için kodunuzda bir öznitelik ekleyebilirsiniz. Ardından, büyük veri kümeleri için birden fazla köşe dağıtılabilen ve hızlandırır, işi paralel olarak çalıştırmak.

İçin özyinelemeli olmayan özyinelemeli Azaltıcı değiştirmek için algoritmanız ilişkili olduğundan emin olmak gerekir. Örneğin, toplam ilişkilendirilebilir ve ORTANCA değil. Giriş ve çıkış için Azaltıcı aynı şemaya tutmak emin olmanız gerekir.

Özyinelemeli Azaltıcı özniteliği:

    [SqlUserDefinedReducer(IsRecursive = true)]

Kod örneği:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>2. seçenek: Mümkünse satır düzeyinde Birleştirici modu kullanın

Benzer şekilde belirli dengesiz anahtarı birleşim durumları için satır sayısı ipucu, çalışmayı aynı anda çalıştırılabilecek böylece birden fazla köşe için çok büyük dengesiz anahtar değer kümeleri dağıtmak Birleştirici modu çalışır. Birleştirici modu veri dengesizliği sorunları gideremezsiniz ancak büyük dengesiz anahtar değer kümeleri için bazı ek Yardım sağlayabilir.

Varsayılan olarak, sağ satır kümesi ve sol satır kümesini ayrılamayan anlamına gelen tam Birleştirici modudur. Sol/sağ/iç modunu ayarlama, satır düzeyi birleştirme sağlar. Sistem, karşılık gelen satır kümeleri ayırır ve bunları paralel olarak birden fazla köşe uygulamasına dağıtır. Birleştirici modu yapılandırmadan önce ancak karşılık gelen satır kümeleri ayrılabilir emin olmak dikkatli olun.

Aşağıdaki örnek, bir ayrılmış sol satır kümesi gösterir. Tek bir giriş satır soldan her çıkış satır bağlıdır ve potansiyel olarak sağa aynı anahtar değerine sahip tüm satırlarda bağlıdır. Birleştirici modu sol ayarlarsanız, sistemin küçük parçalara ayarlamak büyük sol satır ayırır ve bunları birden çok köşeler için atar.

![Birleştirici modu çizim](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Yanlış Birleştirici modu ayarlarsanız, birlikte daha az verimlidir ve sonuçlar hatalı olabilir.

Birleştirici modu öznitelikleri:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Sol (ve büyük olasılıkla tüm satırların aynı anahtar değerine sahip sağa) tek bir giriş satır her çıkış satır bağlıdır.

- qlUserDefinedCombiner(Mode=CombinerMode.Right): Her çıkış satır, sağa (ve büyük olasılıkla tüm satırların aynı anahtar değerine sahip soldan) tek bir giriş satır bağlıdır.

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Sol ve sağda aynı değere sahip tek bir giriş satırı her çıkış satır bağlıdır.

Kod örneği:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
