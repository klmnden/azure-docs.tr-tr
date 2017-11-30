---
title: "SendGrid Azure işlevlerini kullanma | Microsoft Docs"
description: "Azure işlevleri SendGrid kullanmayı gösterir"
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: glenga
ms.openlocfilehash: 444e06ff24ea7f909a24d482aba26f890040d980
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a>SendGrid Azure işlevlerini kullanma

## <a name="sendgrid-overview"></a>SendGrid genel bakış

Azure işlevleri destekler SendGrid birkaç satır kod ve SendGrid hesabı ile e-posta iletileri göndermek işlevlerinizi etkinleştirmek için bağlamaları çıktı.

SendGrid API bir Azure işlevi kullanmak için olmalıdır bir [SendGrid hesap](http://SendGrid.com). Ayrıca, bir SendGrid API anahtarı olması gerekir. SendGrid hesabınızda oturum açın ve'ı tıklatın **ayarları** sonra **API anahtarı** bir API anahtarı oluşturmak için. Yaklaşan bir adımda kullanırken kullanılabilir bu anahtarı tutun.

Şimdi bir Azure işlevi uygulama oluşturmaya hazırsınız.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma 

Azure işlevi uygulamaları bir veya daha fazla Azure işlevleri için kapsayıcı görevi görür. Azure işlevleri yalnızca - bir işlevi olan. Her Azure işlevi çalıştırmak işlevinin neden olan bir olay bir tetikleyici bağlıdır.
Her işlevi, giriş herhangi bir sayıda içermiyor ya da bağlamaları çıktı. Bağlamaları bir işlevde kullanabileceğiniz hizmetleridir. SendGrid e-posta göndermek için kullanabileceğiniz bir çıktı bağlaması var. 

1. Azure portalında oturum açın ve [Azure işlev uygulaması oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) veya varolan bir işlev uygulaması açın. 
2. Azure bir işlev oluşturun. Basit tutmak için bir el ile tetikleyici ve C# ' ı seçin. 

 ![Bir Azure işlevi oluşturma](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>SendGrid bir Azure işlevi uygulamada kullanmak için yapılandırma

SendGrid API anahtarınıza bir işlevde kullanmak üzere bir uygulama ayarı olarak depolamanız gerekir. Apikey ile yapılan alan gerçek SendGrid API anahtarınıza değildir, ancak bir uygulama ayarı gerçek API anahtarınıza temsil eden tanımlayın. Herhangi bir kod veya kaynak kod denetimine iade dosyalarını ayrı olduğundan bu şekilde anahtarınızın depolanması güvenlik için kalır.

- Oluşturma bir **AppSettings** işlevi uygulamanızın anahtar **uygulama ayarları**.

 ![Bir Azure işlevi oluşturma](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>SendGrid çıkış bağlamaları yapılandırma

SendGrid Azure kullanılabilir işlevi çıktı bağlama. Çıkış bağlama bir SendGrid oluşturmak için:

1. Git **tümleştir** Azure portalında işlevinin sekmesi.
2. Tıklatın **yeni çıktı** bir SendGrid oluşturmak için bağlama çıktı.
3. Doldurmak **API anahtarı** ve **ileti parametre adı** özellikleri. İsterseniz, diğer özellikler şimdi girin veya bunun yerine kod. Bu ayarlar varsayılan olarak kullanılabilir.

 ![SendGrid çıkış bağlamaları yapılandırma](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Bir işlev bağlama ekleme adlı bir dosya oluşturur **function.json** , işlevin klasöründeki. Bu dosya aynı Azure işlev içinde gördüğünüz bilgileri içerir **tümleştir** sekmesinde ancak JSON'de biçimlendirin. Ayarı **apikey ile yapılan**, **ileti**, ve **gelen** alanları aşağıdaki girişleri oluşturmak **function.json** dosyası: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Tercih ederseniz, bu değiştirebilen kendiniz dosyasını doğrudan.

Oluşturulan ve işlev ve işlev uygulaması yapılandırılmış göre bir e-posta göndermek üzere kod yazabilirsiniz.

## <a name="write-code-that-creates-and-sends-email"></a>Oluşturur ve e-posta gönderir. kod yazma

SendGrid API oluşturmak ve bir e-posta göndermek için gereken tüm komutları içerir.  

- İşlev kodu aşağıdaki kodla değiştirin:

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change to email of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

İlk satır içerir fark ```#r``` SendGrid bütünleştirilmiş koduna başvuruyor yönergesi. Bundan sonra kullanabileceğiniz bir ```using``` deyimi daha kolay erişim bu ad alanındaki nesneleri. Kod içinde örnekleri oluşturma ```Mail```, ```Personalization```, ve ```Content``` e-posta oluştur nesneleri SendGrid API'sinden. SendGrid ileti geri döndüğünüzde sağlar. 

İşlev imzası ayrıca fazladan çıkış parametresi türü içeren ```Mail``` adlı ```message```. Her ikisi de giriş ve bağlamaları express kendilerini kod işlevi parametre olarak çıkış. 

2. Tıklayarak kodunuzu test **Test** ve iletiye girerek **istek gövdesinde** tıklatarak alan **çalıştırmak** düğmesi.

 ![Kodunuzu test](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. SendGrid e-posta gönderdiğini doğrulamak için e-posta denetleyin. Adım 1'den kodda adresine gidin ve iletiden içermesi **istek gövdesinde**.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede SendGrid hizmet oluşturmak ve e-posta göndermek için nasıl kullanılacağı gösterilmiştir. Uygulamalarınızda Azure İşlevleri’ni kullanma hakkında daha fazla bilgi almak için aşağıdaki konulara bakın: 

- [En iyi uygulamalar için Azure işlevleri](functions-best-practices.md) Azure işlevleri oluşturulurken kullanılacak bazı en iyi uygulamaları listeler.

- [Azure işlevleri Geliştirici Başvurusu](functions-reference.md) işlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için Programcı Başvurusu.

- [Azure işlevlerini test etme](functions-test-a-function.md) çeşitli araçları ve teknikleri işlevlerinizi test etmek için açıklar.