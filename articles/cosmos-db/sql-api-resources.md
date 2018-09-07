---
title: Azure Cosmos DB kaynak modeli ve kavramları | Microsoft Docs
description: Azure Cosmos DB hiyerarşik modelinin veritabanları, koleksiyonlar, kullanıcı tanımlı işlev (UDF), belgelerin, kaynakları ve daha fazlasını yönetmek için izinleri hakkında bilgi edinin.
keywords: Hiyerarşik modeli, cosmosdb, azure, Microsoft azure
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0869881ace689d12272affb38d3689965e107e8f
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050941"
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Azure Cosmos DB hiyerarşik kaynak modeli ve temel kavramları

Azure Cosmos DB tarafından yönetilen veritabanı varlıklar olarak adlandırılır **kaynakları**. Her kaynak, mantıksal bir URI ile benzersiz şekilde tanımlanır. Standart HTTP fiilleri, istek/yanıt üst bilgileri ve durum kodlarını kullanarak kaynaklarla etkileşim kurabilir. 

Bu makalede, aşağıdaki soruları yanıtlamaktadır:

* Azure Cosmos DB'nin kaynak modeli nedir?
* Ne sistem kaynakları yerine kullanıcı tarafından tanımlanan kaynakları tanımlanır?
* Bir kaynağı nasıl ele?
* Koleksiyonları ile nasıl çalışır?
* Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevlerle (UDF) ile nasıl çalışırım?

## <a name="hierarchical-resource-model"></a>Hiyerarşik kaynak modeli
Aşağıdaki diyagramda gösterildiği gibi Azure Cosmos DB hiyerarşik **kaynak modeli** mantıksal ve kararlı bir URI her adreslenebilir bir veritabanı hesabı altında kaynak kümesi oluşur. Bir kaynak kümesini denir bir **akışı** bu makaledeki. 

> [!NOTE]
> Azure Cosmos DB sunar, ayrıca RESTful kendi iletişim modelde olduğu gibi bir üst düzeyde verimli TCP protokolü aracılığıyla [SQL .NET istemcisi API](sql-api-sdk-dotnet.md).
> 
> 

![Azure Cosmos DB hiyerarşik kaynak modeli][1]  
**Hiyerarşik kaynak modeli**   

Kaynaklar ile çalışmaya başlamak için şunları yapmanız gerekir [bir veritabanı hesabı oluşturma](create-sql-api-dotnet.md) Azure aboneliğinizi kullanarak. Bir veritabanı hesabı bir dizi oluşabilir **veritabanları**, her biri birden çok içeren **koleksiyonları**, her sırayla içerir, **saklı yordamlar, Tetikleyiciler, UDF'ler, belgeler ve ilgili Ekleri**. Bir veritabanı aynı zamanda ilişkili **kullanıcılar**, her bir dizi **izinleri** koleksiyonları, saklı yordamlar, Tetikleyiciler, UDF'ler, belgeler veya ekleri erişmek için. Veritabanları, kullanıcılar, izinler ve Koleksiyonlar iyi bilinen şemalar sahip sistem tanımlı kaynakları olsa da, belgeler ve ekleri ise rastgele ve kullanıcı tanımlı JSON içeriği bulunur.  

| Kaynak | Açıklama |
| --- | --- |
| Veritabanı hesabı |Bir veritabanı hesabı, veritabanı ve blob depolama için ek sabit miktarda bir dizi ile ilişkilidir. Azure aboneliğinizi kullanarak bir veya daha fazla veritabanı hesabı oluşturabilirsiniz. Daha fazla bilgi için ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Database |Bir veritabanı, koleksiyonlar genelinde bölümlenmiş belge depolama alanının mantıksal bir kapsayıcıdır. Ayrıca kullanıcılar kapsayıcısına bir hizmettir. |
| Kullanıcı |İçin izinlerin kapsamını belirlerken mantıksal ad alanı. |
| İzin |Belirli bir kaynağa erişim için bir kullanıcı ile ilişkili bir yetkilendirme belirteci. |
| Koleksiyon |Koleksiyon, JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. Koleksiyonlar bir veya daha fazla bölümü/sunucuyu kapsayabilir ve neredeyse sınırsız miktarda depolama veya işlemeyi işleyebilecek şekilde ölçeklendirilebilir. |
| Saklı Yordam |Uygulama mantığı bir koleksiyonuna kayıtlı olan ve işlemsel olarak veritabanı altyapısının içinde yürütülmesini JavaScript dilinde yazılan. |
| Tetikleyici |Önce veya sonra bir ya da bir INSERT, yürütülen JavaScript'te yazılmış uygulama mantığını değiştirin ya da silme işlemi. |
| UDF |JavaScript'te yazılmış uygulama mantığı. UDF özel sorgu işleci model ve böylece sorgu dili SQL API'si temel genişletmek etkinleştirin. |
| Belge |Kullanıcı tanımlı (rastgele) JSON içeriği bulunur. Varsayılan olarak, şema tanımlanması gerekir ya da ikincil dizinlerin bir koleksiyona eklenen tüm belgeler için sağlanması gerekmez. |
| Ek |Ek başvurular ve dış blob/medya için ilişkili meta verileri içeren özel bir belgedir. Geliştirici, Cosmos DB tarafından yönetilen blob ya da OneDrive, Dropbox vb. gibi dış blob hizmeti sağlayıcısıyla depolamak seçebilirsiniz. |

## <a name="system-vs-user-defined-resources"></a>Sistem ve kullanıcı tanımlı kaynakları
Veritabanı hesapları, veritabanları, koleksiyonlar, kullanıcılar, izinler, saklı yordamlar, tetikleyiciler ve UDF'ler - gibi kaynakları, tüm sabit bir şemaya sahip ve sistem kaynakları olarak adlandırılır. Buna karşılık, belgeler ve ekler gibi kaynaklar sınırlanmazlar şemaya ve kullanıcı tanımlı kaynakları örnekleridir. Cosmos DB'de sistem ve kullanıcı tanımlı kaynakları temsil edilen ve standart uyumlu JSON olarak yönetilir. Tüm kaynaklar, sistem veya kullanıcı tanımlı, aşağıdaki ortak özelliklere sahiptir:

> [!NOTE]
> Bir kaynak tüm sistem tarafından oluşturulan özelliklerinde kendi JSON gösterimi, bir altçizgi (_) öneki alır.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Özellik</strong></p></td>
            <td valign="top"><p><strong>Kullanıcı tarafından ayarlanabilir veya sistem tarafından oluşturulan?</strong></p></td>
            <td valign="top"><p><strong>Amacı</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan, kaynağın benzersiz ve hiyerarşik tanımlayıcısı</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>ETag iyimser eşzamanlılık denetimi için gereken kaynak</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>Kaynağın son güncelleştirilen zaman damgası</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>Kaynağın benzersiz adreslenebilir URI</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Ya da</p></td>
            <td valign="top"><p>Kullanıcı tanımlı benzersiz adı kaynak (ile aynı bölüm anahtarı değeri). Kullanıcı kimliğiniz belirtmiyor, sistem tarafından oluşturulan bir kimliği olur.</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Hat gösterimine kaynakları
Cosmos DB, JSON standart veya özel Kodlamalar herhangi bir mülkiyet uzantısı kılmaz; Standart uyumlu JSON belgeleri ile çalışır.  

### <a name="addressing-a-resource"></a>Bir kaynak adresleme
URI adreslenebilir tüm kaynaklardır. Değerini **_self** özelliği olan bir kaynağın kaynak göreli URI temsil eder. URI biçimi oluşan /\<akışı\>/ {_rid} yol kesimleri:  

| _Self değeri | Açıklama |
| --- | --- |
| /dbs |Bir veritabanı hesabı altında veritabanlarının akışı |
| /dbs/ {dbName} |Eşleşen değere {dbName} kimliğine sahip veritabanı |
| {dbName} /DBS/ /colls/ |Akış koleksiyonları altında bir veritabanı |
| {dbName} /DBS/ /colls/ {collName} |' % S'değeri {collName} eşleşen bir kimliğe sahip koleksiyon |
| {dbName} /DBS/ /colls/ {collName} / docs |Belgelerin koleksiyonu altında akışı |
| /dbs/{dbName}/colls/{collName}/docs/{docId} |{Belge} değerle eşleşen bir kimliğine sahip belge |
| {dbName} /DBS/ /users/ |Bir veritabanı altında kullanıcı akışı |
| /dbs/{dbName}/users/{userId} |{User} değerle eşleşen bir kimliğe sahip kullanıcı |
| {dbName} /DBS/ /users/ {UserID} / izinleri |Akış kapsamındaki bir kullanıcı izinleri |
| {dbName} /DBS/ /users/ {UserID} /permissions/ {permissionId} |{İzni} değerle eşleşen bir kimliğine sahip iznin |

Her kaynak kimliği özelliği aracılığıyla kullanıma sunulan benzersiz bir kullanıcı tanımlı adı vardır. Not: kullanıcı, bir kimliği belirtmezse belgeler için SDK'ları otomatik olarak belge için benzersiz bir kimliği oluşturur. Belirli bir üst kaynak bağlamı içinde benzersiz olan en fazla 256 karakter kullanıcı tanımlı bir dize kimliğidir. 

Her kaynak _rid özelliği aracılığıyla kullanılabilir olan bir sistem tarafından oluşturulan hiyerarşik kaynak tanımlayıcısı (bir RID da bilinir) de vardır. Belirli bir kaynağın tüm bir hiyerarşiye RID kodlar ve dağıtılmış bir şekilde tutarlılığı zorunlu tutmak için kullanılan uygun iç gösterimi ise. RID bir veritabanı hesabı içinde benzersiz olan ve çapraz bölüm aramaları gerek kalmadan verimli yönlendirme için Cosmos DB tarafından dahili olarak kullanılır. _Self _rid özellikleri ve bir kaynak alternatif ve canonical temsillerini değerlerdir. 

REST API'leri, kaynakları adreslenmesini ve isteklerinin kimliği hem _rid özelliklerini destekler.

## <a name="database-accounts"></a>Veritabanı hesapları
Azure aboneliğinizi kullanarak bir veya daha fazla Cosmos DB veritabanı hesabı sağlayabilirsiniz.

Oluşturma ve Cosmos DB veritabanı hesapları Azure portal aracılığıyla yönetme [ http://portal.azure.com/ ](https://portal.azure.com/). Oluşturma ve bir veritabanı hesabı yönetme yönetici erişimi gerektiren ve yalnızca Azure aboneliğiniz kapsamındaki gerçekleştirilebilir. 

### <a name="database-account-properties"></a>Veritabanı hesabı özellikleri
Sağlama ve bir veritabanı hesabı yönetmenin bir parçası olarak yapılandırın ve aşağıdaki özellikleri okuyun:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Özellik adı</strong></p></td>
            <td valign="top"><p><strong>Açıklama</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Tutarlılık İlkesi</p></td>
            <td valign="top"><p>Veritabanı hesabınız kapsamında tüm koleksiyonlar için varsayılan tutarlılık düzeyini yapılandırmak için bu özelliği ayarlayın. [X-ms-tutarlılık-düzey] istek üst bilgisini kullanarak istek başına tutarlılık düzeyinde geçersiz kılabilirsiniz. <p><p>Bu özellik yalnızca uygulanır <i>kullanıcı tanımlı kaynakları</i>. Tüm sistem tanımlı kaynakları destekleyecek şeiklde yapılandırılan okuma/güçlü tutarlılık ile sorgulama.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Yetkilendirme anahtarları</p></td>
            <td valign="top"><p>Tüm kaynaklar veritabanı hesabı altında yönetimsel erişim sağlayan birincil ve ikincil ana ve salt okunur anahtarları.</p></td>
        </tr>
    </tbody>
</table>

Sağlamaya ek olarak, yapılandırma ve veritabanı hesabınızı Azure portalından yönetme ayrıca programlı olarak oluşturabilir ve Cosmos DB veritabanı hesaplarını kullanarak yönetme [Azure Cosmos DB REST API'lerini](/rest/api/cosmos-db/) yanısıra[istemci SDK'ları](sql-api-sdk-dotnet.md).  

## <a name="databases"></a>Veritabanları
Bir Cosmos DB veritabanı bir mantıksal bir veya daha fazla koleksiyonlarını ve kullanıcılar, aşağıdaki diyagramda gösterildiği gibi kapsayıcıdır. Veritabanları bir Cosmos DB veritabanı hesabı teklif sınırları tabi altında herhangi bir sayıda oluşturabilirsiniz.  

![Veritabanı hesabı ve koleksiyonları hiyerarşik modeli][2]  
**Bir mantıksal kapsayıcıdır kullanıcıları ve koleksiyon veritabanı**

Bir veritabanı içinde koleksiyonlar bölümlenmiş sınırsız belge depolaması içerebilir.

### <a name="elastic-scale-of-an-azure-cosmos-db-database"></a>Azure Cosmos DB veritabanı için esnek ölçeklendirme
Birkaç GB ile için SSD destekli belge depolaması ve sağlanan aktarım hızı petabaytlarca arasında değişen varsayılan – esnek bir Cosmos DB veritabanıdır. 

Geleneksel RDBMS bir veritabanı bir Cosmos DB veritabanında tek bir makineye kapsamında değil. Cosmos DB ile uygulamanızın ölçeğini büyütmek gerektiğinde, koleksiyonlar, veritabanları ya da her ikisini de oluşturabilirsiniz. Aslında, Microsoft içindeki çeşitli birinci taraf uygulamalar Azure Cosmos DB Tüketici ölçekte son derece büyük bir Azure Cosmos DB veritabanları koleksiyonları içeren her binlerce terabaytlık belge depolama ile oluşturarak kullanıyor olmalısınız. Büyütür veya bir veritabanı ekleyerek veya kaldırarak, uygulamanızın ölçek gereksinimlerini karşılamak için koleksiyonları Daralt. 

Herhangi bir sayıda teklif bağlı olan bir veritabanı içinde koleksiyonlar oluşturabilirsiniz. Her koleksiyon veya koleksiyonları (veritabanında) kümesi, SSD depolama ve sizin için seçilen öneri bağlı olarak sağlanan aktarım hızı desteklenen.

Azure Cosmos DB veritabanı de kullanıcıların bir kapsayıcıdır. Bir kullanıcı, Aç, ayrıntılı yetkilendirme ve koleksiyonlar, belgeler ve ekleri için erişim sağlayan bir izin kümesi için mantıksal bir ad alanıdır.  

Azure Cosmos DB kaynak modeldeki diğer kaynaklarla veritabanları, değiştirilmesi silinmiş oluşturulabilir gibi okuma veya kolayca kullanarak numaralandırılan [REST API'leri](/rest/api/cosmos-db/) veya herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). Azure Cosmos DB, okuma veya veritabanı kaynak meta verileri sorgulamak için güçlü tutarlılık garanti eder. Veritabanı otomatik olarak silme koleksiyonları veya içerdiği kullanıcıları erişemez sağlar.   

## <a name="collections"></a>Koleksiyonlar
Bir Cosmos DB koleksiyonu, JSON belgeleri için bir kapsayıcıdır. 

### <a name="elastic-ssd-backed-document-storage"></a>Esnek SSD destekli belge depolama
Bir koleksiyon doğası gereği elastik - otomatik olarak büyür ve belgeleri ekleyip olarak küçülür. Koleksiyonlar, mantıksal kaynaklardır ve bir veya daha fazla fiziksel bölüm veya sunucuları yayılabilir. Bir koleksiyona atanan bölüm sayısı, Cosmos DB depolama boyutuna göre ve koleksiyonu veya koleksiyon kümesi için sağlanan işleme tarafından belirlenir. Cosmos DB'de her bölüm, sabit bir tutar, SSD destekli depolamanın ilişkili olan ve yüksek kullanılabilirlik için çoğaltılır. Bölüm yönetim Azure Cosmos DB tarafından tamamen yönetilir ve karmaşık kod yazın veya bölüm yönetmek gerekmez. Cosmos DB koleksiyonları **sınırsız** depolama ve aktarım hızı açısından. 

### <a name="automatic-indexing-of-collections"></a>Koleksiyonları otomatik dizin oluşturma
Azure Cosmos DB true şemasız veritabanı sistemidir. Varsaymaz veya JSON belgeleri için herhangi bir şema gerektirir. Bir koleksiyona belgelere ekleme, Azure Cosmos DB bunları otomatik olarak dizinleyen ve sorgulama için kullanılabilir. Otomatik belgelerin dizin şema veya ikincil dizinler gerek kalmadan Azure Cosmos DB'nin temel bir özelliktir ve yazma için iyileştirilmiş, kilit içermeyen ve günlük yapılı bir dizin bakım teknikleri tarafından etkinleştirilir. Azure Cosmos DB Sürdürülen birim hala tutarlı sorguları hizmet sırasında son derece hızlı yazma destekler. Belge hem dizin depolama her koleksiyon tarafından tüketilen depolama hesaplamak için kullanılır. Depolama ve performans bir koleksiyon için dizin oluşturma ilkesini yapılandırarak dizin ile ilişkili stillerden denetleyebilirsiniz. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Bir koleksiyonun dizin oluşturma ilkesini yapılandırma
Dizin oluşturma ilkesini her koleksiyonun performans ve depolama dizini oluşturma ile ilişkili dengelemeler yapmanızı sağlar. Dizin yapılandırmasının bir parçası olarak, aşağıdaki seçenekler kullanılabilir:  

* Koleksiyon otomatik olarak tüm belgeler veya dizin oluşturulup oluşturulmadığını seçin. Varsayılan olarak, tüm belgelerin otomatik olarak dizine eklenir. Otomatik dizin oluşturmayı kapatmak ve seçmeli olarak yalnızca belirli belgeler dizine eklemek seçebilirsiniz. Buna karşılık, yalnızca belirli belgelere dışlanacak seçerek belirleyebilirsiniz. Otomatik özelliği true veya false indexingPolicy koleksiyonunun üzerinde olacak şekilde ayarlayarak ve ekleme, değiştirme veya bir belgeyi silme sırasında [x-ms-indexingdirective] istek üst bilgisini kullanarak bunu gerçekleştirebilirsiniz.  
* Dahil etmek veya belirli yollar veya belgelerinizi desenleri dizinden hariç tutmak isteyip istemediğinizi seçin. Ayar includedPaths ve bir koleksiyonun indexingPolicy üzerinde excludedPaths sırasıyla bunu gerçekleştirebilirsiniz. Ayrıca, depolama ve performans stillerden aralığı ve karma sorgular için belirli bir yol desenleri için yapılandırabilirsiniz. 
* Arasında zaman uyumlu (tutarlı) seçin ve zaman uyumsuz (lazy) dizin güncelleştirmeler. Varsayılan olarak, dizin üzerinde her ekleme, değiştirme veya silme koleksiyonuna bir belgenin zaman uyumlu olarak güncelleştirilir. Bu belge okuma işlemleri olarak aynı tutarlılık düzeyi tarafından gönderilen sorguları sağlar. Azure Cosmos DB yazma için iyileştirilmiş ve belge yazma zaman uyumlu dizin Bakımı ve tutarlı sorguları hizmet ile birlikte Sürdürülen birimleri destekler olsa da, belirli koleksiyonlar kendi dizini gevşek güncelleştirmek için yapılandırabilirsiniz. Yavaş dizin yazma performansı artırır ve öncelikli olarak okuma yoğun koleksiyonlar için toplu alımı senaryolar için idealdir.

Dizin oluşturma ilkesini koleksiyonunda PUT yürüterek değiştirilebilir. Bu, aracılığıyla elde edilen [istemci SDK'sı](sql-api-sdk-dotnet.md), [Azure portalında](https://portal.azure.com), veya [REST API'leri](/rest/api/cosmos-db/).

### <a name="querying-a-collection"></a>Bir koleksiyonu sorgulama
Bir koleksiyonu içindeki belgeler rastgele şemalar sahip olabilir ve herhangi bir şemayı ya da ikincil dizinlerin önceden sağlamadan bir koleksiyonu içindeki belgeler sorgulayabilirsiniz. Koleksiyonunu kullanarak sorgulayabilirsiniz [Azure Cosmos DB SQL söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/dn782250.aspx), zengin hiyerarşik, ilişkisel ve uzamsal işleçler ve UDF'ler JavaScript tabanlı aracılığıyla genişletilebilirlik sağlar. JSON dil bilgisi, ağaç olarak JSON belgelerinin ağaç düğümleri olarak etiketli modelleme sağlar. Bu hem SQL API'SİNİN otomatik dizin oluşturma teknikleri hem de Azure Cosmos DB SQL diyalekti tarafından kötüye. SQL sorgu dili, üç ana yönlerini oluşur:   

1. Doğal olarak hiyerarşik sorgular ve tahminleri dahil olmak üzere ağaç yapısına harita sorgu işlemleri küçük bir dizi. 
2. Bir alt kümesini oluşturma, filtre, tahminler, toplamlar ve kendinden birleştirmeler dahil olmak üzere ilişkisel işlemleri. 
3. Saf JavaScript tabanlı: çalışmak UDF'ler (1) ve (2).  

Azure Cosmos DB sorgu modelini işlevselliği, verimlilik ve Basitlik arasında bir denge dener. Azure Cosmos DB veritabanı altyapısı, yerel olarak derler ve SQL sorgu deyimleri yürütür. Bir koleksiyon kullanarak sorgulayabilirsiniz [REST API'leri](/rest/api/cosmos-db/) veya herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). .NET SDK'sı bir LINQ sağlayıcısı ile birlikte gelir.

> [!TIP]
> SQL API'sini deneme ve bizim veri kümesinde SQL sorguları çalıştırmanızı [sorgu oyun alanı](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Çok Belgeli işlemler
Veritabanı işlemleri verilerin eş zamanlı yapılan değişiklikler başa çıkmak için güvenli ve tahmin edilebilir bir programlama modeli sağlar. RDBMS iş mantığı yazmak için geleneksel yolu yazmaktır **saklı yordamlar** ve/veya **Tetikleyicileri** ve işlem tabanlı yürütmesi için veritabanı sunucusu için gönderin. RDBMS iki farklı programlama dilleriyle dağıtılacak uygulama programcısı gereklidir: 

* (İşlem olmayan) uygulama programlama dili (örneğin, JavaScript, Python, C#, Java, vb.)
* T-SQL veritabanı tarafından yerel olarak yürütülen işlem programlama dili

Yürütülen JavaScript uygulama mantığının doğrudan koleksiyonlar açısından saklı yordamları üzerinden derin taahhüdü sayesinde JavaScript ve JSON doğrudan veritabanı altyapısının içinde Azure Cosmos DB sezgisel programlama modeli sağlar ve tetiklenir. Bu hem aşağıdakileri sağlar:

* Verimli uygulaması eşzamanlılık denetimi, JSON nesnesi grafiklerin doğrudan veritabanı altyapısının içinde dizin otomatik kurtarma
* Denetim akışı doğal olarak ifade etmek, değişken kapsamı, atama ve özel durum işleme temelleri doğrudan programlama dilini JavaScript açısından veritabanı işlemleriyle tümleştirme

Koleksiyon düzeyinde kayıtlı JavaScript mantığının sonra verilen koleksiyon veritabanı işlemleri belgeler üzerinde verebilir. Azure Cosmos DB, JavaScript tabanlı örtük olarak sarmalar saklı yordamları ve Tetikleyicileri anlık görüntü yalıtımıyla çevresel ACID işlemi içinde bir koleksiyonu içindeki belgeler üzerinde. Yürütme sürecinde JavaScript bir özel durum oluşturursa tüm işlem iptal edilir. Sonuçta elde edilen programlama modeli basittir güçlü. JavaScript geliştiricileri yine de kendi tanıdık dil yapıları ve kitaplık temelleri kullanırken "kalıcı" bir programlama modeli alın.   

Doğrudan arabellek havuzu ile aynı adres alanında veritabanı altyapısının içinde JavaScript yürütme yeteneği, iyi performanslı ve bir koleksiyonun belgelerde veritabanı işlemlerini işlem tabanlı olarak yürütülmesini sağlar. Ayrıca, JSON için derin bir taahhüt Cosmos DB'nin veritabanı altyapısı yapar ve JavaScript uygulama türü sistemleri ve veritabanı arasındaki tüm empedans uyuşmazlığı ortadan kaldırır.   

Bir koleksiyonu oluşturduktan sonra saklı yordamlar, tetikleyiciler ve UDF'ler koleksiyonunu kullanarak kaydedebilirsiniz [REST API'leri](/rest/api/cosmos-db/) veya herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). Kayıt sonrasında başvurabilir ve bunları yürütün. Aşağıdaki saklı yordamı tamamen JavaScript'te yazılmış göz önünde bulundurun, aşağıdaki kod (kitap adı ve yazarın adı) iki bağımsız değişkeni alır ve yeni bir belge oluşturur, bir belge için sorgular ve ardından içerdiği tüm örtük bir ACID işlemi içinde güncelleştirir. Bir JavaScript özel durum oluşturulursa, yürütme sırasında herhangi bir noktada, tüm işlemi durdurur.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

İstemci "Yukarıdaki JavaScript mantığının işlem tabanlı yürütmesi HTTP POST aracılığıyla veritabanına sevk edebilir". HTTP yöntemleri kullanma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB kaynaklarını RESTful etkileşim](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Veritabanı yerel olarak JSON ve JavaScript anlıyor olduğundan, hiçbir tür sistemi uyuşmazlığı, yok "Veya eşleme" veya gerekli kod oluşturma Sihirli dikkat edin.   

Bir koleksiyon ve belgelerin geçerli koleksiyon içeriği sunan bir iyi tanımlanmış bir nesne modeli üzerinden bir koleksiyondaki saklı yordamları ve Tetikleyicileri etkileşim.  

SQL API'si koleksiyonlarında oluşturulabilir, silinen, okuma veya numaralandırılmış kolayca kullanarak [REST API'leri](/rest/api/cosmos-db/) veya herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). SQL API'si, her zaman okuma veya bir koleksiyonun meta verileri sorgulamak için güçlü tutarlılık sağlar. Otomatik olarak bir koleksiyon silme, belgeler, ekleri, saklı yordamlar, Tetikleyiciler erişemez ve UDF'ler içerdiği sağlar.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF)
Önceki bölümde açıklandığı gibi doğrudan veritabanı altyapısının içinde bir işlem içinde çalıştırmak için uygulama mantığı yazabilirsiniz. Uygulama mantığını tamamen JavaScript'te yazılmış ve bir saklı yordam, tetikleyici veya bir UDF modellenir. JavaScript kodunu bir saklı yordam ya da bir tetikleyici içinde ekleyebilirsiniz, değiştirme, silme, okuma veya sorgu bir koleksiyonu içindeki belgeler. Öte yandan, bir UDF içinden JavaScript olamaz ekleyin, değiştirin veya belgeler silme. UDF'ler, sorgunun sonuç kümesinde belgelerinin numaralandırır ve başka bir sonuç kümesi üretir. Çoklu kiracı için Azure Cosmos DB, katı ayırma tabanlı kaynak İdaresi zorlar. Her bir saklı yordam, tetikleyici veya bir UDF, işlemini gerçekleştirmek için işletim sistemi kaynaklarının sabit bir kuantum alır. Ayrıca, saklı yordamlar, tetikleyiciler ve UDF'ler karşı dış JavaScript kitaplıklarını bağlanamıyor ve bunlar için ayrılmış kaynak bütçeleri aşarsanız kara listede. Kaydetme, saklı yordamlar, tetikleyiciler ve UDF'ler REST API'lerini kullanarak bir koleksiyon ile kaydı kaldırma.  Kayıt sırasında bir saklı yordam, tetikleyici veya bir UDF önceden derlenmiş ve daha sonra yürütülen bayt kodu depolanır. Aşağıdaki bölümde, kaydetme, yürütün ve bir saklı yordam, tetikleyici ve bir UDF kaydını silmek için Azure Cosmos DB JavaScript SDK'sını nasıl kullanabileceğinizi gösterir. JavaScript SDK'sı basit üzerindeki bir sarmalayıcıdır [REST API'leri](/rest/api/cosmos-db/). 

### <a name="registering-a-stored-procedure"></a>Bir saklı yordam kaydedilemedi
Bir saklı yordam kayıt yeni bir saklı yordam kaynak HTTP POST aracılığıyla bir koleksiyon oluşturur.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Bir saklı yordam yürütme
İstek gövdesindeki yordam parametreleri geçirerek varolan bir saklı yordam kaynağa karşı bir HTTP POST göndererek depolanan yordamının yürütülmesi gerçekleştirilir.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Bir saklı yordam kaydı siliniyor
Bir saklı yordam kaydını varolan bir saklı yordam kaynağa karşı bir HTTP DELETE veren tarafından gerçekleştirilir.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Öncesi tetiği kaydetme
Bir tetikleyici kaydını HTTP POST aracılığıyla bir koleksiyon üzerinde yeni bir tetikleyici kaynak oluşturarak yapılır. Tetikleyici bir öncesi veya sonrası tetikleyici ve işlem türü (örneğin, oluşturma, değiştirme, silme veya tüm) ilişkilendirilmiş olabilir, belirtebilirsiniz.   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Öncesi bir tetikleyici yürütme
Bir belge kaynak isteği üstbilgisi aracılığıyla PUT/POST/DELETE isteği veren zaman varolan bir tetikleyiciyi adını belirterek bir tetikleyici yürütmesi gerçekleştirilir.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Öncesi bir tetikleyici kaydı siliniyor
Bir tetikleyici kaydını var olan bir tetikleyici kaynağa karşı bir HTTP DELETE veren aracılığıyla gerçekleştirilir.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Bir UDF kaydediliyor
Bir UDF kaydı HTTP POST aracılığıyla bir koleksiyon üzerinde yeni bir UDF kaynak oluşturarak yapılır.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Sorgunun bir parçası olarak bir UDF çalıştırma
Bir UDF SQL sorgusunun bir parçası belirtilebilir ve çekirdek genişletmek için bir yol kullanılan [SQL sorgu dili](sql-api-sql-query-reference.md).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Bir UDF kaydı siliniyor
Bir UDF kaydı var olan bir UDF kaynağa karşı bir HTTP DELETE vererek yalnızca gerçekleştirilir.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Yukarıdaki kod aracılığıyla kayıt (POST), silme (PUT), / liste okuma (GET) ve yürütme (POST) gösteriyordu ancak [JavaScript SDK'sı](https://github.com/Azure/azure-documentdb-js), ayrıca [REST API'leri](/rest/api/cosmos-db/) veya diğer [istemci SDK'ları](sql-api-sdk-dotnet.md). 

## <a name="documents"></a>Belgeler
Ekleyin, değiştirin, silebilir, okuma, listeleme ve rastgele JSON belgelerinin bir koleksiyondaki sorgu. Azure Cosmos DB, herhangi bir şema kılmaz ve ikincil dizinler koleksiyonu içindeki belgeler üzerinde sorgulama desteklemek için gerekli değildir. Bir belge için boyut üst sınırı 2 MB'dir.   

Gerçek anlamda açık bir veritabanı hizmeti olan, Azure Cosmos DB herhangi bir özel veri türleri (örneğin, tarih saat) veya belirli kodlamaları JSON belgeleri için stok değil. Azure Cosmos DB, çeşitli belgeler arasındaki ilişkileri kod oluşturma için hiçbir özel JSON kurallarını gerektirmez; Azure Cosmos DB SQL söz dizimini güçlü hiyerarşik ve ilişkisel sorgu işleçleri için sorgu ve proje belgelerini herhangi bir özel ek açıklamaları veya ilişkileri belgeleri kullanarak kod oluşturma gerek olmadan ayırt edici özellikleri sağlar.  

Diğer tüm kaynaklar ile belgeleri, değiştirilmesi silindi, okuma, oluşturulabilir gibi listelenmiş ve REST API'leri veya herhangi bir kolayca kullanarak sorgulanan [istemci SDK'ları](sql-api-sdk-dotnet.md). Tüm iç içe geçmiş ekleri için karşılık gelen kota ayarlama, anında bir belgeyi silme serbest bırakır. Belge okuma tutarlılık düzeyi veritabanı hesabındaki tutarlılık İlkesi izler. Bu ilke, uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak istek başına temelinde geçersiz kılınabilir. Belgeleri sorgulama sırasında okuma tutarlılığı koleksiyonunda belirlenen dizin oluşturma modu izler. "Tutarlı olmasını sağlamak için", bu hesabın tutarlılık İlkesi izler. 

## <a name="attachments-and-media"></a>Ekleri ve medya
Azure Cosmos DB sayesinde ikili blobları/medya depolamak için Azure Cosmos DB (hesap başına en fazla 2 GB) ile veya kendi Uzak medya mağazasında. Ayrıca, bir ortam eki adlı özel bir belgede açısından meta verileri temsil etmek sağlar. Ek Azure Cosmos DB'de depolanan başka bir yerde medya/blob başvuran bir özel (JSON) belgedir. Bir uzak medya depolanan bir ortam meta veriler (örneğin, konum, yazar vb.) yakalayan yalnızca özel bir belgede ektir. 

Yer işaretleri, Derecelendirmeler, beğenilerin/Beğenmediklerinizi vb. bir e-kitap için belirli bir kullanıcı, ilişkili meta verileri, Yorumlar dahil olmak üzere vurgular ve açıklamaların depolamak için Azure Cosmos DB kullanan bir sosyal okuma uygulamayı düşünün.   

* Kitap içeriğini ya da medya depolama alanında depolanan Azure Cosmos DB veritabanı hesabını parçası veya bir uzak medya deposu olarak kullanılabilir. 
* Bir uygulama her kullanıcının meta verileri ayrı bir belge olarak depolayabilir--Kitap1 Can'ın meta verilerini /colls/joe/docs/book1 tarafından başvurulan bir belge gibi depolanır. 
* Bir kullanıcının belirli bir kitap içerik sayfalarına işaret eden ekler, ilgili belge altında Örneğin, /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 vb. depolanır. 

Yukarıda listelenen örnekler, kaynak hiyerarşisi iletmek için kolay bir kimlik kullanın. Benzersiz kaynak kimlikleri üzerinden REST API'leri aracılığıyla erişilen kaynaklar. 

Azure Cosmos DB tarafından yönetilen ortam için ortam URI'sini tarafından ekin _ortam özelliği başvuruyor. Azure Cosmos DB emin olmanızı tüm bekleyen başvuruları bırakıldığında ortam için çöp toplama. Azure Cosmos DB, otomatik olarak yeni bir medyayı karşıya yükleme eki oluşturur ve yeni eklenen medyaya işaret edecek şekilde _ortam doldurur. Sizin tarafınızdan (örneğin, OneDrive, Azure depolama, DropBox, vb.) yönetilen bir uzak blob deposuna medya depolamayı seçerseniz, medyanın başvurmak için ekleri yine de kullanabilirsiniz. Bu durumda, ek kendiniz oluşturmanız ve kendi _ortam özelliğini doldurmak.   

Diğer tüm kaynaklar ile ekleri oluşturulabilir, yerini, silindi, okuma veya REST API veya istemci SDK'larından birini kullanarak kolayca numaralandırılır. Belgelerle yönteminde olduğu gibi veritabanı hesabındaki tutarlılık İlkesi ekleri okuma tutarlılık düzeyini izler. Bu ilke, uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak istek başına temelinde geçersiz kılınabilir. Ekler için sorgulanırken okuma tutarlılığı koleksiyonunda belirlenen dizin oluşturma modu izler. "Tutarlı olmasını sağlamak için", bu hesabın tutarlılık İlkesi izler. 
 

## <a name="users"></a>Kullanıcılar
Bir Azure Cosmos DB kullanıcısının izinleri gruplandırması için mantıksal bir ad alanı temsil eder. Bir Azure Cosmos DB kullanıcısının, bir kimlik yönetimi sistemi veya önceden tanımlanmış uygulama rolü bir kullanıcıya karşılık gelebilir. Azure Cosmos DB için bir kullanıcı yalnızca bir veritabanı altında izin kümesini gruplamak için bir soyutlamayı temsil eder.   

Uygulamanızda çok kiracılı uygulama için kullanıcıların gerçek kullanıcılarınıza veya uygulamanızın kiracılar için karşılık gelen Azure Cosmos DB içinde oluşturabilirsiniz. Ardından, çeşitli koleksiyonlar, belgeler, ekleri, vb. bir erişim denetimi karşılık gelen belirli bir kullanıcının izinlerini oluşturabilirsiniz.   

Uygulamalarınızı, kullanıcı büyümesi ile ölçeklendirmek gereksinim duyduğunuz kadar parça için çeşitli yollar verilerinizi benimseyebilirsiniz. Tüm kullanıcılarınıza gibi model oluşturabilirsiniz:   

* Her kullanıcı için bir veritabanı eşler.
* Her kullanıcı bir koleksiyona eşler. 
* Birden fazla kullanıcıya karşılık gelen belgeleri adanmış bir koleksiyonuna gidin. 
* Birden fazla kullanıcıya karşılık gelen belge koleksiyonları kümesine gidin.   

Belirli bir parçalama stratejisi ne olursa olsun seçtiğiniz Azure Cosmos DB veritabanındaki kullanıcılar olarak gerçek kullanıcıların model ve her kullanıcı için ayrıntılı izinler ilişkilendirin.  

![Kullanıcı koleksiyonları][3]  
**Parçalama stratejileri ve modelleme kullanıcılar**

Diğer kaynaklar gibi kullanıcıların Azure Cosmos DB'de oluşturulabilir, değiştirilen, silindi, okuma veya REST API veya istemci SDK'ları birini kullanarak kolayca numaralandırılır. Azure Cosmos DB, her zaman okuma veya bir kullanıcı kaynak meta verileri sorgulamak için güçlü tutarlılık sağlar. Bu, belirtmemiz otomatik olarak bir kullanıcının silinmesi içerdiği izinleri erişemediğini sağlar olur. Silinen kullanıcı arka planda bir parçası olarak Azure Cosmos DB kota izinleri geri kazanır olsa da, silinen izinleri anında yeniden kullanılabilir kullanabilmeniz için.  

## <a name="permissions"></a>İzinler
Erişim denetimi açısından, veritabanı hesapları, veritabanları, kullanıcılar ve izin gibi kaynakları olarak kabul edilir *Yönetim* kaynaklar olduğundan, bu yönetim izinleri gerektirir. Öte yandan, koleksiyonlar gibi kaynakları belgeler, ek saklı yordamlar, Tetikleyiciler, ve UDF'ler altında belirli bir veritabanı kapsamlı ve kabul *uygulama kaynaklarını*. Kaynaklar ve onlara (yani yönetici ve kullanıcı) erişim rolleri iki türü için karşılık gelen, yetkilendirme modelini iki tür tanımlar *erişim anahtarları*: *ana anahtarı* ve  *Kaynak anahtarı*. Ana anahtarı veritabanı hesabı, bir parçasıdır ve geliştirici (veya yönetici) için sağlanan kimin veritabanı hesabı sağlama. Hem yönetim hem de uygulama kaynaklara erişim yetkisi vermek için kullanılabilir, bu ana anahtarı yönetici semantiği vardır. Buna karşılık, bir kaynak anahtarı erişimine izin veren bir ayrıntılı erişim anahtarı olan bir *belirli* Uygulama kaynağı. Bu nedenle, bir veritabanı kullanıcısı ve kullanıcı izinlerini belirli bir kaynak için (örneğin, toplama, belge, ek, saklı yordam, tetikleyici veya UDF) arasındaki ilişkiyi yakalar.   

Bir kaynak anahtarı almak için tek yolu, belirli bir kullanıcı altında bir izni kaynak oluşturmaktır. Oluşturma veya izni almak için bir ana anahtar yetkilendirme üst bilgisinde sunulmalıdır. Kaynak erişimini ve kullanıcı izni kaynak bağlar. Bir izni kaynak oluşturduktan sonra kullanıcının yalnızca ilgili kaynak erişim kazanmak için ilişkili kaynak anahtarı sunmak olmalıdır. Bu nedenle, bir kaynak anahtarı mantıksal ve compact kaynağının bir temsili izni görüntülenebilir.  

Diğer tüm kaynaklar ile Azure Cosmos DB'de izinleri oluşturulabilir, yerini, silindi, okuma veya REST API veya istemci SDK'larından birini kullanarak kolayca numaralandırılır. Azure Cosmos DB, her zaman okuma veya iznin meta verileri sorgulamak için güçlü tutarlılık sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
HTTP komutları kullanarak kaynakları ile çalışma hakkında daha fazla bilgi [Azure Cosmos DB kaynaklarını RESTful etkileşim](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/sql-api-resources/resources1.png
[2]: media/sql-api-resources/resources2.png
[3]: media/sql-api-resources/resources3.png

