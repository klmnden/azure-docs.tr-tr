---
title: Python Hızlı Başlangıç için Azure Bilişsel hizmetler, Microsoft Translator konuşma tanıma API'si | Microsoft Docs
description: Microsoft Translator konuşma tanıma API'si, Azure üzerinde Microsoft Bilişsel hizmetler kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jaswel
ms.openlocfilehash: 8bf904b2029790d64a806fcf4a7e4860579a5a2f
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39204859"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>Microsoft Translator konuşma tanıma API'si ile Python için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede, Microsoft Translator konuşma tanıma API'si .wav dosyasında konuşulan kelimeleri için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Python 3.x](https://www.python.org/downloads/) bu kodu çalıştırmak için.

Yüklemek ihtiyacınız olacak [websocket-client paketini](https://pypi.python.org/pypi/websocket-client) Python için.

Koddan derleme yürütülebilir dosya ile aynı klasörde yer alan "speak.wav" adlı bir .wav dosyası gerekir. Bu .wav dosya standart PCM 16 bit, 16 kHz, mono biçiminde olmalıdır. Böyle bir .wav dosyasından edinebilirsiniz [metin okuma API'si](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Microsoft Translator konuşma tanıma API'si**. Ücretli aboneliğe anahtarından gerekir, [Azure panosuna](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Konuşmayı çevirme

Aşağıdaki kod, konuşma bir dilden diğerine çevirir.

1. Sık kullandığınız IDE'de yeni Python projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```python
# -*- coding: utf-8 -*-

# To install this package, run:
#   pip install websocket-client
import websocket

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'wss://dev.microsofttranslator.com'
path = '/speech/translate';
params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa'
uri = host + path + params

input_file = 'speak.wav'
output_file = 'speak2.wav'

output = bytearray ()

def on_open (client):
    print ("Connected.")

# r = read. b = binary.
    with open (input_file, mode='rb') as file:
        data = file.read()

    print ("Sending audio.")
    client.send (data, websocket.ABNF.OPCODE_BINARY)
# Make sure the audio file is followed by silence.
# This lets the service know that the audio input is finished.
    print ("Sending silence.")
    client.send (bytearray (32000), websocket.ABNF.OPCODE_BINARY)

def on_data (client, message, message_type, is_last):
    global output
    if (websocket.ABNF.OPCODE_TEXT == message_type):
        print ("Received text data.")
        print (message)
# For some reason, we receive the data as type websocket.ABNF.OPCODE_CONT.
    elif (websocket.ABNF.OPCODE_BINARY == message_type or websocket.ABNF.OPCODE_CONT == message_type):
        print ("Received binary data.")
        print ("Is last? " + str(is_last))
        output = output + message
        if (True == is_last):
# w = write. b = binary.
            with open (output_file, mode='wb') as file:
                file.write (output)
                print ("Wrote data to output file.")
            client.close ()
    else:
        print ("Received data of type: " + str (message_type))

def on_error (client, error):
    print ("Connection error: " + str (error))

def on_close (client):
    print ("Connection closed.")

client = websocket.WebSocketApp(
    uri,
    header=[
        'Ocp-Apim-Subscription-Key: ' + key
    ],
    on_open=on_open,
    on_data=on_data,
    on_error=on_error,
    on_close=on_close
)

print ("Connecting...")
client.run_forever()
```

**Konuşma yanıt Çevir**

Başarılı "speak2.wav" adlı bir dosya oluşturulmasını oluşur. Dosya çevirisi "speak.wav içinde" konuşulan sözcükleri içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator Konuşma Öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Translator konuşma genel bakış](../overview.md)
[API Başvurusu](http://docs.microsofttranslator.com/speech-translate.html)
