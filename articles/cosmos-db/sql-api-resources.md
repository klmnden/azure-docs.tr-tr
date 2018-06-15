---
title: Azure Cosmos DB kaynak modeli ve kavramları | Microsoft Docs
description: Azure Cosmos veritabanı hiyerarşik modelinin veritabanları, koleksiyonları, kullanıcı tanımlı işlev (UDF), belgeler, kaynakları ve daha fazlasını yönetmek için izinler hakkında bilgi edinin.
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
ms.openlocfilehash: 21b1e69573d2ddd31979e6c23dd7f3bd130cadbe
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34798025"
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Azure Cosmos DB hiyerarşik kaynak modeli ve temel kavramları

Azure Cosmos DB yönetir veritabanı varlıklar olarak adlandırılır **kaynakları**. Her kaynak mantıksal URI'leri ile benzersiz olarak tanımlanır. Standart HTTP fiillerini, istek/yanıt üstbilgilerini ve durum kodlarını kullanarak kaynakları ile etkileşim kurabilirsiniz. 

Bu makalede aşağıdaki sorular yanıtlanmaktadır:

* Azure Cosmos veritabanı kaynak modeli nedir?
* Hangi sistem kaynakları kullanıcı tanımlı kaynakları aksine tanımlanmış?
* Bir kaynak nasıl ele?
* Koleksiyonları ile nasıl çalışır?
* Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) ile nasıl çalışırım?

Aşağıdaki videoda Azure Cosmos DB Program Yöneticisi Barış Liu Azure Cosmos DB kaynak modeli aracılığıyla anlatılmaktadır. 

> [!VIDEO https://www.youtube.com/embed/luWFgTP0IL4]
>
>

## <a name="hierarchical-resource-model"></a>Hiyerarşik kaynak modeli
Aşağıdaki diyagramda gösterildiği gibi Azure Cosmos DB hiyerarşik **kaynak modeli** kaynaklar mantıksal ve kararlı bir URI her adreslenebilir bir veritabanı hesabı altında kümesi oluşur. Bir kaynak kümesi denir bir **akış** bu makalede. 

> [!NOTE]
> Azure Cosmos DB sunar aynı zamanda olan RESTful kendi iletişim modelini, son derece verimli bir TCP protokolü aracılığıyla kullanılabilen [SQL .NET istemcisi API](sql-api-sdk-dotnet.md).
> 
> 

![Azure Cosmos DB hiyerarşik kaynak modeli][1]  
**Hiyerarşik kaynak modeli**   

Kaynaklarla çalışmaya başlamak için şunları yapmanız gerekir [bir veritabanı hesabı oluşturma](create-sql-api-dotnet.md) Azure aboneliğinizi kullanarak. Veritabanı hesabı bir dizi oluşabilir **veritabanları**, her biri birden çok içeren **koleksiyonları**, her sırayla içerir, **saklı yordamlar, Tetikleyiciler, UDF'ler, belgeler ve ilgili Ekleri**. Bir veritabanı da ilişkili **kullanıcılar**, her bir dizi **izinleri** koleksiyonları, saklı yordamlar, Tetikleyiciler, UDF'ler, belgeler veya ekleri erişmek için. Veritabanları, kullanıcılar, izinler ve Koleksiyonlar iyi bilinen şemalar sahip sistem tanımlı kaynakları olsa da, belgeler ve ekleri rasgele, kullanıcı tanımlı JSON içeriği içerir.  

| Kaynak | Açıklama |
| --- | --- |
| Veritabanı hesabı |Veritabanı hesabı, bir dizi veritabanları ve ekleri için blob storage'nın sabit bir tutar ile ilişkilidir. Azure aboneliğinizi kullanarak bir veya daha fazla veritabanı hesabı oluşturabilirsiniz. Daha fazla bilgi için ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Database |Bir veritabanı, koleksiyonlar genelinde bölümlenmiş belge depolama alanının mantıksal bir kapsayıcısıdır. Ayrıca, Kullanıcılar kapsayıcısı unutulmamalıdır. |
| Kullanıcı |İzinlerin kapsamını belirlerken için mantıksal ad alanı. |
| İzin |Belirli bir kaynağa erişim için bir kullanıcı ile ilişkili bir yetki belirteci. |
| Koleksiyon |Koleksiyon, JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. Koleksiyonlar bir veya daha fazla bölümü/sunucuyu kapsayabilir ve neredeyse sınırsız miktarda depolama veya işlemeyi işleyebilecek şekilde ölçeklendirilebilir. |
| Saklı Yordam |Uygulama mantığı ile bir koleksiyon kayıtlı ve işlemsel olarak veritabanı altyapısının içinde yürütülen JavaScript yazılmış. |
| Tetikleyici |Önce veya sonra ya da bir ekleme yürütülen JavaScript'te yazılmış uygulama mantığını değiştirin ya da silme işlemi. |
| UDF |JavaScript'te yazılmış bir uygulama mantığı. UDF'ler özel sorgu işleci model ve böylece SQL API sorgu dili çekirdek genişletmek etkinleştirin. |
| Belge |Kullanıcı tanımlı (rastgele) JSON içeriği bulunur. Varsayılan olarak, hiçbir şema tanımlanması gerekiyor ya da ikincil dizinlerin bir koleksiyona eklenmiş tüm belgeleri için sağlanması gerekmez. |
| Ek |Ek başvurular ve dış blob/medya için ilişkili meta verileri içeren özel bir belgedir. Geliştirici Cosmos DB tarafından yönetilen blob yok veya OneDrive, Dropbox, vb. gibi bir dış blob hizmeti sağlayıcısında depolamak seçebilirsiniz. |

## <a name="system-vs-user-defined-resources"></a>Sistem kaynakları kullanıcı tanımlı karşılaştırması
Veritabanı hesaplarını, veritabanları, koleksiyonları, kullanıcılar, izinler, saklı yordamlar, tetikleyiciler ve UDF'ler - gibi kaynakları tüm sabit şemasına sahip ve sistem kaynaklarını denir. Buna karşılık, belgeler ve ekler gibi kaynakları herhangi bir kısıtlama şemasına sahip ve kullanıcı tanımlı kaynaklar olarak gösterilebilir. Cosmos DB'de sistem ve kullanıcı tanımlı kaynakları temsil ve standart uyumlu JSON olarak yönetilir. Tüm kaynaklar, sistem veya kullanıcı tanımlı, aşağıdaki ortak özellikleri vardır:

> [!NOTE]
> Bir kaynak tüm sistem tarafından oluşturulan özelliklerinde kendi JSON gösterimi, alt çizgi (_) ile öneki alır.
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
            <td valign="top"><p>Sistem tarafından oluşturulan, kaynak benzersiz ve hiyerarşik tanıtıcısı</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>İyimser eşzamanlılık denetimi için gerekli kaynağının ETag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>Kaynağın en son güncelleştirilen zaman damgası</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Sistem tarafından oluşturulan</p></td>
            <td valign="top"><p>Kaynağın benzersiz adreslenebilir URI</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Her iki</p></td>
            <td valign="top"><p>Kullanıcı tanımlı benzersiz kaynağın adı (aynı bölüm anahtarı değeri ile). Kullanıcı Kimliği belirtmiyorsa, sistem tarafından oluşturulan kimliği olan</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Hat gösterimine kaynakların
Cosmos DB herhangi özel uzantıları için JSON standart veya özel Kodlamalar zorunlu kılabilir değil; Standart uyumlu JSON belgeleri ile çalışır.  

### <a name="addressing-a-resource"></a>Bir kaynak adresleme
URI adreslenebilir tüm kaynaklardır. Değeri **_self** kaynak özelliği, kaynağın göreli URI temsil eder. URI biçimi oluşan /\<akış\>/ {_rid} yol kesimleri:  

| _Self değeri | Açıklama |
| --- | --- |
| /dbs |Bir veritabanı hesabı altında veritabanlarının besleme |
| /dbs/ {dbName} |{DbName} değerle eşleşen bir kimliğe sahip veritabanı |
| /dbs/ {dbName} /colls/ |Bir veritabanı altında koleksiyonların besleme |
| /dbs/ {dbName} /colls/ {collName} |{CollName} değerle eşleşen bir kimliğe sahip koleksiyonu |
| /dbs/ {dbName} /colls/ {collName} / docs |Bir koleksiyon altında belgelerin besleme |
| /dbs/{dbName}/colls/{collName}/docs/{docId} |{Doc} değerle eşleşen bir kimliğiyle belge |
| /dbs/ {dbName} /users/ |Kullanıcıların bir veritabanı altında besleme |
| /dbs/{dbName}/users/{userId} |{Kullanıcısı} değerle eşleşen bir kimliğe sahip kullanıcı |
| /dbs/ {dbName} /users/ {UserID} / izinleri |Akış kapsamındaki bir kullanıcı izinleri |
| /dbs/ {dbName} /users/ {UserID} /permissions/ {permissionId} |{İzni} değerle eşleşen bir kimliğe sahip izni |

Her kaynak kimliği özelliği aracılığıyla kullanıma sunulan benzersiz bir kullanıcı tanımlı adı vardır. Not: kullanıcı bir kimlik belirlemezse belgeler için SDK'ları otomatik olarak belge için benzersiz bir kimliği oluşturur. Bir özel üst kaynak bağlamı içinde benzersizdir en fazla 256 karakterden oluşan kullanıcı tanımlı bir dize kimliğidir. 

Her kaynak _rid özelliği aracılığıyla kullanılabilir olduğu bir sistem tarafından oluşturulan hiyerarşik kaynak tanımlayıcısı (bir RID da bilinir) da sahiptir. Tüm belirli bir kaynak hiyerarşisini RID kodlar ve dağıtılmış bir şekilde tutarlılığı zorlamak için kullanılan uygun iç gösterimi ise. RID bir veritabanı hesabı içinde benzersizdir ve çapraz bölüm aramaları gerek kalmadan verimli yönlendirme için Cosmos DB tarafından dahili olarak kullanılır. Bir kaynak alternatif ve kurallı gösterimlerini _self ve _rid özelliklerinin değerlerdir. 

REST API'leri kaynakları adreslenmesini ve isteklerinin kimliği ve _rid özellikleri tarafından destekler.

## <a name="database-accounts"></a>Veritabanı hesapları
Azure aboneliğinizi kullanarak bir veya daha fazla Cosmos DB veritabanı hesaplarını sağlayabilirsiniz.

Oluşturma ve Azure portalında aracılığıyla Cosmos DB veritabanı hesaplarını yönetme [ http://portal.azure.com/ ](https://portal.azure.com/). Oluşturma ve bir veritabanı hesabı yönetme yönetimsel erişim gerektirir ve yalnızca Azure aboneliğinizde gerçekleştirilebilir. 

### <a name="database-account-properties"></a>Veritabanı hesabı özellikleri
Hazırlama ve bir veritabanı hesabı yönetme bir parçası olarak yapılandırın ve aşağıdaki özellikleri okuyun:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Özellik adı</strong></p></td>
            <td valign="top"><p><strong>Açıklama</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Tutarlılık İlkesi</p></td>
            <td valign="top"><p>Veritabanı hesabınız altındaki tüm koleksiyonlar için varsayılan tutarlılık düzeyi yapılandırmak için bu özelliği ayarlayın. [X-ms-tutarlılık-düzey] istek üstbilgisi kullanarak istek başına temelinde tutarlılık düzeyi geçersiz kılabilirsiniz. <p><p>Bu özellik yalnızca uygulanır <i>kullanıcı tanımlı kaynakları</i>. Tanımlanan kaynakları desteklemek için yapılandırılmış olan tüm sistem okuma/ile güçlü tutarlılık sorgular.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Yetkilendirme anahtarları</p></td>
            <td valign="top"><p>Veritabanı hesabı altında kaynakların tümünü yönetimsel erişim sağlayan birincil ve ikincil ana ve salt okunur anahtarları.</p></td>
        </tr>
    </tbody>
</table>

Sağlama ek olarak, yapılandırma ve Azure portalından, veritabanı hesabınızı yönetme program aracılığıyla da oluşturabilir ve Cosmos DB veritabanı hesaplarını kullanarak yönetmek [Azure Cosmos DB REST API'lerini](/rest/api/cosmos-db/) yanısıra[istemci SDK'ları](sql-api-sdk-dotnet.md).  

## <a name="databases"></a>Veritabanları
Cosmos DB veritabanı bir mantıksal bir veya daha fazla koleksiyonlarını ve kullanıcılar, aşağıdaki çizimde gösterildiği gibi kapsayıcıdır. Herhangi bir sayıda teklif sınırları tabi Cosmos DB veritabanı hesabı altındaki veritabanları oluşturabilirsiniz.  

![Veritabanı hesabı ve koleksiyonları hiyerarşik modeli][2]  
**Bir veritabanı kullanıcıları ve koleksiyonlar, mantıksal bir kapsayıcısıdır**

Bir veritabanı içinde koleksiyonlar bölümlenmiş sınırsız belge depolama içerebilir.

### <a name="elastic-scale-of-an-azure-cosmos-db-database"></a>Bir Azure Cosmos DB veritabanının esnek ölçeklendirme
Varsayılan olarak yedeklenen SSD belge depolama ve sağlanan işleme petabaytlarca için birkaç GB ile değişen – esnek bir Cosmos DB veritabanıdır. 

Bir veritabanında geleneksel RDBMS Cosmos DB veritabanında tek bir makineye kapsamlı olmayan. Uygulamanızın ölçeğini büyütmek gerektiği Cosmos DB ile daha çok koleksiyon, veritabanları veya her ikisi de oluşturabilirsiniz. Aslında, Microsoft içindeki çeşitli birinci taraf uygulamalardan Azure Cosmos DB Tüketici ölçekte son derece büyük Azure Cosmos DB veritabanları koleksiyonları içeren her binlerce belge depolama terabayt ile oluşturarak kullanmakta olduğunuz. Büyütür veya bir veritabanı ekleyerek veya kaldırarak, uygulamanızın ölçek gereksinimlerini karşılamak için koleksiyonları küçültür. 

Herhangi bir sayıda teklif tabi bir veritabanı içinde koleksiyonlar oluşturabilirsiniz. Her koleksiyon veya koleksiyonları (içinde bir veritabanı) kümesi, SSD depolama ve işleme bağlı olarak seçilen teklif sağladığınız yedeklenen.

Bir Azure Cosmos DB veritabanı kullanıcı aynı zamanda bir kapsayıcıdır. Bir kullanıcı, Aç, hassas yetkilendirme ve koleksiyonlar, belgeler ve ekleri erişimi sağlayan izinler kümesi için bir mantıksal ad alanıdır.  

Azure Cosmos DB kaynak modeli diğer kaynaklar ile veritabanları, değiştirilmesi silinmiş oluşturulabilir gibi okuma veya kolayca kullanarak numaralandırılan [REST API'leri](/rest/api/cosmos-db/) ya da herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). Azure Cosmos DB okuma veya bir veritabanı kaynak meta verileri sorgulamak için güçlü tutarlılığı garanti altına alır. Veritabanını otomatik olarak silme koleksiyonları veya içerdiği kullanıcılar hiçbirine erişemiyor sağlar.   

## <a name="collections"></a>Koleksiyonlar
Cosmos DB koleksiyon, JSON belgeleri için bir kapsayıcıdır. 

### <a name="elastic-ssd-backed-document-storage"></a>Esnek yedeklenen SSD belge depolama
Bir koleksiyon doğası gereği esnek - otomatik olarak büyüdükçe ve belgeleri ekleyip olarak küçülür. Koleksiyonlar mantıksal kaynaklar ve bir veya daha fazla fiziksel bölümleri veya sunucuları yayılabilir. Bir koleksiyona atanan bölüm sayısı Cosmos depolama boyutuna göre DB ve koleksiyon veya bir koleksiyon kümesi için sağlanan verime göre belirlenir. Cosmos DB her bölümün SSD yedekli depolama ilişkili sabit bir tutar sahiptir ve yüksek kullanılabilirlik için çoğaltılır. Bölüm yönetimi tam olarak Azure Cosmos DB tarafından yönetilen ve karmaşık kodlar yazmak veya bölüm yönetmek zorunda kalmazsınız. Cosmos DB koleksiyonlarıdır **sınırsız** depolama ve işleme açısından. 

### <a name="automatic-indexing-of-collections"></a>Koleksiyonları otomatik dizin oluşturma
Azure Cosmos DB true şemasız veritabanı sistemidir. Varsaymaz veya JSON belgeleri için herhangi bir şema gerektirir. Belgeleri bir koleksiyona eklemek gibi Azure Cosmos DB bunları otomatik olarak dizinler ve sorgulamak için kullanılabilir. Otomatik belgelerin dizin şemasını ya da ikincil dizinlerin gerek kalmadan Azure Cosmos DB'nin anahtar bir özelliktir ve yazma iyileştirilmiş kilidi serbest ve günlük yapılı dizin bakım teknikleri tarafından etkinleştirilir. Azure Cosmos DB hala tutarlı sorguları hizmet veren sırasında son derece hızlı yazmalar sürekli hacmi destekler. Belge ve dizin depolama her koleksiyon tarafından kullanılan depolama hesaplamak için kullanılır. Depolama ve performans ilişkili bir koleksiyon için dizin oluşturma ilkesini yapılandırarak dizin ile dengelemeler kontrol edebilirsiniz. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Bir dizin oluşturma ilkesini yapılandırma
Dizin oluşturma ilkesini her koleksiyonun performans ve depolama dizin ile ilişkili dengelemeler yapmanızı sağlar. Dizin oluşturma yapılandırmasının bir parçası olarak size aşağıdaki seçenekler kullanılabilir:  

* Koleksiyon otomatik olarak tüm belgelerin veya dizinleri olup olmadığını seçin. Varsayılan olarak, tüm belgeler otomatik olarak dizine alınır. Otomatik dizin oluşturma devre dışı bırakma ve yalnızca belirli belgeler için dizin seçerek eklemek seçebilirsiniz. Buna karşılık, yalnızca belirli belgeleri dışlamak seçmeli olarak seçebilirsiniz. Bu otomatik özelliği doğru veya yanlış bir koleksiyon indexingPolicy olacak şekilde ayarlayarak ve [x-ms-indexingdirective] istek üstbilgisi ekleme, değiştirme veya bir belgeyi silme sırasında kullanarak gerçekleştirebilirsiniz.  
* Dahil etmek veya belirli yollar veya belgelerinizi düzenleri dizinden hariç tutmak isteyip istemediğinizi seçin. Bu ayar includedPaths ve bir koleksiyon indexingPolicy üzerinde excludedPaths sırasıyla elde edebilirsiniz. Ayrıca, depolama ve performans dengelemeler belirli yolu desenler için aralığı ve karma sorgular için de yapılandırabilirsiniz. 
* Zaman uyumlu arasında (tutarlı) seçin ve zaman uyumsuz (yavaş) dizin güncelleştirmeleri. Varsayılan olarak, dizin her ekleme, değiştirme veya koleksiyona bir belgeyi silme zaman uyumlu olarak güncelleştirilir. Bu belge okuma aynı tutarlılık düzeydeki vermenizin sorguları sağlar. Azure Cosmos DB yazma en iyi duruma getirilmiş ve zaman uyumlu dizin Bakım ve tutarlı sorguları hizmet veren birlikte belge yazma sürekli birimi destekleyen olsa da, belirli koleksiyonlar kendi dizini gevşek güncelleştirmek için yapılandırabilirsiniz. Yavaş dizin daha fazla yazma performansı artırır ve toplu alım senaryoları öncelikle okuma ağır koleksiyonları için idealdir.

Dizin oluşturma ilkesini koleksiyonda PUT yürüterek değiştirilebilir. Bu, aracılığıyla elde [istemci SDK](sql-api-sdk-dotnet.md), [Azure portal](https://portal.azure.com), veya [REST API'leri](/rest/api/cosmos-db/).

### <a name="querying-a-collection"></a>Bir koleksiyonu sorgulama
Bir koleksiyon içindeki belgelerde rasgele şemalar sahip olabilir ve herhangi bir şemayı ya da ikincil dizinlerin önceden sağlamadan bir koleksiyon içinde belgeleri sorgulayabilirsiniz. Koleksiyonu kullanarak sorgulama yapabilirsiniz [Azure Cosmos DB SQL söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/dn782250.aspx), zengin hiyerarşik, ilişkisel ve uzamsal işleçler ve genişletilebilirlik UDF'ler JavaScript tabanlı aracılığıyla sağlar. JSON dil bilgisi ağaç düğümleri olarak etiketli ağaçlar JSON belgeleri modellenmesini sağlar. Bu hem SQL API'nin otomatik dizin oluşturma teknikleri ve bunun yanı sıra Azure Cosmos veritabanı SQL diyalekti tarafından yararlanan. SQL sorgu dili üç ana yönlerini oluşur:   

1. Doğal olarak hiyerarşik sorgular ve tahminleri dahil olmak üzere ağaç yapısı eşleme sorgu işlemleri küçük bir dizi. 
2. Bir alt kümesini oluşturma, filtre, projeksiyonları, toplamalar ve kendi kendine birleşim dahil olmak üzere ilişkisel işlemler. 
3. Saf JavaScript çalışmak UDF'ler (1) ve (2) bağlı.  

Azure Cosmos DB sorgu modelini işlevselliği, verimliliği ve Basitlik arasında bir denge dener. Azure Cosmos DB veritabanı altyapısı yerel olarak derler ve SQL sorgu ifadeleri çalıştırır. Bir koleksiyonu kullanarak sorgulama yapabilirsiniz [REST API'leri](/rest/api/cosmos-db/) ya da herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). .NET SDK'sı LINQ sağlayıcı ile birlikte gelir.

> [!TIP]
> Out SQL API deneyin ve kümemize karşı SQL sorguları çalıştırma [Query Playground](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Çoklu belge işlemler
Veritabanı işlemleri veri eşzamanlı değişiklik yapılacağı için güvenli ve tahmin edilebilir bir programlama modeli sağlar. RDBMS içinde iş mantığı yazmak için geleneksel yolu yazmaktır **saklı yordamlar** ve/veya **Tetikleyicileri** ve işlem yürütme için veritabanı sunucusu için sevk. RDBMS iki farklı programlama dilleriyle dağıtılacak uygulama Programcı gereklidir: 

* (İşlemsel olmayan) uygulama programlama dili (örneğin, JavaScript, Python, C#, Java, vb.)
* T-SQL, yerel veritabanı tarafından yürütülen işlem programlama dili

Yürütülen JavaScript uygulama mantığını doğrudan saklı yordamlar bakımından koleksiyonları dayanarak, derin taahhüt, JavaScript ve JSON için doğrudan veritabanı altyapısının içinde Azure Cosmos DB sezgisel bir programlama modeli sağlar ve tetikler. Bu hem birini verir:

* Eşzamanlılık verimli uyarlamasını denetlemek, Kurtarma, doğrudan veritabanı altyapısının içinde JSON nesne grafiklerinin dizin otomatik
* Denetim akışı doğal olarak ifade, değişken kapsamı, atama ve özel durum işleme temelleri veritabanı işlemleri doğrudan programlama dili JavaScript bakımından ile tümleştirme

Bir koleksiyon düzeyinde kayıtlı JavaScript mantığı, ardından belirli koleksiyon belgelerde veritabanı işlemlerini verebilir. Azure Cosmos DB örtük JavaScript tabanlı sarmalar saklı yordamları ve Tetikleyicileri anlık görüntü yalıtımıyla çevresel ACID işlemi içinde bir koleksiyon içindeki belgelerde arasında. Yürütme sürecinde JavaScript bir özel durum oluşturursa tüm işlem iptal edilir. Sonuçta elde edilen programlama modeli basittir henüz güçlü. JavaScript geliştiricilerin kendi tanıdık dil yapıları ve kitaplık temelleri kullanmaya devam ederken "dayanıklı" programlama modeli alın.   

Doğrudan arabellek havuzu ile aynı adres alanında veritabanı altyapısının içinde JavaScript yürütme yeteneğini kullanıcı ve bir koleksiyon belgeleri karşı veritabanı işlemleri işlem tabanlı olarak yürütülmesini sağlar. Ayrıca, JSON için derin taahhüdü Cosmos DB veritabanı altyapısı yapar ve JavaScript uygulama türü sistemleri ve veritabanı arasındaki tüm empedanslı uyumsuzluğu ortadan kaldırır.   

Bir koleksiyonu oluşturduktan sonra saklı yordamlar, tetikleyiciler ve UDF'lerin koleksiyonunu kullanarak kaydedebilirsiniz [REST API'leri](/rest/api/cosmos-db/) ya da herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). Kayıttan sonra başvuru ve bunları yürütün. Tamamen JavaScript'te yazılmış aşağıdaki saklı yordamı düşünün, aşağıdaki kodu (rehberi adını ve yazar adı) iki bağımsız değişkeni alır ve yeni bir belge oluşturur, bir belge için sorgular ve tüm örtük ACID işlemi içinde onu – güncelleştirir. JavaScript özel durum, yürütme sırasında herhangi bir noktada, tüm işlem durdurur.

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

İstemci "HTTP POST ile işlem yürütme için veritabanı yukarıdaki JavaScript mantığı sevk edebilir". HTTP yöntemleri kullanma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB kaynakları ile RESTful etkileşimleri](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

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


Veritabanı JSON ve JavaScript'i yerel olarak anlar olduğundan, hiçbir tür sistemi uyuşmazlığı, yok "Veya eşleme" ya da gerekli kod oluşturma Sihirli fark.   

Saklı yordamları ve Tetikleyicileri bir koleksiyon ve belgeler bir koleksiyonda geçerli koleksiyon içeriği sunan bir iyi tanımlanmış nesne modeli aracılığıyla etkileşim.  

SQL API koleksiyonlarda oluşturulabilir, silinen, okuma veya numaralandırılmış kolayca kullanarak [REST API'leri](/rest/api/cosmos-db/) ya da herhangi bir [istemci SDK'ları](sql-api-sdk-dotnet.md). SQL API her zaman okuma veya bir koleksiyon meta verileri sorgulamak için güçlü tutarlılık sağlar. Bir koleksiyonun otomatik olarak silineceği belgeleri, ekleri, saklı yordamlar, Tetikleyiciler hiçbirine erişemiyor ve UDF'ler içerdiği sağlar.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF)
Önceki bölümde açıklandığı gibi doğrudan veritabanı altyapısının içinde bir işlem içinde çalıştırmak için uygulama mantığını yazabilirsiniz. Uygulama mantığını tamamen JavaScript'te yazılmış ve bir saklı yordam, tetikleyici veya bir UDF modellenir. JavaScript kodu saklı yordam veya bir tetikleyici içinde ekleyebilirsiniz, değiştirme, silme, okuma veya sorgu belgeleri bir koleksiyon içinde. Öte yandan, bir UDF içinden JavaScript olamaz eklemek, değiştirmek veya belgeleri silin. UDF'ler, belgeler bir sorgunun sonuç kümesinin listeleme ve başka bir sonuç kümesi üretir. Çoklu kiracı için bir katı ayırma tabanlı kaynak İdaresi Azure Cosmos DB zorlar. Her saklı yordam, tetikleyici veya bir UDF işini yapmak için işletim sistemi kaynaklarının sabit Zamanlayıcının alır. Ayrıca, saklı yordamlar, tetikleyiciler ve UDF'ler karşı dış JavaScript kitaplıklarını bağlayamazsınız ve kendisine ayrılan kaynak bütçe aşarsanız kara listede. Kayıt, saklı yordamlar, tetikleyiciler ve UDF'ler REST API'lerini kullanarak bir koleksiyonla kaydını silin.  Kayıt sırasında bir saklı yordam, tetikleyici veya bir UDF önceden derlenmiş ve daha sonra yürütülen bayt kodu olarak depolanır. Aşağıdaki bölümde, kaydetme, yürütme ve saklı yordam, tetikleyici ve bir UDF kaydı için Azure Cosmos DB JavaScript SDK'sı nasıl kullanabileceğiniz gösterilmektedir. Basit bir sarmalayıcı biter JavaScript SDK'sı [REST API'leri](/rest/api/cosmos-db/). 

### <a name="registering-a-stored-procedure"></a>Saklı yordam kaydetme
Kayıt bir saklı yordam yeni bir saklı yordam kaynak HTTP POST ile bir koleksiyon oluşturur.  

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

### <a name="executing-a-stored-procedure"></a>Saklı yordam yürütme
Bir saklı yordam yürütme istek gövdesindeki yordam parametreleri geçirerek bir HTTP POST varolan bir saklı yordam kaynağı karşı vererek yapılır.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Saklı yordam kaydı siliniyor
Saklı yordam kaydı silinirken bir HTTP DELETE varolan bir saklı yordam kaynağı karşı vererek yapılır.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Ön tetikleyici kaydetme
Bir tetikleyici kaydını HTTP POST ile bir koleksiyon üzerinde yeni bir tetikleyici kaynak oluşturarak yapılır. Tetikleyici bir öncesi veya sonrası tetikleyici ve işlem türü (örneğin, oluşturma, değiştirme, silme veya tüm) ilişkilendirilmiş olabilir, belirtebilirsiniz.   

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

### <a name="executing-a-pre-trigger"></a>Ön tetikleyici yürütme
Tetikleyici yürütmesi, istek üstbilgisi aracılığıyla belge kaynağının PUT/POST/DELETE isteği vermeden sırasındaki varolan bir tetikleyicinin adını belirterek yapılır.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Ön tetikleyici kaydını silme
Bir tetikleyici kaydını bir HTTP DELETE varolan bir tetikleyici kaynağı karşı veren aracılığıyla gerçekleştirilir.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Bir UDF kaydetme
Bir UDF kaydını HTTP POST ile bir koleksiyon üzerinde yeni bir UDF kaynak oluşturarak yapılır.  

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

### <a name="executing-a-udf-as-part-of-the-query"></a>Sorgu parçası olarak bir UDF yürütme
Bir UDF SQL sorgusunun bir parçası olarak belirtilen ve çekirdek genişletmek için bir yol kullanılan [SQL sorgu dili](sql-api-sql-query-reference.md).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Bir UDF kaydı siliniyor
Bir UDF kaydını yalnızca bir HTTP DELETE varolan bir UDF kaynağı karşı vererek yapılır.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Yukarıdaki kod parçacıkları aracılığıyla kaydı (POST), kayıt silme (PUT), okuma/listesi (GET) ve yürütme (POST) gösterdi rağmen [JavaScript SDK'sı](https://github.com/Azure/azure-documentdb-js), de kullanabilirsiniz [REST API'leri](/rest/api/cosmos-db/) veya diğer [istemci SDK'ları](sql-api-sdk-dotnet.md). 

## <a name="documents"></a>Belgeler
Eklemek, değiştirmek, silmek, okuyabilir, listeleme ve bir koleksiyondaki rastgele JSON belgelerinin sorgu. Azure Cosmos DB herhangi bir şema zorunlu kılabilir değil ve ikincil dizinler koleksiyonu belgelerde üzerinden sorgulama desteklemek için gerekli değildir. Bir belgenin boyutu üst sınırı 2 MB'tır.   

Gerçekten açık veritabanı hizmeti olan, Azure Cosmos DB herhangi bir özel veri türleri (örneğin, tarih saat) veya belirli kodlamaları JSON belgeleri için stok değil. Azure Cosmos DB çeşitli belgeler arasındaki ilişkileri kod oluşturma için özel JSON kurallarını gerektirmez; Azure Cosmos DB SQL söz dizimi güçlü hiyerarşik ve ilişkisel sorgu işleçleri için sorgu ve proje belgeler herhangi bir özel ek açıklama veya kullanarak belgeler arasında ilişkiler kod oluşturma gerek olmadan özellikleri ayırt sağlar.  

Tüm diğer kaynaklarla belgeleri, değiştirilmesi silinmiş, okuma, oluşturulabilir olarak numaralandırılmış ve kolayca REST API'leri veya herhangi birini kullanarak sorgulanan [istemci SDK'ları](sql-api-sdk-dotnet.md). Bir belgenin anında silinmesi tüm iç içe ek karşılık gelen kota boşaltır. Belgeleri okuma tutarlılığı düzeyini tutarlılık ilkesi veritabanı hesabındaki izler. Bu ilke, uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak istek başına temelinde geçersiz kılınabilir. Belgeleri sorgulanırken okuma tutarlılığı koleksiyonda dizin oluşturma modu izler. "İçin tutarlı", bu hesabın tutarlılık ilke aşağıdaki gibidir. 

## <a name="attachments-and-media"></a>Ekleri ve ortam
Azure Cosmos DB verir ikili BLOB'lar/medyası depolama için Azure Cosmos DB (hesap başına en fazla 2 GB) ile ya da ya da kendi uzaktan medya deposunda. Ayrıca, bir ortam eki adlı özel bir belge bakımından meta verileri temsil etmek sağlar. Ek Azure Cosmos veritabanı başka bir yerde depolanan medya/blob başvuruda bulunan özel bir (JSON) belgedir. Yalnızca Uzak medya depolamada depolanan bir medya meta veriler (örneğin, konum, yazar vb.) yakalayan özel belge ektir. 

Yer işaretleri, derecelendirme, yöntemlerine/Beğenmediklerinizi vb. bir e-kitap için belirli bir kullanıcının ilişkili meta verileri, Yorumlar dahil olmak üzere vurgular ve Azure Cosmos DB ek açıklamaların depolamak için kullandığı bir sosyal okuma uygulaması düşünün.   

* Kitap içeriğini ya da ortam depolamada depolanan Azure Cosmos DB veritabanı hesabı parçası veya bir uzak medya deposu olarak kullanılabilir. 
* Bir uygulamayı ayrı bir belge olarak her kullanıcının meta verileri depolayabilir--Örneğin, Can'ın meta verilerini Kitap1 /colls/joe/docs/book1 tarafından başvurulan bir belge depolanır. 
* Bir kullanıcının belirli bir kitap içerik sayfalarına işaret eden ekler, ilgili belge altında Örneğin, /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 vb. depolanır. 

Yukarıda listelenen örnekler kolay kimlikleri kaynak hiyerarşisi iletmek için kullanın. Kaynaklar, REST API'leri aracılığıyla benzersiz kaynak kimlikleri aracılığıyla erişilir. 

Azure Cosmos DB tarafından yönetilen ortam için ekin _media özelliği medya tarafından URI'sini başvurur. Azure Cosmos DB uyduğundan emin olabilirsiniz tüm bekleyen başvurularını bırakılan medya için atık toplama. Azure Cosmos DB otomatik olarak yeni bir ortam yüklediğinizde eki oluşturur ve yeni eklenen medyaya işaret edecek şekilde _media doldurur. Sizin tarafınızdan (örneğin, OneDrive, Azure Storage, DropBox, vb.) yönetilen bir uzak blob Mağazası'nda media depolamayı seçerseniz, ekleri medya başvurmak için kullanmaya devam edebilirsiniz. Bu durumda, ek kendiniz oluşturmanız ve onun _media özelliğini doldurmak.   

Tüm diğer kaynaklarla ekleri oluşturulabilir gibi yerini silinmiş, okuma veya REST API'leri veya herhangi bir istemci SDK'ları kolayca kullanarak numaralandırılır. Belgelerle olduğu gibi veritabanı hesabındaki tutarlılık İlkesi ekleri okuma tutarlılığı düzeyini izler. Bu ilke, uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak istek başına temelinde geçersiz kılınabilir. Ekler için sorgulanırken okuma tutarlılığı koleksiyonda dizin oluşturma modu izler. "İçin tutarlı", bu hesabın tutarlılık ilke aşağıdaki gibidir. 
 

## <a name="users"></a>Kullanıcılar
Bir Azure Cosmos DB kullanıcı izinleri gruplandırması için mantıksal bir ad alanı temsil eder. Bir Azure Cosmos DB kullanıcı bir kullanıcı için bir kimlik yönetimi sistemi veya önceden tanımlanmış uygulama rolü karşılık gelebilir. Azure Cosmos DB için bir kullanıcı yalnızca bir veritabanı altında izinleri gruplandırmak için bir Özet temsil eder.   

Çoklu kiracı uygulamanızda uygulamak için gerçek kullanıcılarınız veya uygulamanızın kiracılar için karşılık gelen Azure Cosmos veritabanı kullanıcıları oluşturabilirsiniz. Ardından, çeşitli koleksiyonlar, belgeler, ekleri, vb. bir erişim denetimi için karşılık gelen belirli bir kullanıcının izinlerini de oluşturabilirsiniz.   

Uygulamalarınızı, kullanıcı büyümesiyle ölçeklendirmek gereksinim duyduğunuz kadar parça için çeşitli şekillerde verilerinizi benimseyebilirsiniz. Her kullanıcılarınızın gibi model oluşturabilirsiniz:   

* Her kullanıcı için bir veritabanı eşler.
* Her kullanıcı bir koleksiyona eşler. 
* Birden fazla kullanıcıya karşılık gelen belgeleri adanmış bir koleksiyona gidin. 
* Birden fazla kullanıcıya karşılık gelen belgeleri koleksiyonları kümesine gidin.   

Belirli parçalama stratejisi bağımsız olarak seçtiğiniz Azure Cosmos DB veritabanındaki kullanıcı olarak gerçek kullanıcılarınızın model ve her kullanıcının hassas izinleri ilişkilendirebilirsiniz.  

![Kullanıcı koleksiyonları][3]  
**Parçalama stratejilerini ve modelleme kullanıcılar**

Diğer tüm kaynaklar gibi kullanıcılar Azure Cosmos veritabanı oluşturulabilir, değiştirilen, silinen, okuma veya REST API'leri veya herhangi bir istemci SDK'ları kolayca kullanarak numaralandırılan. Azure Cosmos DB her zaman okuma veya kullanıcı kaynağı meta verileri sorgulamak için güçlü tutarlılık sağlar. Kullanıcı otomatik olarak silme içerdiği izinleri hiçbirine erişemiyor sağlar işaret eden değer olur. Silinen kullanıcı arka planda bir parçası olarak izinleri kota Azure Cosmos DB kaldırsa olsa bile, silinen izinleri kullanılabilir hemen yeniden kullanabilmeniz için.  

## <a name="permissions"></a>İzinler
Bir erişim denetimi açısından bakıldığında, veritabanı hesaplarını, veritabanları, kullanıcılar ve izni gibi kaynakları kabul edilen *Yönetim* bunlar yönetim izinleri gerektirir olduğundan kaynaklar. Diğer taraftan, koleksiyonları dahil kaynakların belgeleri, ekleri, saklı yordamlar, Tetikleyiciler, ve UDF'ler altında verilen bir veritabanı kapsamlı ve kabul *uygulama kaynakları*. İki tür kaynakları ve bunlara (yani yönetici ve kullanıcı) erişim rolleri için karşılık gelen, iki tür yetkilendirme modelini tanımlar *erişim anahtarları*: *ana anahtar* ve  *Kaynak anahtarı*. Ana anahtar veritabanı hesabı, bir parçasıdır ve geliştirici (veya yönetici) için sağlanan kimin veritabanı hesabı sağlama. Yönetim ve uygulama kaynaklara erişim yetkisi vermek için kullanılabilir, bu ana anahtar Yöneticisi semantiği sahiptir. Buna karşılık, kaynak anahtarı erişimine izin veren bir ayrıntılı erişim anahtarıdır bir *belirli* Uygulama kaynağı. Bu nedenle, bir veritabanı kullanıcısı ve belirli bir kaynak için (örneğin, koleksiyon, belge, ek, saklı yordam, tetikleyici veya UDF) kullanıcının sahip olduğu izinleri arasındaki ilişkiyi yakalar.   

Kaynak anahtarı edinmek için yalnızca belirli bir kullanıcının altında izni kaynak oluşturarak yoludur. Oluşturma veya izni almak için bir ana anahtar yetkilendirme üstbilgisinde sunulmalıdır. Kaynak, uygulamaya erişim ve kullanıcı izni kaynak bağlar. Bir izin kaynak oluşturduktan sonra kullanıcı ilgili kaynak erişim sağlamak için ilişkili kaynak anahtarı sunmak yeterlidir. Bu nedenle, bir kaynak anahtarı izni kaynak mantıksal ve compact gösterimi görüntülenebilir.  

Tüm diğer kaynaklarla Azure Cosmos veritabanı izinleri oluşturulabileceği gibi değiştirilen silinmiş, okuma veya REST API'leri veya herhangi bir istemci SDK'ları kolayca kullanarak numaralandırılır. Azure Cosmos DB her zaman okuma veya izin meta verileri sorgulamak için güçlü tutarlılık sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
HTTP komutları kullanarak kaynakları ile çalışma hakkında daha fazla bilgi [Azure Cosmos DB kaynakları ile RESTful etkileşimleri](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/sql-api-resources/resources1.png
[2]: media/sql-api-resources/resources2.png
[3]: media/sql-api-resources/resources3.png

