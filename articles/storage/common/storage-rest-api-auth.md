---
title: "Kimlik doğrulama dahil olmak üzere Azure Storage Services REST API işlemleri çağırma | Microsoft Docs"
description: "Kimlik doğrulama dahil olmak üzere Azure Storage Services REST API işlemleri çağırma"
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/27/2017
ms.author: robinsh
ms.openlocfilehash: 73921f7fd4de65513f647db92b737a79f1043182
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="using-the-azure-storage-rest-api"></a>Azure Storage REST API kullanma

Bu makalede, Blob Depolama hizmeti REST API'lerini kullanma ve arama hizmeti için kimlik doğrulaması yapmayı gösterir. Açısından bakıldığında REST çağrısı yapma REST ve hiçbir fikir hakkında hiçbir şey bilmez birinin yazılır, ancak bir uygulama geliştiricisi değildir. REST çağrısı başvuru belgelerine bakın ve gerçek bir REST çağrısı – Çevir bkz hangi alanların Git nerede? REST görüşmesi ayarlayabilmeniz öğrenme sonra diğer depolama hizmeti REST API'leri birini kullanmak için bu bilgiyi kullanabilir.

## <a name="prerequisites"></a>Ön koşullar 

Uygulama kapsayıcıları bir depolama hesabı için blob depolama birimindeki listeler. Bu makaledeki kod denemek için aşağıdaki öğeleri gerekir: 

* Yükleme [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) aşağıdaki iş yükü ile:
    - Azure geliştirme

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Genel amaçlı depolama hesabı. Herhangi bir depolama hesabı yoksa, kullanarak bir tane oluşturabilirsiniz [Azure portal](https://portal.azure.com), [PowerShell](storage-quickstart-create-storage-account-powershell.md), veya [Azure CLI](storage-quickstart-create-storage-account-cli.md).

* Bu makaledeki örnek, bir depolama hesabında kapsayıcıları listesinde gösterilmiştir. Çıkışı görmek için bazı kapsayıcıları başlamadan önce depolama depolama hesabındaki blob ekleyin.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Örnek uygulama, C# ile yazılmış bir konsol uygulamasıdır.

Kullanım [git](https://git-scm.com/) geliştirme ortamınızı uygulamaya bir kopyasını indirmek için. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Bu komut, yerel git klasörünüze depoya klonlar. Visual Studio çözümü açmak için storage-dotnet-rest-api-with-auth klasörü arayın, açın ve üzerinde StorageRestApiAuth.sln çift tıklayın. 

## <a name="why-do-i-need-to-know-rest"></a>Neden REST bilmeniz gerekiyor mu?

REST kullanma bilerek yararlı bir yetenektir. Azure ürün ekibi, yeni özellikler sık serbest bırakır. Çoğu zaman, yeni özellikler REST arabirimi aracılığıyla erişilebilir, ancak henüz aracılığıyla ortaya olmayan **tüm** için depolama istemcisi kitaplıklarını veya kullanıcı arabirimini (örneğin, Azure portalı). Her zaman en son ve en kullanmak istiyorsanız, REST öğrenme gerekli değildir. Ayrıca, Azure Storage ile etkileşim kurmak için kendi kitaplığı yazmak istediğiniz ya da bir SDK veya depolama istemci kitaplığı sahip olmayan bir programlama dili ile Azure depolama erişmek istediğiniz REST API'sini kullanabilirsiniz.

## <a name="what-is-rest"></a>REST nedir?

REST anlamına gelir *temsili durum aktarımı*. Belirli bir tanımı için kullanıma [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer).

Temel olarak, REST kullanabileceğiniz ne zaman bir mimaridir API'larını çağırma veya API'ler çağrılacak kullanılabilir hale getirme. Her iki tarafında olanlar bağımsızdır ve hangi diğer yazılımların REST alırken veya gönderirken kullanılan çağırır. Mac, Windows, Linux, bir Android telefon veya tablet, iPhone, iPod veya web sitesi üzerinde çalışan bir uygulama yazmak ve tüm bu platformlar için aynı REST API'yi kullanın. REST API çağrıldığında veri ve/veya çıkışı geçirilebilir. REST API çağrılır – hangi önemli istekte geçirilen bilgileri ve yanıtta sağlanan verileri platformudur önemli değildir.

## <a name="heres-the-plan"></a>Plan İşte

Örnek Proje bir depolama hesabında kapsayıcıları listeler. REST API belgeleri bilgileri gerçek kodunuzu nasıl karşılık gelen anladığınızda, diğer REST çağrılarını anlayıp kolaydır. 

Bakarsanız [Blob hizmeti REST API'si](/rest/api/storageservices/fileservices/Blob-Service-REST-API), blob depolama alanında gerçekleştirebileceğiniz işlemlerin bakın. Sarmalayıcılar REST API'leri geçici depolama istemcisi kitaplıklarını değildir; bunlar sizin için erişim depolama birimine doğrudan REST API'ları kullanmadan kolaylaştırır. Ancak, yukarıda belirtildiği gibi bazen REST API depolama istemci kitaplığı yerine kullanmak istediğiniz.

## <a name="rest-api-reference-list-containers-api"></a>REST API Başvurusu: Liste kapsayıcıları API

Sayfa için REST API Başvurusu bakalım [ListContainers](/rest/api/storageservices/fileservices/List-Containers2) bazı alanlar alınacağı istek ve yanıt kodu ile sonraki bölümde yeri anlamaları için işlemi.

**İstek yöntemi**: alın. Bu fiil request nesnesi bir özellik olarak belirttiğiniz HTTP yöntemidir. Bu eylem için diğer değerler, HEAD, PUT ve silme, aradığınız API bağlı olarak içerir.

**İstek URI'si**: Bu blob depolama hesabı uç noktasından oluşturulur https://myaccount.blob.core.windows.net/?comp=list `http://myaccount.blob.core.windows.net` ve kaynak dizesi `/?comp=list`.

[URI parametreleri](/rest/api/storageservices/fileservices/List-Containers2#uri-parameters): ListContainers çağrılırken kullanabileceğiniz ek sorgu parametrelerini vardır. Bu parametreler birkaç olan *zaman aşımı* (saniye cinsinden) çağrısı ve *önek*, filtreleme için kullanılan.

Başka bir yardımcı parametresi *maxresults:* bu değerden daha fazla kapsayıcıları kullanılabilir durumdaysa yanıt gövdesi içerecek bir *NextMarker* sonraki dönmek için İleri kapsayıcı gösterir öğesi İstek. Bu özelliği kullanmak için sağladığınız *NextMarker* olarak değer *işaret* parametresi bir sonraki istekte yaptığınızda URI. Bu özelliği kullanıldığında, disk belleği sonuçları aracılığıyla için benzerdir. 

Ek parametreler kullanmak için bu örnek gibi değerine sahip kaynak dizeye ekleyin:

```
/?comp=list&timeout=60&maxresults=100
```

[İstek üst](/rest/api/storageservices/fileservices/List-Containers2#request-headers)**:** gerekli ve isteğe bağlı istek üstbilgileri Bu bölümde listelenmektedir. Üç üstbilgilerinin gereklidir: bir *yetkilendirme* , üst *x-ms-date* (içerir. istek için UTC saati), ve *x-ms-version* (REST sürümünü belirtir. API) kullanın. Dahil olmak üzere *x-ms-istemci-request-id* için herhangi bir şey bu alan için değer ayarlayabilirsiniz; günlük kaydı etkinleştirildiğinde depolama analytics günlüklere yazılır isteğe bağlı – üst bilgilerindedir.

[İstek gövdesinde](/rest/api/storageservices/fileservices/List-Containers2#request-body)**:** ListContainers için hiçbir istek gövdesinde yok. İstek gövdesi tüm PUT işlemleri uygulamak için bir saklı erişim ilkeleri XML listesinde göndermenize olanak sağlayan SetContainerAccessPolicy yanı sıra BLOB karşıya yüklenirken kullanılır. Saklı erişim ilkeleri makalesinde açıklanan [kullanarak paylaşılan erişim imzaları (SAS)](storage-dotnet-shared-access-signature-part-1.md).

[Yanıt durum kodu](/rest/api/storageservices/fileservices/List-Containers2#status-code)**:** Tells bilmeniz gereken herhangi bir durum kodu. Bu örnekte, bir HTTP durum kodu 200 Tamam. HTTP durum kodları tam bir listesi için kullanıma [durum kodu tanımları](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). Storage REST API'leri için belirli hata kodları görmek için bkz: [ortak REST API hata kodları](/rest/api/storageservices/common-rest-api-error-codes)

[Yanıt üstbilgileri](/rest/api/storageservices/fileservices/List-Containers2#response-headers)**:** bunlar *içerik türü*; *x-ms-request-id* (istek kimliği, varsa geçirilen); *x-ms-version* (kullanılan Blob hizmetine sürümünü gösterir) ve *tarih* (UTC, söyler ne zaman isteği yapıldı).

[Yanıt gövdesi](/rest/api/storageservices/fileservices/List-Containers2#response-body): Bu alan istenen verileri sağlayan bir XML yapısıdır. Bu örnekte, yanıt kapsayıcıları ve bunların özelliklerini listesidir.

## <a name="creating-the-rest-request"></a>REST isteği oluşturma

Birkaç başlatmadan – notları üretimde çalıştırırken güvenlik için her zaman kullanın HTTP yerine HTTPS. Bu alıştırmada amaçları doğrultusunda, istek ve yanıt verilerini görüntüleyebilmeniz HTTP kullanmanız gerekir. Gerçek REST çağrılarını İstek ve yanıt bilgileri görüntülemek için indirebilirsiniz [Fiddler](http://www.telerik.com/fiddler) ya da benzer bir uygulama. Visual Studio çözümünde, depolama hesabı adı ve anahtar sınıfında sabit kodlanmış olan ve ListContainersAsyncREST yöntemi depolama hesabı adı ve depolama hesabı anahtarı REST isteği çeşitli bileşenleri oluşturmak için kullanılan yöntemleri geçirir . Gerçek dünya uygulama depolama hesabı adı ve anahtar ortam değişkenleri, bir yapılandırma dosyasında bulunması veya bir Azure anahtar Kasası'alınmalıdır.

Bizim örnek projesinde, tüm sınıf alın ve kendi çözüme ekleyin ve bunu "olduğu gibi." kullanmanızı fikir ile ayrı bir sınıf yetkilendirme üst bilgisi oluşturmak için kodu. Azure Storage çoğu REST API çağrıları için yetkilendirme üst bilgisi kodu çalışır.

Bir HttpRequestMessage nesnesi, isteği oluşturmak için Program.cs ListContainersAsyncREST gidin. İstek oluşturmaya yönelik adımlar şunlardır: 

* Hizmet çağırmak için kullanılacak URI oluşturun. 
* HttpRequestMessage nesnesi oluşturun ve yükü ayarlayın. Biz her şeyi geçirme değil çünkü yükü ListContainersAsyncREST için null şeklindedir.
* X-ms-date ve x-ms-version için istek üstbilgileri ekleyin.
* Yetkilendirme üst bilgisi almak ve bunu ekleyin.

Gereksinim duyduğunuz bazı temel bilgileri: 

*  ListContainers için **yöntemi** olan `GET`. Bu değer, istek başlatılırken ayarlanır. 
*  **Kaynak** değer olacak şekilde API çağrılan, belirten URI sorgu kısmı `/?comp=list`. Daha önce belirtildiği gibi kaynak hakkında bilgi gösterir başvuru belgeleri sayfasında olan [ListContainers API](/rest/api/storageservices/fileservices/List-Containers2).
*  URI, bu depolama hesabı için Blob Hizmeti uç noktası oluşturma ve kaynak birleştirme oluşturulur. Değeri **istek URI'si** olan yukarı sona `http://contosorest.blob.core.windows.net/?comp=list`.
*  ListContainers için **requestBody** null ve hiçbir ek **üstbilgileri**.

Farklı API'leri, içinde gibi diğer parametreleri olabilir *Ifmatch*. Ifmatch burada kullanabileceğinize bir örnek PutBlob çağrılırken ' dir. Bu durumda, bir eTag öğesine Ifmatch ayarlamak ve sağladığınız eTag geçerli eTag blob üzerinde eşleşirse, yalnızca blob güncelleştirir. Başka birinin eTag alma itibaren blob güncelleştirdi, kullanıcıların değişiklik geçersiz kılınmış olmaz. 

Öncelikle, ayarlamış `uri` ve `payload`. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Ardından, yöntem ayarını istek örneği `GET` ve URI sağlama.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

X-ms-date ve x-ms-version için istek üstbilgileri ekleyin. Bu kod ayrıca çağrısı için gerekli tüm ek istek üstbilgileri eklediğiniz yerdir. Bu örnekte, hiçbir ek üstbilgi vardır. Ek üstbilgilerinde başarılı bir API SetContainerACL örnektir. BLOB Depolama için "x-ms-blob-genel-erişim" ve değeri için erişim düzeyi adlı bir başlık ekler.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-04-17");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Authorization üstbilgisi oluşturur yöntemini çağırın ve istek üstbilgileri ekleyin. Makalenin sonraki bölümlerinde authorization üstbilgisi oluşturma görürsünüz. Bu kod parçacığında görebilirsiniz GetAuthorizationHeader yöntemi adıdır:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Bu noktada, `httpRequestMessage` ile yetkilendirme üstbilgileri tam REST isteği içeriyor. 

## <a name="call-the-rest-api-with-the-request"></a>İstek ile REST API çağrısı

İstek sahip olduğunuza göre REST isteği göndermek için SendAsync çağırabilirsiniz. SendAsync API'sini çağırır ve yanıt geri alır. StatusCode yanıt inceleyin (200 Tamam değil), yanıtı ayrıştırılamadı. Bu durumda, kapsayıcıların bir XML listesini alın. İstek oluşturmak, istek yürütülemiyor ve kapsayıcılar listesi için yanıt incelemek için GetRESTRequest yöntemi çağırmak için kodu bakalım.

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

Bir ağ algılayıcısı gibi çalıştırırsanız [Fiddler](https://www.telerik.com/fiddler) SendAsync çağrısı yaparken, istek ve yanıt bilgileri görebilirsiniz. Bir göz atalım. Depolama hesabı adı *contosorest*.

**İsteği:**

```
GET /?comp=list HTTP/1.1
```

**İstek üstbilgilerini:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Durum kodu ve yanıt üstbilgileri yürütme sonrasında döndürdü:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 04-17
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Yanıt gövdesi (XML):** için ListContainers, bu gösterir kapsayıcıları ve özelliklerinin listesi.

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

İsteği oluşturmak için hizmeti çağırıp sonuçlarını ayrıştırmak nasıl anladığınıza göre authorization üstbilgisi oluşturma görelim. Bu başlığı oluşturma karmaşık, ancak iyi haber çalışma kodu sahip olduğunda, tüm depolama hizmeti REST API'leri için çalışır durumda olduğunu.

## <a name="creating-the-authorization-header"></a>Authorization üstbilgisi oluşturma

Kavramsal olarak açıklayan bir makale yoktur (kod) nasıl gerçekleştirileceği [Azure Storage Hizmetleri için kimlik doğrulaması](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services).
Şimdi bu makale aşağı tam olarak biçimlendirebilir gereklidir ve kodu gösterir.

İlk olarak, bir paylaşılan anahtar kimlik doğrulaması kullanın. Yetkilendirme üstbilgi biçimi şöyle görünür:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

Bir karma tabanlı ileti kimlik doğrulama kodu (HMAC) istekten oluşturulur ve SHA256 algoritmasını kullanarak, daha sonra Base64 kodlaması kullanarak kodlanmış hesaplanan imza bir alandır. Bu var mı? (Var askıda, word bile heard henüz *standart hale getirilirse* henüz.)

Bu kod parçacığını paylaşılan anahtar imza dizesinin biçimi gösterir:

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

Bu alanların çoğu nadiren kullanılır. BLOB Depolama için fiil, md5, içerik uzunluğu, standart hale getirilirse üstbilgileri ve standart hale getirilirse kaynak belirtin. Diğer boş bırakabilirsiniz (ancak, put `\n` boş oldukları bilmesi için).

CanonicalizedHeaders ve CanonicalizedResource nelerdir? İyi sorusu. Aslında, ortalama ne yaptığını standart hale getirilirse? Microsoft Word bile, word olarak tanımıyor. İşte [Wikipedia diyor Standartlaştırma hakkında](http://en.wikipedia.org/wiki/Canonicalization): *bilgisayar bilimi içinde Standartlaştırma (bazen Standartlaştırma veya normalleştirme) birden fazla olası sahip veri dönüştürme için bir işlemdir "standard", "normal" veya kurallı forma gösterimi.* Buna normal seslendir, bu öğeleri (örneğin, standart hale getirilirse üstbilgileri durumunda üst bilgiler) listesini almak ve gerekli biçime standartlaştırmak anlamına gelir. Temel olarak, Microsoft biçimi karar ve eşleşen gerekir.

Yetkilendirme üst bilgisi oluşturmak için gerekli olduğundan bu iki Kurallaştırılan alanlarla başlayalım.

**Kurallaştırılan üstbilgileri**

Bu değer oluşturmak için "- ms-" ile başlamalıdır ve bunları sıralama üstbilgileri almak, ardından bir dizeye biçimlendirme `[key:value\n]` bir dizeye birleştirilmiş örnekleri. Bu örnekte, Kurallaştırılan üstbilgileri şöyle görünür: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-04-17\n
```

Bu çıktı oluşturmak için kullanılan kod aşağıdaki gibidir:

```csharp 
private static string GetCanonicalizedHeaders(HttpRequestMessage httpRequestMessage)
{
    var headers = from kvp in httpRequestMessage.Headers
                    where kvp.Key.StartsWith("x-ms-", StringComparison.OrdinalIgnoreCase)
                    orderby kvp.Key
                    select new { Key = kvp.Key.ToLowerInvariant(), kvp.Value };

    StringBuilder sb = new StringBuilder();

    // Create the string in the right format; this is what makes the headers "canonicalized" --
    //   it means put in a standard format. http://en.wikipedia.org/wiki/Canonicalization
    foreach (var kvp in headers)
    {
        StringBuilder headerBuilder = new StringBuilder(kvp.Key);
        char separator = ':';

        // Get the value for each header, strip out \r\n if found, then append it with the key.
        foreach (string headerValues in kvp.Value)
        {
            string trimmedValue = headerValues.TrimStart().Replace("\r\n", String.Empty);
            headerBuilder.Append(separator).Append(trimmedValue);

            // Set this to a comma; this will only be used 
            //   if there are multiple values for one of the headers.
            separator = ',';
        }
        sb.Append(headerBuilder.ToString()).Append("\n");
    }
    return sb.ToString();
}      
```

**Kurallaştırılan kaynak**

Bu imza dizesinin parçası istek tarafından hedeflenen depolama hesabını temsil eder. İstek URI'si olduğunu unutmayın `<http://contosorest.blob.core.windows.net/?comp=list>`, gerçek hesap adıyla (`contosorest` bu durumda). Bu örnekte, bu döndürülür:

```
/contosorest/\ncomp:list
```

Sorgu parametreleri varsa, bu olanlar da içerir. Burada, aynı zamanda ek sorgu parametrelerini ve birden çok değer içeren sorgu parametrelerini işleyen kodu verilmiştir. ListContainers yöntemi bunların tümü gerekmez olsa bile tüm olasılıkları, dahil etmek istediğiniz şekilde tüm REST API için çalışması için bu kodu oluşturmakta olduğunuz unutmayın.

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

Kurallaştırılan dizeleri ayarlamak, authorization üstbilgisi oluşturmak ne bakalım. Bu makalede daha önce görüntülenen StringToSign biçimlerinin ileti imzası dizesi oluşturarak başlayın. Bu kavram kolaydır açıklamaları kod içinde kullanma açıklamak için bu nedenle İşte, Authorization Üstbilgisi döndürür son yöntemi:

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

Bu kodu çalıştırdığınızda, sonuçta elde edilen MessageSignature şöyle görünür:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-04-17\n/contosorest/\ncomp:list
```

AuthorizationHeader son değeri şöyledir:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

AuthorizationHeader yanıt göndermeden önce istek üst bilgilerinde yerleştirilen son başlığıdır.

Bu, kod ile birlikte bir araya Storage Hizmetleri REST API'leri çağırmak için kullanılacak bir isteği oluşturmak için kullanabileceğiniz bir sınıf için bilmeniz gereken her şeyi içerir.

## <a name="how-about-another-example"></a>Başka bir örnek ne dersiniz? 

ListBlobs için kapsayıcı çağırmak için kodu değiştirmek nasıl bakalım *kapsayıcı-1*. Bu kapsayıcı, URI ve yanıtı ayrıştırılamadı nasıl yalnızca farklar listeleme kodunu için neredeyse aynıdır. 

Başvuru belgelerini bakarsanız [ListBlobs](/rest/api/storageservices/fileservices/List-Blobs), yöntem olduğunu fark *almak* ve RequestURI:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

ListContainersAsyncREST içinde ListBlobs için API için URI ayarlar kodu değiştirin. Kapsayıcı adı **kapsayıcı-1**.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Ardından yanıt işlemek burada BLOB kapsayıcıları yerine aramak için kodu değiştirin.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Bu örneği çalıştırdığınızda, sonuçları aşağıdaki gibi alın:

**Kurallaştırılan üstbilgileri:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-04-17\n
```

**Kurallaştırılan kaynak:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-04-17\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

Aşağıdaki değerleri arasındadır [Fiddler](http://www.telerik.com/fiddler):

**İsteği:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**İstek üstbilgilerini:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-04-17
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Durum kodu ve yanıt üstbilgileri yürütme sonrasında döndürdü:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-04-17
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Yanıt gövdesi (XML):** bu XML yanıtı BLOB'ları ve özelliklerinin listesini gösterir. 

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

Bu makalede, blob depolama kapsayıcıları veya da BLOB'ları bir kapsayıcıda listesini almak için REST API için istekte öğrendiniz. Ayrıca REST API çağrısı için yetkilendirme imza oluşturma, REST istekte kullanmak ve yanıt incelemek öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [BLOB hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
* [Dosya hizmeti REST API'si](/rest/api/storageservices/file-service-rest-api)
* [Kuyruk hizmeti REST API'si](/rest/api/storageservices/queue-service-rest-api)