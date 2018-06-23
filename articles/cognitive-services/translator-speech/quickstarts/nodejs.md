---
title: Bilişsel hizmetler Azure, Microsoft Çeviricisi konuşma API için node.js hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Microsoft Çeviricisi konuşma API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: d469fa008ba8acaf505fa09596dd739d5cc7744c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352271"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-nodejs"></a>Microsoft Çeviricisi konuşma API'si Node.js ile için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede Microsoft Çeviricisi konuşma API .wav dosyasında konuşulan kelimeleri için nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Node.js 6](https://nodejs.org/en/download/) bu kodu çalıştırmak için.

Yüklemenize gerek [Websocket paket](https://www.npmjs.com/package/websocket) Node.js için.

Aşağıdaki kod derleme yürütülebilir dosya ile aynı klasörde "speak.wav" adlı bir .wav dosyası gerekir. Bu .wav dosyası standart PCM 16 bit, 16 kHz mono biçiminde olmalıdır. Bu tür bir .wav dosyasından edinebilirsiniz [Çeviricisi metin seslendir API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Sahip olmanız gerekir bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Microsoft Çeviricisi konuşma API**. Ücretli abonelik anahtarından gerekir, [Azure Pano](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Konuşma Çevir

Aşağıdaki kod, bir dilden diğerine konuşma çevirir.

1. Sık kullanılan IDE içinde yeni bir Node.js projesi oluşturun.
2. Aşağıda sunulan kodu ekleyin.
3. Değiştir `key` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
4. Programını çalıştırın.

```nodejs
/* To install this dependency, run:
npm install websocket
*/
var wsClient = require('websocket').client;
var fs = require('fs');
/* To install this dependency, run:
npm install stream-buffers
*/
var streamBuffers = require('stream-buffers');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let key = 'ENTER KEY HERE';

let host = 'wss://dev.microsofttranslator.com';
let path = '/speech/translate';
let params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa';
let uri = host + path + params;

/* The input .wav file is in PCM 16bit, 16kHz, mono format.
You can obtain such a .wav file using the Translator Text Speak API. See:
http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak
*/
let input_path = 'speak.wav';

let output_path = 'speak2.wav';

function receive(message) {
    console.log ("Received message of type: " + message.type + ".");

    if (message.type == 'utf8') {
        var result = JSON.parse(message.utf8Data)
        console.log("Response type: " + result.type + ".");
        console.log("Recognized input as: " + result.recognition);
        console.log("Translation: " + result.translation);
    }
    else if (message.type == 'binary') {
/* This is needed to make sure message.binaryData is defined. See:
https://stackoverflow.com/questions/17546953/cant-access-object-property-even-though-it-exists-returns-undefined
*/
        let binary_data = JSON.parse (JSON.stringify (message.binaryData));

        if (binary_data.type == 'Buffer') {
/* Put the binary data in a Buffer - do not write it directly to the file. */
            let buffer = new Buffer(binary_data.data, 'binary');
/* Write the contents of the Buffer to the file. */
            fs.writeFile (output_path, buffer, 'binary', function (error) {
/* fs.writeFile calls the error-handling function even if there is no error, in which case error === null. See:
https://stackoverflow.com/questions/44473868/node-js-fs-writefile-err-returns-null
*/
                if (error !== null) {
                    console.log ("Error: " + error);
                }
            });
        }
    }
}

function send(connection, filename) {
    
    var myReadableStreamBuffer = new streamBuffers.ReadableStreamBuffer({
        frequency: 100,
        chunkSize: 32000
    });

/* Make sure the audio file is followed by silence.
This lets the service know that the audio input is finished. */
    myReadableStreamBuffer.put(fs.readFileSync(filename));
    myReadableStreamBuffer.put(new Buffer(3200000));
    myReadableStreamBuffer.stop();

    myReadableStreamBuffer.on('data', function (data) {
        connection.sendBytes(data);
    });

    myReadableStreamBuffer.on('end', function () {
        console.log('Closing connection.');
        connection.close(1000);
    });
}

function connect() {
    var ws = new wsClient();

    ws.on('connectFailed', function (error) {
        console.log('Connection error: ' + error.toString());
    });
                            
    ws.on('connect', function (connection) {
        console.log('Connected.');

        connection.on('message', receive);
        
        connection.on('close', function (reasonCode, description) {
            console.log('Connection closed: ' + reasonCode);
        });

        connection.on('error', function (error) {
            console.log('Connection error: ' + error.toString());
        });
        
        send(connection, input_path);
    });

    ws.connect(uri, null, null, { 'Ocp-Apim-Subscription-Key' : key });
}

connect();
```

**Konuşma yanıt Çevir**

Başarılı "speak2.wav" adlı bir dosya oluşturma oluşur. Dosya çevirisi "speak.wav içinde" konuşulan sözcüklerin içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çevirici Konuşma Öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Çevirici konuşma genel bakış](../overview.md)
[API Başvurusu](http://docs.microsofttranslator.com/speech-translate.html)
