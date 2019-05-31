---
title: Kimlik doğrulama dahil olmak üzere Azure Storage Services REST API işlemleri çağırma | Microsoft Docs
description: Kimlik doğrulama dahil olmak üzere Azure Storage Services REST API işlemleri çağırma
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 38a120747734cbe4af8804a3e7596fc11a2c2eb3
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306654"
---
# <a name="using-the-azure-storage-rest-api"></a>Azure Depolama REST API’sini kullanma

Bu makalede, Blob Depolama hizmeti REST API kullanma ve hizmet çağrısı kimlik doğrulaması yapmayı gösterir. Bu bakış açısıyla REST araması yapmak nasıl bir şey REST ve hiçbir fikriniz hakkında bilen bir geliştirici olarak yazılır. REST çağrısı başvuru belgelerine bakın ve bir gerçek REST çağrısı – Çevir öğrenin hangi alanların Git nerede? REST çağrısı yapmayı edindikten sonra diğer depolama hizmeti REST API'leri kullanmak için bu bilgiyi yararlanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar 

Uygulama, bir depolama hesabı için blob depolama kapsayıcıları listeler. Bu makalede kod denemek için aşağıdaki öğeler gerekir: 

* Yükleme [Visual Studio 2019](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) aşağıdaki iş yüküyle birlikte sağlanır:
    - Azure geliştirme

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Genel amaçlı depolama hesabı. Bir depolama hesabı henüz sahip değilseniz, bkz. [depolama hesabı oluşturma](storage-quickstart-create-account.md).

* Bu makaledeki örnek depolama hesabındaki kapsayıcıları listesinde gösterilmiştir. Çıktıyı görmek için bazı kapsayıcıları başlamadan önce depolama depolama hesabındaki BLOB ekleyin.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Örnek uygulama, C# dilinde yazılmış bir konsol uygulamasıdır.

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için storage-dotnet-rest-api-with-auth klasörü arayın, açın ve üzerinde StorageRestApiAuth.sln çift tıklayın. 

## <a name="what-is-rest"></a>REST nedir?

REST anlamına gelir *temsili durum aktarımı*. Belirli bir tanımı için kullanıma [Wikipedia](https://en.wikipedia.org/wiki/Representational_state_transfer).

Temel olarak, REST ne zaman kullanabileceğiniz bir mimaridir API'leri çağırma veya API'leri çağrılacak kullanılabilir hale getirme. Her iki tarafında neler olduğunu bağımsızdır ve diğer yazılımları gönderme veya geri alma sırasında kullanılan çağırır. Mac, Windows, Linux, Android telefon veya tablet, iPhone, iPod veya web sitesi üzerinde çalışan bir uygulama yazma ve tüm bu platformlar için aynı REST API'yi kullanın. REST API çağrıldığında, ve/veya dışarı veri geçirilebilir. REST API adlandırılır – hangi önemli istekte geçirilen bilgileri ve yanıt olarak sunulan veri platformudur önemli değildir.

REST kullanma bilmek yararlı bir yetenektir. Azure ürün ekibine sık yeni özellikler serbest bırakır. Çoğu zaman, yeni özellikler REST arabirimi aracılığıyla erişilebilir. Bazı durumlarda, ancak özellikleri aracılığıyla ortaya henüz **tüm** depolama istemci kitaplıkları veya kullanıcı Arabirimi (örneğin, Azure portalı). Her zaman en son ve en kullanmak istiyorsanız, REST öğrenme bir gereksinimdir. Ayrıca, Azure depolama ile etkileşimde bulunmak için kendi kitaplık yazmak istediğiniz veya bir SDK ya da depolama istemci kitaplığı sahip olmayan bir programlama dili ile Azure depolamaya erişmek istiyorsanız, REST API kullanabilirsiniz.

## <a name="about-the-sample-application"></a>Örnek uygulama hakkında

Örnek uygulama, bir depolama hesabındaki kapsayıcıları listeler. REST API belgelerini bilgileri gerçek kodunuzu nasıl karşılık gelen anladığınızda, diğer REST çağrılarını çözmesini kolaydır. 

Bakarsanız [Blob hizmeti REST API'si](/rest/api/storageservices/Blob-Service-REST-API), tüm blob depolama alanında gerçekleştirebileceğiniz işlemler. Depolama istemci kitaplıkları vardır ve REST API'ler etrafında sarmalayıcıları – bunlar sizin için erişim depolama için doğrudan REST API'lerini kullanarak olmadan kolaylaştırır. Ancak, yukarıda belirtildiği gibi bazen REST API depolama istemci kitaplığı yerine kullanmak istediğiniz.

## <a name="rest-api-reference-list-containers-api"></a>REST API Başvurusu: Kapsayıcıları API listesi

Sayfa için REST API başvurusundaki göz atalım [ListContainers](/rest/api/storageservices/List-Containers2) işlemi. Bu bilgiler nerede bazı alanlar istek ve yanıt alınması anlamanıza yardımcı olur.

**İstek yöntemi**: AL. Bu fiili bir istek nesnesi özelliği olarak belirttiğiniz HTTP yöntemidir. Bu eylem için diğer değerler, HEAD, PUT ve DELETE, aradığınız API bağlı olarak içerir.

**İstek URI'si**: https://myaccount.blob.core.windows.net/?comp=list  Bu blob depolama hesabı uç noktasından oluşturulmuş `http://myaccount.blob.core.windows.net` ve kaynak dizesi `/?comp=list`.

[URI parametreleri](/rest/api/storageservices/List-Containers2#uri-parameters): ListContainers çağırırken kullanabileceğiniz ek sorgu parametreleri vardır. Birkaç bu parametreleri olan *zaman aşımı* çağrısı (saniye cinsinden) için ve *önek*, filtreleme için kullanılır.

Başka bir yararlı parametresi *maxresults:* bu değerden daha fazla kapsayıcı yoksa yanıt gövdesi içerecek bir *NextMarker* sonraki döndürülecek sonraki kapsayıcıyı belirten öğe İstek. Bu özelliği kullanmak için sağlamanız *NextMarker* olarak değer *işaret* parametresi sonraki isteğinde bulunduğunda URI. Bu özelliği kullanırken için disk belleği sonuçları ile benzerdir. 

Ek parametreler kullanmak için şu örnekteki gibi bir değeri ile kaynak dizesi ekleyin:

```
/?comp=list&timeout=60&maxresults=100
```

[İstek üst bilgileri](/rest/api/storageservices/List-Containers2#request-headers) **:** Bu bölümde, gerekli ve isteğe bağlı bir istek üst bilgilerini listeler. Üç üst bilgiler gereklidir: bir *yetkilendirme* başlık *x-ms-date* (içeriyor. istek için UTC saati), ve *x-ms-version* (REST sürümünü belirtir API) kullanın. Dahil olmak üzere *x-ms-istemci-request-id* üst için herhangi bir şey bu alan için değer ayarlayabilirsiniz; günlüğe kaydetme etkinleştirilmişse depolama analizi günlüklere yazılır isteğe bağlı – bilgilerindedir.

[İstek gövdesi](/rest/api/storageservices/List-Containers2#request-body) **:** Hiçbir istek gövdesi için ListContainers yoktur. İstek gövdesi tüm PUT işlemleri uygulamak için bir XML saklı erişim ilkeleri listesinde göndermenize olanak tanıyan SetContainerAccessPolicy yanı sıra, BLOB'ları karşıya yüklenirken kullanılır. Saklı erişim ilkeleri makalesinde açıklanan [paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).

[Yanıt durum kodu](/rest/api/storageservices/List-Containers2#status-code) **:** Bilmeniz gereken herhangi bir durum kodları söyler. Bu örnekte, bir HTTP durum kodu 200 Tamam. HTTP durum kodlarının tam bir listesi için kullanıma [durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). Depolama REST API'leri için belirli hata kodlarını görmek için bkz: [ortak REST API hata kodları](/rest/api/storageservices/common-rest-api-error-codes)

[Yanıt üstbilgileri](/rest/api/storageservices/List-Containers2#response-headers) **:** Bunlar *içerik türü*; *x-ms-request-id*, geçirilen; istek kimliği olduğu *x-ms-version*, kullanılan; Blob hizmetinin sürümü gösterir ve *tarih*, UTC biçiminde olduğunu ve ne zaman istek söyler yapıldı.

[Yanıt gövdesi](/rest/api/storageservices/List-Containers2#response-body): Bu alan, istenen veri sağlayan bir XML yapısıdır. Bu örnekte, yanıtı kapsayıcıları ve onların özelliklerini listesidir.

## <a name="creating-the-rest-request"></a>REST isteği oluşturma

Birkaç başlatmadan – notları üretimde çalışırken güvenlik için her zaman kullanın HTTP yerine HTTPS. Bu alıştırmanın amacı doğrultusunda, istek ve yanıt verilerini görüntülemek için HTTP kullanmanız gerekir. Gerçek REST çağrılarını İstek ve yanıt bilgileri görüntülemek için indirebileceğiniz [Fiddler](https://www.telerik.com/fiddler) ya da benzer bir uygulama. Visual Studio'da çözüm, depolama hesabı adı ve anahtarı sınıfında sabittir. ListContainersAsyncREST yöntemi REST isteği çeşitli bileşenleri oluşturmak için kullanılan yöntemleri için depolama hesabı adı ve depolama hesabı anahtarı geçirir. Gerçek dünya uygulamada, depolama hesabı adını ve anahtarını ortam değişkenleri, bir yapılandırma dosyasında bulunabilir veya bir Azure Key Vault'tan alınabilir.

Bizim örnek projesinde yetkilendirme üst bilgisi oluşturmak için kod içinde ayrı bir sınıftır. Size tüm sınıf alın ve kendi çözümünüze ekleyin ve "olduğu gibi." kullanabileceğini olur Yetkilendirme üst bilgisi kod, çoğu Azure depolama REST API çağrıları için çalışır.

Derleme bir HttpRequestMessage nesnesi olan isteği için program.CS'de Webhostbuilder'a ListContainersAsyncREST gidin. İstek oluşturmaya yönelik adımlar şunlardır: 

* Hizmet çağırmak için kullanılacak URI oluşturun. 
* HttpRequestMessage nesnesi oluşturun ve yük ayarlayın. Biz de herhangi bir şey geçmiyor çünkü ListContainersAsyncREST için null bir yüktür.
* X-ms-date ve x-ms-version için istek üst bilgilerini ekleyin.
* Yetkilendirme üst bilgisi almak ve ekleyin.

Bazı temel bilgileri: 

*  ListContainers için **yöntemi** olduğu `GET`. Bu değer, istek örneklerken ayarlanır. 
*  **Kaynak** değeri, bu nedenle API çağrılan, gösteren URI sorgu bölümünü olan `/?comp=list`. Daha önce belirtildiği gibi ilgili bilgileri gösteren başvuru belgeleri sayfasında kaynaktır [ListContainers API](/rest/api/storageservices/List-Containers2).
*  URI, Blob Hizmeti uç noktası için bu depolama hesabı oluşturma ve kaynak birleştirerek oluşturulur. Değeri **istek URI'si** olan yukarı sona erer `http://contosorest.blob.core.windows.net/?comp=list`.
*  ListContainers için **ıncludesearchresults: true** null ve hiçbir ek **üstbilgileri**.

Farklı bir API, aşağıdakiler gibi diğer parametreleri olabilir *Ifmatch*. Ifmatch burada kullanabileceğinize bir örnek, PutBlob çağırırken ' dir. Bu durumda, Ifmatch bir eTag öğesine ayarlayın ve yalnızca geçerli eTag blob üzerindeki sağladığınız eTag ile eşleşiyorsa blobu güncelleştirir. ETag alınırken beri başkası blob güncelleştirdi, değişikliğine geçersiz olmayacaktır. 

İlk olarak ayarlamak `uri` ve `payload`. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Ardından, yöntem ayarını istek örneği `GET` ve URI'si sağlama.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

X-ms-date ve x-ms-version için istek üst bilgilerini ekleyin. Bu kod ayrıca çağrı için gerekli ek istek üst bilgileri eklediğiniz yerdir. Bu örnekte, ek üstbilgi vardır. Fazladan üst bilgiler başarılı bir API SetContainerACL örneğidir. BLOB Depolama için erişim düzeyini "x-ms-blob-public-erişim" ve değeri olarak adlandırılan bir başlık ekler.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-07-29");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Yetkilendirme üst bilgisi oluşturur yöntemini çağırın ve istek üstbilgileri ekleyin. Yetkilendirme üst bilgisi oluşturma makalenin sonraki bölümlerinde görürsünüz. Yöntemi, bu kod parçacığında görebileceğiniz gibi GetAuthorizationHeader adıdır:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Bu noktada, `httpRequestMessage` yetkilendirme üst bilgilerle tamamlandı REST isteği içeriyor. 

## <a name="call-the-rest-api-with-the-request"></a>İstek ile REST API çağırma

İstek olduğuna göre REST isteği göndermek için SendAsync çağırabilir. SendAsync API'yi çağıran ve yanıtı geri alır. Yanıt durum kodu inceleyin (200 Tamam olduğunu), ardından yanıtı ayrıştıramadı. Bu durumda, kapsayıcıların bir XML listesini alın. İstek oluşturma, isteği yürütün ve sonra kapsayıcıları listesi yanıtı inceleyin GetRESTRequest yöntemi çağırmak için kod bakalım.

```csharp 
    // Send the request.
    using (HttpResponseMessage httpResponseMessage = 
      await new HttpClient().SendAsync(httpRequestMessage, cancellationToken))
    {
        // If successful (status code = 200), 
        //   parse the XML response for the container names.
        if (httpResponseMessage.StatusCode == HttpStatusCode.OK)
        {
            String xmlString = await httpResponseMessage.Content.ReadAsStringAsync();
            XElement x = XElement.Parse(xmlString);
            foreach (XElement container in x.Element("Containers").Elements("Container"))
            {
                Console.WriteLine("Container name = {0}", container.Element("Name").Value);
            }
        }
    }
}
```

Ağ algılayıcı gibi çalıştırırsanız [Fiddler](https://www.telerik.com/fiddler) SendAsync çağrısı yaparken, istek ve yanıt bilgileri görebilirsiniz. Şimdi bir göz atalım. Depolama hesabının adıdır *contosorest*.

**İstek:**

```
GET /?comp=list HTTP/1.1
```

**İstek Bağlıkları:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Yürütmeden sonra döndürülen durum kodu ve yanıt üst bilgileri:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Yanıt gövdesi (XML):** ListContainers için bu kapsayıcıları ve özelliklerinin listesini gösterir.

```xml  
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
  ServiceEndpoint="http://contosorest.blob.core.windows.net/">
  <Containers>
    <Container>
      <Name>container-1</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:39:48 GMT</Last-Modified>
        <Etag>"0x8D46CBD5A7C301D"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-2</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:40:50 GMT</Last-Modified>
        <Etag>"0x8D46CBD7F49E9BD"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-3</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:10 GMT</Last-Modified>
        <Etag>"0x8D46CBD8B243D68"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-4</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:25 GMT</Last-Modified>
        <Etag>"0x8D46CBD93FED46F"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
        </Properties>
      </Container>
      <Container>
        <Name>container-5</Name>
        <Properties>
          <Last-Modified>Thu, 16 Mar 2017 22:41:39 GMT</Last-Modified>
          <Etag>"0x8D46CBD9C762815"</Etag>
          <LeaseStatus>unlocked</LeaseStatus>
          <LeaseState>available</LeaseState>
        </Properties>
      </Container>
  </Containers>
  <NextMarker />
</EnumerationResults>
```

Ayrıştırma sonuçlarını isteği oluştur ve hizmeti çağırmak nasıl anladığınıza göre yetkilendirme üst bilgisi oluşturmak nasıl görelim. Bu başlığı oluşturma karmaşık olmakla birlikte güzel bir haberimiz var çalışma kodu aldıktan sonra tüm depolama hizmeti REST API'leri için çalışıyor.

## <a name="creating-the-authorization-header"></a>Yetkilendirme üst bilgisi oluşturuluyor

> [!TIP]
> Azure depolama BLOB'ları ve Kuyruklar için artık Azure Active Directory (Azure AD) tümleştirmeyi destekler. Azure AD, Azure Depolama'ya bir talep yetkilendirmek için bir çok daha basit deneyim sunar. REST işlemlerini yetkilendirmek için Azure AD kullanarak daha fazla bilgi için bkz: [Azure Active Directory ile kimlik doğrulama](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory). Azure depolama ile Azure AD tümleştirme genel bakış için bkz. [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md).

Kavramsal olarak açıklayan bir makale yok (kod) nasıl gerçekleştirileceğini [kimlik doğrulaması için Azure Storage Hizmetleri](/rest/api/storageservices/Authorization-for-the-Azure-Storage-Services).
Şimdi aşağı ilgili makaleye tam olarak biçimlendirebilir gereklidir ve kodu gösterir.

İlk olarak, bir paylaşılan anahtar kimlik doğrulaması kullanın. Yetkilendirme üst bilgisi biçimi şöyle görünür:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

Bir karma tabanlı ileti kimlik doğrulama kodu (HMAC) istekten oluşturulan ve SHA256 algoritmasını kullanarak, daha sonra Base64 kodlama kullanılarak kodlanır hesaplanan imzası bir alandır. Bu var mı? (Biraz bekleyin, word heard olmasanız bile *kurallı* henüz.)

Bu kod parçacığı, paylaşılan anahtar imzası dizesi biçimi göstermektedir:

```csharp  
StringToSign = VERB + "\n" +  
               Content-Encoding + "\n" +  
               Content-Language + "\n" +  
               Content-Length + "\n" +  
               Content-MD5 + "\n" +  
               Content-Type + "\n" +  
               Date + "\n" +  
               If-Modified-Since + "\n" +  
               If-Match + "\n" +  
               If-None-Match + "\n" +  
               If-Unmodified-Since + "\n" +  
               Range + "\n" +  
               CanonicalizedHeaders +  
               CanonicalizedResource;  
```

Bu alanların çoğu nadiren kullanılır. BLOB Depolama için FİİLİ, md5, içerik uzunluğu, kurallı üst bilgileri ve kurallı bir kaynak belirtin. Diğerleri boş bırakabilirsiniz (ancak, put `\n` boş oldukları bilmesi için).

CanonicalizedHeaders ve CanonicalizedResource nedir? İyi soru. Aslında, ne yaptığını ortalama kurallı? Microsoft Word bile, bir sözcük tanımaz. İşte [Wikipedia belirten standart hale getirme hakkında](https://en.wikipedia.org/wiki/Canonicalization): *Bilgisayar biliminde standart hale getirme (bazen Standardizasyon veya normalleştirme), "standart", "normal" veya kurallı bir form birden fazla olası gösterimine sahip verileri dönüştürmek için bir işlemdir.* Buna normal konuşurken, (örneğin, üstbilgiler kurallı üst bilgileri söz konusu olduğunda) öğelerinin listesini alabilir ve bunları gerekli bir biçime standart hale getirmek bu anlamına gelir. Temel olarak, Microsoft karar biçimi ve onunla eşleşecek şekilde gerekir.

Yetkilendirme üst bilgisi oluşturmak için gerekli olduğundan bu iki Kurallaştırılan alanları ile başlayalım.

**Kurallaştırılan üstbilgileri**

Bu değer oluşturmak için "- ms-" başlayıp sıralayabilirsiniz üst bilgileri almak, ardından bunları bir dizeye biçimlendirme `[key:value\n]` örnekleri, bir dizeye birleştirilir. Bu örnekte, Kurallaştırılan üstbilgileri şöyle görünür: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-07-29\n
```

Bu çıktı oluşturmak için kullanılan kod aşağıdaki gibidir:

```csharp 
private static string GetCanonicalizedHeaders(HttpRequestMessage httpRequestMessage)
{
    var headers = from kvp in httpRequestMessage.Headers
        where kvp.Key.StartsWith("x-ms-", StringComparison.OrdinalIgnoreCase)
        orderby kvp.Key
        select new { Key = kvp.Key.ToLowerInvariant(), kvp.Value };

    StringBuilder headersBuilder = new StringBuilder();

    // Create the string in the right format; this is what makes the headers "canonicalized" --
    //   it means put in a standard format. https://en.wikipedia.org/wiki/Canonicalization
    foreach (var kvp in headers)
    {
        headersBuilder.Append(kvp.Key);
        char separator = ':';

        // Get the value for each header, strip out \r\n if found, then append it with the key.
        foreach (string headerValue in kvp.Value)
        {
            string trimmedValue = headerValue.TrimStart().Replace("\r\n", string.Empty);
            headersBuilder.Append(separator).Append(trimmedValue);

            // Set this to a comma; this will only be used
            // if there are multiple values for one of the headers.
            separator = ',';
        }

        headersBuilder.Append("\n");
    }

    return headersBuilder.ToString();
}
```

**Kurallaştırılan kaynak**

Bu imza dizesinin parçası, istek tarafından hedeflenen depolama hesabını temsil eder. İstek URI'si olduğunu unutmayın `<http://contosorest.blob.core.windows.net/?comp=list>`, gerçek hesap adına sahip (`contosorest` bu durumda). Bu örnekte, bu döndürülür:

```
/contosorest/\ncomp:list
```

Bu örnek, sorgu parametreleri varsa, bu parametreleri de içerir. Ek sorgu parametreleri ve sorgu parametreleri ile birden fazla değer aynı zamanda işleyen kod aşağıdaki gibidir. Bu kod tüm REST API'leri için çalışmaya oluşturuyorsunuz unutmayın. ListContainers yöntemi tümünün gerekmiyor olsa bile tüm olasılıkları dahil etmek istediğiniz.

```csharp 
private static string GetCanonicalizedResource(Uri address, string storageAccountName)
{
    // The absolute path will be "/" because for we're getting a list of containers.
    StringBuilder sb = new StringBuilder("/").Append(storageAccountName).Append(address.AbsolutePath);

    // Address.Query is the resource, such as "?comp=list".
    // This ends up with a NameValueCollection with 1 entry having key=comp, value=list.
    // It will have more entries if you have more query parameters.
    NameValueCollection values = HttpUtility.ParseQueryString(address.Query);

    foreach (var item in values.AllKeys.OrderBy(k => k))
    {
        sb.Append('\n').Append(item).Append(':').Append(values[item]);
    }

    return sb.ToString();
}
```

Kurallaştırılan dizeleri ayarlamak, yetkilendirme üst oluşturma adımları gözden geçirelim. Bu makalede daha önce görüntülenen StringToSign biçiminde bir dize iletisi imzasının oluşturarak başlayın. Bu kavramı kolaydır kod açıklamaları kullanan açıklamak için burada olur, yetkilendirme üst bilgisi döndüren son yöntemi:

```csharp
internal static AuthenticationHeaderValue GetAuthorizationHeader(
    string storageAccountName, string storageAccountKey, DateTime now,
    HttpRequestMessage httpRequestMessage, string ifMatch = "", string md5 = "")
{
    // This is the raw representation of the message signature.
    HttpMethod method = httpRequestMessage.Method;
    String MessageSignature = String.Format("{0}\n\n\n{1}\n{5}\n\n\n\n{2}\n\n\n\n{3}{4}",
                method.ToString(),
                (method == HttpMethod.Get || method == HttpMethod.Head) ? String.Empty
                  : httpRequestMessage.Content.Headers.ContentLength.ToString(),
                ifMatch,
                GetCanonicalizedHeaders(httpRequestMessage),
                GetCanonicalizedResource(httpRequestMessage.RequestUri, storageAccountName),
                md5);

    // Now turn it into a byte array.
    byte[] SignatureBytes = Encoding.UTF8.GetBytes(MessageSignature);

    // Create the HMACSHA256 version of the storage key.
    HMACSHA256 SHA256 = new HMACSHA256(Convert.FromBase64String(storageAccountKey));

    // Compute the hash of the SignatureBytes and convert it to a base64 string.
    string signature = Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes));

    // This is the actual header that will be added to the list of request headers.
    AuthenticationHeaderValue authHV = new AuthenticationHeaderValue("SharedKey",
        storageAccountName + ":" + Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes)));
    return authHV;
}
```

Bu kodu çalıştırdığınızda, sonuçta elde edilen MessageSignature şu örnekteki gibi görünür:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-07-29\n/contosorest/\ncomp:list
```

AuthorizationHeader için son değer şu şekildedir:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

AuthorizationHeader yanıt göndermeden önce istek üst bilgilerinde yer son başlığıdır.

Depolama Hizmetleri REST API'leri çağırmak için bir istek oluşturmak bir sınıf bir araya için bilmeniz gereken her şeyi kapsar.

## <a name="how-about-another-example"></a>Başka bir örnek ne dersiniz? 

Kapsayıcı için ListBlobs çağıran kodu değiştirme göz atalım *kapsayıcı 1*. Bu kod, kapsayıcılar, URI ve nasıl, yanıtı ayrıştırmayı tek farklılık listeleme kod neredeyse aynıdır. 

İçin başvuru belgeleri bakarsanız [ListBlobs](/rest/api/storageservices/List-Blobs), yöntem olduğunu bulmak *alma* ve RequestURI:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

ListContainersAsyncREST, API'ye ListBlobs için URI ayarlayan kodu değiştirin. Kapsayıcı adı **kapsayıcı 1**.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Ardından yanıt işlemek burada BLOB kapsayıcılar yerine aramak için kodu değiştirin.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Bu örneği çalıştırdığınızda, sonuçları aşağıdaki gibi alın:

**Kurallaştırılan üst bilgileri:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-07-29\n
```

**Kurallaştırılan kaynak:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-07-29\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

Aşağıdaki değerleri arasındadır [Fiddler](https://www.telerik.com/fiddler):

**İstek:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**İstek Bağlıkları:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-07-29
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Yürütmeden sonra döndürülen durum kodu ve yanıt üst bilgileri:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Yanıt gövdesi (XML):** Bu XML yanıtı, blobları ve özelliklerinin listesini gösterir. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
    ServiceEndpoint="http://contosorest.blob.core.windows.net/" ContainerName="container-1">
    <Blobs>
        <Blob>
            <Name>DogInCatTree.png</Name>
            <Properties><Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
            <Etag>0x8D52D5C4A4C96B0</Etag>
            <Content-Length>419416</Content-Length>
            <Content-Type>image/png</Content-Type>
            <Content-Encoding />
            <Content-Language />
            <Content-MD5 />
            <Cache-Control />
            <Content-Disposition />
            <BlobType>BlockBlob</BlobType>
            <LeaseStatus>unlocked</LeaseStatus>
            <LeaseState>available</LeaseState>
            <ServerEncrypted>true</ServerEncrypted>
            </Properties>
        </Blob>
        <Blob>
            <Name>GuyEyeingOreos.png</Name>
            <Properties>
                <Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
                <Etag>0x8D52D5C4A25A6F6</Etag>
                <Content-Length>167464</Content-Length>
                <Content-Type>image/png</Content-Type>
                <Content-Encoding />
                <Content-Language />
                <Content-MD5 />
                <Cache-Control />
                <Content-Disposition />
                <BlobType>BlockBlob</BlobType>
                <LeaseStatus>unlocked</LeaseStatus>
                <LeaseState>available</LeaseState>
                <ServerEncrypted>true</ServerEncrypted>
            </Properties>
            </Blob>
        </Blobs>
    <NextMarker />
</EnumerationResults>
```

## <a name="summary"></a>Özet

Bu makalede, blob depolama REST API için bir istekte öğrendiniz. İstekle birlikte, kapsayıcıların bir listesi veya bir kapsayıcıdaki blobları listesini alabilirsiniz. REST API çağrısı için yetkilendirme imza oluşturma ve REST istekte kullanma öğrendiniz. Son olarak, yanıtı inceleyin hakkında bilgi edindiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [BLOB hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
* [Dosya hizmeti REST API'si](/rest/api/storageservices/file-service-rest-api)
* [Kuyruk hizmeti REST API'si](/rest/api/storageservices/queue-service-rest-api)
