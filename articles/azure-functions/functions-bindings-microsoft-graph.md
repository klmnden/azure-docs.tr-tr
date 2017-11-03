---
title: "Azure işlevleri Microsoft Graph bağlamaları | Microsoft Docs"
description: "Microsoft Graph Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/19/2017
ms.author: mahender
ms.openlocfilehash: 8cf2e4e9e9007549dbdc931b4485c4230c536479
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-microsoft-graph-bindings"></a>Azure işlevleri Microsoft Graph bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırmak ve Microsoft Graph Tetikleyicileri ve bağlamaları Azure işlevlerinde çalışmak açıklanmaktadır.
Bu, Azure işlevleri verilerini, Öngörüler ve olayları çalışmak için kullanabileceğiniz [Microsoft Graph](https://graph.microsoft.io).

Microsoft Graph uzantısı aşağıdaki bağlamaları sağlar:
- Bir [kimlik doğrulama belirteci girişi bağlamayı](#token-input) tüm Microsoft Graph API'si ile etkileşime olanak tanır.
- Bir [Excel tablosu girişi bağlamayı](#excel-input) Excel'den veri okumasına izin verir.
- Bir [Excel tablosu çıktı bağlama](#excel-output) Excel verilerini değiştirmenize olanak sağlar.
- Bir [OneDrive dosya girişi bağlamayı](#onedrive-input) OneDrive üzerinden dosyaları okumasına izin verir.
- Bir [çıkış bağlama OneDrive dosyası](#onedrive-output) OneDrive dosyaları yazmanıza izin verir.
- Bir [Outlook ileti çıktı bağlama](#outlook-output) Outlook aracılığıyla e-posta göndermenizi sağlar.
- Bir koleksiyonu [Microsoft Graph Web kancası Tetikleyicileri ve bağlamaları](#webhooks) Microsoft Graph'tan olaylarına tepki olanak tanır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note] 
> Microsoft Graph bağlamaları şu anda önizlemede.

## <a name="setting-up-the-extensions"></a>Uzantıları ayarlama

Microsoft Graph bağlamaları aracılığıyla kullanılabilen _uzantıları bağlama_. Bağlama, Azure işlevleri çalışma zamanı için isteğe bağlı bileşenler uzantılarıdır. Bu bölümde Microsoft Graph ve kimlik doğrulama belirteci uzantıları ayarlamak nasıl yapacağınızı gösterir.

### <a name="enabling-functions-20-preview"></a>İşlevler 2.0 Önizleme etkinleştirme

Bağlama uzantıları yalnızca yalnızca Azure işlevleri 2.0 Önizleme için kullanılabilir. 

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl](functions-versions.md).

### <a name="installing-the-extension"></a>Uzantısı yükleme

Azure portalından bir uzantı yüklemeniz için bir şablonu veya hangi başvurduğu bağlama için gidin gerekir. Yeni bir işlev oluşturun ve şablon seçim ekranına karşın, "Microsoft Graph" senaryo seçin. Bu senaryodan şablonlardan birini seçin. Alternatif olarak, var olan bir işlev "Tümleştirme" sekmesine gidin ve bu konuda ele bağlamaları birini seçin.

Her iki durumda da, yüklenecek uzantısı belirten bir uyarı görüntülenir. Tıklatın **yükleme** uzantısı elde edilir.

> [!Note] 
> Her bir uzantı yalnızca işlev uygulaması bir kez yüklü olması gerekir. Portal yükleme işlemi tüketim plan üzerinde 10 dakikaya kadar sürebilir.

Visual Studio kullanıyorsanız, bu NuGet paketlerini yükleyerek uzantıları alabilirsiniz:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-app-service-authentication--authorization"></a>App Service kimlik doğrulamasını yapılandırma / yetkilendirme

Bu konuda anlatılan bağlamaları kullanılması için bir kimlik gerektirir. Bu izinleri zorlamak ve etkileşimleri denetlemek Microsoft Graph sağlar. Uygulamanızı veya uygulamaya erişen bir kullanıcı kimliği olabilir. Bu kimliğini yapılandırmak için ayarlanan gerekecektir [App Service kimlik doğrulama / yetkilendirme](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) Azure Active Directory ile. İşlevlerinizi gerektiren herhangi bir kaynak izin istemeniz gerekir.

> [!Note] 
> Microsoft Graph uzantısı yalnızca AAD kimlik doğrulamasını destekler. Kullanıcıların iş veya Okul hesabı ile oturum açmanız gerekir.

Uzantıyı yüklemek için istemi aşağıda Azure portalını kullanıyorsanız, App Service kimlik doğrulamasını yapılandırmak için soran bir uyarı görürsünüz / yetkilendirme ve istek şablonu veya bağlama tüm izinleri gerektirir. Tıklatın **AAD yapılandırma şimdi** veya **izinleri şimdi eklemek** uygun şekilde.






<a name="token-input"></a>
## <a name="auth-token-input-binding"></a>Kimlik doğrulama belirteci giriş bağlama

Bu bağlama, belirli bir kaynak için bir AAD belirtecini alır ve kodunuzu bir dize olarak sağlar. Kaynak için hangi olabilir uygulama izinleri. 

### <a name="configuring-an-auth-token-input-binding"></a>Bir kimlik doğrulama belirteci giriş bağlama yapılandırma

Bağlama herhangi bir AAD izin gerektirmez, ancak belirteç nasıl kullanıldığını bağlı olarak ek izinler istemeniz gerekebilir. Belirteçle erişmek istediğiniz kaynak gereksinimlerini denetleyin.

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - için kimlik doğrulama belirteci işlevi kod içinde kullanılan değişken adı. Bkz: [bir kimlik doğrulama belirteci kullanılarak bağlama kodundan giriş](#token-input-code).|
|**türü**|Gerekli - kümesine olmalıdır `token`.|
|**yönü**|Gerekli - kümesine olmalıdır `in`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Kaynak**|Gerekli - belirteç istenmektedir bir AAD kaynak URL.|

<a name="token-input-code"></a>
### <a name="using-an-auth-token-input-binding-from-code"></a>Kimlik doğrulama bir belirteç giriş bağlama kodundan kullanma

Belirteç, her zaman koduna bir dize olarak sunulur.

#### <a name="sample-getting-user-profile-information"></a>Örnek: kullanıcı profili bilgilerini alma

Bir HTTP tetikleyicisi bir belirteç giriş bağlamayla tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# örnek Microsoft Graph için bir HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür:

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

Aşağıdaki JS örnek Microsoft Graph için bir HTTP çağrısı yapmak için bu belirteci kullanır ve sonucu döndürür. İçinde `function.json` yukarıdaki değiştirme `$return` için `res` ilk.

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





<a name="excel-input"></a>
## <a name="excel-table-input-binding"></a>Excel tablosu giriş bağlama

Bu bağlama Onedrive'da depolanan bir Excel tablosu içeriğini okur.

### <a name="configuring-an-excel-table-input-binding"></a>Bir Excel tablosu giriş bağlama yapılandırma

Bu bağlama aşağıdaki AAD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuma|

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - işlev kodu Excel tablosu için kullanılan değişken adı. Bkz: [bir Excel tablosu kullanarak giriş kodundan bağlama](#excel-input-code).|
|**türü**|Gerekli - kümesine olmalıdır `excel`.|
|**yönü**|Gerekli - kümesine olmalıdır `in`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**yol**|Gerekli - onedrive'da Excel çalışma kitabı yolu.|
|**worksheetName**|Tablo bulunduğu çalışma sayfası.|
|**tableName**|Tablonun adı. Belirtilmezse, çalışma kitabının içeriğini kullanılır.|

<a name="excel-input-code"></a>
### <a name="using-an-excel-table-input-binding-from-code"></a>Koddan bir Excel tablosu giriş bağlama işlemini kullanma

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- String [] [']
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)

#### <a name="sample-reading-an-excel-table"></a>Örnek: bir Excel tablosu okuma

Bir HTTP tetikleyicisi bir Excel giriş bağlamayla tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Belirtilen tablo içeriğini okur ve kullanıcıya döndürür aşağıdaki C# örnek ekler:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

Belirtilen tablo içeriğini okur ve kullanıcıya döndürür aşağıdaki JS örneği ekler. İçinde `function.json` yukarıdaki değiştirme `$return` için `res` ilk.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

<a name="excel-output"></a>
## <a name="excel-table-output-binding"></a>Excel tablosu bağlama çıktı

Bu bağlama Onedrive'da depolanan bir Excel tablosu içeriğini değiştirir.

### <a name="configuring-an-excel-table-output-binding"></a>Bir Excel tablosu yapılandırma bağlama çıktı

Bu bağlama aşağıdaki AAD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim elde edin|

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - için kimlik doğrulama belirteci işlevi kod içinde kullanılan değişken adı. Bkz: [bir Excel tablosu kullanarak çıktıyı kodundan bağlama](#excel-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `excel`.|
|**yönü**|Gerekli - kümesine olmalıdır `out`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**yol**|Gerekli - onedrive'da Excel çalışma kitabı yolu.|
|**worksheetName**|Tablo bulunduğu çalışma sayfası.|
|**tableName**|Tablonun adı. Belirtilmezse, çalışma kitabının içeriğini kullanılır.|
|**güncelleştirme türü**|Gerekli - tabloya yapmak için değişiklik türü. Aşağıdaki değerlerden biri olabilir:<ul><li><code>update</code>-OneDrive tabloda içeriğini değiştirir.</li><li><code>append</code>-Yükü OneDrive tabloda sonuna yeni satırlar oluşturarak ekler.</li></ul>|

<a name="excel-output-code"></a>
### <a name="using-an-excel-table-output-binding-from-code"></a>Bir Excel tablosu kullanarak çıktıyı kodundan bağlama

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- String [] [']
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)

#### <a name="sample-adding-rows-to-an-excel-table"></a>Örnek: bir Excel tablosuna satır ekleme

Aşağıdaki olduğunu varsayalım bir Excel ile bir HTTP tetikleyicisi tanımlar function.json bağlama çıktı:

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


Aşağıdaki C# örnek sorgu dizesi girişten (tek sütunlu varsayılır) tablosuna yeni bir satır temelinde ekler:

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

Aşağıdaki JS örnek sorgu dizesi girişten (tek sütunlu varsayılır) tablosuna yeni bir satır temelinde ekler. İçinde `function.json` yukarıdaki değiştirme `$return` için `res` ilk.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```




<a name="onedrive-input"></a>
## <a name="onedrive-file-input-binding"></a>OneDrive dosya giriş bağlama

Bu bağlama Onedrive'da depolanan bir dosyanın içeriğini okur.

### <a name="configuring-a-onedrive-file-input-binding"></a>OneDrive dosya giriş bağlama yapılandırma

Bu bağlama aşağıdaki AAD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuma|

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - işlev kodu dosya için kullanılan değişken adı. Bkz: [OneDrive dosyasını kullanarak giriş kodundan bağlama](#onedrive-input-code).|
|**türü**|Gerekli - kümesine olmalıdır `onedrive`.|
|**yönü**|Gerekli - kümesine olmalıdır `in`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**yol**|Gerekli - onedrive'da dosyasının yolu.|

<a name="onedrive-input-code"></a>
### <a name="using-a-onedrive-file-input-binding-from-code"></a>Koddan OneDrive dosya giriş bağlama işlemini kullanma

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Byte]
- Akış
- Dize
- Microsoft.Graph.DriveItem

#### <a name="sample-reading-a-file-from-onedrive"></a>Örnek: bir dosyayı OneDrive üzerinden okuma

Bir HTTP tetikleyicisi OneDrive giriş bağlama ile tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# örnek sorgu dizesinde belirtilen dosyasını okur ve uzunluğu kaydeder:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

Aşağıdaki JS örnek sorgu dizesinde belirtilen dosyasını okur ve uzunluğunu döndürür. İçinde `function.json` yukarıdaki değiştirme `$return` için `res` ilk.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```


<a name="onedrive-output"></a>
## <a name="onedrive-file-output-binding"></a>Çıkış OneDrive dosyası bağlama

Bu bağlama Onedrive'da depolanan bir dosyanın içeriğini değiştirir.

### <a name="configuring-a-onedrive-file-output-binding"></a>Çıkış bağlama bir OneDrive yapılandırma dosyası

Bu bağlama aşağıdaki AAD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişim elde edin|

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - işlev kodu dosyası için kullanılan değişken adı. Bkz: [OneDrive dosyasını kullanarak çıktıyı kodundan bağlama](#onedrive-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `onedrive`.|
|**yönü**|Gerekli - kümesine olmalıdır `out`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**yol**|Gerekli - onedrive'da dosyasının yolu.|

<a name="onedrive-output-code"></a>
### <a name="using-a-onedrive-file-output-binding-from-code"></a>Bir OneDrive kullanarak çıkış dosyası kodundan bağlama

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Byte]
- Akış
- Dize
- Microsoft.Graph.DriveItem

#### <a name="sample-writing-to-a-file-in-onedrive"></a>Örnek: OneDrive dosyasında yazma

Aşağıdaki olduğunu varsayalım bir HTTP tetikleyicisi bir OneDrive ile tanımlar function.json bağlama çıktı:

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

Aşağıdaki C# Örnek Sorgu dizesinden metni alır ve arayanın OneDrive kökündeki (FunctionsTest.txt yukarıdaki config tanımlandığı gibi) metin dosyasına yazar:

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
Aşağıdaki JS örnek Sorgu dizesinden metni alır ve arayanın OneDrive kökünde bir metin dosyasına (Yapılandırma'da tanımlandığı gibi FunctionsTest.txt) yazar. İçinde `function.json` yukarıdaki değiştirme `$return` için `res` ilk.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```



<a name="outlook-output"></a>
## <a name="outlook-message-output-binding"></a>Outlook ileti bağlama çıktı

Outlook aracılığıyla bir posta iletisi gönderir.

### <a name="configuring-an-outlook-message-output-binding"></a>Outlook iletisi yapılandırma bağlama çıktı

Bu bağlama aşağıdaki AAD izinleri gerektirir:
|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Bir kullanıcı olarak posta gönderme|

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `outlook`.|
|**yönü**|Gerekli - kümesine olmalıdır `out`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |

<a name="outlook-output-code"></a>
### <a name="using-an-outlook-message-output-binding-from-code"></a>Outlook iletisi kullanarak çıktıyı kodundan bağlama

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- Dize
- Özel nesne türleri (yapısal model bağlama kullanarak)

#### <a name="sample-sending-an-email-through-outlook"></a>Örnek: Outlook e-posta gönderme

Aşağıdaki olduğunu varsayalım Outlook iletisiyle bir HTTP tetikleyicisi tanımlar function.json bağlama çıktı:

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

Aşağıdaki C# örnek, bir posta sorgu dizesinde belirtilen bir alıcı çağrıyı yapandan gönderir:

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

Aşağıdaki JS örnek, bir posta sorgu dizesinde belirtilen bir alıcı çağrıyı yapandan gönderir:

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





<a name="webhooks"></a>
## <a name="microsoft-graph-webhook-bindings"></a>Microsoft Graph Web kancası bağlamaları

Web kancası Microsoft Graph olaylara tepki olanak sağlar. Web kancası desteklemek için işlevleri tepki oluşturmak ve yenilemek için gereken _Web kancası abonelikleri_. Bir tam Web kancası çözümü aşağıdaki bağlamaları bileşimini gerektirir:
- A [Microsoft Graph Web kancası tetikleyici](#webhook-trigger) için gelen bir Web kancası tepki olanak tanır.
- A [Microsoft Graph Web kancası abonelik girişi bağlamayı](#webhook-input) mevcut abonelikleri listesinde ve isteğe bağlı olarak bunları Yenile olanak tanır.
- A [Microsoft Graph Web kancası abonelik çıktı bağlama](#webhook-output) oluşturmak veya Web kancası abonelikleri silmek olanak tanır.

Bağlamaları kendilerini herhangi bir AAD izin gerektirmez, ancak tepki istediğiniz kaynak türü için ilgili izinleri istemeniz gerekir. Hangi izinleri gerekiyor her kaynak türü için bir listesi için bkz: [abonelik izinleri](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Web kancası hakkında daha fazla bilgi için bkz: [kancalarını Microsoft Graph ile çalışma].





<a name="webhook-trigger"></a>
### <a name="microsoft-graph-webhook-trigger"></a>Microsoft Graph Web kancası tetikleyici

Bu tetikleyici için gelen bir Web kancası Microsoft Graph'tan tepki vermek bir işlev sağlar. Bu tetikleyici her örneği için bir Microsoft Graph kaynak türü tepki.

#### <a name="configuring-a-microsoft-graph-webhook-trigger"></a>Microsoft Graph Web kancası tetikleyici yapılandırma

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `graphWebhook`.|
|**yönü**|Gerekli - kümesine olmalıdır `trigger`.|
|**Kaynak türü**|Gerekli - grafik kaynak bu işlev için Web kancası kullanmalıdır. Aşağıdaki değerlerden biri olabilir:<ul><li><code>#Microsoft.Graph.Message</code>-Outlook iletileri yapılan değişiklikler.</li><li><code>#Microsoft.Graph.DriveItem</code>-OneDrive kök öğelerine yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Contact - changes made to personal contacts in Outlook.</code></li><li><code>#Microsoft.Graph.Event - changes made to Outlook calendar items.</code></li></ul>|

> [!Note]
> Bir işlev uygulaması karşı kayıtlı bir işlevi yalnızca olabilir bir verilen `resourceType` değer.

#### <a name="using-a-microsoft-graph-webhook-trigger-from-code"></a>Microsoft Graph Web kancası tetikleyici kodundan kullanma

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Kaynak için ilgili Microsoft Graph SDK türleri, örn., Microsoft.Graph.Message, Microsoft.Graph.DriveItem yazın
- Özel nesne türleri (yapısal model bağlama kullanarak)

Bu bağlama kod içinde kullanma örnekleri için lütfen bkz [Microsoft Graph Web kancası örnekleri](#webhook-samples).



<a name="webhook-input"></a>
### <a name="microsoft-graph-webhook-subscription-input-binding"></a>Microsoft Graph Web kancası abonelik giriş bağlama

Bu bağlama, bu işlev uygulaması tarafından yönetilen Aboneliklerin listesini almanıza olanak tanır. Bağlama işlevi uygulama depolama biriminden okur ve uygulama dışında oluşturulan diğer abonelikler yansıtmaz.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-input-binding"></a>Bir Microsoft Graph Web kancası abonelik giriş bağlama yapılandırma

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**yönü**|Gerekli - kümesine olmalıdır `in`.|
|**Filtre**| Varsa kümesine `userFromRequest`, bağlama yalnızca arama kullanıcıya ait abonelikleri alacak sonra (yalnızca geçerli [HTTP tetikleyicisini]).| 

#### <a name="using-a-microsoft-graph-webhook-subscription-input-binding-from-code"></a>Microsoft Graph bir Web kancası abonelik giriş bağlama kodundan kullanma

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- String]
- Özel nesne türü dizileri
- Newtonsoft.Json.Linq.JObject]
- Microsoft.Graph.Subscription]

Bu bağlama kod içinde kullanma örnekleri için lütfen bkz [Microsoft Graph Web kancası örnekleri](#webhook-samples).


<a name="webhook-output"></a>
### <a name="microsoft-graph-webhook-subscription-output-binding"></a>Microsoft Graph Web kancası abonelik bağlama çıktı

Bu bağlama, oluşturma, silme ve Microsoft Graph Web kancası Aboneliklerde Yenile olanak sağlar.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-output-binding"></a>Abonelik bağlama çıktı Microsoft Graph Web kancası yapılandırma

Bağlama aşağıdaki özellikleri destekler:

|Özellik|Açıklama|
|--------|--------|
|**adı**|Gerekli - posta iletisi için işlevi kod içinde kullanılan değişken adı. Bkz: [Outlook iletisi kullanarak çıktıyı kodundan bağlama](#outlook-output-code).|
|**türü**|Gerekli - kümesine olmalıdır `graphWebhookSubscription`.|
|**yönü**|Gerekli - kümesine olmalıdır `out`.|
|**kimlik**|Gerekli - eylemi gerçekleştirmek için kullanılan kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>-Yalnızca geçerli [HTTP tetikleyicisini]. Arayan Kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>-Belirtilen kimliğe sahip bir daha önce oturum açma kullanıcı kimliğini kullanır. Bkz: <code>userId</code> özelliği.</li><li><code>userFromToken</code>-Belirtilen belirtecin tarafından temsil edilen kimliğini kullanır. Bkz: <code>userToken</code> özelliği.</li><li><code>clientCredentials</code>-İşlevi uygulama kimliğini kullanır.</li></ul>|
|**Kullanıcı Kimliği** |Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromId`. Daha önce oturum açma kullanıcı ile ilişkili kullanıcı asıl kimliği.|
|**userToken**|Gerekli olduğunda ve yalnızca _kimlik_ ayarlanır `userFromToken`. İşlev uygulaması için geçerli bir belirteç. |
|**Eylem**|Gerekli - bağlama eylem gerçekleştirmesi gerektiğini belirtir. Aşağıdaki değerlerden biri olabilir:<ul><li><code>create</code>-Yeni bir abonelik kaydeder.</li><li><code>delete</code>-Belirtilen bir abonelik siler.</li><li><code>refresh</code>-Süresinin dolmasını tutmak için belirtilen abonelik yeniler.</li></ul>|
|**subscriptionResource**|Gerekli olduğunda ve yalnızca _eylem_ ayarlanır `create`. Değişiklikleri izlenen Microsoft Graph kaynağı belirtir. Bkz: [kancalarını Microsoft Graph ile çalışma]. |
|**changeType**|Gerekli olduğunda ve yalnızca _eylem_ ayarlanır `create`. Bir bildirim oluşturacak abone olduğunuz kaynak değişiklik türünü belirtir. Desteklenen değerler: `created`, `updated`, `deleted`. Virgülle ayrılmış bir liste kullanarak birden çok değer birleştirilebilir.|

#### <a name="using-a-microsoft-graph-webhook-subscription-output-binding-from-code"></a>Microsoft Graph Web kancası kullanarak abonelik kodundan bağlama çıktı

Bağlama .NET işlevlerine aşağıdaki türü ortaya çıkarır:
- Dize
- Microsoft.Graph.Subscription

Bu bağlama kod içinde kullanma örnekleri için lütfen bkz [Microsoft Graph Web kancası örnekleri](#webhook-samples).
 
<a name="webhook-samples"></a>
### <a name="microsoft-graph-webhook-samples"></a>Microsoft Graph Web kancası örnekleri

#### <a name="sample-creating-a-subscription"></a>Örnek: bir abonelik oluşturma

Bir HTTP tetikleyicisi bir abonelik çıktı oluşturma eylemini kullanarak bağlama tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# Örnek arama kullanıcı Outlook iletisi aldığında, bu işlev uygulaması uyarır bir Web kancası kaydeder:

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

Aşağıdaki JS örnek arama kullanıcı Outlook iletisi aldığında, bu işlev uygulaması uyarır bir Web kancası kaydeder:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

#### <a name="sample-handling-notifications"></a>Örnek: bildirimleri işleme

Outlook iletileri işlemek için grafik Web kancası tetikleyici tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# örnek gelen posta iletilerine yanıt verir ve bu alıcı tarafından gönderilen ve konu "Azure işlevleri" içeren gövde kaydeder:

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

Aşağıdaki JS örnek gelen posta iletilerine yanıt verir ve bu alıcı tarafından gönderilen ve konu "Azure işlevleri" içeren gövde günlüğe kaydeder:

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

#### <a name="sample-deleting-subscriptions"></a>Örnek: abonelikler siliniyor

Bir HTTP tetikleyicisi bir abonelik giriş bağlama ve silme eylemini kullanarak bağlama abonelik çıkış ile tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# örnek arayan kullanıcı için tüm abonelikleri alır ve bunları siler:

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

Aşağıdaki JS örnek arayan kullanıcı için tüm abonelikleri alır ve bunları siler:

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

#### <a name="sample-refreshing-subscriptions"></a>Örnek: Abonelikleri yenileme

Abonelikleri yenilemek için iki yaklaşım vardır:
- Tüm abonelikleri ile mücadele etmek için uygulama kimliği kullanın. Bu bir Azure Active Directory Yöneticisi'nden izni gerektirir Azure işlevleri tarafından desteklenen tüm dillerde tarafından kullanılabilir.
- Her kullanıcı kimliğini el ile bağlayarak her abonelikle ilişkili kimliğini kullan Bu bağlama gerçekleştirmek için özel kod gerektirir. Bu, yalnızca .NET işlevleri tarafından kullanılabilir.

Her iki seçenek aşağıda verilmiştir.

**Uygulama Kimliği kullanma**

Bağlama ve uygulama kimliğini kullanarak bir abonelik giriş bağlaması abonelik çıkış ile Zamanlayıcı tetikleyicisi tanımlar aşağıdaki function.json olduğunu varsayalım:

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

Aşağıdaki C# örnek uygulama kimliğini kullanarak bir süreölçeri, Aboneliklerde yenilenir:

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

Aşağıdaki JS örnek uygulama kimliğini kullanarak bir süreölçeri, Aboneliklerde yenilenir:

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

**Dinamik kullanıcı kimlikleri kullanma**

Bir zamanlayıcı tetikleyicisi tanımlar aşağıdaki function.json olduğunu varsayalım alternatif seçeneği için abonelik bağlama ertelemeyi işlev kodu bağlama giriş:

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

Aşağıdaki C# örnek kodda çıkış bağlama oluşturarak her kullanıcının kimliğini kullanarak bir süreölçer Aboneliklerde yenilenir:
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



[HTTP tetikleyicisini]: functions-bindings-http-webhook.md
[kancalarını Microsoft Graph ile çalışma]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
