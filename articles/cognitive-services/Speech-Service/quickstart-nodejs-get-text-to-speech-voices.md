---
title: 'Hızlı Başlangıç: Metin okuma sesleri, Node.js - konuşma hizmetlerinin listesi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir bölge/Node.js kullanarak uç noktası için standart ve sinir sesleri tam listesini almak nasıl öğreneceksiniz. Liste, JSON olarak döndürülür ve ses kullanılabilirlik bölgeye göre değişir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/02/2019
ms.author: erhopf
ms.openlocfilehash: 08936ca0fe2fe10c332df146edd541c75df325e0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067571"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak metin okuma seslerini'nın listesini alın

Bu hızlı başlangıçta, bir bölge/Node.js kullanarak uç noktası için standart ve sinir sesleri tam listesini almak nasıl öğreneceksiniz. Liste, JSON olarak döndürülür ve ses kullanılabilirlik bölgeye göre değişir. Desteklenen bölgelerin bir listesi için bkz. [bölgeleri](regions.md).

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma Hizmetleri kaynağa sahip. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma Hizmetleri için bir Azure aboneliği anahtarı. [Ücretsiz edinin! ](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirir

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Ardından bu kod parçacığını projenizde `get-voices.js` adlı bir dosyaya kopyalayın.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write the list of languages to a file
const fs = require('fs');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request request-promise`.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metin okuma REST API, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu işlevi kullanarak bir erişim belirteci için konuşma Hizmetleri abonelik anahtarınızı birbiriyle değiştirir `issueToken` uç noktası.

Bu örnek, konuşma Hizmetleri aboneliğinize Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `uri`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu projenize kopyalayın:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteci ile kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

Sonraki bölümde, işlev sesleri listesini almak ve JSON çıkışını dosyaya kaydet oluşturacağız.

## <a name="make-a-request-and-save-the-response"></a>Bir istekte bulunmak ve yanıt Kaydet

Burada, derleme isteği ve döndürülen seslerle listesine kaydetmek için yedekleyeceksiniz. Bu örnek, Batı ABD uç nokta kullanmakta olduğunuz varsayılır. Kaynağınız için farklı bir bölgede kayıtlı değilse, güncelleştirdiğinizden emin olun `uri`. Daha fazla bilgi için [konuşma Hizmetleri bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Ardından, istek için gerekli üst bilgileri ekleyin. Son olarak, hizmete istek yapacaksınız. İstek başarılı olur ve 200 durum kodu döndürülmesine ise yanıt dosyasına yazılır.

```javascript
function textToSpeech(accessToken) {
    let options = {
        method: 'GET',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/voices/list',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json'
        }
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('voices.json'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, bir zaman uyumsuz işlev oluşturmaktır. Bu işlev abonelik anahtarınız bir ortam değişkenini okumak, bir belirteç almak, istek için bekleyin ve ardından dosya JSON yanıtı yazmak.

Ortam değişkenleriyle bilginiz veya, bir abonelik anahtarı kodlanmış bir dize olarak test etmek tercih ederseniz değiştirin `process.env.SPEECH_SERVICE_KEY` abonelik anahtarınızı dize olarak.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
node get-voices.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da node.js örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
