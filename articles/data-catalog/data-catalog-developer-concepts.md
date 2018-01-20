---
title: "Veri Kataloğu Geliştirici kavramları | Microsoft Docs"
description: "Anahtar kavramlar Kataloğu REST API'si aracılığıyla sunulan olarak Azure veri Kataloğu kavramsal modelde giriş."
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 48d4a33f7667786f2eb8851ed69dedc206e777ae
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="azure-data-catalog-developer-concepts"></a>Azure veri Kataloğu Geliştirici kavramları
Microsoft **Azure veri Kataloğu** kitle kaynak veri kaynağı meta verilerini ve veri kaynağı bulma için özellikleri sağlayan bir tam olarak yönetilen bir bulut hizmetidir. Geliştiriciler kendi REST API'leri aracılığıyla hizmetini kullanabilirsiniz. Hizmetinde uygulanan kavramları anlama başarıyla ile tümleştirmek geliştiricilere yönelik önemli **Azure veri Kataloğu**.

## <a name="key-concepts"></a>Önemli kavramlar
**Azure veri Kataloğu** kavramsal model dört temel kavramları hakkında temel: **katalog**, **kullanıcılar**, **varlıklar**, ve  **Ek açıklamalar**.

![Kavram][1]

*Şekil 1 - Azure veri Kataloğu Basitleştirilmiş kavramsal model*

### <a name="catalog"></a>Katalog
A **katalog** kuruluş depolar tüm meta veriler için üst düzey bir kapsayıcısıdır. Bir **katalog** Azure hesap başına izin verilen. Katalog bağlı bir Azure aboneliği, ancak yalnızca bir **katalog** herhangi belirli Azure hesabı için bir hesap birden fazla abonelik olabilir olsa da oluşturulabilir.

Bir katalog içeren **kullanıcılar** ve **varlıklar**.

### <a name="users"></a>Kullanıcılar
Kullanıcılardır eylemleri gerçekleştirmek için izinlere sahip güvenlik sorumlularının (katalogda arama, eklemek, düzenlemek veya kaldırmak öğeleri, vb.) Kataloğu.

Bir kullanıcı sahip birçok farklı rol vardır. Bölüm rolleri ve yetkilendirme rolleri hakkında daha fazla bilgi için bkz.

Tek tek kullanıcılar ve güvenlik grupları eklenebilir.

Azure veri Kataloğu, kimlik ve erişim yönetimi için Azure Active Directory kullanır. Her bir katalog kullanıcı hesabı için Active Directory üyesi olması gerekir.

### <a name="assets"></a>Varlıklar
A **katalog** veri varlıklarını içerir. **Varlıklar** katalog tarafından yönetilen ayrıntı birim şunlardır.

Bir varlık ayrıntı veri kaynağına göre değişir. SQL Server ya da Oracle veritabanı için bir varlık bir tablo veya görünüm olabilir. SQL Server Analysis Services için bir varlık, bir ölçü, boyut veya bir ana performans göstergesi (KPI) olabilir. SQL Server Raporlama Hizmetleri için bir varlık bir rapordur.

Bir **varlık** Kataloğu'ndan ekleyip şeydir. Sonuç, geri alma gelen birimidir **arama**.

Bir **varlık** adı, konumu ve türü ve daha fazla açıklayan ek açıklamaları oluşur.

### <a name="annotations"></a>Ek Açıklamalar
Ek açıklamalar meta veri varlıkları hakkında gösteren öğelerdir.

Örnek açıklamalarının açıklama, etiketler, şema, belge, vb. verilebilir. Ek açıklama türleri ve varlık türleri tam bir listesi varlık nesne modeli bölümünde verilmiştir.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Kitle kaynak ek açıklamalar ve kullanıcı perspektifinden (fikir çokluğu)
Bir anahtar Azure veri kataloğu nasıl sistemde kitle kaynak meta verilerin desteklediği yönüdür. Aygıtlardır wiki yaklaşımın – burada yalnızca bir fikir ve son yazıcı WINS – yan yana sistemde Canlı birden çok görüşlerini Azure veri Kataloğu modeli sağlar.

Bu yaklaşım, farklı kullanıcıların farklı perspektiflerini belirli bir varlık üzerinde sahip olduğu gerçek dünya kurumsal verilerin yansıtır:

* Veritabanı Yöneticisi toplu ETL işlemleri için hizmet düzeyi sözleşmelerine veya kullanılabilir işleme penceresi hakkında bilgiler sağlayabilir
* Bir veri steward varlık uygulandığı iş süreçlerini ya da iş uygulanmış sınıflandırmaları hakkında bilgi sağlayabilir
* Finans analist verileri süre sonu raporlama görevleri sırasında nasıl kullanıldığı hakkında bilgi sağlayabilir

Bu örnek desteklemek için her bir kullanıcı – DBA, veri steward ve analist – bir açıklama ve kataloğa kayıtlı tek bir tabloya ekleyebilirsiniz. Tüm açıklamaları sistemde korunur ve Azure veri Kataloğu Portalı'nda tüm açıklamaları görüntülenir.

JSON yükündeki nesne türleri genellikle diziler özellikleri için bir singleton burada bekleyebilirsiniz şekilde bu deseni nesne modeli içindeki öğelerin çoğu uygulanır.

Örneğin, varlık altında kök açıklama nesneleri dizisidir. Dizi özelliğinin "Açıklamalar" olarak adlandırılır. Açıklama nesnesi bir özellik - açıklama sahiptir. Düzen açıklama nesne türleri açıklamasını alır her bir kullanıcı kullanıcı tarafından sağlanan değer için oluşturulan yöneliktir.

UX sonra nasıl birleşimi görüntüleneceğini seçebilirsiniz. Görüntü için üç farklı düzenleri vardır.

* Basit düzeni "Tümünü Göster" dir. Bu modelinde tüm nesneler bir liste görünümünde gösterilir. Azure veri Kataloğu portalını UX açıklamasını bu deseni kullanır.
* Başka bir desen "Merge" dir. Bu modelinde farklı kullanıcılar tüm değerleri birlikte, kaldırılan yineleme ile birleştirilir. Azure veri Kataloğu portalında UX bu deseni etiketler ve uzmanlar özellikleri gösterilebilir.
* Üçüncü bir desen "son yazan kazanır" dir. Bu modelinde yalnızca yazdığınız en son değer gösterilir. friendlyName bu deseni örneğidir.

## <a name="asset-object-model"></a>Varlık nesne modeli
Anahtar kavramlar bölümünde tanıtılan **Azure veri Kataloğu** nesne modeli varlıklar ya da ek açıklamalar öğeleri içerir. Öğelerinin isteğe bağlı ya da gerekli özellikler vardır. Bazı özellikler tüm öğeler için geçerlidir. Bazı özellikler tüm varlıklar için geçerlidir. Bazı özellikleri yalnızca belirli varlık türleri için geçerlidir.

### <a name="system-properties"></a>Sistem özellikleri
<table><tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>timestamp</td><td>Tarih Saat</td><td>En son ne zaman öğesi değiştirildi. Bu alan, bir öğe eklendiğinde ve öğenin her güncelleştirildiğinde sunucu tarafından oluşturulur. Bu özelliğin değeri giriş, göz ardı edilir işlemleri yayımlayın.</td></tr><tr><td>id</td><td>Uri</td><td>Mutlak url öğesinin (salt okunur). Öğesi için benzersiz adreslenebilir URI değil.  Bu özelliğin değeri giriş, göz ardı edilir işlemleri yayımlayın.</td></tr><tr><td>type</td><td>Dize</td><td>(Salt okunur) varlık türü.</td></tr><tr><td>ETag</td><td>Dize</td><td>Katalog öğeleri güncelleştirme işlemleri gerçekleştirirken iyimser eşzamanlılık denetimi için kullanılabilir öğe sürümüne karşılık gelen bir dize. "*" herhangi bir değer eşleştirmek için kullanılır.</td></tr></table>

### <a name="common-properties"></a>Ortak Özellikler
Bu özellikleri tüm kök varlık türleri ve tüm ek açıklama türleri için geçerlidir.

<table>
<tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>fromSourceSystem</td><td>Boole</td><td>Öğenin veri (Sql Server veritabanı, Oracle veritabanı gibi) bir kaynak sistemden türetilmiş veya bir kullanıcı tarafından yazılan olup olmadığını gösterir.</td></tr>
</table>

### <a name="common-root-properties"></a>Ortak kök özellikleri
<p>
Bu özellikleri tüm kök varlık türleri için geçerlidir.

<table><tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>ad</td><td>Dize</td><td>Veri kaynağı konumu bilgileri türetilmiş bir adı</td></tr><tr><td>dsl</td><td>DataSourceLocation</td><td>Benzersiz olarak veri kaynağını tanımlayan ve varlık tanımlayıcılarıdır biridir. (Çift kimlik bölümüne bakın).  Dsl yapısını protokolü ve kaynak türüne göre değişir.</td></tr><tr><td>veri kaynağı</td><td>Datasourceınfo</td><td>Daha fazla ayrıntı varlık türü.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Bu varlık en son kaydedilen kullanıcının açıklar.  Kullanıcı (upn) ve bir görünen ad (Soyadı ve adı) için benzersiz kimliğini içerir.</td></tr><tr><td>containerId</td><td>Dize</td><td>Veri kaynağı için kapsayıcı varlık kimliği. Bu özellik kapsayıcı türü için desteklenmiyor.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Yaygın olarak kullanılan tek olmayan ek açıklama özellikleri
Bu özellikleri tüm tek olmayan ek açıklama türleri için geçerlidir (birden çok olmasına izin verilen ek açıklamaları varlık başına).

<table>
<tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>anahtar</td><td>Dize</td><td>Açıklama geçerli koleksiyonu içinde benzersiz olarak tanımlayan bir kullanıcı belirtilen anahtar. Anahtar uzunluğu 256 karakterden uzun olamaz.</td></tr>
</table>

### <a name="root-asset-types"></a>Kök varlık türleri
Kök varlık türleri ve kataloğa kayıtlı veri varlıklarını çeşitli türlerde temsil eden bu türleridir. Her kök türü için varlık ve görünüme dahil ek açıklamaları açıklar bir görünüm yok. Görünüm adı, REST API kullanarak bir varlık yayımlarken karşılık gelen {view_name} url kesimi kullanılmalıdır.

<table><tr><td><b>Varlık türü (Görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri türü</b></td><td><b>İzin verilen ek açıklamaları</b></td><td><b>Açıklamalar</b></td></tr><tr><td>Tabloda ("Tablo")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Şema<p>ColumnDescription<p>ColumnTag<p> Uzman<p>Önizleme<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Belgeler<p></td><td>Bir tabloda hiç tablo verisi temsil eder.  Örneğin: SQL tablosu, SQL görünümü, Analysis Services Tabular tablo, Analysis Services çok boyutlu boyut, Oracle tablo, vs.   </td></tr><tr><td>Ölçü ("ölçüler")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir Analysis Services ölçü temsil eder.</td></tr><tr><td></td><td>ölçü</td><td>Sütun</td><td></td><td>Ölçü açıklayan meta veriler</td></tr><tr><td></td><td>isCalculated </td><td>Boole</td><td></td><td>Ölçüyü veya hesaplanan olmadığını belirtir.</td></tr><tr><td></td><td>measureGroup</td><td>Dize</td><td></td><td>Ölçü için fiziksel kapsayıcı</td></tr><td>KPI ("kpis")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler</td><td></td></tr><tr><td></td><td>measureGroup</td><td>Dize</td><td></td><td>Ölçü için fiziksel kapsayıcı</td></tr><tr><td></td><td>goalExpression</td><td>Dize</td><td></td><td>Sayısal MDX ifadesi veya bir hesaplama KPI hedef değerini döndürür.</td></tr><tr><td></td><td>valueExpression</td><td>Dize</td><td></td><td>KPI gerçek değerini döndüren bir MDX sayısal ifade.</td></tr><tr><td></td><td>statusExpression</td><td>Dize</td><td></td><td>Zaman içinde belirtilen bir noktada KPI durumunu temsil eden MDX ifadesi.</td></tr><tr><td></td><td>trendExpression</td><td>Dize</td><td></td><td>Zaman içinde KPI değerini değerlendiren bir MDX ifadesi. Eğilim belirli iş bağlamında yararlıdır zamana dayalı ölçüt olabilir.</td>
<tr><td>Rapor ("rapor")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir SQL Server Reporting Services rapor temsil eder </td></tr><tr><td></td><td>assetCreatedDate</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Dize</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Dize</td><td></td><td></td></tr><tr><td>Kapsayıcı ("kapsayıcıları")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir SQL veritabanı, bir Azure BLOB kapsayıcısı veya bir Analysis Services modeli gibi diğer varlıklar bir kapsayıcıyı temsil eder.</td></tr></table>

### <a name="annotation-types"></a>Ek açıklama türleri
Ek açıklama türü kataloğun içindeki diğer tür atanmış meta veri türlerini temsil eder.

<table>
<tr><td><b>Ek açıklama türü (iç içe Görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>Açıklama ("Açıklamalar")</td><td></td><td></td><td>Bu özellik, bir varlık için bir açıklama içerir. Sistem, her bir kullanıcı kendi açıklama ekleyebilirsiniz.  Yalnızca bu kullanıcı açıklaması nesnesine düzenleyebilirsiniz.  (Admins ve varlık sahipleri açıklama nesneyi silmek ancak düzenlemek değil). Sistem, kullanıcıların açıklamaları ayrı ayrı tutar.  Bu nedenle açıklamaları her varlık (büyük olasılıkla bilgi içeren bir veri kaynağından türetilmiş yanı sıra varlık hakkında bilgilerini katıldığını her kullanıcı için bir tane) üzerinde bir dizi yoktur.</td></tr>
<tr><td></td><td>açıklama</td><td>dize</td><td>Varlık kısa bir açıklama (2-3 satırlar)</td></tr>

<tr><td>Etiket ("etiketler")</td><td></td><td></td><td>Bu özellik, bir varlık için bir etiket tanımlar. Sistem, her bir kullanıcı bir varlık için birden çok etiket ekleyebilirsiniz.  Etiket nesneleri oluşturan kullanıcının bunları düzenleyebilirsiniz.  (Yöneticiler ve varlık sahipleri etiketi nesneyi silmek ancak düzenlemek değil). Sistem, kullanıcıların etiketleri ayrı ayrı tutar.  Bu nedenle her varlık etiketi nesneler dizisi yok.</td></tr>
<tr><td></td><td>etiket</td><td>dize</td><td>Varlık açıklayan bir etiket.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Bu özellik, bir varlık için bir kolay ad içerir. FriendlyName tek ek açıklama - bir varlık için yalnızca bir FriendlyName eklenebilir.  FriendlyName nesnesi oluşturan kullanıcının düzenleyebilirsiniz. (Yöneticiler ve varlık sahipleri FriendlyName nesneyi silmek ancak düzenlemek değil). Sistem, kullanıcıların kolay adlar ayrı ayrı tutar.</td></tr>
<tr><td></td><td>FriendlyName</td><td>dize</td><td>Varlık kolay adı.</td></tr>

<tr><td>Şeması ("şema")</td><td></td><td></td><td>Şema verilerinin yapısını açıklar.  Özniteliği (sütun, öznitelik, alan, vb.) adlarını listeler, de diğer meta veri türleri.  Bu bilgiler tüm veri kaynağından türetilir.  Şema tek ek açıklama - yalnızca bir şema için bir varlık eklenebilir.</td></tr>
<tr><td></td><td>sütunları</td><td>Sütun]</td><td>Sütun nesnelerinin bir dizisi. Bunlar, veri kaynağından türetilmiş bilgilerle sütun açıklanmaktadır.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Bu özellik, bir sütun için bir açıklama içerir.  Sistem, her bir kullanıcı birden çok sütun (en çok her bir sütunun) için kendi açıklamalar ekleyebilirsiniz. ColumnDescription nesneleri oluşturan kullanıcının bunları düzenleyebilirsiniz.  (Yöneticiler ve varlık sahipleri ColumnDescription nesneyi silmek ancak düzenlemek değil). Sistem, bu kullanıcının sütun açıklamaları ayrı ayrı tutar.  Bu nedenle her varlık (sütun bilgilerini büyük olasılıkla bilgi içeren bir veri kaynağından türetilmiş yanı sıra sütun hakkında katıldığını her kullanıcı için her bir) üzerinde ColumnDescription nesnelerinin bir dizisi yok.  Eşitlenmemiş sınıflandırıp ColumnDescription geniş şemaya bağlı. ColumnDescription şemada artık bir sütun açıklayabilir.  Bu, açıklama ve şema eşitlenmiş tutmak için yazıcı kadar olur.  Veri kaynağı sütunları açıklama bilgilere sahip ve bunlar Aracı çalıştırırken oluşturulacak ek ColumnDescription nesneler.</td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu açıklama başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>açıklama</td><td>Dize</td><td>kısa bir açıklama (2-3 satırları) sütun.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Bu özellik, bir sütun için bir etiket içerir. Her kullanıcı sisteminin belirtilen sütun için birden çok etiket ekleyebilirsiniz ve birden çok sütun için etiketler ekleyebilirsiniz. ColumnTag nesneleri oluşturan kullanıcının bunları düzenleyebilirsiniz. (Yöneticiler ve varlık sahipleri ColumnTag nesneyi silmek ancak düzenlemek değil). Sistem, bu kullanıcıların sütun etiketleri ayrı ayrı tutar.  Bu nedenle her varlık üzerinde ColumnTag nesnelerinin bir dizisi yok.  Eşitlenmemiş sınıflandırıp ColumnTag geniş şemaya bağlı. ColumnTag şemada artık bir sütun açıklayabilir.  Bu sütun etiketini ve şema eşitlenmiş tutmak için yazıcı kadar olur.</td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu etiket başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>etiket</td><td>Dize</td><td>Sütunu açıklayan bir etiket.</td></tr>

<tr><td>Uzman ("uzmanlar")</td><td></td><td></td><td>Bu özellik veri kümesindeki bir uzman kabul bir kullanıcıyı içerir. Uzmanlar opinions(descriptions) Kabarcık açıklamaları listelerken UX üstüne. Her bir kullanıcı kendi uzmanlar belirtebilirsiniz. Yalnızca bu kullanıcının uzmanlar nesneyi düzenleyebilirsiniz. (Admins ve varlık sahipleri Uzman nesneleri silmek ancak düzenlemek değil).</td></tr>
<tr><td></td><td>uzman</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Önizleme ("Önizleme")</td><td></td><td></td><td>Önizleme üst 20 satır varlık verilerinin ekran görüntüsünü içerir. Önizleme yalnızca algılama için oluşturma (tablo için kullanılabilir ancak ölçü mantıklıdır) varlıklar bazı türleri.</td></tr>
<tr><td></td><td>önizleme</td><td>Object]</td><td>Bir sütunu temsil eden nesneler dizisi.  Her nesnenin bir özellik eşlemesi satır için bu sütun için bir değer içeren bir sütuna vardır.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mime türü</td><td>dize</td><td>İçeriğin MIME türü.</td></tr>
<tr><td></td><td>içerik</td><td>dize</td><td>Bu veri varlığına erişmek hakkında yönergeler. İçeriği bir URL, bir e-posta adresi veya yönergeler kümesi olabilir.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>Int</td><td>Veri kümesi satır sayısı</td></tr>
<tr><td></td><td>boyut</td><td>uzun</td><td>Veri kümesinin bayt cinsinden boyutu.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>dize</td><td>En son ne zaman şeması değiştirildi.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>dize</td><td>Veri kümesi değiştirildiği son tarih (veri, değişiklik, eklendi veya Sil)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sütunları</td></td><td>ColumnDataProfile]</td><td>Sütun veri profiller bir dizi.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Dize</td><td>Bu sınıflandırma başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>Sınıflandırma</td><td>Dize</td><td>Bu sütundaki veri sınıflandırması.</td></tr>

<tr><td>Belgeleri ("belgeleri")</td><td></td><td></td><td>Belirli bir varlık ile ilişkili yalnızca bir belge olabilir.</td></tr>
<tr><td></td><td>mime türü</td><td>dize</td><td>İçeriğin MIME türü.</td></tr>
<tr><td></td><td>içerik</td><td>dize</td><td>Documentation içeriği.</td></tr>

</table>

### <a name="common-types"></a>Genel türleri
Genel türleri özelliklerini türleri olarak kullanılabilir, ancak öğeleri değildir.

<table>
<tr><td><b>Ortak türü</b></td><td><b>Özellikleri</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>Datasourceınfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sourceType</td><td>dize</td><td>Veri kaynağı türünü açıklar.  Örneğin: SQL Server, Oracle veritabanı, vs.  </td></tr>
<tr><td></td><td>objectType</td><td>dize</td><td>Veri kaynağı nesnesi türünü açıklar. Örneğin: Tablo, SQL Server için görüntüleyin.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protokol</td><td>dize</td><td>Gereklidir. Veri kaynağı ile iletişim kurmak için kullanılan bir protokol açıklar. Örneğin: SQl Server, Oracle, vb. için "oracle" için "tds". Başvurmak [veri kaynağı başvurusu belirtimi - DSL yapısı](data-catalog-dsr.md) şu anda desteklenen protokollerin listesi için.</td></tr>
<tr><td></td><td>Adres</td><td>Sözlük<string, object></td><td>Gereklidir. Başvurulan veri kaynağı tanımlamak için kullanılan protokol belirli veri kümesini adresidir. Belirli bir protokol için kapsamlı adresi, yani Protokolü bilmeden anlamsız veridir.</td></tr>
<tr><td></td><td>kimlik doğrulaması</td><td>dize</td><td>İsteğe bağlı. Veri kaynağı ile iletişim kurmak için kullanılan kimlik doğrulama düzeni. Örneğin: windows, oauth vb.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Sözlük<string, object></td><td>İsteğe bağlı. Bir veri kaynağına bağlanma hakkında ek bilgiler.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Arka uç tüm AAD karşı sağlanan özellikler doğrulama yayımlama sırasında gerçekleştirmez.</td></tr>
<tr><td></td><td>UPN</td><td>dize</td><td>Kullanıcının benzersiz e-posta adresi. ObjectID sağlanmazsa, veya "lastRegisteredBy" özelliği, aksi takdirde isteğe bağlı bağlamında belirtilmelidir.</td></tr>
<tr><td></td><td>objectId</td><td>Guid</td><td>Kullanıcı veya güvenlik grubu AAD kimliği. İsteğe bağlı. UPN, aksi takdirde isteğe bağlı sağlanmadı varsa belirtilmelidir.</td></tr>
<tr><td></td><td>firstName</td><td>dize</td><td>Kullanıcı (görüntüleme amaçlarıyla) adı. İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerli. Güvenlik sorumlusu "rol", "izinleri" ve "uzmanlar" için sağlanırken belirtilemez.</td></tr>
<tr><td></td><td>Soyadı</td><td>dize</td><td>Kullanıcının soyadını (görüntüleme amaçlarıyla). İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerli. Güvenlik sorumlusu "rol", "izinleri" ve "uzmanlar" için sağlanırken belirtilemez.</td></tr>

<tr><td>Sütun</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>dize</td><td>Sütun veya öznitelik adı.</td></tr>
<tr><td></td><td>type</td><td>dize</td><td>sütun veya özniteliğin veri türü. İzin verilen türleri varlık veri kaynak türü bağlıdır.  Yalnızca bir alt türleri desteklenir.</td></tr>
<tr><td></td><td>maxLength</td><td>Int</td><td>Sütun veya özniteliği için izin verilen maksimum uzunluğu. Veri kaynağından türetilmiş. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>duyarlık</td><td>bayt</td><td>Sütun veya özniteliği için hassasiyet. Veri kaynağından türetilmiş. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>IsNullable</td><td>Boole</td><td>Sütun veya bir null değere sahip izin verilip verilmediğine. Veri kaynağından türetilmiş. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>
<tr><td></td><td>ifade</td><td>dize</td><td>Hesaplanmış bir sütun değeri geçerliyse, bu alanın değeri ifade eder ifade içeriyor. Veri kaynağından türetilmiş. Yalnızca bazı kaynak türleri için geçerlidir.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>dize</td><td>Sütunun adı</td></tr>
<tr><td></td><td>type </td><td>dize</td><td>Sütunun türü</td></tr>
<tr><td></td><td>dk </td><td>dize</td><td>Veri kümesindeki en düşük değer</td></tr>
<tr><td></td><td>en çok </td><td>dize</td><td>Veri kümesindeki en büyük değer</td></tr>
<tr><td></td><td>ort </td><td>double</td><td>Veri kümesindeki ortalama değer</td></tr>
<tr><td></td><td>STDEV </td><td>double</td><td>Veri kümesi için standart sapma</td></tr>
<tr><td></td><td>nullCount </td><td>Int</td><td>Veri kümesindeki bir null değer sayısı</td></tr>
<tr><td></td><td>distinctCount  </td><td>Int</td><td>Veri kümesi ayrı değerleri sayısı</td></tr>


</table>

## <a name="asset-identity"></a>Varlık Kimliği
Azure veri Kataloğu katalog içindeki varlık gidermek için kullanılan varlık kimliğini oluşturmak için DataSourceLocation "dsl" özelliği "Adres" özellik paketi "Protokolü" ve kimlik özelliklerini kullanır.
Örneğin, "tds" Protokolü kimlik Özellikler "server", "veritabanı", "şema" ve "nesne" vardır. Protokol ve kimlik özellikleri bileşimleri, SQL Server tablo varlık kimliğini oluşturmak için kullanılır.
Azure veri Kataloğu sağlar adresinde listelenmiş birkaç yerleşik veri kaynağı protokolleri [veri kaynağı başvurusu belirtimi - DSL yapısı](data-catalog-dsr.md).
Desteklenen protokolleri kümesi programlı olarak Genişletilebilir (veri Kataloğu REST API başvuru bakın). Katalog yöneticileri özel veri kaynağı protokolleri kaydedebilirsiniz. Aşağıdaki tabloda özel bir protokol kaydetmek için gereken özellikleri açıklanmaktadır.

### <a name="custom-data-source-protocol-specification"></a>Özel veri kaynağı protokolü belirtimi
<table>
<tr><td><b>Tür</b></td><td><b>Özellikleri</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad alanı</td><td>dize</td><td>Ad alanı protokolü. Namespace olmalıdır 1 ila 255 karakterden uzun nokta (.) tarafından ayrılmış bir veya daha fazla boş bölümleri içerir. Her bölüm 1 ile 255 karakter uzunluğunda olmalıdır, bir harf ile başlamalı ve yalnızca harfler ve sayılar içeren.</td></tr>
<tr><td></td><td>ad</td><td>dize</td><td>Protokol adı. Adı 1 ila 255 karakter uzunluğunda olmalıdır, bir harf ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty[]</td><td>Liste kimlik özelliklerinin, en az bir, ancak hiçbir 20'den fazla özelliklerine sahip olması gerekir. Örneğin: "server", "veritabanı", "şema", "nesne" olan "tds" Protokolü kimlik özelliklerinin.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Kimlik listesini ayarlar. Geçerli varlığın kimliğini temsil etmek kimlik özellikler kümesini tanımlar. En az bir, ancak hiçbir 20'den fazla kümeleri içermelidir. Örneğin: {"server", "veritabanı", "şema" ve "nesne"} olan Sql Server tablosu varlık kimliğini tanımlar "tds" Protokolü için bir kimlik.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>dize</td><td>Özelliğin adı. Adı 1 ile 100 karakterden uzun bir harf ile başlangıç için olmalıdır ve yalnızca harf ve sayı içerebilir.</td></tr>
<tr><td></td><td>type</td><td>dize</td><td>Özelliğin türü. Desteklenen değerler: "bool", boolean ","bayt","guid","int","tamsayı","uzun","dize","url"</td></tr>
<tr><td></td><td>ignoreCase</td><td>bool</td><td>Servis talebi özelliğin değerini kullanırken yok sayılıp sayılmayacağını belirtir. Yalnızca "dize" türündeki özellikler için belirtilebilir. Varsayılan değer false şeklindedir.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>bool[]</td><td>Durum her URL'nin yol kesimi için yok sayılıp sayılmayacağını belirtir. Yalnızca "url" türündeki özellikler için belirtilebilir. Varsayılan değer [] false'tur.</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>dize</td><td>Kimlik kümesinin adı.</td></tr>
<tr><td></td><td>properties</td><td>string[]</td><td>Bu kimlik dahil kimlik özelliklerinin listesini ayarlayın. Yinelenen değer içeremez. Kimlik kümesi tarafından başvurulan her bir özellik Protokolü "identityProperties" listesinde tanımlanması gerekir.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Rolleri ve yetkilendirme
Microsoft Azure veri Kataloğu varlıkları ve ek açıklamaları CRUD işlemleri için yetkilendirme özellikleri sağlar.

## <a name="key-concepts"></a>Önemli kavramlar
Azure veri Kataloğu iki yetkilendirme mekanizması kullanır:

* Rol tabanlı yetkilendirme
* İzni tabanlı bir yetkilendirme

### <a name="roles"></a>Roller
Üç rol vardır: **yönetici**, **sahibi**, ve **katkıda bulunan**.  Her rol kapsam ve aşağıdaki tabloda özetlenen hakları sahiptir.

<table><tr><td><b>Rol</b></td><td><b>Kapsam</b></td><td><b>Hakları</b></td></tr><tr><td>Yönetici</td><td>Katalog (tüm varlıklar/ek açıklamaları kataloğunda)</td><td>Okuma silme ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Sahip</td><td>Her varlık (kök öğe)</td><td>Okuma silme ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Katılımcı</td><td>Her bir tek varlığı ve ek açıklaması</td><td>Okuma güncelleştirme Delete ViewRoles Not: Tüm hakları katkıda bulunan öğe üzerinde okuma iptal edilirse iptal</td></tr></table>

> [!NOTE]
> **Okuma**, **güncelleştirme**, **silmek**, **ViewRoles** hakları herhangi bir öğeye (varlık veya ek açıklama) sırasında geçerli **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** yalnızca kök varlık için geçerlidir.
> 
> **Silme** öğeyi ve alt öğeler veya bunun altındaki tek öğe sağ uygulanır. Örneğin, bir varlık silindiğinde de bu varlık için ek açıklamaları silinir.
> 
> 

### <a name="permissions"></a>İzinler
İzni erişim denetimi girdileri listesidir. Her erişim denetim girdisi güvenlik sorumlusu hakkı kümesi atar. İzinler, yalnızca bir varlığı (diğer bir deyişle, kök öğe) belirtilebilir ve varlık ve tüm alt öğeler için geçerlidir.

Sırasında **Azure veri Kataloğu** Önizleme, yalnızca **okuma** sağa bir varlık görünürlüğü kısıtlamak senaryosunu sağlamak üzere izin listesinde desteklenir.

Varsayılan olarak tüm kimliği doğrulanmış kullanıcının sahip **okuma** sağ katalogdaki herhangi bir öğenin görünürlüğü sorumluları izinleri kümesine kısıtlanmadığı sürece.

## <a name="rest-api"></a>REST API
**PUT** ve **POST** görünümü öğesi istekleri rolleri ve izinleri denetlemek için kullanılabilir: öğesi yükü ek olarak, iki sistem özellikler belirtilebilir **rolleri** ve  **izinleri**.

> [!NOTE]
> **izinleri** yalnızca bir kök öğe için geçerlidir.
> 
> **Sahibi** rolü yalnızca bir kök öğe için geçerlidir.
> 
> Bir öğe katalogda oluşturulduğunda, varsayılan olarak, **katkıda bulunan** şu anda kimliği doğrulanmış kullanıcının ayarlanır. Öğesi herkes tarafından güncelleştirilebilir gerekiyorsa **katkıda bulunan** ayarlanmalıdır &lt;herkes&gt; özel bir güvenlik sorumlusu **rolleri** öğesi ilk çalıştırıldığında özelliği yayımlanan (bakın Aşağıdaki örnek için). **Katkıda bulunan** değiştirilemez ve aynı yaşam süresi sırasında bir öğe kalır (hatta **yönetici** veya **sahibi** değiştirme yetkisine sahip değil **katkıda bulunan**). Açık ayarı için desteklenen tek değerdir **katkıda bulunan** olan &lt;herkesin&gt;: **katkıda bulunan** yalnızca bir öğe oluşturan bir kullanıcı olabilir veya &lt;herkes &gt;.
> 
> 

### <a name="examples"></a>Örnekler
**Kümesine katkıda bulunan &lt;herkesin&gt; öğeyi yayımlarken.**
Özel bir güvenlik sorumlusu &lt;herkesin&gt; objectID "00000000-0000-0000-0000-000000000201" sahiptir.
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Bazı HTTP istemci uygulamaları otomatik olarak bir 302 yanıt sunucusundan gelen istekleri reddetmesini ancak genellikle istek yetkilendirme üstbilgileri Şerit. Yetkilendirme üst bilgisi istekleri için Azure veri Kataloğu yapmak için gerekli olduğundan, Authorization Üstbilgisi hala Azure veri Kataloğu tarafından belirtilen bir yeniden yönlendirme konumu için bir istek verilene sağlanır emin olmalısınız. Aşağıdaki örnek kod, .NET HttpWebRequest nesnesi kullanarak göstermektedir.
> 
> 

**Gövde**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Sahiplerini atamak ve var olan bir kök öğe için görünürlüğü kısıtlama**: **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> PUT içinde bir öğe yükü gövdesinde belirtmek için zorunlu değildir: PUT, yalnızca rolleri ve/veya izinlerini güncelleştirmek için kullanılabilir.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
