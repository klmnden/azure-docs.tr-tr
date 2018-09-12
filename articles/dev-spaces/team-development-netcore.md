---
title: .NET Core ve VS Code kullanarak Azure Dev Spaces ile ekip geliştirmesi | Microsoft Docs
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
ms.openlocfilehash: 602e2a691dfa150c2e8332cb6dca070dbdd57901
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162096"
---
# <a name="team-development-with-azure-dev-spaces"></a>Azure Dev Spaces ile Ekip Geliştirmesi

Bu öğreticide, farklı geliştirme ortamlarında aynı anda çalışmak için birden çok geliştirme alanı kullanarak aynı kümede ayrı işleri ayrı geliştirme alanlarında tutmayı öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![Birden çok kapsayıcı](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* için örnek kod indirme
Zamandan kazanmak adına örnek kodu bir GitHub deposundan indirelim. https://github.com/Azure/dev-spaces adresine gidip **Kopyala veya İndir**’i seçerek GitHub deposunu indirin. Bu bölümün kodu `samples/dotnetcore/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde* `mywebapi` klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. VS Code hata ayıklama çubuğu görüntülendiğinde hazır olduğunu anlarsınız.
1. Uç nokta URL'si http://localhost:\<portnumber\> benzeri bir URL olacaktır. **İpucu: VS Code durum çubuğunda tıklanabilir bir URL görüntülenir.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamımızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. Varsayılan `ValuesController` GET API’sini çağırmak için URL’ye `/api/values` ekleyin. 
1. Tüm adımları başarılı olduysa, `mywebapi` hizmetinden bir yanıt alındığını görebilmelisiniz.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. About yöntemi için kodu *değiştirin*:

    ```csharp
    public async Task<IActionResult> About()
    {
        ViewData["Message"] = "Hello from webfrontend";
        
        using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

        return View();
    }
    ```

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Daha sonra bunun takımlara işbirliğine dayalı geliştirme açısından nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. `api/values/{id}` GET isteklerini işleyen `Get(int id)` yönteminde bir kesme noktası ayarlayın.
1. `webfrontend` projesinde, `mywebapi/api/values` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını çağırın ve her iki hizmette de kodun üzerinden geçin.
1. Web uygulamasında Hakkında sayfası iki hizmet tarafından birleştirilmiş bir ileti görüntüler: "Hello from webfrontend and Hello from mywebapi."


Bravo! Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

[!INCLUDE [](includes/team-development-1.md)]

Şimdi nasıl çalıştığını görelim. `mywebapi` için VS Code penceresine gidin ve `string Get(int id)` yönteminde bir kod düzenlemesi yapın; örneğin:

```csharp
[HttpGet("{id}")]
public string Get(int id)
{
    return "mywebapi now says something new";
}
```


[!INCLUDE [](includes/team-development-2.md)]

[!INCLUDE [](includes/well-done.md)]

[!INCLUDE [](includes/clean-up.md)]
