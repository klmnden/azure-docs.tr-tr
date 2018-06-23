---
title: Bilişsel hizmetler Azure, Microsoft Çeviricisi konuşma API için Python hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Microsoft Çeviricisi konuşma API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 29722b3fa7fe61aff0b2406002453020d999ea41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352192"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>Microsoft Çeviricisi konuşma API'si ile Python için hızlı başlangıç 
<a name="HOLTop"></a>

Bu makalede Microsoft Çeviricisi konuşma API .wav dosyasında konuşulan kelimeleri için nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Python 3.x](https://www.python.org/downloads/) bu kodu çalıştırmak için.

Yüklemeniz gerekecek [websocket-client paketini](https://pypi.python.org/pypi/websocket-client) Python için.

Aşağıdaki kod derleme yürütülebilir dosya ile aynı klasörde "speak.wav" adlı bir .wav dosyası gerekir. Bu .wav dosyası standart PCM 16 bit, 16 kHz mono biçiminde olmalıdır. Bu tür bir .wav dosyasından edinebilirsiniz [Çeviricisi metin seslendir API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Sahip olmanız gerekir bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Microsoft Çeviricisi konuşma API**. Ücretli abonelik anahtarından gerekir, [Azure Pano](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Konuşma Çevir

Aşağıdaki kod, bir dilden diğerine konuşma çevirir.

1. Sık kullanılan IDE içinde yeni bir Python projesi oluşturun.
2. Aşağıda sunulan kodu ekleyin.
3. Değiştir `key` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
4. Programını çalıştırın.

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

Başarılı "speak2.wav" adlı bir dosya oluşturma oluşur. Dosya çevirisi "speak.wav içinde" konuşulan sözcüklerin içerir.

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çevirici Konuşma Öğreticisi](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Çevirici konuşma genel bakış](../overview.md)
[API Başvurusu](http://docs.microsofttranslator.com/speech-translate.html)
