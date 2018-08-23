---
title: Node.js Hızlı Başlangıç için Azure Bilişsel hizmetler, Microsoft Translator konuşma tanıma API'si | Microsoft Docs
description: Microsoft Translator konuşma tanıma API'si, Azure üzerinde Microsoft Bilişsel hizmetler kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: e652ee1e201e60d0d75f10bb8fceb4fbcd205381
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "41987500"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-nodejs"></a>Microsoft Translator konuşma tanıma API'si ile Node.js için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede, Microsoft Translator konuşma tanıma API'si .wav dosyasında konuşulan kelimeleri için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Node.js 6](https://nodejs.org/en/download/) bu kodu çalıştırmak için.

Yüklemeniz gereken [Websocket paket](https://www.npmjs.com/package/websocket) Node.js için.

Aşağıdaki kod derleme yürütülebilir dosya ile aynı klasörde yer alan "speak.wav" adlı bir .wav dosyası gerekir. Bu .wav dosya standart PCM 16 bit, 16 kHz, mono biçiminde olmalıdır. Böyle bir .wav dosyasından edinebilirsiniz [metin okuma API'si](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Microsoft Translator konuşma tanıma API'si**. Ücretli aboneliğe anahtarından gerekir, [Azure panosuna](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Konuşmayı çevirme

Aşağıdaki kod, konuşma bir dilden diğerine çevirir.

1. Sık kullandığınız IDE'de yeni bir Node.js projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

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
You can obtain such a .wav file using the Text to Speech API. See:
https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech
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
This lets the service know that the audio file is finished.
At 32 bytes per millisecond, this is 100 seconds of silence. */
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

Başarılı "speak2.wav" adlı bir dosya oluşturulmasını oluşur. Dosya çevirisi "speak.wav içinde" konuşulan sözcükleri içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator Konuşma Öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Translator konuşma genel bakış](../overview.md)
[API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
