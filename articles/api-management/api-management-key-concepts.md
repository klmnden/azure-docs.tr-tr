<properties 
    pageTitle="API Management temel kavramları" 
    description="API'ler, ürünler, roller, gruplar ve diğer API Management temel kavramları hakkında bilgi edinin." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="05/24/2016" 
    ms.author="sdanie"/>

#API Management nedir?

API Management, kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. Her yerdeki işletmeler, bir dijital platform olarak işlemlerini genişletmek, yeni kanallar oluşturmak, yeni müşteriler bulmak ve mevcut müşterilerle daha derin etkileşimi yürütmeyi amaçlar. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel yetkinlikler sağlar.

Azure API Management’e genel bakış için aşağıdaki videoyu izleyin ve erişim denetimi, hız sınırlaması, izleme, olay günlüğüne kaydetme ve yanıt önbelleğe alma gibi birçok özelliği minimum çabayla API’nize eklemek için API Management’i kullanmayı öğrenin.

> [AZURE.VIDEO azure-api-management-overview]

Yöneticiler API Management’i kullanmak için API'ler oluşturur. Her API bir veya daha fazla işlemden oluşur ve her API bir veya daha fazla ürüne eklenebilir. Bir API kullanmak için, geliştiriciler bu API’yi içeren ürüne abone olur ve böylece geçerli olabilecek kullanım ilkelerine tabi olarak API işlemini çağırabilir.

Bu konuda API Management temel kavramlarına ilişkin genel bir bakış verilmiştir.

>[AZURE.NOTE] Daha fazla bilgi için bkz. [Bulut tabanlı API Management: API'lerin Gücünden Yararlanma](http://j.mp/ms-apim-whitepaper) PDF teknik incelemesi. API Management hakkında CITO Research tarafından hazırlanan tanıtım amaçlı bu teknik inceleme şunları içerir: 
>
> - Yaygın API gereksinimleri ve sorunları
>     - API'leri ayırma ve cepheleri sunma
>     - Geliştiricilerin hızla hazır olmalarını sağlama
>     - Erişimi güvenli hale getirme
>     - Analizler ve ölçümler
> - API Management platformuyla denetim ve öngörü elde etme
> - Bulut ile şirket içi çözüm kullanımının karşılaştırması
> - Azure API Management

## <a name="apis"> </a>API’ler ve işlemler

API'ler bir API Management hizmet örneğinin temelini oluşturur. Her API geliştiricilere sunulan bir işlemler kümesini temsil eder. Her API, API’yi uygulayan arka uç hizmetine başvuru içerir ve bunun işlemleri arka uç hizmeti tarafından uygulanan işlemlere eşlenir. API Management işlemleri; URL eşleme, sorgu ve yol parametreleri, istek ve yanıt içeriği ve işlem yanıtını önbelleğe alma üzerinde sahip olunan denetim sayesinde yüksek oranda yapılandırılabilir niteliktedir. Hızı sınırı, kotalar ve IP kısıtlama ilkeleri de API veya tek işlem düzeyinde uygulanabilir.

Daha fazla bilgi için bkz. [API oluşturma][] ve [API’ye işlem ekleme][].


## <a name="products"> </a> Ürünler

Ürünler API'lerin geliştiricilerin kullanımına nasıl sunulduğudur. API Management ürünleri bir ya da daha fazla API’ye sahiptir. Başlık, açıklama ve kullanım koşulları ile yapılandırılırlar. Ürünler **Açık** veya **Korumalı** olabilir. Korumalı ürünleri kullanabilmek için bunlara abone olmak gerekir, açık ürünler abonelik olmadan kullanılabilir. Bir ürün geliştiriciler tarafından kullanılmaya hazır olduğunda yayımlanabilir. Yayımlandıktan sonra, geliştiriciler tarafından görüntülenebilir (ve abone olunan korumalı ürünler söz konusu olduğunda). Abonelik onayı ürün düzeyinde yapılandırılır ve yönetici onayı gerektirebilir ya da otomatik olarak onaylanır.

Gruplar, ürünlerin geliştiricilere görünürlüğünü yönetmek için kullanılır. Ürünler gruplara görünürlük sağlar ve geliştiriciler ait oldukları gruplar tarafından görünür olan ürünleri görüntüleyip bunlara abone olabilir. 

Daha fazla bilgi için [Ürün oluşturma ve yayımlama][] konusunu ve aşağıdaki videoyu inceleyin.

> [AZURE.VIDEO using-products]

## <a name="groups"> </a> Gruplar

Gruplar, ürünlerin geliştiricilere görünürlüğünü yönetmek için kullanılır. API Management aşağıdaki sabit sistem gruplarına sahiptir.

-   **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
-   **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
-   **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları kullanabilir](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Daha fazla bilgi için bkz. [Grupları oluşturma ve kullanma][].

## <a name="developers"> </a> Geliştiriciler

Geliştiriciler API Management hizmet örneğindeki kullanıcı hesaplarını temsil eder. Geliştiriciler yöneticiler tarafından oluşturulabilir ve davet edilebilir ya da [Geliştirici portalı][] üzerinden kaydolabilir. Her geliştirici bir veya daha fazla grubun üyesidir ve bu gruplara görünürlük sağlayan ürünlere abone olabilir.

Bir ürüne abone olan geliştiricilere ürün için birincil ve ikincil anahtar verilir. Bu anahtar ürün API’lerine çağrılar yapılırken kullanılır.

Daha fazla bilgi için bkz. [Geliştirici oluşturma ve davet etme][] ve [Grupları geliştiricilerle ilişkilendirme][].

## <a name="policies"> </a> İlkeler

İlkeler, yayımcının API’nin davranışını yapılandırma yoluyla değiştirmesini sağlayan güçlü API Management özellikleridir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. Sık kullanılan deyimler, XML’den JSON’a biçim dönüştürmeyi ve bir geliştiriciden gelen çağrıların sayısını sınırlamak üzere çağrı hızını sınırlamayı ve çeşitli ilkeleri içerir.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) ve [Değişken ayarla](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için [Gelişmiş İlkeler](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies) ve [İlke ifadeleri](https://msdn.microsoft.com/library/azure/dn910913.aspx) bölümleriyle aşağıdaki videoyu inceleyin.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

API Management ilkelerinin tam listesi için bkz. [İlke başvurusu][]. İlkeleri yapılandırma ve kullanma hakkında daha fazla bilgi için bkz. [API Management ilkeleri][]. Hız sınırı ve kota ilkeleri içeren bir ürün oluşturmaya ilişkin öğretici için bkz. [Gelişmiş ürün ayarları oluşturma ve yapılandırma][]. Gösteri için aşağıdaki videoya bakın.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"> </a> Geliştirici portalı

Geliştirici portalı, geliştiricilerin API’leriniz hakkında bilgi alabileceği, işlemleri görüntüleyebileceği ve çağırabileceği ve ürünlere abone olabileceği yerdir. Müşteri adayları, geliştirici portalını ziyaret edebilir, API’lerle işlemleri görüntüleyebilir ve portala kaydolabilir. Geliştirici portalınızın URL’si, API Management hizmet örneğinizin Klasik Azure Portalı’ndaki panoda yer alır.

Özel içerik ekleyerek, stilleri özelleştirerek ve marka bilgilerinizi ekleyerek, geliştirici portalınızın genel görünümünü özelleştirebilirsiniz.

## API Management ve API ekonomisi

API Management hakkında daha fazla bilgi için Microsoft Ignite 2015 konferansına ait aşağıdaki sunuyu izleyin.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[API’ler ve işlemler]: #apis
[Ürünler]: #products
[Gruplar]: #groups
[Geliştiriciler]: #developers
[İlkeler]: #policies
[Geliştirici portalı]: #developer-portal

[API oluşturma]: api-management-howto-create-apis.md
[API’ye işlem ekleme]: api-management-howto-add-operations.md
[Ürün oluşturma ve yayımlama]: api-management-howto-add-products.md
[Grupları oluşturma ve kullanma]: api-management-howto-create-groups.md
[Grupları geliştiricilerle ilişkilendirme]: api-management-howto-create-groups.md#associate-group-developer
[Gelişmiş ürün ayarları oluşturma ve yapılandırma]: api-management-howto-product-with-rules.md
[Geliştirici oluşturma ve davet etme]: api-management-howto-create-or-invite-developers.md
[İlke başvurusu]: api-management-policy-reference.md
[API Management ilkeleri]: api-management-howto-policies.md
[API Management hizmet örneği oluşturma]: api-management-get-started.md#create-service-instance



 



<!---HONumber=Jun16_HO2-->


