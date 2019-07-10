---
title: 'Hızlı Başlangıç: Derinlikli okuyucu başlatan bir web uygulaması oluşturma (C#)'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, sıfırdan bir web uygulaması derleme ve sürükleyici okuyucu işlevsellik ekler.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: a4da8d23e78fde9b936bcf9258eec137bcdf9231
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704129"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Hızlı Başlangıç: Derinlikli okuyucu başlatan bir web uygulaması oluşturma (C#)

[Sürükleyici okuyucu](https://www.onenote.com/learningtools) okuma kavramayı geliştirmek için kendini kanıtlamış teknikleri uygulayan aralığında tasarlanmış bir araçtır.

Bu hızlı başlangıçta, sıfırdan bir web uygulaması derleme ve sürükleyici okuyucu SDK'sını kullanarak derinlikli okuyucu tümleştirin. Tam çalışma örnek bu hızlı başlangıç kullanılabilir [burada](https://github.com/microsoft/immersive-reader-sdk/tree/master/samples/quickstart-csharp).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Bir abonelik anahtarı sürükleyici okuyucu. İzleyerek bir tane alın [bu yönergeleri](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

## <a name="create-a-web-app-project"></a>Bir web uygulaması projesi oluşturma

Visual Studio'da yerleşik Model-View-Controller ile ASP.NET Core Web uygulaması şablonunu kullanarak yeni bir proje oluşturun.

![Yeni Proje](./media/vswebapp.png)

![Yeni ASP.NET Core Web uygulaması](./media/vsmvc.png)

## <a name="acquire-an-access-token"></a>Erişim belirteci alma

Bu sonraki adım için abonelik anahtarınızı ve uç nokta gerekir. Abonelik anahtarınızı sürükleyici okuyucu kaynağınızın Azure portalındaki anahtarlar sayfasında bulabilirsiniz. Uç noktanız genel bakış sayfasında bulabilirsiniz.

Projeye sağ tıklayarak _Çözüm Gezgini_ ve **nıcı parolalarını Yönet**. Bu adlı bir dosya açar _secrets.json_. Bu dosyanın içeriğini aşağıdakilerle değiştirin, uygun yerlerde abonelik anahtarını ve uç noktası sağlama.

```json
{
  "SubscriptionKey": YOUR_SUBSCRIPTION_KEY,
  "Endpoint": YOUR_ENDPOINT
}
```

Açık _Controllers\HomeController.cs_ve yerine `HomeController` aşağıdaki kodla sınıfı.

```csharp
public class HomeController : Controller
{
    private readonly string SubscriptionKey;
    private readonly string Endpoint;

    public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
    {
        SubscriptionKey = configuration["SubscriptionKey"];
        Endpoint = configuration["Endpoint"];

        if (string.IsNullOrEmpty(Endpoint) || string.IsNullOrEmpty(SubscriptionKey))
        {
            throw new ArgumentNullException("Endpoint or subscriptionKey is null!");
        }
    }

    public IActionResult Index()
    {
        return View();
    }

    [Route("token")]
    public async Task<string> Token()
    {
        return await GetTokenAsync();
    }

    /// <summary>
    /// Exchange your Azure subscription key for an access token
    /// </summary>
    private async Task<string> GetTokenAsync()
    {
        using (var client = new System.Net.Http.HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
            using (var response = await client.PostAsync(Endpoint, null))
            {
                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
```

## <a name="add-sample-content"></a>Örnek içerik Ekle

Bu web uygulamasını şimdi bazı örnek içerik ekleyeceğiz. Açık _Views\Home\Index.cshtml_ ve bu örnek ile otomatik olarak oluşturulan kodu değiştirin:

```html
<h1 id='title'>Geography</h1>
<span id='content'>
    <p>The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.</p>
</span>

<div class='immersive-reader-button' data-button-style='iconAndText' onclick='launchImmersiveReader()'></div>

@section scripts {
<script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js'></script>
<script type='text/javascript' src='https://code.jquery.com/jquery-3.3.1.min.js'></script>
<script type='text/javascript'>
    function getImmersiveReaderTokenAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/token',
                type: 'GET',
                success: token => {
                    resolve(token);
                }
            });
        });
    }

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        ImmersiveReader.launchAsync(token, content, { uiZIndex: 1000000 });
    }
</script>
}
```

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Menü çubuğundan seçin **hata ayıklama > hata ayıklamayı Başlat**, veya basın **F5** uygulamayı başlatmak için.

Tarayıcınızda görmeniz gerekir:

![Örnek uygulama](./media/quickstart-result.png)

"Sürükleyici okuyucu" düğmesine tıkladığınızda, sürükleyici sayfasında içerikle başlatılan okuyucu görürsünüz.

![Tam Ekran Okuyucu](./media/quickstart-immersive-reader.png)

## <a name="next-steps"></a>Sonraki adımlar

* Görünüm [öğretici](./tutorial.md) sürükleyici okuyucu SDK'sı ile başka neler yapabileceğinizi görmek için
* Keşfedin [sürükleyici okuyucu SDK](https://github.com/Microsoft/immersive-reader-sdk) ve [sürükleyici okuyucu SDK başvurusu](./reference.md)
