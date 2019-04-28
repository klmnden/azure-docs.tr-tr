---
title: Azure veri Kataloğu Geliştirici kavramları
description: Temel kavramlar olarak Kataloğu REST API aracılığıyla kullanıma sunulan Azure veri Kataloğu kavramsal model giriş.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 42e4b545a48bcbd0ad4b7faf077ebdbfe21648b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61002684"
---
# <a name="azure-data-catalog-developer-concepts"></a>Azure veri Kataloğu Geliştirici kavramları
Microsoft **Azure veri Kataloğu** kitle kaynak veri kaynağı meta verilerini ve veri kaynağı bulma için özellikler sağlayan bir tam olarak yönetilen bir bulut hizmetidir. Geliştiriciler, hizmet, REST API'leri aracılığıyla kullanabilir. Hizmette uygulanan kavramları anlamak, başarılı bir şekilde tümleştirmek geliştiricilere yönelik önemli **Azure veri Kataloğu**.

## <a name="key-concepts"></a>Önemli kavramlar
**Azure veri Kataloğu** kavramsal model dört temel kavramları dayanır: **Kataloğu**, **kullanıcılar**, **varlıklar**, ve **ek açıklamalar**.

![Kavram][1]

*Şekil 1 - Azure veri Kataloğu Basitleştirilmiş kavramsal model*

### <a name="catalog"></a>Katalog
A **Kataloğu** kuruluş depolar tüm meta veriler için üst düzey bir kapsayıcıdır. Bir **Kataloğu** Azure hesap başına izin verilir. Katalog bağlı bir Azure aboneliği, ancak tek **Kataloğu** belirli bir Azure hesabı için bir hesap birden fazla abonelik olsa bile oluşturulabilir.

Katalog içeren **kullanıcılar** ve **varlıklar**.

### <a name="users"></a>Kullanıcılar
Eylemleri gerçekleştirmek için izinlere sahip güvenlik sorumlularının kullanıcılardır (katalogda arama, ekleme, düzenleme veya kaldırma öğeleri vb.) Kataloğu.

Bir kullanıcı sahip çeşitli farklı roller bulunur. Bölüm rolleri ve yetkilendirme rolleri hakkında daha fazla bilgi için bkz.

Bireysel kullanıcılar ve güvenlik gruplarına eklenebilir.

Azure veri Kataloğu, kimlik ve erişim yönetimi için Azure Active Directory kullanır. Her bir katalog kullanıcı hesabı için Active Directory üyesi olmalıdır.

### <a name="assets"></a>Varlıklar
A **Kataloğu** veri varlıklarını içerir. **Varlıklar** Kataloğu tarafından yönetilen ayrıntı birimidir.

Ayrıntı düzeyi bir varlığın veri kaynağına göre değişir. SQL Server veya Oracle Database'e için bir varlık, bir tablo veya görünüm olabilir. SQL Server Analysis Services için bir varlık, bir ölçü, bir boyut ya da bir ana performans göstergesi (KPI) olabilir. SQL Server Raporlama Hizmetleri için bir varlığın bir rapordur.

Bir **varlık** Kataloğu'ndan ekleyip şeydir. Sonuç, geri alma gelen birimidir **arama**.

Bir **varlık** adı, konum ve türünü ve daha fazla açıklayan ek açıklamalar oluşur.

### <a name="annotations"></a>ek açıklamaları
Ek açıklamalar varlıklar hakkındaki meta verileri temsil eden öğelerdir.

Örnek ek açıklamaları, açıklama, etiketler, şema, belgeler vb. verilebilir. Varlık türleri ve ek açıklama türleri tam listesi varlık nesnesi modeli bölümünde verilmiştir.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Kitle kaynak ek açıklamalar ve kullanıcı perspektif (fikrim çokluğu)
Bir anahtar, Azure veri Kataloğu'na nasıl sistemde kitle kaynak meta verileri destekler yönüdür. Karşılık olarak wiki bir yaklaşıma – burada yalnızca bir fikrim ve son yazıcı WINS-Azure veri Kataloğu modeli yan yana sistemde Canlı birden çok fikirlerini verir.

Bu yaklaşım, farklı kullanıcılar belirli bir varlık üzerinde farklı perspektiflerini burada olabilir gerçek dünyada kurumsal veri yansıtır:

* Bir veritabanı Yöneticisi hizmet düzeyi sözleşmeleri veya kullanılabilir işleme penceresi hakkında bilgi için toplu ETL işlemleri sağlayabilir.
* Veri Görevlisi varlık uygulandığı iş süreçlerine veya iş uygulanmış sınıflandırmaları hakkında bilgi sağlayabilir.
* Finans analist süre sonu raporlama görevleri sırasında verilerin nasıl kullanıldığı hakkında bilgi sağlayabilir.

Bu örnekte desteklemek için her bir kullanıcı – DBA, veri Görevlisi ve analist – bir açıklama ve kataloğa kayıtlı tek bir tabloya ekleyebilirsiniz. Tüm açıklamaları sistemde korunur ve Azure veri Kataloğu Portalı'nda tüm açıklamaları görüntülenir.

JSON yükündeki nesne türleri genellikle diziler özellikleri için burada tek beklediğiniz şekilde bu düzen nesne modeli içindeki öğelerin çoğu için uygulanır.

Örneğin, varlık altında kök açıklama nesnelerinin bir dizisi ' dir. Dizi özelliği "Açıklamalar" olarak adlandırılır. Bir açıklama nesnesi bir özellik - açıklama sahiptir. Bir açıklama nesne türleri açıklama her kullanıcı için kullanıcı tarafından sağlanan değer oluşturulan modelidir.

UX sonra nasıl görüntüleneceğini bileşimini seçebilirsiniz. Görüntü için üç farklı desenleri vardır.

* En basit desen, "Show All" olduğundan. Bu düzende, tüm nesneler liste görünümünde gösterilir. Azure veri Kataloğu portalı UX açıklaması için bu deseni kullanır.
* "Birleştirme" başka bir desendir. Bu düzende, farklı kullanıcılar tüm değerleri kaldırılmış bir yineleme ile birlikte birleştirilir. Azure veri Kataloğu portalında UX bu düzeni örnekleri etiketleri ve uzmanlar özelliklerdir.
* "Son yazıcı WINS" buna üçüncü bir desendir. Bu düzende, yalnızca yazdığınız en son değer gösterilir. friendlyName Bu düzenin bir örnektir.

## <a name="asset-object-model"></a>Varlık nesne modeli
Anahtar kavramlar bölümünde sunulduğu şekilde **Azure veri Kataloğu** nesne modeli, varlıklar veya ek açıklamaları olabilir öğeleri içerir. Öğeleriniz özellikleri, isteğe bağlı veya gerekli olabilir. Bazı özellikler tüm öğeler için geçerlidir. Bazı özellikler tüm varlıklar için geçerlidir. Bazı özellikler, yalnızca belirli bir varlık türleri için geçerlidir.

### <a name="system-properties"></a>Sistem özellikleri
<table><tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>timestamp</td><td>DateTime</td><td>Öğeye erişildiği son zaman. Bu alan, bir öğe eklendiğinde ve bir öğesi her güncelleştirildiğinde sunucu tarafından oluşturulur. Bu özelliğin değeri giriş yoksayılır yayımlama işlemleri.</td></tr><tr><td>id</td><td>Uri</td><td>(Salt okunur) öğesinin mutlak url. Bu öğe için benzersiz adreslenebilir URI değil.  Bu özelliğin değeri giriş yoksayılır yayımlama işlemleri.</td></tr><tr><td>type</td><td>String</td><td>(Salt okunur) varlık türü.</td></tr><tr><td>Etag</td><td>String</td><td>Katalog öğeleri güncelleştirme işlemleri gerçekleştirirken iyimser eşzamanlılık denetimi için kullanılabilir öğe sürümüne karşılık gelen bir dize. "*" herhangi bir değeri eşlemek için kullanılabilir.</td></tr></table>

### <a name="common-properties"></a>Ortak Özellikler
Bu özellikler, tüm kök varlık türleri ve tüm ek açıklama türleri için geçerlidir.

<table>
<tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>fromSourceSystem</td><td>Boolean</td><td>Öğenin veri (Sql Server veritabanı, Oracle Database gibi) bir kaynak sistemi türetilen veya bir kullanıcı tarafından yazılmış olup olmadığını gösterir.</td></tr>
</table>

### <a name="common-root-properties"></a>Ortak kök Özellikler
<p>
Bu özellikler, tüm kök varlık türleri için geçerlidir.

<table><tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr><tr><td>ad</td><td>String</td><td>Veri kaynağı konumu bilgilerinden türetilen bir ad</td></tr><tr><td>dsl</td><td>Veri</td><td>Benzersiz veri kaynağını tanımlayan ve varlığın tanımlayıcıları biridir. (Çift kimlik bölümüne bakın).  Dsl yapısını protokolü ve kaynak türüne göre değişir.</td></tr><tr><td>Veri kaynağı</td><td>Datasourceınfo</td><td>Varlık türü hakkında daha fazla bilgi.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Bu varlık en kısa süre önce kaydolan kullanıcı açıklar.  Kullanıcı (upn) ve bir görünen ad (soyad ve ad) için benzersiz kimliğini içerir.</td></tr><tr><td>Containerıd</td><td>String</td><td>Veri kaynağı için kapsayıcı varlık kimliği. Bu özellik, kapsayıcı türü için desteklenmiyor.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Ortak olmayan ek özellikler
Bu özellikler, tüm singleton olmayan ek açıklama türleri için geçerlidir (birden fazla izin verilen ek açıklamaları, varlık başına).

<table>
<tr><td><b>Özellik adı</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>anahtar</td><td>String</td><td>Bir kullanıcı tarafından belirtilen ek açıklama geçerli koleksiyonunda benzersiz olarak tanımlayan anahtar. Anahtar uzunluğu 256 karakterden uzun olamaz.</td></tr>
</table>

### <a name="root-asset-types"></a>Kök varlık türleri
Kök varlık türleri ve kataloğa kayıtlı veri varlıklarını çeşitli türlerini temsil eden bu türleridir. Her kök türü için varlık ve ek açıklamalar görünümde açıklayan bir görünüm yok. Görünüm adı, REST API kullanarak bir varlığı yayımlarken karşılık gelen {view_name} url kesimini kullanılmalıdır.

<table><tr><td><b>Varlık türü (Görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri türü</b></td><td><b>İzin verilen ek açıklamaları</b></td><td><b>Açıklamalar</b></td></tr><tr><td>Tablo ("Tablo")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Şema<p>ColumnDescription<p>ColumnTag<p> Uzman<p>Önizleme<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Belgeler<p></td><td>Tablo hiç tablo verisi temsil eder.  Örneğin: SQL tablo, SQL görünümü, Analysis Services tablolu tablosu, Analysis Services çok boyutlu boyut, Oracle tablo, vs.   </td></tr><tr><td>Ölçü birimi ("ölçüler")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir Analysis Services ölçüsü temsil eder.</td></tr><tr><td></td><td>ölçü</td><td>Sütun</td><td></td><td>Ölçü açıklayan meta verileri</td></tr><tr><td></td><td>isCalculated </td><td>Boolean</td><td></td><td>Ölçüyü veya hesaplanan varsa belirtir.</td></tr><tr><td></td><td>measureGroup</td><td>String</td><td></td><td>Ölçü için fiziksel kapsayıcı</td></tr><td>KPI'yı ("KPI'lar")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler</td><td></td></tr><tr><td></td><td>measureGroup</td><td>String</td><td></td><td>Ölçü için fiziksel kapsayıcı</td></tr><tr><td></td><td>goalExpression</td><td>String</td><td></td><td>Sayısal MDX ifadesi veya bir hesaplama KPI hedef değerini döndürür.</td></tr><tr><td></td><td>valueExpression</td><td>String</td><td></td><td>KPI gerçek değerini döndüren bir MDX sayısal ifade.</td></tr><tr><td></td><td>statusExpression</td><td>String</td><td></td><td>Zaman içinde belirli bir noktada KPI durumunu temsil eden bir MDX ifadesi.</td></tr><tr><td></td><td>trendExpression</td><td>String</td><td></td><td>KPI değeri zaman içinde değerlendirilen MDX ifadesi. Eğilim belirli iş bağlamında yararlıdır zamana bağlı ölçütü olabilir.</td>
<tr><td>Rapor ("rapor")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir SQL Server Reporting Services rapor temsil eder. </td></tr><tr><td></td><td>assetCreatedDate</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>String</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>String</td><td></td><td></td></tr><tr><td>Kapsayıcı ("kapsayıcı")</td><td></td><td></td><td>Açıklama<p>FriendlyName<p>Etiket<p>Uzman<p>AccessInstruction<p>Belgeler<p></td><td>Bu tür bir SQL veritabanı, bir Azure BLOB kapsayıcısı veya bir Analysis Services modeli gibi diğer varlıklar bir kapsayıcıyı temsil eder.</td></tr></table>

### <a name="annotation-types"></a>Ek açıklama türleri
Ek açıklama türleri katalog içindeki diğer türlerine atanan meta veri türlerini temsil eder.

<table>
<tr><td><b>Ek açıklama türü (iç içe Görünüm adı)</b></td><td><b>Ek Özellikler</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>Açıklaması ("Açıklamalar")</td><td></td><td></td><td>Bu özellik, bir varlık için bir açıklama içerir. Sistemin her bir kullanıcı kendi açıklama ekleyebilirsiniz.  Yalnızca bu kullanıcının açıklama nesneyi düzenleyebilirsiniz.  (Yöneticiler ve varlık sahipleri açıklama nesneyi silmek ancak düzenleme olmadan). Sistem, kullanıcıların açıklamaları ayrı ayrı tutar.  Bu nedenle her varlık (büyük olasılıkla bir veri kaynağından elde edilen bilgileri içeren ek olarak, varlık hakkında bilgilerini katkılarıyla her kullanıcı için bir tane) üzerinde açıklamaları dizisi yok.</td></tr>
<tr><td></td><td>açıklama</td><td>string</td><td>Varlık kısa açıklaması (2-3 satır)</td></tr>

<tr><td>Etiketi ("tags")</td><td></td><td></td><td>Bu özellik, bir varlık için bir etiket tanımlar. Her bir kullanıcı sistemin bir varlık için birden çok etiket ekleyebilirsiniz.  Etiket nesneleri oluşturan kullanıcı bunları düzenleyebilirsiniz.  (Yöneticiler ve varlık sahipleri etiket nesneyi silmek ancak düzenleme olmadan). Sistem, kullanıcıların etiket ayrı ayrı tutar.  Bu nedenle her varlık etiketi nesneleri dizisi yok.</td></tr>
<tr><td></td><td>etiket</td><td>string</td><td>Varlığı açıklayan bir etiketi.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Bu özellik, bir varlık için bir kolay ad içerir. FriendlyName tek ek açıklama - FriendlyName tek bir varlık için eklenebilir.  FriendlyName nesnesi oluşturan kullanıcı düzenleyebilirsiniz. (Yöneticiler ve varlık sahipleri FriendlyName nesnesini silme ancak düzenleme olmadan). Sistem, kullanıcıların kolay adlar ayrı ayrı tutar.</td></tr>
<tr><td></td><td>FriendlyName</td><td>string</td><td>Varlık kolay adı.</td></tr>

<tr><td>Şemaya ("şeması")</td><td></td><td></td><td>Şema veri yapısını açıklar.  Bu öznitelik (sütunu, öznitelik, alan, vb.) adlarını listeler, türleri de diğer meta veriler.  Bu bilgiler, tüm veri kaynağından türetilir.  Şema tek ek açıklama - yalnızca bir şema için bir varlık eklenebilir.</td></tr>
<tr><td></td><td>Sütunları</td><td>[Sütun]</td><td>Sütun nesneleri dizisi. Bunlar, veri kaynağından elde edilen bilgileri ile sütun açıklanmaktadır.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Bu özellik, bir sütun için bir açıklama içerir.  Sistemin her kullanıcı birden çok sütun (en fazla sütun başına) için kendi açıklamalar ekleyebilirsiniz. ColumnDescription nesneleri oluşturan kullanıcı bunları düzenleyebilirsiniz.  (Yöneticiler ve varlık sahipleri ColumnDescription nesnesini silme ancak düzenleme olmadan). Sistem, bu kullanıcının sütun açıklamaları ayrı ayrı tutar.  Bu nedenle her varlık (büyük olasılıkla bir veri kaynağından elde edilen bilgileri içeren ek sütun hakkında bilgilerini katkılarıyla her bir kullanıcı için sütun başına bir adet) ColumnDescription nesneleri dizisi yok.  Eşitlenmemiş alabilmeniz ColumnDescription gevşek şemaya bağlı. Şemada artık bir sütun ColumnDescription açıklayabilir.  Bu açıklama ve şema eşitlenmiş şekilde tutmanızı sağlayacak kadar yazardır.  Veri kaynağı de sütun açıklaması bilgilere sahip ve bunlar Aracı çalıştırırken oluşturulacak ek ColumnDescription nesneler.</td></tr>
<tr><td></td><td>ColumnName</td><td>String</td><td>Bu açıklama başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>açıklama</td><td>String</td><td>kısa bir açıklaması (2-3 satır) sütun.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Bu özellik, bir sütun için bir etiket içerir. Sistemin her bir kullanıcı belirtilen sütun için birden fazla etiket ekleyebilir ve birden çok sütun için etiketler ekleyebilirsiniz. ColumnTag nesneleri oluşturan kullanıcı bunları düzenleyebilirsiniz. (Yöneticiler ve varlık sahipleri ColumnTag nesnesini silme ancak düzenleme olmadan). Sistem, bu kullanıcıların sütun etiketleri ayrı ayrı tutar.  Bu nedenle her varlık üzerinde ColumnTag nesnelerinin bir dizisi yok.  Eşitlenmemiş alabilmeniz ColumnTag gevşek şemaya bağlı. Şemada artık bir sütun ColumnTag açıklayabilir.  Bu sütun etiketi ve şema eşitlenmiş şekilde tutmanızı sağlayacak kadar yazardır.</td></tr>
<tr><td></td><td>ColumnName</td><td>String</td><td>Bu etiket başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>etiket</td><td>String</td><td>Sütun açıklayan bir etiketi.</td></tr>

<tr><td>Uzman ("uzmanlara")</td><td></td><td></td><td>Bu özellik, Uzman veri kümesi olarak kabul edilir bir kullanıcıyı içerir. Uzmanlara opinions(descriptions) Kabarcık açıklamaları listelerken UX üstüne. Her kullanıcının kendi uzmanlar belirtebilirsiniz. Yalnızca bu kullanıcının uzmanlar nesneyi düzenleyebilirsiniz. (Yöneticiler ve varlık sahipleri Uzman nesneleri silin ancak düzenleme olmadan).</td></tr>
<tr><td></td><td>uzman</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Önizleme ("önizlemeler")</td><td></td><td></td><td>Önizleme, üst 20 satırları varlık için verilerin anlık görüntüsünü içerir. Önizleme yalnızca mantıklı bazı türleri (tablo için ölçü ancak mantıklıdır) varlıkları için.</td></tr>
<tr><td></td><td>önizleme</td><td>Object]</td><td>Bir sütunu temsil eden nesneleri dizisi.  Her nesne bir özellik eşlemesi için bu sütun satır için bir değer olan bir sütuna sahiptir.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mime türü</td><td>string</td><td>İçeriğin MIME türü.</td></tr>
<tr><td></td><td>content</td><td>string</td><td>Bu veri varlığına nasıl erişebileceklerini yönelik yönergeler. İçeriği bir URL, bir e-posta adresi veya yönergeleri kümesi olabilir.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>Veri kümesindeki satır sayısı</td></tr>
<tr><td></td><td>boyut</td><td>uzun</td><td>Veri kümesinin bayt cinsinden boyutu.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>string</td><td>Son şema değiştirildi</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>string</td><td>Veri kümesi olarak değiştirildiği son zamanı (veri eklendiğinde, değiştirilmesi veya Sil)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Sütunları</td></td><td>ColumnDataProfile]</td><td>Sütun veri profillerinin bir dizisi.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ColumnName</td><td>String</td><td>Bu sınıflandırma başvurduğu sütunun adı.</td></tr>
<tr><td></td><td>sınıflandırma</td><td>String</td><td>Bu sütunda veri sınıflandırması.</td></tr>

<tr><td>Belgeleri ("Belgeler")</td><td></td><td></td><td>Belirli bir varlık ile ilişkili yalnızca bir belge sağlayabilirsiniz.</td></tr>
<tr><td></td><td>mime türü</td><td>string</td><td>İçeriğin MIME türü.</td></tr>
<tr><td></td><td>content</td><td>string</td><td>Documentation içeriği.</td></tr>

</table>

### <a name="common-types"></a>Genel türleri
Genel türleri özelliklerini türleri kullanılabilir, ancak öğeleri değildir.

<table>
<tr><td><b>Ortak tür</b></td><td><b>Özellikleri</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>
<tr><td>Datasourceınfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kaynak türü</td><td>string</td><td>Veri kaynağı türünü açıklar.  Örneğin: SQL Server, Oracle veritabanı, vb.  </td></tr>
<tr><td></td><td>ObjectType</td><td>string</td><td>Veri kaynağı nesne türü açıklanır. Örneğin: Tablo, SQL Server için görünümü.</td></tr>

<tr><td>Veri</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protokol</td><td>string</td><td>Gereklidir. Veri kaynağı ile iletişim kurmak için kullanılan bir protokolü açıklar. Örneğin: SQl Server, Oracle, vb. "oracle" için "tds". Başvurmak <a href="https://docs.microsoft.com/azure/data-catalog/data-catalog-dsr">veri kaynağı başvurusu belirtiminin - DSL yapısı</a> şu anda desteklenen protokollerin listesi için.</td></tr>
<tr><td></td><td>adres</td><td>Sözlük<string, object></td><td>Gereklidir. Başvurulan veri kaynağını tanımlamak için kullanılan protokole özel veri kümesi adresidir. Belirli bir protokol için kapsamlı adresi anlamına gelir Protokolü farkında olmadan anlamsız verilerdir.</td></tr>
<tr><td></td><td>kimlik doğrulaması</td><td>string</td><td>İsteğe bağlı. Veri kaynağı ile iletişim kurmak için kullanılan kimlik doğrulama düzeni. Örneğin: windows, oauth vb.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Sözlük<string, object></td><td>İsteğe bağlı. Bir veri kaynağına bağlanma hakkında ek bilgiler.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Arka uç, AAD karşı sağlanan özellikler herhangi bir doğrulama yayımlama sırasında gerçekleştirmez.</td></tr>
<tr><td></td><td>UPN</td><td>string</td><td>Kullanıcının benzersiz e-posta adresi. ObjectID sağlanmazsa, veya "lastRegisteredBy" özelliği, aksi halde isteğe bağlı bağlamında belirtilmesi gerekir.</td></tr>
<tr><td></td><td>objectId</td><td>Guid</td><td>Kullanıcı veya güvenlik grubu AAD kimliği. İsteğe bağlı. UPN, aksi halde isteğe bağlı sağlanmadı durumunda belirtilmelidir.</td></tr>
<tr><td></td><td>FirstName</td><td>string</td><td>(Görüntüleme amacıyla) kullanıcı adı. İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerli. Güvenlik sorumlusu "rolleri", "izinler" ve "uzmanlara" sağlanırken belirtilemez.</td></tr>
<tr><td></td><td>Soyadı</td><td>string</td><td>Son kullanıcı adını (görüntüleme amacıyla). İsteğe bağlı. Yalnızca "lastRegisteredBy" özelliği bağlamında geçerli. Güvenlik sorumlusu "rolleri", "izinler" ve "uzmanlara" sağlanırken belirtilemez.</td></tr>

<tr><td>Sütun</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Sütun veya öznitelik adı.</td></tr>
<tr><td></td><td>type</td><td>string</td><td>sütun veya öznitelik veri türü. İzin verilen türler, varlık veri kaynak türü bağlıdır.  Yalnızca bir alt türleri desteklenir.</td></tr>
<tr><td></td><td>maxLength</td><td>int</td><td>Sütun veya özniteliği için izin verilen maksimum uzunluğu. Veri kaynağından türetilir. Yalnızca, bazı kaynak türleri için de geçerlidir.</td></tr>
<tr><td></td><td>duyarlık</td><td>bayt</td><td>Duyarlık sütunu veya öznitelik. Veri kaynağından türetilir. Yalnızca, bazı kaynak türleri için de geçerlidir.</td></tr>
<tr><td></td><td>IsNullable</td><td>Boolean</td><td>Sütun null değeri olup olmamasına izin verilip verilmediğine. Veri kaynağından türetilir. Yalnızca, bazı kaynak türleri için de geçerlidir.</td></tr>
<tr><td></td><td>İfade</td><td>string</td><td>Değer hesaplanmış bir sütun ise bu alan değerini ifade ifadesi içerir. Veri kaynağından türetilir. Yalnızca, bazı kaynak türleri için de geçerlidir.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ColumnName </td><td>string</td><td>Sütunun adı</td></tr>
<tr><td></td><td>type </td><td>string</td><td>Sütun türü</td></tr>
<tr><td></td><td>dk </td><td>string</td><td>Veri kümesindeki en düşük değer</td></tr>
<tr><td></td><td>en çok </td><td>string</td><td>Veri kümesindeki en yüksek değer</td></tr>
<tr><td></td><td>ort </td><td>double</td><td>Veri kümesindeki ortalama değer</td></tr>
<tr><td></td><td>STDEV </td><td>double</td><td>Veri kümesi için standart sapma</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Veri kümesi null değerleri sayısı</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>Veri kümesindeki ayrı değerlerin sayısı</td></tr>


</table>

## <a name="asset-identity"></a>Varlık Kimliği
Azure veri Kataloğu, varlık Kataloğu içinde ele almak için kullanılan varlık kimliğini oluşturmak için veri "dsl" özelliği "Adres" özellik paketinden "protokol" ve kimlik özelliklerini kullanır.
Örneğin, "tds" Protokolü, kimlik özellikleri "server", "veritabanı", "şema" ve "nesne" vardır. Protokol ve kimlik özelliklerini bileşimleri, SQL Server tablo varlık kimliğini oluşturmak için kullanılır.
Azure veri Kataloğu sağlar, listelenen birçok yerleşik veri kaynağı Protokolü [veri kaynağı başvurusu belirtiminin - DSL yapısı](data-catalog-dsr.md).
Desteklenen protokoller kümesini programlı bir şekilde genişletilebilir (veri Kataloğu REST API'si başvurusu bakın). Katalog yöneticileri, özel veri kaynağı protokolleri kaydedebilirsiniz. Aşağıdaki tabloda özel Protokolü kaydetme için gereken özellikleri açıklanmaktadır.

### <a name="custom-data-source-protocol-specification"></a>Özel veri kaynağı protokolü belirtimi
<table>
<tr><td><b>Tür</b></td><td><b>Özellikleri</b></td><td><b>Veri türü</b></td><td><b>Açıklamalar</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad alanı</td><td>string</td><td>Ad alanı protokolü. Namespace olmalıdır 1 ila 255 karakter uzunluğunda olmalı, nokta (.) ile ayrılmış bir veya daha fazla boş olmayan bölüm içermelidir. Her parça gerekir 1 ila 255 karakter uzunluğunda olmalı, bir harf ile başlamalı ve yalnızca harf ve rakam içermelidir.</td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Protokol adı. Adı 1 ila 255 karakter uzunluğunda olmalı, bir harf ile başlamalı ve yalnızca harf, rakam ve tire (-) karakteri içermelidir.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty]</td><td>Kimlik özelliklerinin listesi, en az bir, ancak hiçbir 20'den fazla özelliklerine sahip olması gerekir. Örneğin: "server", "veritabanı", "şema", "nesne" olan "tds" protokolünün kimlik özellikleri.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Kimlik listesini ayarlar. Geçerli varlığın kimliğini temsil eden kimlik özellikler kümesini tanımlar. En az bir, ancak hiçbir 20'den fazla kümeleri içermelidir. Örneğin: {"sunucusu", "veritabanı", "şema" ve "nesne"}, Sql Server tablosunu varlık kimliğini tanımlayan "tds" Protokolü için ayarlanmış bir kimlik.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Özelliğin adı. Ad 1 ila 100 karakter uzunluğunda olmalı, bir harfle olmalıdır ve yalnızca harf ve rakam içerebilir.</td></tr>
<tr><td></td><td>type</td><td>string</td><td>Özelliğin türü. Desteklenen değerler: "Boole", Boole ","bayt","GUID","int","tamsayı","uzun","dize","url"</td></tr>
<tr><td></td><td>IgnoreCase</td><td>bool</td><td>Durum özelliğinin değeri kullanırken yoksayılıp yoksayılmaması gerektiğini gösterir. Yalnızca "dize" türüne sahip özellikler için belirtilebilir. Varsayılan değer false'tur.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>bool [']</td><td>Durum her URL'nin yol bölümü için yoksayılıp yoksayılmaması gerektiğini gösterir. Yalnızca "url" türündeki özellikler için belirtilebilir. Varsayılan değer [] false'tur.</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>ad</td><td>string</td><td>Kimlik adı ayarlayın.</td></tr>
<tr><td></td><td>properties</td><td>string[]</td><td>Bu kimliğin dahil kimlik özelliklerinin listesini ayarlayın. Bu, yinelenen öğeler içeremez. Kimlik kümesi tarafından başvurulan her bir özellik Protokolü "identityProperties" listesinde tanımlanması gerekir.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Rolleri ve yetkilendirme
Microsoft Azure veri Kataloğu, varlıklar ve ek açıklamalar CRUD işlemleri için yetkilendirme özellikleri sağlar.

## <a name="key-concepts"></a>Önemli kavramlar
Azure veri Kataloğu iki yetkilendirme mekanizmalarını kullanır:

* Rol tabanlı yetkilendirme
* İzni tabanlı yetkilendirme

### <a name="roles"></a>Roller
Üç rol vardır: **Yönetici**, **sahibi**, ve **katkıda bulunan**.  Her bir rolü, kapsam ve aşağıdaki tabloda özetlenen hakları vardır.

<table><tr><td><b>Rol</b></td><td><b>Kapsam</b></td><td><b>Hakları</b></td></tr><tr><td>Yönetici</td><td>Katalog (tüm varlıklar/ek açıklamalarda katalog)</td><td>Delete ViewRoles okuma

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Sahip</td><td>Her varlık (kök öğe)</td><td>Delete ViewRoles okuma

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Katılımcı</td><td>Her ayrı varlığı ve ek açıklama</td><td>Okuma güncelleştirme silme ViewRoles Not: Tüm hakları öğe üzerinde okuma katkıda bulunan ' iptal edilirse iptal</td></tr></table>

> [!NOTE]
> **Okuma**, **güncelleştirme**, **Sil**, **ViewRoles** haklarıdır herhangi bir öğeye (varlık veya ek açıklama) sırasında geçerli **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** yalnızca kök varlık için geçerlidir.
> 
> **Silme** öğeyi ve alt öğelerini veya altındaki tek öğe sağa uygulanır. Örneğin, bir varlığı silme aynı zamanda bu varlık için ek açıklamaları siler.
> 
> 

### <a name="permissions"></a>İzinler
Erişim denetimi girdileri bir listesi olarak izindir. Her erişim denetim girişi hakları için bir güvenlik sorumlusu kümesi atar. İzinler yalnızca bir varlık (diğer bir deyişle, kök öğesi) olarak belirtilebilir ve varlık ve tüm alt öğeleri için geçerlidir.

Sırasında **Azure veri Kataloğu** Önizleme, yalnızca **okuma** sağ izinler listesinde, bir varlığın görünürlüğü kısıtlamak senaryoyu etkinleştirmek için desteklenir.

Varsayılan olarak herhangi bir kimliği doğrulanmış kullanıcı sahip **okuma** sağ kataloğundaki herhangi bir öğe için görünürlük izinleri sorumlu grubunu sınırlı olduğu sürece.

## <a name="rest-api"></a>REST API
**PUT** ve **POST** görünüm öğesi istekleri rolleri ve izinleri denetlemek için kullanılabilir: öğesi yüküne ek olarak, iki sistemi özellikler belirtilebilir **rolleri** ve  **izinleri**.

> [!NOTE]
> **izinleri** yalnızca bir kök öğesi için geçerlidir.
> 
> **Sahibi** rolü yalnızca bir kök öğesi için geçerlidir.
> 
> Katalogda bir öğe oluşturulduğunda varsayılan olarak, **katkıda bulunan** kimliği doğrulanmış kullanıcı olarak ayarla. Öğe herkes tarafından güncelleştirilebilir gerekiyorsa **katkıda bulunan** ayarlanmalıdır &lt;herkes&gt; özel bir güvenlik sorumlusu **rolleri** öğesi ilk olduğunda özellik yayımlanan (bakın Aşağıdaki örnek için). **Katkıda bulunan** değiştirilemez ve yaşam süresi sırasında bir öğe aynı kalır (hatta **yönetici** veya **sahibi** değiştirme hakkına sahip değil **katkıdabulunan**). Açık ayarı için desteklenen tek değerdir **katkıda bulunan** olduğu &lt;herkes&gt;: **Katkıda bulunan** yalnızca bir öğe oluşturan bir kullanıcı olabilir veya &lt;herkes&gt;.
> 
> 

### <a name="examples"></a>Örnekler
**Kümesine katkıda bulunan &lt;herkes&gt; öğeyi yayımlarken.**
Özel bir güvenlik sorumlusu &lt;herkes&gt; objectID "00000000-0000-0000-0000-000000000201" vardır.
  **POST** https:\//api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Bazı HTTP istemci uygulamaları otomatik olarak bir 302 yanıt sunucusundan gelen istekleri yeniden gönderin ancak genellikle istekten yetkilendirme üst bilgileri kaldırın. Yetkilendirme üst bilgisi, Azure veri Kataloğu'na isteğinde bulunmak için gerekli olduğundan, yetkilendirme üst bilgisi hala bir yeniden yönlendirme, Azure veri Kataloğu tarafından belirtilen konuma bir isteği yeniden olduğunuzda sağlanan emin olmanız gerekir. Aşağıdaki örnek kod, .NET HttpWebRequest nesnesi kullanarak göstermektedir.
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

  **Sahipler atayın ve var olan bir kök öğenin görünürlüğü kısıtlama**: **PUT** https:\//api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

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
> KOY gövdesinde bir öğe yükü belirtmek için zorunlu: PUT yalnızca rolleri ve/veya izinlerinizi güncelleştirmek için kullanılabilir.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
