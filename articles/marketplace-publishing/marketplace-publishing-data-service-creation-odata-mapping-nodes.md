---
title: "Veri Hizmeti Market oluşturmaya Kılavuzu | Microsoft Docs"
description: "Oluşturma, sertifika ve bir veri hizmeti için dağıtma konusunda ayrıntılı yönergeler Azure Market satın alın."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8ff76ea21ba684ae2a2afcb74d66b4912d7be053
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Var olan bir web hizmetini CSDL aracılığıyla OData eşleme için düğümleri şemasını anlama
> [!IMPORTANT]
> **Şu anda artık ekleme duyuyoruz herhangi yeni veri hizmeti yayımcılar. Yeni dataservices listesine onaylanmamış.** Üzerinde AppSource yayımlamak istediğiniz bir SaaS iş uygulaması varsa daha fazla bilgi bulabilirsiniz [burada](https://appsource.microsoft.com/partners). Bir Iaas uygulamalar varsa veya Azure marketi, yayımlamak istediğiniz Geliştirici hizmet, daha fazla bilgi bulabilirsiniz [burada](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Bu belge, bir OData protokolü için CSDL eşleme düğümü yapısı açıklamak yardımcı olur. Unutulmaması önemlidir düğümü yapısı iyi biçimlendirilmiş XML. Bu nedenle, OData eşleme tasarlarken kök, üst ve alt şema geçerlidir.

## <a name="ignored-elements"></a>Yoksayılan öğeleri
Web hizmetinin meta verileri içeri aktarma sırasında Azure Marketi arka uç tarafından kullanılacak yapmayacağınız üst düzey CSDL öğeler (XML düğümlerini) bağlıdır. Bunlar, mevcut olabilir ancak göz ardı edilir.

| Öğesi | Kapsam |
| --- | --- |
| Öğesini kullanarak |Düğüm, alt düğümleri ve tüm öznitelikleri |
| Documentation öğesi |Düğüm, alt düğümleri ve tüm öznitelikleri |
| ComplexType |Düğüm, alt düğümleri ve tüm öznitelikleri |
| İlişkilendirme öğesi |Düğüm, alt düğümleri ve tüm öznitelikleri |
| Genişletilmiş özellik |Düğüm, alt düğümleri ve tüm öznitelikleri |
| EntityContainer |Yalnızca aşağıdaki öznitelikleri göz ardı edilir: *genişletir* ve *AssociationSet* |
| Şema |Yalnızca aşağıdaki öznitelikleri göz ardı edilir: *Namespace* |
| Functionımport |Yalnızca aşağıdaki öznitelikleri göz ardı edilir: *modu* (ln varsayılan değerini kabul edilir) |
| EntityType |Yalnızca aşağıdaki alt düğümler göz ardı edilir: *anahtar* ve *PropertyRef* |

Aşağıdaki ayrıntı çeşitli CSDL XML düğümlerin (eklenir ve öğeleri göz ardı) değişiklikleri açıklar.

## <a name="functionimport-node"></a>Functionımport düğümü
Bir Functionımport düğümü için son kullanıcı bir hizmet sunan bir URL (giriş noktası) temsil eder. Düğümün URL nasıl ele açıklayan verir, hangi parametreleri son kullanıcı için kullanılabilir ve nasıl Bu parametreler sağlanır.

Bu düğümün ayrıntılarını [burada] bulundu[MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

Ek öznitelikler (veya öznitelikleri eklemeler) verilmiştir Functionımport düğümü tarafından sunulur:

**d:BaseUri** -Marketi'nde kullanıma sunulan REST Kaynak URI şablonunu. Market şablon REST web hizmeti sorguları oluşturmak için kullanılır. URI şablonunu parameterName parametresinin adı olduğu {parameterName} biçiminde parametreleri yer tutucular içerir. Örn apiVersion = {apiVersion}.
Parametreleri URI parametreleri olarak veya URI yolu bir parçası olarak görünmesi izin verilir. Görünüm yolunda söz konusu olduğunda bunlar her zaman zorunlu (null'olarak işaretlenemez). *Örnek:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Ad** -alınan işlevin adı.  Csdl'deki tanımlı diğer adlar ile aynı olamaz.  Örn Adı "GetModelUsageFile" =

**EntitySet** *(isteğe bağlı)* - işlevi bir koleksiyon, varlık türleri değerini döndürürse **EntitySet** koleksiyonu ait olduğu varlık kümesi gerekir. Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır. *Örnek:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(isteğe bağlı)* -URI tarafından döndürülen öğelerin türünü belirtir.  İşlev bir değer döndürmezse, bu öznitelik kullanmayın. Desteklenen türler şunlardır:

* **Koleksiyon (<Entity type name>)**: tanımlı varlık türleri koleksiyonunu belirtir. Ad EntityType düğüm adı özniteliği mevcut değil. Bir örnek koleksiyonudur (WXC. HourlyResult).
* **Ham (<mime type>)**: ham belge/kullanıcıya döndürülen blob belirtir. Örnek Raw(image/jpeg) diğer örnekleri olan:

  * ReturnType="Raw(text/plain)"
  * ReturnType = "derleme (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** -disk belleği REST kaynak tarafından nasıl işleneceğini belirtir. Parametre değerleri kullanılan süslü braches içinde örneğin sayfa {$page} = & ItemsPerPage sıfırdan küçük {$size} = seçenekleri kullanılabilir:

* **Hiçbiri:** hiçbir disk belleği kullanılabilir
* **Atla:** disk belleği mantıksal "Atla" ve "Al" (üst) ile ifade edilir. M öğeleri Atla atlar ve Al sonraki N öğeleri döndürür. Parametre değeri: $skip
* **Take:** Al sonraki N öğesini döndürür. Parametre değeri: $take
* **PageSize:** disk belleği mantıksal sayfasını ve boyutu (sayfa başına öğe) ile ifade edilir. Sayfa döndürülen geçerli sayfasını temsil eder. Parametre değeri: $page
* **Boyut:** boyutu her bir sayfa için döndürülen öğe sayısını temsil eder. Parametre değeri: $size

**d:AllowedHttpMethods** *(isteğe bağlı)* -hangi fiil REST kaynak tarafından ele belirtir. Ayrıca, belirtilen değeri kabul edilen fiili kısıtlar.  Varsayılan POST =.  *Örnek:* `d:AllowedHttpMethods="GET"` seçenekleri kullanılabilir:

* **GET:** genellikle verileri döndürmek için kullanılır
* **POST:** genellikle yeni veri eklemek için kullanılır
* **PUT:** genellikle verileri güncelleştirmek için kullanılır
* **SİLME:** veri silmek için kullanılır

Functionımport düğüm içinde ek alt düğümleri (CSDL belgelerinde ele alınmamaktadır) şunlardır:

**d:RequestBody** *(isteğe bağlı)* -istek gövdesi isteği gönderilmek üzere bir gövde beklediğini belirtmek için kullanılır. İstek gövdesinde belirtilen parametrelerine. Süslü ayraç içinde örneğin belirtilmiştir {parameterName}. Bu parametreler için içerik sağlayıcının hizmeti aktarılan gövdesine kullanıcı girişinden eşlenir. RequestBody öğesi adı httpMethod özniteliği var. Öznitelik iki değer sağlar:

* **POST:** bir HTTP POST isteğiyse kullanılan
* **GET:** bir HTTP GET isteği ise, kullanılan

    Örnek:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:namespaces** ve **d:Namespace** -bu düğüm işlev içeri aktarma (URI uç noktası) tarafından döndürülen XML öğesinde tanımlanan ad alanları açıklar. Arka uç hizmeti tarafından döndürülen XML ad alanları döndürülen içeriği ayırt etmek için herhangi bir sayıda içerebilir. **D:Map veya d:Match XPath sorguları kullandıysanız bu ad alanlarının tümünü listelenmiş olması gerekir.** D:Namespaces düğüm d:Namespace düğümleri kümesi/listesi içerir. Bunların her birini arka uç hizmeti yanıtında kullanılacak bir ad alanını listeler. Öznitelik d:Namespace düğümünün şunlardır:

* **d:prefix:** hizmet tarafından döndürülen XML sonuçlarında ör f:FirstName, f öneki olduğu f:LastName görülen ad alanı öneki.
* **d:Uri:** tam sonuç belgede kullanılan ad alanı URI'si. Önek çalışma zamanında çözülene değeri temsil eder.

**d:ErrorHandling** *(isteğe bağlı)* -bu düğüm hata işleme koşulları içerir. Koşullardan her biri içerik sağlayıcının hizmet tarafından döndürülen sonuç doğrulanır. Bir koşul önerilen bir HTTP hata kodunu eşleşmesi durumunda bir hata iletisi için son kullanıcı döndürülür.

**d:ErrorHandling** *(isteğe bağlı)* ve **d:Condition** *(isteğe bağlı)* -bir koşul düğümü tarafından döndürülen sonuç işaretli bir koşul tutar sağlayıcının hizmet içerik. Aşağıdakiler **gerekli** öznitelikleri:

* **d:match:** animasyonun çıktı XML belirli bir düğüm/değeri içerik sağlayıcı mevcut olup olmadığını doğrulayan bir XPath ifadesi. XPath karşı çıktı çalıştırın ve bir eşleşme ya da false koşul Aksi durumda ise, true değerini döndürmelidir.
* **d:HttpStatusCode:** koşul Marketi tarafından durumda döndürülmesi gereken HTTP durum kodu ile eşleşir. Market hataları kullanıcının HTTP durum kodları üzerinden signalizes. HTTP durum kodları listesini http://en.wikipedia.org/wiki/HTTP_status_code kullanılabilir
* **d:ErrorMessage:** – HTTP durum kodu ile – son kullanıcıya döndürülen hata iletisi. Bu, tüm gizli içermiyor kolay hata iletisi olmalıdır.

**d:title** *(isteğe bağlı)* -başlık işlevinin açıklayan sağlar. Başlık değeri geldiği

* Hizmet isteğinden döndürülen yanıt başlığı nerede belirten isteğe bağlı eşleme özniteliği (xpath).
* - Veya - düğüm değeri olarak belirtilen başlığı.

**d:Rights** *(isteğe bağlı)* -işlev ile ilişkili hakları (örneğin telif hakkı). Hakları değeri geldiği:

* Hizmet isteğinden döndürülen yanıt hakları nerede belirten isteğe bağlı eşleme özniteliği (xpath).
* - Veya - düğüm değeri olarak belirtilen hakları.

**d:description** *(isteğe bağlı)* - A kısa işlevi için bir açıklama. Açıklama değeri'ten gelen

* Hizmet isteğinden döndürülen yanıt açıklaması nerede belirten isteğe bağlı eşleme özniteliği (xpath).
* - Veya -düğüm değeri olarak belirtilen açıklaması.

**d:EmitSelfLink** - *yukarıdaki örnekte "Functionımport 'disk 'belleği döndürülen veriler aracılığıyla için" konusuna bakın*

**d:EncodeParameterValue** -OData için isteğe bağlı genişletme

**d:QueryResourceCost** -OData için isteğe bağlı genişletme

**d:Map** -OData için isteğe bağlı genişletme

**d:Headers** -OData için isteğe bağlı genişletme

**d:Headers** -OData için isteğe bağlı genişletme

**d:Value** -OData için isteğe bağlı genişletme

**d:HttpStatusCode** -OData için isteğe bağlı genişletme

**d:ErrorMessage** -OData için isteğe bağlı genişletme

## <a name="parameter-node"></a>Parametre düğümü
Bu düğümün temsil ettiği URI şablonunu bir parçası olarak kullanıma sunulan bir parametre / istek Functionımport düğümünde belirtilen gövdesi.

Çok yararlı Ayrıntılar belge sayfası "Parametre öğesi" düğüm hakkındaki bulunduğu konum [burada](http://msdn.microsoft.com/library/ee473431.aspx) (kullanımı **diğer sürüm** belgeleri görüntülemek gerekirse farklı bir sürüm seçmek için açılır). *Örnek:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parametre özniteliği | Gereklidir | Değer |
| --- | --- | --- |
| Ad |Evet |Parametrenin adı. Büyük küçük harf duyarlı!  TabanURI büyük küçük harf duyarlı. **Örnek:**`<Property Name="IsDormant" Type="Byte" />` |
| Tür |Evet |Parametre türü. Değer olmalıdır bir **EDMSimpleType** veya model kapsamında olmayan bir karmaşık tür. Daha fazla bilgi için "6 desteklenen parametre/özellik türleri" bölümüne bakın.  (Büyük küçük harfe duyarlı! İlk karakter büyük harf, rest küçük.)  Ayrıca bkz, [kavramsal Model türü (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Örnek:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modu |Hayır |**İçinde**, Out ve Inout parametresi bir giriş, çıkış veya giriş/çıkış parametresi olduğuna bağlı olarak. (Yalnızca "IN" Azure Marketi'nde kullanılabilir.) **Örnek:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| maxLength |Hayır |Parametresinin uzunluğu izin verilen en fazla. **Örnek:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Duyarlılık |Hayır |Parametre hassasiyet. **Örnek:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Ölçek |Hayır |Parametre ölçeğini. **Örnek:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

CSDL belirtimi eklenen öznitelikleri şunlardır:

| Parametre özniteliği | Açıklama |
| --- | --- |
| **d:Regex** *(isteğe bağlı)* |Giriş parametresi değeri doğrulamak için kullanılan bir regex ifadesi. Giriş değeri deyim eşleşmezse, değeri reddedilir. Bu da olası değerler, örneğin belirtilmesine izin verir ^ [0-9] +? $ yalnızca sayılar izin vermek için. **Örnek:** ' < parametre adı "ad" modu = "İçinde" Type = "Dize" d =: boş değer atanabilir = "false" d:Regex = "^ [a-zA-Z] * $" d:Description "bir boşluk ya da alfasayısal olmayan İngilizce olmayan karakterler içeremez ad" d:SampleValues = "Hasan = |
| **d:Enum** *(isteğe bağlı)* |Bir kanal ayrılmış parametresi için geçerli değerlerin listesi. Değerlerin türü parametresi tanımlı türü ile eşleşmesi gerekir. Örnek: ' İngilizce |
| **d: boş değer atanabilir** *(isteğe bağlı)* |Bir parametre null olup olamayacağını tanımlanmasına olanak sağlar. Varsayılan değer: true. Ancak, URI şablonunu yolunda bir parçası olarak sunulan parametreleri null olamaz. Öznitelik bu parametrelerin – false olarak ayarlandığında, kullanıcı girişi geçersiz kılındı. **Örnek:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(isteğe bağlı)* |Arabirimdeki bir istemciye Not olarak görüntülenecek bir örnek değer.  Yani bir kanal ayrılmış listesini kullanarak birden fazla değer eklemek mümkündür ' bir |

## <a name="entitytype-node"></a>EntityType düğümü
Bu düğüm Marketi'nden son kullanıcıya döndürülür türlerinden birini temsil eder. Ayrıca, son kullanıcıya döndürülen değerlere içerik sağlayıcının hizmet tarafından döndürülen çıktının eşlemesinden içerir.

Bu düğümün ayrıntılarını konumunda bulunan [burada](http://msdn.microsoft.com/library/bb399206.aspx) (kullanımı **diğer sürüm** belgeleri görüntülemek gerekirse farklı bir sürüm seçmek için açılır.)

| Öznitelik adı | Gereklidir | Değer |
| --- | --- | --- |
| Ad |Evet |Varlık türünün adı. **Örnek:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType |Hayır |Tanımlanıyorsa varlık türünün temel türü başka bir varlık türünün adı. **Örnek:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

CSDL belirtimi eklenen öznitelikleri şunlardır:

**d:Map** -hizmet çıkış karşı yürütülen bir XPath ifadesi. Buradan nereye ATOM akışı gibi hizmet çıkış yineleyin, öğeleri kümesini içeren varsayılır yineleyin Giriş düğümleri kümesi yok. Her bu düğümler yinelenen bir kayıt içerir. XPath tek bir kaydın değerlerini tutan içerik sağlayıcının hizmet sonuç bağımsız tekrarlanan düğümünde işaret belirtilir. Örnek: hizmetinden çıktı

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath ifadesi / /foo olması nedeniyle çubuğunu her çubuğunu düğüm çıktıda yinelenen olandır ve son kullanıcıya döndürülen gerçek içeriği içerir.

**Anahtar** -bu öznitelik Marketi tarafından göz ardı edilir. REST tabanlı web services, genel bir birincil anahtar sunmayın.

## <a name="property-node"></a>Özelliği düğümü
Bu düğüm kaydının bir özellik içerir.

Bu düğümün ayrıntılarını konumunda bulunan [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (kullanımı **diğer sürüm** belgeleri görüntülemek gerekirse farklı bir sürüm seçmek için açılır.) *Örnek:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Gerekli | Değer |
| --- | --- | --- |
| Ad |Evet |Özelliğin adı. |
| Tür |Evet |Özellik değeri türü. Özellik değeri türü olmalıdır bir **EDMSimpleType** veya model kapsamında (bir tam ad tarafından gösterilen) bir karmaşık türü. Daha fazla bilgi için kavramsal Model türü (CSDL) bakın. |
| Boş değer atanabilir |Hayır |**Doğru** (varsayılan değer) veya **False** özelliği null bir değere sahip gerekmediğini bağlı olarak. Not: sürümünde belirttiği CSDL [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) ad alanı, bir karmaşık tür özelliği null atanabilir olmalıdır = "False". |
| defaultValue |Hayır |Özelliğin varsayılan değeri. |
| maxLength |Hayır |Özellik değeri en büyük uzunluğu. |
| FixedLength |Hayır |**Doğru** veya **False** özellik değeri fiexed uzunlukta bir dize olarak depolanan bağlı olarak. |
| Duyarlılık |Hayır |En büyük sayısal değeri korumak için basamak sayısını ifade eder. |
| Ölçek |Hayır |Sayısal değer korumak için ondalık basamak sayısı. |
| Unicode |Hayır |**Doğru** veya **False** olup özellik değeri olması bağlı olarak bir UNICODE dizesi depolanır. |
| Harmanlama |Hayır |Veri kaynağında kullanılacak harmanlama sırası belirten bir dize. |
| ConcurrencyMode |Hayır |**Hiçbiri** (varsayılan değer) veya **sabit**. Değer ayarlanmışsa **sabit**, iyimser eşzamanlılık denetimlerinde özellik değeri kullanılır. |

CSDL belirtimi için eklenene ek öznitelikleri şunlardır:

**d:Map** -hizmet karşı yürütülen XPath ifadesi çıkış ve çıktının bir özellik ayıklar. Belirtilen XPath EntityType düğümün XPath seçili yinelenen düğüm görelidir. Çıktı her çıktı düğümlerinin örneğin yalnızca bir kez özgün hizmetinde bulunan bir telif hakkı bildirimi gibi statik kaynak dahil, ancak her satır OData çıkış mevcut olması gereken izin vermek için bir mutlak XPath belirtmek mümkündür. Örnek hizmetinden:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

XPath ifadesi burada baz0 düğümü içerik sağlayıcının hizmetinden almak için ./bar/baz0 olacaktır.

**d:CharMaxLength** -dize türü için uzunluk üst sınırını belirtebilirsiniz. DataService CSDL örneğe bakın

**d:IsPrimaryKey** -sütun tablosunun/görünümünün birincil anahtar olup olmadığını gösterir. DataService CSDL örneğe bakın.

**d:isExposed** -tablo şemasını sunulur belirler (genellikle gerçek). DataService CSDL örneğe bakın

**d:IsView** *(isteğe bağlı)* - Bu tablo yerine bir görünümü temel alıyorsa true.  DataService CSDL örneğe bakın

**d:Tableschema** -DataService CSDL örnek bakın

**d:ColumnName** -tablo/görünüm sütununun adı.  DataService CSDL örneğe bakın

**d:IsReturned** -hizmet istemciye bu değer göstermiyorsa belirler Boolean.  DataService CSDL örneğe bakın

**d:IsQueryable** -sütun bir veritabanı sorgusu kullanılıp kullanılamayacağını belirler Boolean.   DataService CSDL örneğe bakın

**d:OrdinalPosition** -x, 1'den tablodaki sütun sayısı için sütunun sayısal görünümü, x, tablo veya Görünüm konumudur.  DataService CSDL örneğe bakın

**d:DatabaseDataType** -veritabanı, yani SQL veri türü sütununun veri türü. DataService CSDL örneğe bakın

## <a name="supported-parametersproperty-types"></a>Desteklenen parametreler/özellik türleri
Parametreleri ve özellikleri için desteklenen türler şunlardır: (Büyük küçük harf duyarlı)

| İlkel türler | Açıklama |
| --- | --- |
| Null |Bir değer yokluğu temsil eder |
| Boole değeri |İkili değerli mantığı matematiksel kavramını temsil eder |
| Bayt |İmzasız 8 bit tam sayı değeri |
| Tarih saat |Tarih ve saat 1 Ocak 1753 gece 12:00:00 gece arasında değişen değerler ile temsil eder 11:59:59 P.M, Aralık 9999 M.S. |
| Ondalık |Sabit duyarlık ve ölçek ile sayı değerleri temsil eder. Bu tür negatif 10'dan arasında değişen bir sayısal değer açıklayabilirsiniz ^ 255 + 1 pozitif 10 ^ 255 -1 |
| Çift |Kayan noktalı sayı ± 2.23e-308 ile ± 1, 79E yaklaşık aralığını değerlerle gösterebilir 15 basamağa duyarlık ile temsil eden +308. **Ondalık Exel verme sorunu nedeniyle kullanın:** |
| Tek |Kayan noktalı sayı ± 1.18e-38 ile ± 3.40e yaklaşık aralığını değerlerle gösterebilir 7 basamağa duyarlık ile temsil eden +38 |
| GUID |Bir 16 bayt (128-bit) benzersiz tanımlayıcısı değeri temsil eder |
| Int16 |İşaretli 16 bit tam sayı değerini temsil eder |
| Int32 |İşaretli 32 bit tamsayı değeri temsil eder |
| Int64 |İşaretli 64-bit tamsayı değeri temsil eder |
| Dize |Sabit - veya değişken uzunlukta karakter veri temsil eder |

## <a name="see-also"></a>Ayrıca Bkz.
* Genel OData eşleme işlemi ve amacı anlamak ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme](marketplace-publishing-data-service-creation-odata-mapping.md) tanımları, yapılar ve yönergeleri gözden geçirmek için.
* Örnekler gözden geçirme ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme örnekler](marketplace-publishing-data-service-creation-odata-mapping-examples.md) örnek kodu görmek ve kod sözdizimi ve bağlamı anlamak için.
* Veri Hizmeti Azure Marketinde yayımlama için belirtilen yol dönmek için bu makaleyi okuyun [veri hizmeti yayımlama Kılavuzu](marketplace-publishing-data-service-creation.md).
