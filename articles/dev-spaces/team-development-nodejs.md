---
title: VS Code kullanarak Azure Dev Spaces ile ekip geliştirmesi | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: b4c355c864f83bcd76c310fecb0f26dd3372e760
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162759"
---
# <a name="team-development-with-azure-dev-spaces"></a>Azure Dev Spaces ile ekip geliştirmesi

Bu öğreticide, farklı geliştirme ortamlarında aynı anda çalışmak için birden çok geliştirme alanı kullanarak aynı kümede ayrı işleri ayrı geliştirme alanlarında tutmayı öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>*mywebapi* için örnek kodu açma
Bu kılavuz için `samples` adlı klasörün altında zaten `mywebapi` örnek kodunuz olmalıdır (yoksa, https://github.com/Azure/dev-spaces adresine gidin ve **Kopyala veya İndir**'i seçerek GitHub deposunu indirin.) Bu bölümün kodu `samples/nodejs/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde* `mywebapi` klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. VS Code hata ayıklama çubuğu görüntülendiğinde hazır olduğunu anlarsınız.
1. Uç nokta URL'sini not alın; http://localhost:\<portnumber\> benzeri bir URL olacaktır. **İpucu: VS Code durum çubuğunda tıklanabilir bir URL görüntülenir.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamınızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. `mywebapi` hizmetinden bir yanıt görmeniz gerekir ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. Şu kod satırlarını `server.js` dosyasının en üstüne ekleyin:
    ```javascript
    var request = require('request');
    ```

3. `/api` GET işleyicisinin kodunu *değiştirin*. İsteği işlerken, o da `mywebapi` hizmetine bir çağrı yapar ve her iki hizmetten de sonuçları döndürür.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Daha sonra bunun takımlara işbirliğine dayalı geliştirme açısından nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. Varsayılan GET `/` işleyicisinde bir kesme noktası ayarlayın.
1. `webfrontend` projesinde, `http://mywebapi` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını açın ve her iki hizmette de kodun üzerinden geçin. Web uygulamasında iki hizmet tarafından birleştirilmiş bir ileti görüntüleniyor olmalıdır: "Hello from webfrontend and Hello from mywebapi."

Bravo! Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

[!INCLUDE [](includes/team-development-1.md)]

Şimdi nasıl çalıştığını görün:
1. `mywebapi` için VS Code penceresine gidin ve varsayılan GET `/` işleyicisinde bir kod düzenlemesi yapın; örneğin:

    ```javascript
    app.get('/', function (req, res) {
        res.send('mywebapi now says something new');
    });
    ```

[!INCLUDE [](includes/team-development-2.md)]

[!INCLUDE [](includes/well-done.md)]

[!INCLUDE [](includes/clean-up.md)]




