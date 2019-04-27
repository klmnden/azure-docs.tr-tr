---
title: "Hızlı Başlangıç: Translator konuşma tanıma API'si Python"
titlesuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: v-jaswel
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: quickstart
ms.date: 07/17/2018
ms.author: v-jaswel
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9a6afc4dfb25a2a5f6e778fbda877a93269a96eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60535540"
---
# <a name="quickstart-translator-speech-api-with-python"></a>Hızlı Başlangıç: Translator konuşma tanıma API'si ile Python
<a name="HOLTop"></a>

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Bu makalede Translator Konuşma Çevirisi API'si’ni kullanarak bir .wav dosyasında konuşulan sözcüklerin nasıl çevrileceği gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [Python 3.x](https://www.python.org/downloads/) sürümü gereklidir.

Python için [websocket-client paketini](https://pypi.python.org/pypi/websocket-client) indirmeniz gerekir.

Aşağıdaki koddan derlediğiniz yürütülebilir dosya ile aynı klasörün içinde "speak.wav" adlı bir .wav dosyanız olmalıdır. Bu .wav dosyası standart PCM, 16 bit, 16 kHz, mono biçiminde olmalıdır.

**Microsoft Translator Konuşma Çevirisi API'sine** sahip bir [Bilişsel Hizmetler API hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Azure panonuzdan](https://portal.azure.com/#create/Microsoft.CognitiveServices) ücretli bir abonelik anahtarına ihtiyacınız olacak.

## <a name="translate-speech"></a>Konuşmayı çevirme

Aşağıdaki kod, konuşmayı bir dilden diğerine çevirir.

1. Sık kullandığınız IDE'de yeni bir Python projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `key` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
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

**Konuşma yanıtını çevirme**

"speak2.wav" adlı bir dosya oluşturulduğunda sonuç başarılıdır. Dosya, "speak.wav" içinde konuşulan sözcüklerin çevirisini içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator Konuşma Çevirisi öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz.

[Translator Konuşma Çevirisine genel bakış](../overview.md)
[API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
