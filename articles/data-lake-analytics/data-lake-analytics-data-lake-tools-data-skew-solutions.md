---
title: Visual Studio için Azure Data Lake araçları kullanarak veri eğme sorunlarını çözmek | Microsoft Docs
description: Visual Studio için Azure Data Lake araçları kullanarak veri eğme sorunları için sorun giderme olası çözümleri.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 2e1d33b5d2392832899fd30636e9d40231fc74ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Visual Studio için Azure Data Lake araçları kullanarak veri eğme sorunlarını gidermek

## <a name="what-is-data-skew"></a>Eğme veri nedir?

Kısaca belirtildiği gibi veri eğme bir aşırı gösterilen değeridir. Vergi döndürür, her ABD durumu için bir examiner denetlemek için 50 vergi examiners atadığınız düşünün. Popülasyonun var. küçük olduğu için Wyoming examiner az yapmak için vardır. İzmir'de, ancak examiner durumun büyük popülasyon nedeniyle çok meşgul tutulur.
    ![Veri eğme sorun örneği](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

Senaryomuzda verileri düz olmayan şekilde tüm vergi examiners arasında bazı examiners diğerlerinden daha çalışmalıdır anlamı dağıtılır. Kendi işinde sık vergi examiner örnek gibi durumlarla karşılaşırsınız. Daha fazla teknik koşulları bir köşesinin eşlerine çok daha fazla veri alır, projenin tamamı sonunda diğerlerinden daha fazla ve bu iş köşe yapan bir durum yavaşlatır. Kötüsü, işi başarısız olabilir, köşe, örneğin, olabileceğinden 5 saatlik çalışma zamanı sınırlaması ve 6 GB bellek kısıtlaması.

## <a name="resolving-data-skew-problems"></a>Veri eğme sorunlarını çözme

Visual Studio için Azure Data Lake araçları, işinizi veri eğme sorunlu olup olmadığını belirlemenize yardımcı olabilir. Bir sorun varsa, bu bölümdeki çözümleri deneyerek çözülebilir.

## <a name="solution-1-improve-table-partitioning"></a>Çözüm 1: Tablo bölümleme geliştirmek

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a>Seçenek 1: asimetrik anahtar değeri önceden filtre

İş mantığınızın etkilemez varsa, yüksek sıklığı değerlerin önceden filtre uygulayabilirsiniz. Örneğin, çok sayıda 000 000 000 GUID sütununda varsa, bu değer bir araya istemeyebilirsiniz. Önce toplama, yazabileceğiniz "WHERE GUID!"000 000 000"=" yüksek sıklıkta değeri filtre uygulamak için.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Seçenek 2: farklı bir bölüm veya dağıtım anahtarı seçin

Önceki örnekte, yalnızca vergi denetim iş yükü ülke'dünyanın dört bir yanındaki denetlemek istiyorsanız, anahtar olarak kimlik numarasını seçerek veri dağıtım artırabilir. Farklı bir bölüm veya dağıtım anahtarı çekme bazen verileri daha eşit olarak dağıtabilirsiniz, ancak bu seçenek, iş mantığınızı etkilemez emin olmanız gerekir. Örneğin, her durum için vergi toplam hesaplamak için belirtmek istediğiniz _durumu_ bölüm anahtarı olarak. Bu sorunu yaşamaya devam ederseniz, seçenek 3 kullanmayı deneyin.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Seçenek 3: daha fazla bölüm veya dağıtım anahtarları Ekle

Yalnızca kullanmak yerine _durumu_ bir bölüm anahtarı olarak bölümleme için birden fazla anahtar kullanabilirsiniz. Örneğin, eklemeyi göz önünde bulundurun _posta kodu_ veri bölüm boyutlarını küçültmek ve verileri daha eşit olarak dağıtmak için ek bölüm anahtarı olarak.

### <a name="option-4-use-round-robin-distribution"></a>Seçenek 4: hepsini dağıtım kullanın

Bölüm ve dağıtım için uygun bir anahtar bulamazsanız, hepsini dağıtım kullanmayı deneyebilirsiniz. Hepsini dağıtım tüm satırları eşit olarak değerlendirir ve rastgele bunları karşılık gelen demet yerleştirir. Veri dağılımla, ancak yere göre bilgi, aynı zamanda bazı işlemler için iş performansı düşürebilir bir dezavantajı kaybeder. Ayrıca, asimetrik anahtar için toplama yine de yapıyorsanız, veri eğme sorun korunur. Hepsini bir kez deneme dağıtımı hakkında daha fazla bilgi için U-SQL tablo dağıtımları bölümüne bakın. [CREATE TABLE (U-SQL): şemasıyla tablo oluşturma](https://msdn.microsoft.com/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-the-query-plan"></a>Çözüm 2: sorgu planı geliştirmek

### <a name="option-1-use-the-create-statistics-statement"></a>Seçenek 1: CREATE STATISTICS deyimini kullanın

U-SQL CREATE STATISTICS deyimi tablolarda sağlar. Bu bildirimi sorgu iyileştiricisi bir tabloda depolanan verileri, gibi özellikleri değer dağıtımı hakkında daha fazla bilgi sağlar. Sorguların çoğu için sorgu iyileştiricisi zaten bir yüksek kaliteli sorgu planı gerekli istatistikleri oluşturur. Bazen, CREATE STATISTICS ile ek istatistikler oluşturarak veya sorgu tasarım değiştirerek sorgu performansını artırmak gerekebilir. Daha fazla bilgi için bkz: [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/library/azure/mt771898.aspx) sayfası.

Kod örneği:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>İstatistik bilgileri otomatik olarak güncelleştirilmez. Bir tablodaki verileri istatistikleri tekrar oluşturmak zorunda kalmadan güncelleştirirseniz, sorgu performansı reddet.

### <a name="option-2-use-skewfactor"></a>Seçenek 2: SKEWFACTOR kullanın

Her durum için vergi toplamak istiyorsanız, GROUP BY durumu, veri eğme kaçınmak olmayan bir yaklaşım kullanmanız gerekir. Ancak, böylece iyileştirici sizin için yürütme planı hazırlama veri eğme anahtarlarında tanımlamak için Sorgunuzdaki veri ipucu sağlar.

Genellikle, parametre 0,5 ve konuya eğme ve 1 anlamı ağır eğme anlamı 0,5 ile 1 olarak ayarlayabilirsiniz. İpucu geçerli deyimi ve tüm aşağı akış deyimleri için yürütme planı iyileştirme etkilediğinden, olası key-wise toplama eğri önce ipucu eklediğinizden emin olun.

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
Diğer birleştirilmiş satır kümesini küçük olduğunu biliyorsanız, SKEWFACTOR ek olarak, belirli eğri anahtarı birleşim durumlarda, size iyileştirici birleştirme önce U-SQL deyiminde ROWCOUNT ipucu ekleyerek söyleyebilir. Bu şekilde, en iyi hale getirme performansını iyileştirmeye yardımcı olmak için bir yayın katılım stratejisi seçebilirsiniz. ROWCOUNT veri eğme sorunu çözmezse, ancak bazı ek Yardım sunabileceğiniz unutmayın.

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

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a>Çözüm 3: kullanıcı tanımlı reducer ve birleştirici geliştirmek

Karmaşık bir işlem mantığı ile mücadele etmek için bir kullanıcı tarafından tanımlanan işleci bazen yazabilir ve iyi yazılmış reducer ve birleştirici bir veri eğme sorun bazı durumlarda azaltmak.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Seçenek 1: bir özyinelemeli reducer mümkünse kullanın

Varsayılan olarak, kullanıcı tanımlı bir reducer anahtarı için iş azaltan anlamına tek köşe dağıtılmış özyinelemeli olmayan modda çalışır. Ancak verilerinizi eğri, büyük veri kümeleri, tek bir köşe işlenmesi ve uzun bir süredir çalıştırın.

Performansı artırmak için kodunuzda özyinelemeli modunda çalışacak şekilde reducer tanımlamak için bir öznitelik ekleyebilirsiniz. Ardından, büyük veri kümeleri için birden çok köşeleri dağıtılabilen ve işinizi hızlandırır paralel olarak çalıştır.

Özyinelemeli olmayan reducer için özyinelemeli değiştirmek için algoritma ilişkilendirilebilir olduğundan emin olmak gerekir. Örneğin, toplam ilişkilendirilebilir ve ORTANCA değil. Giriş ve çıkış reducer için aynı şema tutmak emin olmanız gerekir.

Özyinelemeli reducer öznitelik:

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

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Seçenek 2: satır düzeyi Birleştirici modu, mümkünse kullanın

Benzer şekilde belirli eğri anahtarı birleşim durumlarda ROWCOUNT ipucu, böylece iş aynı anda çalıştırılabilecek birden çok köşe için çok büyük eğri anahtar değer kümeleri dağıtmak Birleştirici modu çalışır. Birleştirici modu veri eğme sorunları çözümlenemiyor ancak büyük eğri anahtar değer kümeleri için bazı ek Yardım sağlayabilir.

Varsayılan olarak, sol satır kümesi ve doğru satır kümesini ayırmanız edemiyor demektir tam Birleştirici modudur. Sol/sağ/iç modunu ayarlama satır düzeyi birleştirme sağlar. Sistem, ilgili satır kümeleri ayırır ve bunları paralel olarak çalışan birden çok köşeleri uygulamasına dağıtır. Ancak, birleştirici modu yapılandırmadan önce ilgili satır kümeleri birbirinden emin olmak dikkatli olun.

Aşağıdaki örnek, ayrılmış sol satır kümesini gösterir. Tek bir giriş satır soldan her çıktı satır bağlıdır ve büyük olasılıkla tüm satırlardan aynı anahtar değeriyle sağ bağımlı. Birleştirici modu sol ayarlarsanız, sistem büyük sol-küçük parçalara satır kümesi ayırır ve birden çok köşeleri atar.

![Birleştirici modu çizim](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Yanlış Birleştirici modu ayarlarsanız, daha az verimli bir bileşimidir ve sonuçları bir sorun olabilir.

Birleştirici modu öznitelikleri:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Soldan (ve büyük olasılıkla tüm satırların aynı anahtar değeriyle sağdan) tek bir giriş satır her çıktı satır bağlıdır.

- qlUserDefinedCombiner(Mode=CombinerMode.Right): her çıktı satır sağa (ve büyük olasılıkla tüm satırların aynı anahtar değeriyle soldan) tek bir giriş satır bağlıdır.

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Sol ve sağda aynı değere sahip tek bir giriş satırdaki her çıktı satır bağlıdır.

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
