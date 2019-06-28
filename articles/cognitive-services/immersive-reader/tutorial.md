---
title: 'Öğretici: Tam Ekran Okuyucu’yu Başlatma (Node.js)'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, sürükleyici okuyucu başlatan bir Node.js uygulaması oluşturacaksınız.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: ac90496c950d8a563bf8794b4c1bb105b6c12232
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444075"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Öğretici: Tam Ekran Okuyucu’yu Başlatma (Node.js)

İçinde [genel bakış](./overview.md), sürükleyici okuyucu ne olduğu hakkında bilgi edindiniz ve nasıl language learners, Gelişmekte olan okuyucular ve farkları öğrenme ile Öğrenciler için okuma kavramayı geliştirmek için kendini kanıtlamış teknikleri uygular. Bu öğretici, sürükleyici okuyucu başlatan bir Node.js web uygulaması oluşturma işlemini kapsar. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Express ile Node.js web uygulaması oluşturma
> * Erişim belirteci alma
> * Örnek içerik sürükleyici okuyucu başlatın
> * İçeriğinizi dili belirtin
> * Derinlikli okuyucu arabirimi dilini belirtin
> * Matematik içerikle sürükleyici okuyucu başlatın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bir abonelik anahtarı sürükleyici okuyucu. İzleyerek bir tane alın [bu yönergeleri](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
* [Node.js](https://nodejs.org/) ve [Yarn](https://yarnpkg.com)
* Bir IDE gibi [Visual Studio kodu](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Express ile Node.js web uygulaması oluşturma

Bir Node.js web uygulaması oluşturma `express-generator` aracı.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

Yarn bağımlılıkları yükler ve bağımlılıkları ekleyin `request` ve `dotenv`, doğrulayacak öğreticinin ilerleyen bölümlerinde.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-access-token"></a>Erişim belirteci alma

Ardından, bir arka uç API abonelik anahtarınızı kullanarak bir erişim belirteci almak için yazın. Bu sonraki adım için abonelik anahtarınızı ve uç nokta gerekir. Abonelik anahtarınızı sürükleyici okuyucu kaynağınızın Azure portalındaki anahtarlar sayfasında bulabilirsiniz. Uç noktanız genel bakış sayfasında bulabilirsiniz.

Abonelik anahtarını ve uç nokta oluşturduktan sonra adlı yeni bir dosya oluşturun _.env_, aşağıdaki kod, değiştirme yapıştırın `{YOUR_SUBSCRIPTION_KEY}` ve `{YOUR_ENDPOINT}` abonelik anahtarını ve uç nokta, sırasıyla.

```text
SUBSCRIPTION_KEY={YOUR_SUBSCRIPTION_KEY}
ENDPOINT={YOUR_ENDPOINT}
```

Genel yapılmalıdır olmayan gizli diziler içerdiğinden bu dosyayı kaynak denetimine işleme değil emin olun.

Ardından, açık _app.js_ ve dosyanın üst kısmına aşağıdakileri ekleyin. Bu abonelik anahtarını ve uç nokta ortam değişkenleri olarak düğümüne yükler.

```javascript
require('dotenv').config();
```

Açık _routes\index.js_ dosya ve aşağıdaki içeri aktarma dosyasının en üstüne:

```javascript
var request = require('request');
```

Ardından, doğrudan bu satırın altına aşağıdaki kodu ekleyin. Bu kod, abonelik anahtarınızı kullanarak bir erişim belirteci alır ve sonra bu belirteci döndüren bir API uç noktası oluşturur.

```javascript
router.get('/token', function(req, res, next) {
  request.post({
    headers: {
        'Ocp-Apim-Subscription-Key': process.env.SUBSCRIPTION_KEY,
        'content-type': 'application/x-www-form-urlencoded'
    },
    url: process.env.ENDPOINT
  },
  function(err, resp, token) {
    return res.send(token);
  });
});
```

Bu API uç noktası çeşit kimlik doğrulaması korunması (örneğin, [OAuth](https://oauth.net/2/)); Bu öğreticinin kapsamı dışındadır eserleridir.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerik sürükleyici okuyucu başlatın

1. Açık _views\layout.pug_ve altında aşağıdaki kodu ekleyin `head` önce etiket `body` etiketi. Bunlar `script` etiketleri yük [sürükleyici okuyucu SDK](https://github.com/Microsoft/immersive-reader-sdk) ve jQuery.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. Açık _views\index.pug_ve içeriğini aşağıdaki kodla değiştirin. Bu kod, bazı örnek içerikle sayfayı doldurur ve sürükleyici okuyucu başlatan bir düğme ekler.

    ```pug
    extends layout

    block content
      h2(id='title') Geography
      p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
      div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
      script.
        function launchImmersiveReader() {
          // First, get a token using our /token endpoint
          $.ajax('/token', { success: token => {
            // Second, grab the content from the page
            const content = {
              title: document.getElementById('title').innerText,
              chunks: [ {
                content: document.getElementById('content').innerText + '\n\n',
                lang: 'en'
              } ]
            };

            // Third, launch the Immersive Reader
            ImmersiveReader.launchAsync(token, content);
          }});
        }
    ```

3. Web uygulamamızı artık hazırdır. Uygulamayı çalıştırarak başlatın:

    ```bash
    npm start
    ```

4. Tarayıcınızı açın ve gidin _http://localhost:3000_ . Yukarıdaki içeriğe sayfada görmeniz gerekir. Tıklayın **sürükleyici okuyucu** sürükleyici okuyucu ile içeriğinizi başlatmak için düğme.

## <a name="specify-the-language-of-your-content"></a>İçeriğinizi dili belirtin

Derinlikli okuyucu birçok farklı diller için destek sunar. İçeriğinizi dili, aşağıdaki adımları izleyerek belirtebilirsiniz.

1. Açık _views\index.pug_ ve aşağıdaki kodu ekleyin `p(id=content)` önceki adımda eklediğiniz etiket. Bu kod, bazı içeriklerin İspanyolca içerik sayfanıza ekler.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. Aşağıdaki çağrı yukarıda JavaScript kodunu ekleyin `ImmersiveReader.launchAsync`. Bu kod, sürükleyici okuyucuya İspanyolca içeriği geçirir.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Gidin _http://localhost:3000_ yeniden. İspanyolca metin sayfasında ve tıkladığınızda görmelisiniz **sürükleyici okuyucu**, sürükleyici okuyucusunda gösterilir.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Derinlikli okuyucu arabirimi dilini belirtin

Varsayılan olarak, derinlikli okuyucu arabirimi dilini tarayıcının dili ayarları eşleşir. Aşağıdaki kod ile derinlikli okuyucu arabirimi dili de belirtebilirsiniz.

1. İçinde _views\index.pug_, çağrısını `ImmersiveReader.launchAsync(token, content)` aşağıdaki kod ile.

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, content, options);
    ```

2. Gidin _http://localhost:3000_ . Derinlikli okuyucu başlattığında, Fransızca arabirimi gösterilir.

## <a name="launch-the-immersive-reader-with-math-content"></a>Matematik içerikle sürükleyici okuyucu başlatın

Matematik içeriği kullanarak derinlikli okuyucusunda dahil edebilirsiniz [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML).

1. Değiştirme _views\index.pug_ çağrısı üstüne aşağıdaki kodu içerecek şekilde `ImmersiveReader.launchAsync`:

    ```javascript
    const mathML = '<math xmlns="https://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. Gidin _http://localhost:3000_ . Derinlikli okuyucu ve En Alta kadar kaydır başlattığında, matematik formül görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedin [sürükleyici okuyucu SDK](https://github.com/Microsoft/immersive-reader-sdk) ve [sürükleyici okuyucu SDK başvurusu](./reference.md)
* Kod örnekleri görüntüle [GitHub](https://github.com/microsoft/immersive-reader-sdk/samples/advanced-csharp)