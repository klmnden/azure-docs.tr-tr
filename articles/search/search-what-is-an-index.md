<properties
    pageTitle="Azure Search dizininizi oluşturma | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search dizini nedir ve nasıl kullanılır?"
    services="search"
    documentationCenter=""
authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="05/31/2016"
    ms.author="ashmaka"/>

# Bir Azure Search dizini oluşturma
> [AZURE.SELECTOR]
- [Genel Bakış](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## Dizin nedir?

Bir *dizin*, Azure Search hizmeti tarafından kullanılan kalıcı bir *belge* ve diğer yapıların deposudur. Bir belge, dizininizdeki aranabilir verilerin tek bir birimidir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Azure Search'te belge eklediğinizde/yüklediğinizde ve arama sorguları gönderdiğinizde, isteklerinizi arama hizmetinizdeki belirli bir dizine gönderirsiniz.

## Bir Azure Search dizinindeki alan türleri ve öznitelikleri

Şemanızı tanımlarken, dizininizdeki her bir alan için ad, tür ve öznitelikler belirtmeniz gerekir. Alan türü, bu alanda depolanan verileri sınıflandırır. Öznitelikler, alanın nasıl kullanıldığını belirtmek için tek tek alanlarda ayarlanır. Aşağıdaki tablolar belirtebileceğiniz türleri ve öznitelikleri numaralandırır.


### Alan türleri
|Tür|Açıklama|
|------------|-----------|
|*Edm.String*|Tam metin arama için isteğe bağlı olarak belirteç haline getirilebilen metin (sözcük bölünmesi, kök ayırma, vb.).|
|*Collection(Edm.String)*|Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir.|
|*Edm.Boolean*|True/false değerlerini içerir.|
|*Edm.Int32*|32 bit tamsayı değerleri.|
|*Edm.Int64*|64 bit tamsayı değerleri.|
|*Edm.Double*|Çift duyarlıklı sayısal veriler.|
|*Edm.DateTimeOffset*|OData V4 biçiminde (ör. `yyyy-MM-ddTHH:mm:ss.fffZ` ve `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`) temsil edilen tarih ve saat değerleri.|
|*Edm.GeographyPoint*|Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta.|

Azure Search'ün [desteklediği veri türleri hakkında MSDN'de](https://msdn.microsoft.com/library/azure/dn798938.aspx) daha ayrıntılı bilgiler edinebilirsiniz.



### Alan öznitelikleri
|Öznitelik|Açıklama|
|------------|-----------|
|*Anahtar*|Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır.|
|*Alınabilir*|Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir.|
|*Filtrelenebilir*|Alanın filtre sorgularında kullanılmasını sağlar.|
|*Sıralanabilir*|Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar.|
|*Modellenebilir*|Kullanıcının bağımsız filtrelemesi için [modellenmiş bir gezinmede](search-faceted-navigation.md) bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler.|
|*Aranabilir*|Alanı tam metin aranabilir şeklinde işaretler.|

Azure Search'ün [dizin öznitelikleri hakkında MSDN'de](https://msdn.microsoft.com/library/azure/dn798941.aspx) daha ayrıntılı bilgiler edinebilirsiniz.



## Bir dizin şemasını tanımlama kılavuzu

Dizininizi tasarlarken, her bir kararı düşünmek için planlama aşamasında zaman ayırın. Her bir alan için [uygun öznitelikler](https://msdn.microsoft.com/library/azure/dn798941.aspx) atanması gerektiğinden, dizininizi tasarlarken arama kullanıcı deneyimini ve iş gereksinimlerinizi göz önünde bulundurmanız önemlidir. Bir dizinin dağıtıldıktan sonra değiştirilmesi, verilerin yeniden oluşturulmasını ve yüklenmesini içerir.


Veri depolama gereksinimleri zamanla değişiyorsa bölüm ekleyerek veya kaldırarak kapasiteyi artırabilir ya da azaltabilirsiniz. Ayrıntılı bilgi için bkz. [Azure'da Search hizmetinizi yönetme](search-manage.md) veya [Hizmet Sınırları](search-limits-quotas-capacity.md).



<!----HONumber=Jun16_HO2-->


