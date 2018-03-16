---
title: "Azure işlevleri için Microsoft Graph bağlamaları"
description: "Microsoft Graph Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 12/20/2017
ms.author: mahender
ms.openlocfilehash: d774f0ca644793235a8c423b052b559d26e289c4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Azure işlevleri için Microsoft Graph bağlamaları

Bu makalede, yapılandırmak ve Microsoft Graph Tetikleyicileri ve bağlamaları Azure işlevlerinde çalışmak açıklanmaktadır. Bu, Azure işlevleri verilerini, Öngörüler ve olayları çalışmak için kullanabileceğiniz [Microsoft Graph](https://graph.microsoft.io).

Microsoft Graph uzantısı aşağıdaki bağlamaları sağlar:
- Bir [kimlik doğrulama belirteci girişi bağlamayı](#token-input) tüm Microsoft Graph API'si ile etkileşime olanak tanır.
- Bir [Excel tablosu girişi bağlamayı](#excel-input) Excel'den veri okumasına izin verir.
- Bir [Excel tablosu çıktı bağlama](#excel-output) Excel verilerini değiştirmenize olanak sağlar.
- A [OneDrive dosya girişi bağlamayı](#onedrive-input) OneDrive üzerinden dosyaları okumasına izin verir.
- A [çıkış bağlama OneDrive dosyası](#onedrive-output) OneDrive dosyaları yazmanıza izin verir.
- Bir [Outlook ileti çıktı bağlama](#outlook-output) Outlook aracılığıyla e-posta göndermenizi sağlar.
- Bir koleksiyonu [Microsoft Graph Web kancası Tetikleyicileri ve bağlamaları](#webhooks) Microsoft Graph'tan olaylarına tepki olanak tanır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note]
> Microsoft Graph bağlamaları olan şu anda Azure işlevleri sürüm önizlemesinde 2.x. İşlevler sürümünde desteklenmez 1.x.

## <a name="packages"></a>Paketler

Kimlik doğrulama belirteci giriş bağlaması sağlanan [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/) NuGet paketi. Diğer Microsoft Graph bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/) paket. Paketler için kaynak kodunu konusu [azure işlevleri microsoftgraph uzantı](https://github.com/Azure/azure-functions-microsoftgraph-extension/) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="setting-up-the-extensions"></a>Uzantıları ayarlama

Microsoft Graph bağlamaları aracılığıyla kullanılabilen _uzantıları bağlama_. Bağlama, Azure işlevleri çalışma zamanı için isteğe bağlı bileşenler uzantılarıdır. Bu bölümde, Microsoft Graph ve kimlik doğrulama belirteci uzantıları ayarlamak gösterilmiştir.

### <a name="enabling-functions-20-preview"></a>İşlevler 2.0 Önizleme etkinleştirme

Bağlama uzantıları yalnızca Azure işlevleri 2.0 Önizleme için kullanılabilir. 

Bir işlev uygulaması işlevleri çalışma zamanı Önizleme 2.0 sürümünü kullanacak şekilde ayarlama hakkında daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl](set-runtime-version.md).

### <a name="installing-the-extension"></a>Uzantısı yükleme

Azure portalından bir uzantı yüklemek için bir şablonu veya başvurduğu bağlama gidin. Yeni bir işlev oluşturun ve şablon seçim ekranına karşın, "Microsoft Graph" senaryo seçin. Bu senaryodan şablonlardan birini seçin. Alternatif olarak, var olan bir işlev "Tümleştirme" sekmesine gidin ve bu makalede ele alınan bağlamaları birini seçin.

Her iki durumda da, yüklenecek uzantısı belirten bir uyarı görüntülenir. Tıklatın **yükleme** uzantısı elde edilir. Her bir uzantı yalnızca işlev uygulaması bir kez yüklü olması gerekir. 

> [!Note] 
> Portal yükleme işlemi tüketim plan üzerinde 10 dakikaya kadar sürebilir.

Visual Studio kullanıyorsanız, uzantıları yükleyerek alabileceğiniz [bu makalenin önceki bölümlerinde listelenen NuGet paketleri](#packages).

### <a name="configuring-authentication--authorization"></a>Kimlik doğrulaması yapılandırma / yetkilendirme

Bu makalede açıklanan bağlamaları kullanılması için bir kimlik gerektirir. Bu izinleri zorlamak ve etkileşimleri denetlemek Microsoft Graph sağlar. Uygulamanızı veya uygulamaya erişen bir kullanıcı kimliği olabilir. Bu kimliğini yapılandırmak için ayarladığınız [App Service kimlik doğrulama / yetkilendirme](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) Azure Active Directory ile. İşlevlerinizi gerektiren herhangi bir kaynak izin istemeniz gerekir.

> [!Note] 
> Microsoft Graph uzantısı yalnızca Azure AD kimlik doğrulamasını destekler. Kullanıcıların iş veya Okul hesabı ile oturum açmanız gerekir.

Azure portalını kullanıyorsanız, uzantıyı yüklemek için bir uyarı istemi aşağıda görürsünüz. App Service kimlik doğrulamasını yapılandırmak için uyarı ister / yetkilendirme ve istek şablonu veya bağlama tüm izinleri gerektirir. Tıklatın **Şimdi Yapılandır Azure AD** veya **izinleri şimdi eklemek** uygun şekilde.



<a name="token-input"></a>
## <a name="auth-token"></a>Kimlik doğrulama belirteci

Kimlik doğrulama belirteci giriş bağlama Azure AD için belirli bir kaynak belirtecini alır ve kodunuzu bir dize olarak sağlar. Kaynak için hangi olabilir uygulama izinleri. 

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#auth-token---example)
* [Öznitelikleri](#auth-token---attributes)
* [Yapılandırma](#auth-token---configuration)
* [Kullanım](#auth-token---usage)

### <a name="auth-token---example"></a>Kimlik doğrulama belirteci - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#auth-token---c-script-example)
* [JavaScript](#auth-token---javascript-example)

#### <a name="auth-token---c-script-example"></a>Kimlik doğrulama belirteci - C# kod örneği

Aşağıdaki örnekte, kullanıcı profili bilgilerini alır.

*Function.json* dosyası belirteci giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

C# kodu Microsoft Graph için bir HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

#### <a name="auth-token---javascript-example"></a>Kimlik doğrulama belirteci - JavaScript örneği

Aşağıdaki örnekte, kullanıcı profili bilgilerini alır.

*Function.json* dosyası belirteci giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

JavaScript kodu Microsoft Graph için bir HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür.

```js
const rp = require('request-promise');

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

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Token` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - için kimlik doğrulama belirteci işlevi kod içinde kullanılan değişken adı. Bkz: [bir kimlik doğrulama belirteci kullanılarak bağlama kodundan giriş](#token-input-code).|
|**Türü**||Gerekli - kümesine olmalıdır `token`.|
|**Yönü**||Gerekli - kümesine olmalıdır `in`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**userId**|**UserId**  |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Kaynak**|**Kaynak**|Gerekli - belirteç istenmektedir bir Azure AD kaynak URL.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Kimlik doğrulama belirteci - kullanım

Bağlama herhangi bir Azure AD izin gerektirmez, ancak belirteç nasıl kullanıldığını bağlı olarak ek izinler istemeniz gerekebilir. Belirteçle erişmek istediğiniz kaynak gereksinimlerini denetleyin.

Belirteç, her zaman koduna bir dize olarak sunulur.




<a name="excel-input"></a>
## <a name="excel-input"></a>Excel giriş

Excel tablosu giriş bağlaması Onedrive'da depolanan bir Excel tablosu içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-input---example)
* [Öznitelikleri](#excel-input---attributes)
* [Yapılandırma](#excel-input---configuration)
* [Kullanım](#excel-input---usage)

### <a name="excel-input---example"></a>Excel giriş - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#excel-input---c-script-example)
* [JavaScript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel girişi - C# kod örneği

Aşağıdaki *function.json* dosya bir Excel giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

Aşağıdaki C# betik kodu, belirtilen tablo içeriğini okur ve kullanıcıya döndürür:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

#### <a name="excel-input---javascript-example"></a>Girişi - Excel JavaScript örneği

Aşağıdaki *function.json* dosya bir Excel giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

Şu JavaScript kodunu belirtilen tablo içeriğini okur ve kullanıcıya döndürür.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

### <a name="excel-input---attributes"></a>Girişi - Excel öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliği.

### <a name="excel-input---configuration"></a>Excel giriş - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Excel` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - işlev kodu Excel tablosu için kullanılan değişken adı. Bkz: [bir Excel tablosu kullanarak giriş kodundan bağlama](#excel-input-code).|
|**Türü**||Gerekli - kümesine olmalıdır `excel`.|
|**Yönü**||Gerekli - kümesine olmalıdır `in`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**userId**|**UserId**  |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yol**|**Path**|Gerekli - onedrive'da Excel çalışma kitabı yolu.|
|**WorksheetName**|**WorksheetName**|Tablo bulunduğu çalışma sayfası.|
|**tableName**|**TableName**|Tablonun adı. Belirtilmezse, çalışma kitabının içeriğini kullanılır.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Excel girişi - kullanım

Bu bağlama aşağıdaki Azure AD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuyun|

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- string[][]
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)










<a name="excel-output"></a>
## <a name="excel-output"></a>Excel çıkış

Excel çıkış bağlama Onedrive'da depolanan bir Excel tablosu içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-output---example)
* [Öznitelikleri](#excel-output---attributes)
* [Yapılandırma](#excel-output---configuration)
* [Kullanım](#excel-output---usage)

### <a name="excel-output---example"></a>Excel çıktısı - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#excel-output---c-script-example)
* [JavaScript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Çıktı - Excel C# kod örneği

Aşağıdaki örnek, bir Excel tablosuyla satır ekler.

*Function.json* dosya bir Excel ile bir HTTP tetikleyicisi tanımlar bağlama çıktı:

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

C# betik kodu, sorgu dizesi girişten (tek sütunlu varsayılır) tablosuna yeni bir satır temelinde ekler:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
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

#### <a name="excel-output---javascript-example"></a>Çıktı - Excel JavaScript örneği

Aşağıdaki örnek, bir Excel tablosuyla satır ekler.

*Function.json* dosya bir Excel ile bir HTTP tetikleyicisi tanımlar bağlama çıktı:

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

Şu JavaScript kodunu sorgu dizesi girişten (tek sütunlu varsayılır) tablosuna yeni bir satır temelinde ekler.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Çıktı - Excel öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliği.

### <a name="excel-output---configuration"></a>Excel çıktısı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Excel` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - için kimlik doğrulama belirteci işlevi kod içinde kullanılan değişken adı. Bkz: [bir Excel tablosu kullanarak çıktıyı kodundan bağlama](#excel-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `excel`.|
|**Yönü**||Gerekli - kümesine olmalıdır `out`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**UserId** |**userId** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yol**|**Path**|Gerekli - onedrive'da Excel çalışma kitabı yolu.|
|**WorksheetName**|**WorksheetName**|Tablo bulunduğu çalışma sayfası.|
|**tableName**|**TableName**|Tablonun adı. Belirtilmezse, çalışma kitabının içeriğini kullanılır.|
|**updateType**|**UpdateType**|Gerekli - tabloya yapmak için değişiklik türü. Aşağıdaki değerlerden biri olabilir:<ul><li><code>update</code> -OneDrive tabloda içeriğini değiştirir.</li><li><code>append</code> -Yükü OneDrive tabloda sonuna yeni satırlar oluşturarak ekler.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Çıktı - Excel kullanımı

Bu bağlama aşağıdaki Azure AD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim elde edin|

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- string[][]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Dosya giriş

OneDrive dosya giriş bağlaması Onedrive'da depolanan bir dosyanın içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-input---example)
* [Öznitelikleri](#file-input---attributes)
* [Yapılandırma](#file-input---configuration)
* [Kullanım](#file-input---usage)

### <a name="file-input---example"></a>Dosya giriş - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#file-input---c-script-example)
* [JavaScript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Giriş - dosya C# kod örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyayı okur.

*Function.json* dosyası OneDrive dosya giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

C# betik kodu sorgu dizesinde belirtilen dosyasını okur ve uzunluğu kaydeder:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Giriş - dosya JavaScript örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyayı okur.

*Function.json* dosyası OneDrive dosya giriş bağlama ile bir HTTP tetikleyicisi tanımlar:

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

Şu JavaScript kodunu sorgu dizesinde belirtilen dosyasını okur ve uzunluğunu döndürür.

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

### <a name="file-input---configuration"></a>Dosya giriş - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `OneDrive` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - işlev kodu dosya için kullanılan değişken adı. Bkz: [OneDrive dosyasını kullanarak giriş kodundan bağlama](#onedrive-input-code).|
|**Türü**||Gerekli - kümesine olmalıdır `onedrive`.|
|**Yönü**||Gerekli - kümesine olmalıdır `in`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**userId**|**UserId**  |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yol**|**Path**|Gerekli - onedrive'da dosyasının yolu.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Giriş - dosya kullanım

Bu bağlama aşağıdaki Azure AD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuyun|

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Byte]
- Akış
- string
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Dosya çıktısı

OneDrive dosya çıktı bağlama Onedrive'da depolanan bir dosyanın içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-output---example)
* [Öznitelikleri](#file-output---attributes)
* [Yapılandırma](#file-output---configuration)
* [Kullanım](#file-output---usage)

### <a name="file-output---example"></a>Dosya çıktısı - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#file-output---c-script-example)
* [JavaScript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Çıktı - dosya C# kod örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyaya yazar.

*Function.json* dosya tanımlayan bir HTTP tetikleyicisi bir OneDrive ile bağlama çıktı:

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

C# betik kodu, Sorgu dizesinden metni alır ve arayanın OneDrive kökünde bir metin dosyasına (önceki örnekte tanımlanan FunctionsTest.txt) Yazar:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```

#### <a name="file-output---javascript-example"></a>Çıktı - dosya JavaScript örneği

Aşağıdaki örnek, Onedrive'da depolanan bir dosyaya yazar.

*Function.json* dosya tanımlayan bir HTTP tetikleyicisi bir OneDrive ile bağlama çıktı:

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

JavaScript kodu Sorgu dizesinden metni alır ve arayanın OneDrive kökündeki (FunctionsTest.txt yukarıdaki config tanımlandığı gibi) metin dosyasına yazar.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Çıktı - dosya öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) özniteliği.

### <a name="file-output---configuration"></a>Dosya çıktısı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `OneDrive` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - işlev kodu dosyası için kullanılan değişken adı. Bkz: [OneDrive dosyasını kullanarak çıktıyı kodundan bağlama](#onedrive-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `onedrive`.|
|**Yönü**||Gerekli - kümesine olmalıdır `out`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**UserId** |**userId** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Yol**|**Path**|Gerekli - onedrive'da dosyasının yolu.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Çıktı - dosya kullanım

Bu bağlama aşağıdaki Azure AD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim elde edin|

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Byte]
- Akış
- string
- Microsoft.Graph.DriveItem





<a name="outlook-output"></a>
## <a name="outlook-output"></a>Outlook çıktı

Outlook ileti bağlama gönderir bir posta iletisi Outlook aracılığıyla çıktı.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#outlook-output---example)
* [Öznitelikleri](#outlook-output---attributes)
* [Yapılandırma](#outlook-output---configuration)
* [Kullanım](#outlook-outnput---usage)

### <a name="outlook-output---example"></a>Outlook çıktısı - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#outlook-output---c-script-example)
* [JavaScript](#outlook-output---javascript-example)

#### <a name="outlook-output---c-script-example"></a>Çıktı - outlook C# kod örneği

Aşağıdaki örnek, bir Outlook e-posta gönderir.

*Function.json* dosya tanımlayan bir HTTP tetikleyicisi bir Outlook ile bağlama çıktı mesajı:

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

C# betik kodu, bir posta sorgu dizesinde belirtilen bir alıcı çağrıyı yapandan gönderir:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
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

#### <a name="outlook-output---javascript-example"></a>Çıktı - outlook JavaScript örneği

Aşağıdaki örnek, bir Outlook e-posta gönderir.

*Function.json* dosya tanımlayan bir HTTP tetikleyicisi bir Outlook ile bağlama çıktı mesajı:

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

JavaScript kodu, bir posta sorgu dizesinde belirtilen bir alıcı çağrıyı yapandan gönderir:

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

### <a name="outlook-output---attributes"></a>Çıktı - outlook öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) özniteliği.

### <a name="outlook-output---configuration"></a>Outlook çıktısı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Outlook` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `outlook`.|
|**Yönü**||Gerekli - kümesine olmalıdır `out`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**userId**|**UserId**  |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Çıktı - outlook kullanımı

Bu bağlama aşağıdaki Azure AD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Bir kullanıcı olarak posta gönderme|

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- string
- Özel nesne türleri (yapısal model bağlama kullanarak)






## <a name="webhooks"></a>Web Kancaları

Web kancası Microsoft Graph olaylara tepki olanak sağlar. Web kancası desteklemek için işlevleri tepki oluşturmak ve yenilemek için gereken _Web kancası abonelikleri_. Bir tam Web kancası çözümü aşağıdaki bağlamaları bileşimini gerektirir:
- A [Microsoft Graph Web kancası tetikleyici](#webhook-trigger) için gelen bir Web kancası tepki olanak tanır.
- A [Microsoft Graph Web kancası abonelik girişi bağlamayı](#webhook-input) mevcut abonelikleri listesinde ve isteğe bağlı olarak bunları Yenile olanak tanır.
- A [Microsoft Graph Web kancası abonelik çıktı bağlama](#webhook-output) oluşturmak veya Web kancası abonelikleri silmek olanak tanır.

Bağlamaları kendilerini Azure AD izinlerin gerektirmez, ancak tepki istediğiniz kaynak türü için ilgili izinleri istemek gerekir. Hangi izinleri gerekiyor her kaynak türü için bir listesi için bkz: [abonelik izinleri](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Web kancası hakkında daha fazla bilgi için bkz: [kancalarını Microsoft Graph ile çalışma].





## <a name="webhook-trigger"></a>Web kancası tetikleyici

Microsoft Graph Web kancası tetikleyici için gelen bir Web kancası Microsoft Graph'tan tepki vermek bir işlev sağlar. Bu tetikleyici her örneği için bir Microsoft Graph kaynak türü tepki.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-trigger---example)
* [Öznitelikleri](#webhook-trigger---attributes)
* [Yapılandırma](#webhook-trigger---configuration)
* [Kullanım](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Web kancası tetikleyici - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#webhook-trigger---c-script-example)
* [JavaScript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Web kancası tetikleyici - C# kod örneği

Aşağıdaki örnek Web kancalarını gelen Outlook iletiler için işler. Kullanmak için bir Web kancası tetiklemek, [abonelik oluşturma](#webhook-output---example), ve [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosya, bir Web kancası tetikleyici tanımlar:

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

C# kodu gelen posta iletilerine yanıt verir ve bu alıcı tarafından gönderilen ve konu "Azure işlevleri" içeren gövde günlüğe kaydeder:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

#### <a name="webhook-trigger---javascript-example"></a>Web kancası tetikleyici - JavaScript örneği

Aşağıdaki örnek Web kancalarını gelen Outlook iletiler için işler. Kullanmak için bir Web kancası tetiklemek, [abonelik oluşturma](#webhook-output---example), ve [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosya, bir Web kancası tetikleyici tanımlar:

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

JavaScript kodu gelen posta iletilerine yanıt verir ve bu alıcı tarafından gönderilen ve konu "Azure işlevleri" içeren gövde kaydeder:

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

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `GraphWebHookTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `graphWebhook`.|
|**Yönü**||Gerekli - kümesine olmalıdır `trigger`.|
|**resourceType**|**ResourceType**|Gerekli - grafik kaynak bu işlev için Web kancası kullanmalıdır. Aşağıdaki değerlerden biri olabilir:<ul><li><code>#Microsoft.Graph.Message</code> -Outlook iletileri yapılan değişiklikler.</li><li><code>#Microsoft.Graph.DriveItem</code> -OneDrive kök öğelerine yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Contact</code> -Outlook Kişisel kişilere yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Event</code> -Outlook Takvim öğelerine yapılan değişiklikler.</li></ul>|

> [!Note]
> Bir işlev uygulaması karşı kayıtlı bir işlevi yalnızca olabilir bir verilen `resourceType` değer.

### <a name="webhook-trigger---usage"></a>Web kancası tetikleyici - kullanım

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Kaynak türü için ilgili aşağıdaki gibi olan Microsoft Graph SDK türleri `Microsoft.Graph.Message` veya `Microsoft.Graph.DriveItem`.
- Özel nesne türleri (yapısal model bağlama kullanarak)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Web kancası giriş

Microsoft Graph Web kancası giriş bağlaması, bu işlev uygulaması tarafından yönetilen Aboneliklerin listesini almanıza olanak tanır. Değil yansıtacak şekilde bağlama işlevi uygulama depolama biriminden uygulama dışında oluşturulan diğer abonelikler okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-input---example)
* [Öznitelikleri](#webhook-input---attributes)
* [Yapılandırma](#webhook-input---configuration)
* [Kullanım](#webhook-input---usage)

### <a name="webhook-input---example"></a>Web kancası giriş - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#webhook-input---c-script-example)
* [JavaScript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Web kancası girişi - C# kod örneği

Aşağıdaki örnek, çağrıyı yapan kullanıcı için tüm abonelikleri alır ve bunları siler.

*Function.json* dosyası abonelik giriş bağlama ile bir HTTP tetikleyicisi tanımlar ve silme eylemi bağlaması bir abonelik çıktı kullanır:

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

C# kodu abonelikleri alır ve bunları siler:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

#### <a name="webhook-input---javascript-example"></a>Web kancası girişi - JavaScript örneği

Aşağıdaki örnek, çağrıyı yapan kullanıcı için tüm abonelikleri alır ve bunları siler.

*Function.json* dosyası abonelik giriş bağlama ile bir HTTP tetikleyicisi tanımlar ve silme eylemi bağlaması bir abonelik çıktı kullanır:

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

### <a name="webhook-input---attributes"></a>Web kancası girişi - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) özniteliği.

### <a name="webhook-input---configuration"></a>Web kancası giriş - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `GraphWebHookSubscription` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**Yönü**||Gerekli - kümesine olmalıdır `in`.|
|**Filtre**|**Filtre**| Varsa kümesine `userFromRequest`, bağlama yalnızca arama kullanıcıya ait abonelikleri alacak sonra (yalnızca geçerli [HTTP tetikleyicisini]).| 

### <a name="webhook-input---usage"></a>Web kancası girişi - kullanım

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- string[]
- Özel nesne türü dizileri
- Newtonsoft.Json.Linq.JObject[]
- Microsoft.Graph.Subscription]





## <a name="webhook-output"></a>Web kancası çıktı

Web kancası abonelik çıkış bağlama oluşturma, silme ve Microsoft Graph Web kancası Aboneliklerde Yenile olanak tanır.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-output---example)
* [Öznitelikleri](#webhook-output---attributes)
* [Yapılandırma](#webhook-output---configuration)
* [Kullanım](#webhook-output---usage)

### <a name="webhook-output---example"></a>Web kancası çıktısı - örnek

Dile özgü örneğe bakın:

* [C# script (.csx)](#webhook-output---c-script-example)
* [JavaScript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Web kancası çıktı - C# kod örneği

Aşağıdaki örnek, bir abonelik oluşturur. Yapabilecekleriniz [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası oluşturma eylemini kullanarak bağlama abonelik çıkışıyla bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
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

C# betik kodu çağıran kullanıcı Outlook iletisi aldığında, bu işlev uygulaması bildirir bir Web kancası kaydeder:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

#### <a name="webhook-output---javascript-example"></a>Çıktı - Web kancası JavaScript örneği

Aşağıdaki örnek, bir abonelik oluşturur. Yapabilecekleriniz [abonelik yenileme](#webhook-subscription-refresh) süresinin dolmasını engellemek için.

*Function.json* dosyası oluşturma eylemini kullanarak bağlama abonelik çıkışıyla bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
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

JavaScript kodu arayan kullanıcı Outlook iletisi aldığında, bu işlev uygulaması uyarır bir Web kancası kaydeder:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Çıktı - Web kancası öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) özniteliği.

### <a name="webhook-output---configuration"></a>Web kancası çıktısı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `GraphWebHookSubscription` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**||Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**Türü**||Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**Yönü**||Gerekli - kümesine olmalıdır `out`.|
|**identity**|**Kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code> -Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code> -Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code> -Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code> -İşlevi uygulama kimliğini kullanır.</li></ul>|
|**userId**|**UserId**  |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|**UserToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Eylem**|**Eylem**|Gerekli - bağlama eylem gerçekleştirmesi gerektiğini belirtir. Aşağıdaki değerlerden biri olabilir:<ul><li><code>create</code> -Yeni bir abonelik kaydeder.</li><li><code>delete</code> -Belirtilen bir abonelik siler.</li><li><code>refresh</code> -Süresinin dolmasını tutmak için belirtilen abonelik yeniler.</li></ul>|
|**subscriptionResource**|**SubscriptionResource**|Gerekli olduğunda ve yalnızca _eylem_ ayarlanır `create`. Değişiklikleri izlenen Microsoft Graph kaynağı belirtir. Bkz: [kancalarını Microsoft Graph ile çalışma]. |
|**changeType**|**ChangeType**|Gerekli olduğunda ve yalnızca _eylem_ ayarlanır `create`. Bir bildirim oluşturacak abone olduğunuz kaynak değişiklik türünü belirtir. Desteklenen değerler: `created`, `updated`, `deleted`. Virgülle ayrılmış bir liste kullanarak birden çok değer birleştirilebilir.|

### <a name="webhook-output---usage"></a>Web kancası çıktı - kullanım

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- string
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Web kancası aboneliği yenileme

Abonelikleri yenilemek için iki yaklaşım vardır:

- Tüm abonelikleri ile mücadele etmek için uygulama kimliği kullanın. Bu bir Azure Active Directory Yöneticisi'nden izni gerektirir Azure işlevleri tarafından desteklenen tüm dillerde tarafından kullanılabilir.
- Her kullanıcı kimliğini el ile bağlayarak her abonelikle ilişkili kimliğini kullan Bu bağlama gerçekleştirmek için özel kod gerektirir. Bu, yalnızca .NET işlevleri tarafından kullanılabilir.

Bu bölüm bir örnek bu yaklaşımların her birinin için içerir:

* [Uygulama Kimliği örneği](#webhook-subscription-refresh---app-identity-example)
* [Kullanıcı Kimliği örneği](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Web kancası aboneliği yenileme - uygulama kimliği örneği

Dile özgü örneğe bakın:

* [C# script (.csx)](#app-identity-refresh---c-script-example)
* [JavaScript](#app-identity-refresh---javascript-example)

### <a name="app-identity-refresh---c-script-example"></a>Uygulama Kimliği yenileme - C# kod örneği

Aşağıdaki örnek, bir aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json* aboneliğini Zamanlayıcı tetikleyicisi tanımlar giriş bağlama ve abonelik çıkış bağlama:

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

C# kodu abonelikleri yenilenir:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

### <a name="app-identity-refresh---c-script-example"></a>Uygulama Kimliği yenileme - C# kod örneği

Aşağıdaki örnek, bir aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json* aboneliğini Zamanlayıcı tetikleyicisi tanımlar giriş bağlama ve abonelik çıkış bağlama:

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

JavaScript kodu abonelikleri yenilenir:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

### <a name="webhook-subscription-refresh---user-identity-example"></a>Web kancası aboneliği yenileme - kullanıcı kimliği örneği

Aşağıdaki örnek, bir aboneliği yenilemek için kullanıcı kimliğini kullanır.

*Function.json* dosya Zamanlayıcı tetikleyicisi tanımlar ve işlev kodu abonelik giriş bağlamaya erteler:

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

C# kodu abonelikleri yeniler ve her kullanıcının kimliğini kullanarak kod içinde çıktı bağlama oluşturur:

```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
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
            log.Info($"Refreshing subscription {subscription}");
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

[HTTP tetikleyicisini]: functions-bindings-http-webhook.md
[kancalarını Microsoft Graph ile çalışma]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
