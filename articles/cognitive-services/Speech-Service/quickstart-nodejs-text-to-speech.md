---
title: 'Hızlı Başlangıç: Metin okuma, Node.js - konuşma Hizmetleri Dönüştür'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Node.js ve metin okuma REST API'sini kullanarak. Bu kılavuzda yer örnek metni konuşma sentezi işaretleme dili (SSML'yi) olarak yapılandırılmıştır. Bu, ses ve konuşma yanıtın dili seçmenize olanak sağlar.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 8cc676355f432a25550b337e5de747c4956d3142
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358890"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Hızlı Başlangıç: Dönüştürme metin okuma Node.js kullanma

Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Node.js ve metin okuma REST API'sini kullanarak. Bu kılavuzdaki istek gövdesi olarak yapılandırılmış [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md), ses ve yanıtın dili seçmenize olanak tanıyan.

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma hizmeti kaynağı olan. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin! ](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirir

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.js` adlı bir dosyaya kopyalayın.

```javascript
// Requires request for HTTP requests
const request = require('request');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request readline-sync`.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarı ve bir komut istemi için TTS oluşturma

Sonraki birkaç bölümde yetkilendirmeyi ele alırken, metin okuma API'si çağırmayı ve yanıt doğrulamak için işlevleri oluşturacaksınız. Bir abonelik anahtarı ekleyerek ve metin girişi için bir istem oluşturmadan başlayalım tıklatın.

```javascript
/*
 * These lines will attempt to read your subscription key from an environment
 * variable. If you prefer to hardcode the subscription key for ease of use,
 * replace process.env.SUBSCRIPTION_KEY with your subscription key as a string.  
 */
const subscriptionKey = process.env.SUBSCRIPTION_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};

// Prompts the user to input text.
let text = readline.question('What would you like to convert to speech? ');
```

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metin okuma REST API, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu örnek için bir erişim belirteci kullanarak konuşma tanıma hizmeti abonelik anahtarınızı birbiriyle değiştirir `issueToken` uç noktası.

Bu işlev, iki bağımsız değişkeni, konuşma Hizmetleri abonelik anahtarınız ve bir geri çağırma işlevini alır. İşlev alınan bir erişim belirteci sonra değeri geri çağırma işlevine geçirir. Metin okuma API'si çağrısı ve Sentezlenen konuşma yanıt kaydetmek için işlev sonraki bölümde oluşturacağız.

Bu örnek, Batı ABD bölgesinde konuşma hizmeti aboneliğiniz olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `uri`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu projenize kopyalayın:

```javascript
function textToSpeech(subscriptionKey, saveAudio) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    };
    // This function retrieve the access token and is passed as callback
    // to request below.
    function getToken(error, response, body) {
        console.log("Getting your token...\n")
        if (!error && response.statusCode == 200) {
            //This is the callback to our saveAudio function.
            // It takes a single argument, which is the returned accessToken.
            saveAudio(body)
        }
        else {
          throw new Error(error);
        }
    }
    request(options, getToken)
}
```

> [!NOTE]
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteci ile kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Bir istekte bulunmak ve yanıt Kaydet

Burada, metin okuma API'si isteği oluşturun ve konuşma yanıt kaydetmek için yedekleyeceksiniz. Bu örnek, Batı ABD uç nokta kullanmakta olduğunuz varsayılır. Kaynağınız için farklı bir bölgede kayıtlı değilse, güncelleştirdiğinizden emin olun `uri`. Daha fazla bilgi için [konuşma hizmet bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Ardından, istek için gerekli üst bilgileri eklemeniz gerekir. Güncelleştirdiğinizden emin olun `User-Agent` kaynak (Azure Portalı'nda bulunur) ve küme adıyla `X-Microsoft-OutputFormat` tercih edilen ses çıkış için. Çıkış biçimleri tam bir listesi için bkz. [ses çıkarır](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#audio-outputs).

Ardından, istek gövdesi konuşma sentezi işaretleme dili (SSML'yi) kullanarak oluşturun. Bu örnek, yapısını tanımlar ve kullandığı `text` daha önce oluşturduğunuz giriş.

>[!NOTE]
> Bu örnekte `JessaRUS` ses tipi. Ses/dillerde Microsoft tam bir listesi sağlanmaktadır için bkz: [dil desteği](language-support.md).
> Markanız için benzersiz, tanınan bir ses oluşturmak istiyorsanız bkz [özel ses tipi oluşturma](how-to-customize-voice-font.md).

Son olarak, hizmete istek yapacaksınız. İstek başarılı ise ve 200 durum kodu döndürülür, konuşma yanıt olarak yazılan `sample.wav`.

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function saveAudio(accessToken) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
      .att('version', '1.0')
      .att('xml:lang', 'en-us')
      .ele('voice')
      .att('xml:lang', 'en-us')
      .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
      .txt(text)
      .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    };
    // This function makes the request to convert speech to text.
    // The speech is returned as the response.
    function convertText(error, response, body){
      if (!error && response.statusCode == 200) {
        console.log("Converting text-to-speech. Please hold...\n")
      }
      else {
        throw new Error(error);
      }
      console.log("Your file is ready.\n")
    }
    // Pipe the response to file.
    request(options, convertText).pipe(fs.createWriteStream('sample.wav'));
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım çağırmaktır `textToSpeech` işlevi.

```javascript
// Start the sample app.
textToSpeech(subscriptionKey, saveAudio);
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar metin okuma örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
node tts.js
```

İstendiğinde, ne olursa olsun metin okuma dönüştürmek istediğiniz içinde yazın. Başarılı olursa, konuşma dosyanın proje klasöründe bulunur. Yürütün, sık kullanılan media player kullanarak.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da node.js örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
