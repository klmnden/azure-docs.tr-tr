---
title: "Azure işlevleri SendGrid bağlamaları | Microsoft Docs"
description: "Azure işlevleri SendGrid bağlamaları başvurusu"
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: rachelap
ms.openlocfilehash: 4cdafbe05e29d8b483c6b0e1daf41a36583d7b5e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure işlevleri SendGrid bağlamaları

Bu makalede, yapılandırma ve Azure işlevlerinde SendGrid bağlamaları çalışmak açıklanmaktadır. SendGrid program aracılığıyla özelleştirilmiş e-posta göndermek için Azure işlevleri kullanabilirsiniz.

Bu makalede, Azure işlevleri geliştiricileri için başvuru bilgilerdir. Azure işlevleri yeniyseniz, aşağıdaki kaynaklarla başlatın:

[İlk Azure işlevinizi oluşturma](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), veya [düğümü](functions-reference-node.md) Geliştirici başvuruları.

## <a name="functionjson-for-sendgrid-bindings"></a>SendGrid bağlamaları için Function.JSON

Azure işlevleri için SendGrid bir çıktı bağlama sağlar. Etkinleştirir bağlama SendGrid çıktı oluşturmak ve göndermek için program aracılığıyla e-posta. 

SendGrid bağlama aşağıdaki özellikleri destekler:

|Özellik  |Açıklama  |
|---------|---------|
|**adı**| Gerekli - istek veya istek gövdesi için işlevi kod içinde kullanılan değişken adı. Bu değer ```$return``` yalnızca bir dönüş değeri olduğunda. |
|**türü**| Gerekli - kümesine olmalıdır `sendGrid`.|
|**yönü**| Gerekli - kümesine olmalıdır `out`.|
|**apikey ile yapılan**| Gerekli - API anahtarınıza işlevi uygulamanın uygulama ayarlarında depolanan adına ayarlanması gerekir. |
|**Hedef**| Alıcının e-posta adresi. |
|**Kaynak**| Gönderenin e-posta adresi. |
|**Konu**| e-postanın konusu. |
|**metin**| e-posta içeriği. |

Örnek **function.json**:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> Azure işlevleri depolar bağlantı bilgilerini ve API anahtarlarını uygulama ayarlarda böylece kaynak denetimi deponuzun denetlenmez. Bu eylem, hassas bilgilerinizi koruma sağlar.
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a>C# SendGrid örneği bağlama çıktı

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-the-sendgrid-output-binding"></a>Bağlama SendGrid düğümü örnek çıkış

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a>Sonraki adımlar
Azure işlevleri için diğer bağlamalar ve tetikleyicileri hakkında bilgi için bkz 
- [Azure işlevleri Tetikleyicileri ve bağlamaları Geliştirici Başvurusu](functions-triggers-bindings.md)

- [En iyi uygulamalar için Azure işlevleri](functions-best-practices.md) Azure işlevleri oluşturulurken kullanılacak bazı en iyi uygulamaları listeler.

- [Azure işlevleri Geliştirici Başvurusu](functions-reference.md) işlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için Programcı Başvurusu.
