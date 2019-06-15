---
title: Azure işlevleri için Microsoft Graph bağlamaları
description: Azure işlevleri'nde Microsoft Graph Tetikleyicileri ve bağlamaları kullanma hakkında bilgi edinin.
services: functions
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: cshoe
ms.openlocfilehash: f112bdf9eacf51852659ab49a5673b0c8bfb0e46
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64511810"
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Azure işlevleri için Microsoft Graph bağlamaları

Bu makalede, yapılandırmak ve Microsoft Graph Tetikleyicileri ve bağlamaları Azure işlevleri'nde çalışmak açıklanmaktadır. Bu, Azure işlevleri veri öngörüleri ve olayları çalışmak için kullanabileceğiniz [Microsoft Graph](https://developer.microsoft.com/graph).

Microsoft Graph uzantı aşağıdaki bağlamaları sağlar:
- Bir [kimlik doğrulama belirteci giriş bağlama](#token-input) , tüm Microsoft Graph API ile etkileşim kurmanızı sağlar.
- Bir [Excel tablosu giriş bağlama](#excel-input) Excel'den veri okumanıza izin verir.
- Bir [Excel tablo çıktı bağlaması](#excel-output) Excel verileri değiştirmenize olanak sağlar.
- A [OneDrive dosya giriş bağlama](#onedrive-input) Onedrive'dan dosyaları okumasına izin verir.
- A [OneDrive dosya çıktı bağlamasını](#onedrive-output) , onedrive'daki dosyalara yazmanızı sağlar.
- Bir [Outlook ileti çıktı bağlamasını](#outlook-output) Outlook e-posta göndermenize olanak sağlar.
- Bir koleksiyonu [Microsoft Graph Web kancası Tetikleyicileri ve bağlamaları](#webhooks) Microsoft Graph'teki olaylara yanıt verir.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note]
> Microsoft Graph bağlamaları şu anda Önizleme için Azure işlevleri sürüm 2.x. İşlevleri sürümde desteklenmez 1.x.

## <a name="packages"></a>Paketler

Kimlik doğrulama belirteci giriş bağlamasına sağlanan [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/) NuGet paketi. Microsoft Graph bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/) paket. Paketler için kaynak kodu konusu [azure işlevleri microsoftgraph uzantı](https://github.com/Azure/azure-functions-microsoftgraph-extension/) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="setting-up-the-extensions"></a>Uzantıları'nı ayarlama

Microsoft Graph bağlamaları aracılığıyla kullanılabilir _uzantılarını bağlama_. Bağlama, Azure işlevleri çalışma zamanı için isteğe bağlı bileşenler uzantılarıdır. Bu bölümde, Microsoft Graph ve kimlik doğrulama belirteci uzantıları ayarlama işlemi gösterilmektedir.

### <a name="enabling-functions-20-preview"></a>İşlevler 2.0 önizlemesini etkinleştirme

Bağlama uzantıları yalnızca Azure işlevleri 2.0 Önizleme sürümünde kullanılabilir. 

İşlevler çalışma zamanı 2.0 önizleme sürümünü kullanmak için bir işlev uygulaması ayarlama hakkında daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

### <a name="installing-the-extension"></a>Uzantıyı yükleme

Azure portalından bir uzantı yüklemek için bir şablon veya başvurduğu bağlama gidin. Yeni bir işlev oluşturun ve Şablon Seçimi ekranındayken, "Microsoft Graph" senaryo seçin. Bu senaryodan olarak şablonlardan birini seçin. Alternatif olarak, var olan bir işlevin "Tümleştirme" sekmesine gidin ve bu makalede ele alınan bağlamaları birini seçin.

Her iki durumda da, uzantının yüklenmesini belirten bir uyarı görüntülenir. Tıklayın **yükleme** uzantı elde edilir. Her bir uzantı yalnızca işlev uygulaması bir kez yüklenmesi gerekir. 

> [!Note] 
> Portal yükleme işlemi, bir tüketim planında 10 dakikaya kadar sürebilir.

Visual Studio kullanıyorsanız, Uzantıları'nı yükleyerek alabilirsiniz [bu makalenin önceki bölümlerinde listelenen NuGet paketlerini](#packages).

### <a name="configuring-authentication--authorization"></a>Yapılandırma kimlik doğrulama / yetkilendirme

Bu makalede açıklanan bağlamaları kullanılacak bir kimlik gerektirir. Bu, Microsoft Graph izinleri uygulamak ve etkileşimleri denetim sağlar. Uygulamanızı veya uygulama erişimi bir kullanıcı kimliği olabilir. Bu kimliğini yapılandırmak için ayarlama [App Service kimlik doğrulaması / yetkilendirme](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) Azure Active Directory ile. İşlevlerinizi gerektiren herhangi bir kaynak izni istemesine izin gerekir.

> [!Note] 
> Microsoft Graph uzantısı yalnızca Azure AD kimlik doğrulamasını destekler. Kullanıcıların bir iş veya Okul hesabınızla oturum açmanız gerekir.

Azure portalını kullanıyorsanız, uzantıyı yüklemek için bir uyarı istemi altında görürsünüz. App Service kimlik doğrulaması yapılandırmak için uyarı ister / yetkilendirme ve istek şablonu veya bağlama herhangi bir izni gerektirir. Tıklayın **yapılandırma, Azure AD artık** veya **izinleri şimdi ekleyin** uygun şekilde.



<a name="token-input"></a>
## <a name="auth-token"></a>Kimlik doğrulama belirteci

Kimlik doğrulama belirteci giriş bağlamasına belirli bir kaynak için bir Azure AD belirtecini alır ve kodunuzda bir dize olarak sağlar. Kendisi için bir kaynak olabilir uygulama izinleri vardır. 

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#auth-token---example)
* [Öznitelikler](#auth-token---attributes)
* [Yapılandırma](#auth-token---configuration)
* [Kullanım](#auth-token---usage)

### <a name="auth-token---example"></a>Kimlik doğrulama belirteci - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#auth-token---c-script-example)
* [JavaScript](#auth-token---javascript-example)

#### <a name="auth-token---c-script-example"></a>Kimlik doğrulama belirteci - C# betiği örneği

Aşağıdaki örnek, kullanıcı profili bilgilerini alır.

*Function.json* dosyası, bir belirteç giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodu, Microsoft Graph için HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers;
using Microsoft.Extensions.Logging; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, ILogger log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

#### <a name="auth-token---javascript-example"></a>Kimlik doğrulama belirteci - JavaScript örneği

Aşağıdaki örnek, kullanıcı profili bilgilerini alır.

*Function.json* dosyası, bir belirteç giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu, Microsoft Graph için HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```

### <a name="auth-token---attributes"></a>Kimlik doğrulama belirteci - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [belirteci](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/TokenBinding/TokenAttribute.cs) özniteliği.

### <a name="auth-token---configuration"></a>Kimlik doğrulama belirteci - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Token` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu için kimlik doğrulama belirteci kullanılır. Bkz: [giriş bağlama koddan bir kimlik doğrulama belirtecini kullanarak](#token-input-code).|
|**type**||Gerekli - kümesine olmalıdır `token`.|
|**direction**||Gerekli - kümesine olmalıdır `in`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**userId**|**Kullanıcı Kimliği**  |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Kaynak**|**Kaynak**|Gereklidir - bir Azure AD kaynak URL'sini belirteç istenmektedir.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Kimlik doğrulama belirteci - kullanım

Bağlama herhangi bir Azure AD izinleri gerektirmez, ancak nasıl kullanıldığına bağlı olarak belirteç ilave izin istemek gerekebilir. Belirteç ile erişmek için istediğinize kaynak gereksinimlerini denetleyin.

Belirteç, kodu her zaman bir dize olarak sunulur.

> [!Note]
> Yerel olarak biriyle geliştirirken `userFromId`, `userFromToken` veya `userFromRequest` seçenekleri, gerekli belirteci olabilir [el ile elde edilen](https://github.com/Azure/azure-functions-microsoftgraph-extension/issues/54#issuecomment-392865857) ve belirtilen `X-MS-TOKEN-AAD-ID-TOKEN` çağıran bir istemci uygulamasından istek üstbilgisi.


<a name="excel-input"></a>
## <a name="excel-input"></a>Excel giriş

Excel tablo giriş bağlaması, Onedrive'da depolanan bir Excel tablosunda içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-input---example)
* [Öznitelikler](#excel-input---attributes)
* [Yapılandırma](#excel-input---configuration)
* [Kullanım](#excel-input---usage)

### <a name="excel-input---example"></a>Excel girişi - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#excel-input---c-script-example)
* [JavaScript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel giriş - C# betiği örneği

Aşağıdaki *function.json* dosyası, bir Excel giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki C# betik kodunu belirtilen tablonun içeriğini okur ve kullanıcıya döndürür:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Microsoft.Extensions.Logging;

public static IActionResult Run(HttpRequest req, string[][] excelTableData, ILogger log)
{
    return new OkObjectResult(excelTableData);
}
```

#### <a name="excel-input---javascript-example"></a>Giriş - Excel JavaScript örneği

Aşağıdaki *function.json* dosyası, bir Excel giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodunu belirtilen tablonun içeriğini okur ve kullanıcıya döndürür.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

### <a name="excel-input---attributes"></a>Giriş - Excel öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliği.

### <a name="excel-input---configuration"></a>Excel girişi - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Excel` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu Excel tablosu için kullanılır. Bkz: [giriş bağlama kod kullanarak bir Excel tablosuna](#excel-input-code).|
|**type**||Gerekli - kümesine olmalıdır `excel`.|
|**direction**||Gerekli - kümesine olmalıdır `in`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**userId**|**Kullanıcı Kimliği**  |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yolu**|Gereklidir - onedrive'daki Excel çalışma kitabı yolu.|
|**worksheetName**|**worksheetName**|Tablonun bulunduğu çalışma sayfası.|
|**TableName**|**TableName**|Tablonun adı. Belirtilmezse çalışma kitabının içeriği kullanılır.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Giriş - Excel kullanımı

Bu bağlama, aşağıdaki Azure AD izinleri gerektirir:

|Resource|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuyun|

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- string[][]
- Microsoft.Graph.WorkbookTable
- Özel nesne türlerinin (yapısal model bağlama kullanarak)










<a name="excel-output"></a>
## <a name="excel-output"></a>Excel çıkış

Excel çıkış bağlaması, Onedrive'da depolanan bir Excel tablosunda içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-output---example)
* [Öznitelikler](#excel-output---attributes)
* [Yapılandırma](#excel-output---configuration)
* [Kullanım](#excel-output---usage)

### <a name="excel-output---example"></a>Çıkış - Excel örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#excel-output---c-script-example)
* [JavaScript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Excel çıkışı - C# betiği örneği

Aşağıdaki örnek, bir Excel tablosuna satır ekler.

*Function.json* HTTP tetikleyicisi olan bir Excel dosyası tanımlar çıktı bağlaması:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu Sorgu dizesinden giriş temel (tek sütunlu varsayılır) tablosuna yeni bir satır ekler:

```csharp
using System.Net;
using System.Text;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, ILogger log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

#### <a name="excel-output---javascript-example"></a>Excel çıkışı - JavaScript örneği

Aşağıdaki örnek, bir Excel tablosuna satır ekler.

*Function.json* HTTP tetikleyicisi olan bir Excel dosyası tanımlar çıktı bağlaması:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodunu Sorgu dizesinden giriş temel (tek sütunlu varsayılır) tablosuna yeni bir satır ekler.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Çıkış - Excel öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliği.

### <a name="excel-output---configuration"></a>Excel çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Excel` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu için kimlik doğrulama belirteci kullanılır. Bkz: [çıktı bağlamasını kod kullanarak bir Excel tablosuna](#excel-output-code).|
|**type**||Gerekli - kümesine olmalıdır `excel`.|
|**direction**||Gerekli - kümesine olmalıdır `out`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |**userId** |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yolu**|Gereklidir - onedrive'daki Excel çalışma kitabı yolu.|
|**worksheetName**|**worksheetName**|Tablonun bulunduğu çalışma sayfası.|
|**TableName**|**TableName**|Tablonun adı. Belirtilmezse çalışma kitabının içeriği kullanılır.|
|**güncelleştirme türü**|**güncelleştirme türü**|Gereklidir - tabloya yapılacak değişikliğin türü. Aşağıdaki değerlerden biri olabilir:<ul><li><code>update</code> -Onedrive tablonun içeriğini değiştirir.</li><li><code>append</code> -Yük sonuna kadar OneDrive tablosunda yeni satırlar oluşturarak ekler.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Excel çıkışı - kullanım

Bu bağlama, aşağıdaki Azure AD izinleri gerektirir:

|Resource|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim sağlama|

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- string[][]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Özel nesne türlerinin (yapısal model bağlama kullanarak)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Dosya girişi

OneDrive dosya giriş bağlaması, Onedrive'da depolanan bir dosyanın içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-input---example)
* [Öznitelikler](#file-input---attributes)
* [Yapılandırma](#file-input---configuration)
* [Kullanım](#file-input---usage)

### <a name="file-input---example"></a>Dosya girişi - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#file-input---c-script-example)
* [JavaScript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Giriş - dosya C# betiği örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyayı okur.

*Function.json* dosya bir OneDrive dosyasını giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodu, sorgu dizesinde belirtilen dosyasını okur ve uzunluğunu kaydeder:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, ILogger log)
{
    log.LogInformation(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Giriş - dosya JavaScript örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyayı okur.

*Function.json* dosya bir OneDrive dosyasını giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodunu, sorgu dizesinde belirtilen dosyasını okur ve uzunluğunu döndürür.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```

### <a name="file-input---attributes"></a>Giriş - dosya öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) özniteliği.

### <a name="file-input---configuration"></a>Dosya girişi - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `OneDrive` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - işlev kodu dosyası için kullanılan bir değişken adı. Bkz: [giriş bağlama koddan bir OneDrive dosyasını kullanarak](#onedrive-input-code).|
|**type**||Gerekli - kümesine olmalıdır `onedrive`.|
|**direction**||Gerekli - kümesine olmalıdır `in`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**userId**|**Kullanıcı Kimliği**  |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yolu**|Gereklidir - onedrive'daki dosyanın yolu.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Giriş - dosya kullanımı

Bu bağlama, aşağıdaki Azure AD izinleri gerektirir:

|Resource|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuyun|

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- byte[]
- Akış
- string
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Dosya çıktısı

OneDrive dosya çıkış bağlaması, Onedrive'da depolanan bir dosyanın içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-output---example)
* [Öznitelikler](#file-output---attributes)
* [Yapılandırma](#file-output---configuration)
* [Kullanım](#file-output---usage)

### <a name="file-output---example"></a>Dosya - çıkış örneği

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#file-output---c-script-example)
* [JavaScript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Dosya çıkışı - C# betiği örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyaya yazar.

*Function.json* dosyası tanımlar HTTP tetikleyicisi olan bir OneDrive çıktı bağlaması:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu Sorgu dizesinden metni alır ve arayanın OneDrive kökünde bir metin dosyasına (yukarıdaki örnekte tanımlanan FunctionsTest.txt) Yazar:

```csharp
using System.Net;
using System.Text;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, ILogger log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    myOneDriveFile.Close();
    return;
}
```

#### <a name="file-output---javascript-example"></a>Çıkış - dosya JavaScript örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyaya yazar.

*Function.json* dosyası tanımlar HTTP tetikleyicisi olan bir OneDrive çıktı bağlaması:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu Sorgu dizesinden metni alır ve arayanın OneDrive kökündeki (FunctionsTest.txt yukarıdaki yapılandırmasını tanımlandığı gibi) metin dosyasına yazar.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Çıkış - dosya öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) özniteliği.

### <a name="file-output---configuration"></a>Dosya - çıkış yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `OneDrive` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - işlev kodu dosyası için kullanılan bir değişken adı. Bkz: [çıktı bağlamasını koddan bir OneDrive dosyasını kullanarak](#onedrive-output-code).|
|**type**||Gerekli - kümesine olmalıdır `onedrive`.|
|**direction**||Gerekli - kümesine olmalıdır `out`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |**userId** |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yolu**|Gereklidir - onedrive'daki dosyanın yolu.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Dosya çıkışı - kullanım

Bu bağlama, aşağıdaki Azure AD izinleri gerektirir:

|Resource|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim sağlama|

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- byte[]
- Akış
- string
- Microsoft.Graph.DriveItem





<a name="outlook-output"></a>
## <a name="outlook-output"></a>Outlook çıkış

Outlook ileti bağlama gönderir Outlook posta iletisi çıktı.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#outlook-output---example)
* [Öznitelikler](#outlook-output---attributes)
* [Yapılandırma](#outlook-output---configuration)
* [Kullanım](#outlook-output---usage)

### <a name="outlook-output---example"></a>Outlook çıkış - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#outlook-output---c-script-example)
* [JavaScript](#outlook-output---javascript-example)

#### <a name="outlook-output---c-script-example"></a>Outlook çıkışı - C# betiği örneği

Aşağıdaki örnek, bir Outlook e-posta gönderir.

*Function.json* dosyası tanımlar HTTP tetikleyicisi olan bir Outlook ileti çıktı bağlamasını:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

C# betik kodu, sorgu dizesinde belirtilen alıcıya arayandan posta gönderir:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static void Run(HttpRequest req, out Message message, ILogger log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

#### <a name="outlook-output---javascript-example"></a>Çıkış - outlook JavaScript örneği

Aşağıdaki örnek, bir Outlook e-posta gönderir.

*Function.json* dosyası tanımlar HTTP tetikleyicisi olan bir Outlook ileti çıktı bağlamasını:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

JavaScript kodu, sorgu dizesinde belirtilen alıcıya arayandan posta gönderir:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```

### <a name="outlook-output---attributes"></a>Çıkış - outlook öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) özniteliği.

### <a name="outlook-output---configuration"></a>Outlook çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Outlook` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu bir posta iletisi için kullanılır. Bkz: [Outlook iletileri kullanarak çıktı bağlamasını koddan](#outlook-output-code).|
|**type**||Gerekli - kümesine olmalıdır `outlook`.|
|**direction**||Gerekli - kümesine olmalıdır `out`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**userId**|**Kullanıcı Kimliği**  |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Çıkış - outlook kullanımı

Bu bağlama, aşağıdaki Azure AD izinleri gerektirir:

|Resource|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı olarak posta gönderme|

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- string
- Özel nesne türlerinin (yapısal model bağlama kullanarak)






## <a name="webhooks"></a>Web Kancaları

Web kancaları Microsoft Graph olaylara tepki olanak sağlar. Web kancaları desteklemek için işlevleri tepki oluşturmak ve yenilemek için gereken _Web kancası aboneliklerini_. Tam Web kancası çözüm aşağıdaki bağlamaları birleşimi gerektirir:
- A [Microsoft Graph Web kancası tetikleyici](#webhook-trigger) tepki vermek için gelen bir Web kancası olanak tanır.
- A [Microsoft Graph Web kancası aboneliği giriş bağlama](#webhook-input) mevcut abonelikleri listeleyin ve isteğe bağlı olarak bunları yenileme olanak tanır.
- A [Microsoft Graph Web kancası aboneliği çıktı bağlamasını](#webhook-output) oluşturun veya Web kancası aboneliklerini silmenize olanak sağlar.

Bağlamaları herhangi bir Azure AD izinleri gerektirmez, ancak tepki istediğiniz kaynak türü için uygun izinleri istemeniz gerekir. Hangi izinleri her kaynak türü için gereklidir ve bir listesi için bkz [abonelik izinleri](https://docs.microsoft.com/graph/api/subscription-post-subscriptions?view=graph-rest-1.0).

Web kancaları hakkında daha fazla bilgi için bkz: [Microsoft Graph Web kancaları ile çalışma].





## <a name="webhook-trigger"></a>Web kancası tetikleyici

Microsoft Graph Web kancası tetikleyici için gelen bir Web kancasını Microsoft Graph'teki tepki vermek bir işlev verir. Bu tetikleyici her örneği için bir Microsoft Graph kaynak türü tepki verebilir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-trigger---example)
* [Öznitelikler](#webhook-trigger---attributes)
* [Yapılandırma](#webhook-trigger---configuration)
* [Kullanım](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Web kancası tetikleyici - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#webhook-trigger---c-script-example)
* [JavaScript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Web kancası tetikleyici - C# betiği örneği

Aşağıdaki örnek, gelen Outlook iletileri için Web kancaları işler. Kullanılacak bir Web kancası tetiklemenin, [abonelik oluşturma](#webhook-output---example), ve [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası, bir Web kancası tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

C# betik kodunu için gelen posta iletilerini tepki verir ve alıcı tarafından gönderilen ve bu konu "Azure işlevleri" içeren gövdesi kaydeder:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;
using Microsoft.Extensions.Logging;

public static async Task Run(Message msg, ILogger log)  
{
    log.LogInformation("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.LogInformation($"Processed email: {msg.BodyPreview}");
    }
}
```

#### <a name="webhook-trigger---javascript-example"></a>Web kancası tetikleyici - JavaScript örneği

Aşağıdaki örnek, gelen Outlook iletileri için Web kancaları işler. Kullanılacak bir Web kancası tetiklemenin, [abonelik oluşturma](#webhook-output---example), ve [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası, bir Web kancası tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

JavaScript kodu için gelen posta iletilerini tepki verir ve alıcı tarafından gönderilen ve bu konu "Azure işlevleri" içeren gövdesi kaydeder:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

### <a name="webhook-trigger---attributes"></a>Web kancası tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [GraphWebHookTrigger](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookTriggerAttribute.cs) özniteliği.

### <a name="webhook-trigger---configuration"></a>Web kancası tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `GraphWebHookTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu bir posta iletisi için kullanılır. Bkz: [Outlook iletileri kullanarak çıktı bağlamasını koddan](#outlook-output-code).|
|**type**||Gerekli - kümesine olmalıdır `graphWebhook`.|
|**direction**||Gerekli - kümesine olmalıdır `trigger`.|
|**resourceType**|**Kaynak türü**|Gereklidir - grafik kaynağı bu işlev için Web kancaları yanıt vermelidir. Aşağıdaki değerlerden biri olabilir:<ul><li><code>#Microsoft.Graph.Message</code> -Outlook iletileri için yapılan değişiklikler.</li><li><code>#Microsoft.Graph.DriveItem</code> -OneDrive kök öğelerine yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Contact</code> -Outlook Kişisel kişileri yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Event</code> -Outlook takvimi öğelerine yapılan değişiklikler.</li></ul>|

> [!Note]
> Bir işlev uygulaması, yalnızca karşı kayıtlı bir işlevi olabilir bir verilen `resourceType` değeri.

### <a name="webhook-trigger---usage"></a>Web kancası tetikleyici - kullanım

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- Microsoft Graph SDK'sı türleri ile ilgili kaynak türü gibi `Microsoft.Graph.Message` veya `Microsoft.Graph.DriveItem`.
- Özel nesne türlerinin (yapısal model bağlama kullanarak)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Web kancası giriş

Microsoft Graph Web kancası giriş bağlaması bu işlev uygulaması tarafından yönetilen Aboneliklerde listesini almanıza olanak tanır. Değil yansıtacak şekilde bağlama uygulama dışında oluşturulan diğer Aboneliklerdeki işlevi uygulama depolama alanından okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-input---example)
* [Öznitelikler](#webhook-input---attributes)
* [Yapılandırma](#webhook-input---configuration)
* [Kullanım](#webhook-input---usage)

### <a name="webhook-input---example"></a>Web kancası girişi - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#webhook-input---c-script-example)
* [JavaScript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Web kancası giriş - C# betiği örneği

Aşağıdaki örnek, çağıran kullanıcı için tüm abonelikleri alır ve bunları siler.

*Function.json* dosyası, bir abonelik giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar ve bir abonelik çıkış bağlaması silme eylemi kullanır:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu abonelikleri alır ve bunları siler:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.LogInformation($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

#### <a name="webhook-input---javascript-example"></a>Giriş - Web kancası JavaScript örneği

Aşağıdaki örnek, çağıran kullanıcı için tüm abonelikleri alır ve bunları siler.

*Function.json* dosyası, bir abonelik giriş bağlama işlemiyle HTTP tetikleyicisi tanımlar ve bir abonelik çıkış bağlaması silme eylemi kullanır:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu abonelikleri alır ve bunları siler:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

### <a name="webhook-input---attributes"></a>Giriş - Web kancası öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) özniteliği.

### <a name="webhook-input---configuration"></a>Web kancası girişi - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `GraphWebHookSubscription` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu bir posta iletisi için kullanılır. Bkz: [Outlook iletileri kullanarak çıktı bağlamasını koddan](#outlook-output-code).|
|**type**||Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**direction**||Gerekli - kümesine olmalıdır `in`.|
|**Filtre**|**Filtre**| Varsa kümesine `userFromRequest`, sonra da bağlama yalnızca çağrıyı yapan kullanıcının sahip olduğu abonelikleri alır (yalnızca geçerli [HTTP tetikleyicisi]).| 

### <a name="webhook-input---usage"></a>Web kancası giriş - kullanım

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- string[]
- Özel bir nesne türü diziler
- Newtonsoft.Json.Linq.JObject]
- Microsoft.Graph.Subscription]





## <a name="webhook-output"></a>Web kancası çıkış

Web kancası aboneliği çıkış bağlaması oluşturma, silme ve Microsoft Graph Web kancası aboneliklerini Yenile olanak tanır.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-output---example)
* [Öznitelikler](#webhook-output---attributes)
* [Yapılandırma](#webhook-output---configuration)
* [Kullanım](#webhook-output---usage)

### <a name="webhook-output---example"></a>Web kancası çıkış - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#webhook-output---c-script-example)
* [JavaScript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Web kancası çıkışı - C# betiği örneği

Aşağıdaki örnek, bir abonelik oluşturur. Yapabilecekleriniz [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası, bir abonelik çıktı bağlama oluşturma eylemini kullanarak HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu çağıran kullanıcı bir Outlook iletisi aldığında, bu işlev uygulaması bildirir bir Web kancası kaydeder:

```csharp
using System;
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, ILogger log)
{
  log.LogInformation("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

#### <a name="webhook-output---javascript-example"></a>Web kancası çıkışı - JavaScript örneği

Aşağıdaki örnek, bir abonelik oluşturur. Yapabilecekleriniz [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası, bir abonelik çıktı bağlama oluşturma eylemini kullanarak HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu çağıran kullanıcı bir Outlook iletisi aldığında, bu işlev uygulaması bildirir bir Web kancası kaydeder:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Web kancası çıkışı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) özniteliği.

### <a name="webhook-output---configuration"></a>Web kancası çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `GraphWebHookSubscription` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**name**||Gereklidir - değişken adı işlev kodu bir posta iletisi için kullanılır. Bkz: [Outlook iletileri kullanarak çıktı bağlamasını koddan](#outlook-output-code).|
|**type**||Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**direction**||Gerekli - kümesine olmalıdır `out`.|
|**Kimlik**|**Kimlik**|Gereklidir - eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisi]. Çağrıyı yapan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirteci tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulamanın kimliğini kullanır.</li></ul>|
|**userId**|**Kullanıcı Kimliği**  |Gerekli ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açmış bir kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**userToken**|Gerekli ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Eylem**|**Eylem**|Gereklidir - bağlama eylem gerçekleştirmesi gerektiğini belirtir. Aşağıdaki değerlerden biri olabilir:<ul><li><code>create</code> -Yeni bir aboneliği kaydeder.</li><li><code>delete</code> -Belirtilen bir aboneliğe siler.</li><li><code>refresh</code> -Süresinin dolmasını tutmak için belirtilen abonelik yeniler.</li></ul>|
|**subscriptionResource**|**subscriptionResource**|Gerekli ve yalnızca _eylem_ ayarlanır `create`. Değişiklik izlemesi Microsoft Graph kaynağını belirtir. Bkz: [Microsoft Graph Web kancaları ile çalışma]. |
|**ChangeType**|**ChangeType**|Gerekli ve yalnızca _eylem_ ayarlanır `create`. Bir bildirim oluşturacak olan abone olunmuş kaynaktaki değişiklik türünü belirtir. Desteklenen değerler şunlardır: `created`, `updated`, `deleted`. Virgülle ayrılmış bir liste kullanarak birden çok değer birleştirilebilir.|

### <a name="webhook-output---usage"></a>Web kancası çıkışı - kullanım

Bağlama .NET işlevlerine aşağıdaki türlerini sunar:
- string
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Web kancası aboneliği Yenile

Abonelik yenileme için iki yaklaşım vardır:

- Tüm aboneliklere dağıtılacak uygulama kimliğini kullanın. Bu bir Azure Active Directory Yöneticisi'nden onay gerektirir Bu, Azure işlevleri tarafından desteklenen tüm dillerde tarafından kullanılabilir.
- Her kullanıcı kimliğini el ile bağlayarak her abonelikle kimliğini kullan Bu bağlayıcı örtükliği gerçekleştirmek için özel kod gerektirir. Bu, yalnızca .NET işlevleri tarafından kullanılabilir.

Bu bölümde bu yaklaşımların her için bir örnek içerir:

* [Uygulama Kimliği örneği](#webhook-subscription-refresh---app-identity-example)
* [Kullanıcı Kimliği örneği](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Web kancası aboneliği Yenile - uygulama kimliği örneği

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#app-identity-refresh---c-script-example)
* JavaScript

### <a name="app-identity-refresh---c-script-example"></a>Uygulama kimliğini yenileme - C# betiği örneği

Aşağıdaki örnek, bir aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json* bir aboneliği olan bir zamanlayıcı tetikleyicisi tanımlar giriş bağlama ve bir abonelik çıktı bağlamasını:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

C# betik kodunu aboneliklerini yeniler:

```csharp
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, ILogger log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.LogInformation($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

### <a name="app-identity-refresh---c-script-example"></a>Uygulama kimliğini yenileme - C# betiği örneği

Aşağıdaki örnek, bir aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json* bir aboneliği olan bir zamanlayıcı tetikleyicisi tanımlar giriş bağlama ve bir abonelik çıktı bağlamasını:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

JavaScript kodu aboneliklerini yeniler:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Refreshing subscription ${existing[i]}`);
        toRefresh.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

### <a name="webhook-subscription-refresh---user-identity-example"></a>Web kancası aboneliği Yenile - kullanıcı kimlik örneği

Aşağıdaki örnek, bir aboneliği yenilemek için kullanıcı kimliğini kullanır.

*Function.json* dosya bir zamanlayıcı tetikleyicisi tanımlar ve işlev kodunu abonelik giriş bağlaması erteler:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# betik kodunu aboneliklerini yeniler ve her kullanıcının kimliğini kullanarak kod içinde çıkış bağlaması oluşturur:

```csharp
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, ILogger log)
{
  log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.LogInformation($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

[HTTP tetikleyicisi]: functions-bindings-http-webhook.md
[Microsoft Graph Web kancaları ile çalışma]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
