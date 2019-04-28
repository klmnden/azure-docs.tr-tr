---
title: Notification Hubs ile Python kullanma
description: Bir Python arka ucunuzdan Azure Notification hubs'ı kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.author: jowargo
ms.date: 01/04/2019
ms.openlocfilehash: 43a691ff9025cdb39786f965be6a2fca1b33bd3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61458453"
---
# <a name="how-to-use-notification-hubs-from-python"></a>Python'dan Notification hubs'ı kullanma

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bir Java/PHP/Python/Ruby MSDN makalesinde açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak uç tüm Notification Hubs özellikleri erişebileceğiniz [Notification Hubs REST API'leri](https://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Bu bildirim gönderen Python uygulamak için bir örnek başvuru uygulaması ve resmi olarak desteklenen bildirim hub'ı Python SDK'sını değil. Örnek Python 3.4 kullanılarak oluşturuldu.

Bu makalede gösterilmektedir için:

- Python'da Notification Hubs özellikleri için bir REST istemcisi oluşturun.
- Bildirim hub'ı REST API'lerine Python arabirimini kullanarak bildirimleri gönderin.
- Bir HTTP REST istek/yanıt dökümü için hata ayıklama/eğitim amaçlı alın.

İzleyebileceğiniz [başlangıç Öğreticisi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) için seçtiğiniz mobil platformda, arka uç bölümü Python'da uygulama.

> [!NOTE]
> Bildirimleri göndermek için örnek kapsamını yalnızca sınırlı ve herhangi bir kayıt yönetim yapmaz.

## <a name="client-interface"></a>İstemci arabirimi

Ana istemci arabirimi kullanılabilir yöntemleri aynı sağlayabilir [.NET Notification Hubs SDK'sı](https://msdn.microsoft.com/library/jj933431.aspx). Bu arabirim, öğreticileri ve örnekleri şu anda bu sitedeki tüm doğrudan çevrilecek sağlar ve internet'teki topluluk tarafından katkıda bulunulan.

Kullanılabilir tüm kod bulabilirsiniz [Python REST sarmalayıcı örneği].

Örneğin, bir istemci oluşturmak için şunu yazın:

```python
isDebug = True
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

Bir Windows bildirim göndermek için:

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

## <a name="implementation"></a>Uygulama

Henüz oluşturmadıysanız, izleyin [Başlangıç eğitmeni] arka ucunuzu uygulamak için sahip olduğu bölümü en son yukarı.

Tam bir REST sarmalayıcı uygulamak için tüm ayrıntıları bulunabilir [MSDN](https://msdn.microsoft.com/library/dn530746.aspx). Bu bölümde bildirim hub'ları REST uç noktalarına erişmesi ve bildirim göndermek için gereken ana adımlar Python uygulamasını açıklar.

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluştur
3. HTTP REST API kullanarak bildirim gönderme

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

Oluşturucusu bağlantı dizesini ayrıştırmak için bir istemci uygulama ana sınıfı şöyledir:

```python
class NotificationHub:
    API_VERSION = "?api-version=2013-10"
    DEBUG_SEND = "&test"

    def __init__(self, connection_string=None, hub_name=None, debug=0):
        self.HubName = hub_name
        self.Debug = debug

        # Parse connection string
        parts = connection_string.split(';')
        if len(parts) != 3:
            raise Exception("Invalid ConnectionString.")

        for part in parts:
            if part.startswith('Endpoint'):
                self.Endpoint = 'https' + part[11:]
            if part.startswith('SharedAccessKeyName'):
                self.SasKeyName = part[20:]
            if part.startswith('SharedAccessKey'):
                self.SasKeyValue = part[16:]
```

### <a name="create-security-token"></a>Güvenlik belirteci oluşturma

Güvenlik belirteci oluşturma ayrıntılarını kullanılabilir [burada](https://msdn.microsoft.com/library/dn495627.aspx).
Aşağıdaki yöntemleri ekleyin `NotificationHub` geçerli istek ve kimlik bilgileri bağlantı dizesinden ayıklanan URI'sini temel belirteci oluşturmak için sınıf.

```python
@staticmethod
def get_expiry():
    # By default returns an expiration of 5 minutes (=300 seconds) from now
    return int(round(time.time() + 300))

@staticmethod
def encode_base64(data):
    return base64.b64encode(data)

def sign_string(self, to_sign):
    key = self.SasKeyValue.encode('utf-8')
    to_sign = to_sign.encode('utf-8')
    signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
    digest = signed_hmac_sha256.digest()
    encoded_digest = self.encode_base64(digest)
    return encoded_digest

def generate_sas_token(self):
    target_uri = self.Endpoint + self.HubName
    my_uri = urllib.parse.quote(target_uri, '').lower()
    expiry = str(self.get_expiry())
    to_sign = my_uri + '\n' + expiry
    signature = urllib.parse.quote(self.sign_string(to_sign))
    auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
    sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
    return sas_token
```

### <a name="send-a-notification-using-http-rest-api"></a>HTTP REST API kullanarak bildirim gönderme

İlk, let tanımlamak bir bildirim temsil eden sınıf.

```python
class Notification:
    def __init__(self, notification_format=None, payload=None, debug=0):
        valid_formats = ['template', 'apple', 'fcm', 'windows', 'windowsphone', "adm", "baidu"]
        if not any(x in notification_format for x in valid_formats):
            raise Exception(
                "Invalid Notification format. " +
                "Must be one of the following - 'template', 'apple', 'fcm', 'windows', 'windowsphone', 'adm', 'baidu'")

        self.format = notification_format
        self.payload = payload

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
        # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        self.headers = None
```

Bu sınıf, bir yerel bildirimi gövdesinde veya bir dizi biçim (yerel platform veya şablonu) ve (Apple sona erme özelliğini ve WNS üst bilgiler gibi) platforma özgü özellikleri içeren bir dizi üstbilgileri, bir şablon bildirim özelliklerini ilişkin bir kapsayıcıdır.

Başvurmak [Notification Hubs REST API belgeleri](https://msdn.microsoft.com/library/dn495827.aspx) ve tüm seçenekler için belirli bildirim platformları biçimleri.

Bu sınıf ile gönderme içinde bildirim yöntemleri artık yazma `NotificationHub` sınıfı.

```python
def make_http_request(self, url, payload, headers):
    parsed_url = urllib.parse.urlparse(url)
    connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

    if self.Debug > 0:
        connection.set_debuglevel(self.Debug)
        # adding this querystring parameter gets detailed information about the PNS send notification outcome
        url += self.DEBUG_SEND
        print("--- REQUEST ---")
        print("URI: " + url)
        print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
        print("--- END REQUEST ---\n")

    connection.request('POST', url, payload, headers)
    response = connection.getresponse()

    if self.Debug > 0:
        # print out detailed response information for debugging purpose
        print("\n\n--- RESPONSE ---")
        print(str(response.status) + " " + response.reason)
        print(response.msg)
        print(response.read())
        print("--- END RESPONSE ---")

    elif response.status != 201:
        # Successful outcome of send message is HTTP 201 - Created
        raise Exception(
            "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

    connection.close()

def send_notification(self, notification, tag_or_tag_expression=None):
    url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

    json_platforms = ['template', 'apple', 'fcm', 'adm', 'baidu']

    if any(x in notification.format for x in json_platforms):
        content_type = "application/json"
        payload_to_send = json.dumps(notification.payload)
    else:
        content_type = "application/xml"
        payload_to_send = notification.payload

    headers = {
        'Content-type': content_type,
        'Authorization': self.generate_sas_token(),
        'ServiceBusNotification-Format': notification.format
    }

    if isinstance(tag_or_tag_expression, set):
        tag_list = ' || '.join(tag_or_tag_expression)
    else:
        tag_list = tag_or_tag_expression

    # add the tags/tag expressions to the headers collection
    if tag_list != "":
        headers.update({'ServiceBusNotification-Tags': tag_list})

    # add any custom headers to the headers collection that the user may have added
    if notification.headers is not None:
        headers.update(notification.headers)

    self.make_http_request(url, payload_to_send, headers)

def send_apple_notification(self, payload, tags=""):
    nh = Notification("apple", payload)
    self.send_notification(nh, tags)

def send_fcm_notification(self, payload, tags=""):
    nh = Notification("fcm", payload)
    self.send_notification(nh, tags)

def send_adm_notification(self, payload, tags=""):
    nh = Notification("adm", payload)
    self.send_notification(nh, tags)

def send_baidu_notification(self, payload, tags=""):
    nh = Notification("baidu", payload)
    self.send_notification(nh, tags)

def send_mpns_notification(self, payload, tags=""):
    nh = Notification("windowsphone", payload)

    if "<wp:Toast>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
    elif "<wp:Tile>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

    self.send_notification(nh, tags)

def send_windows_notification(self, payload, tags=""):
    nh = Notification("windows", payload)

    if "<toast>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/toast'}
    elif "<tile>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/tile'}
    elif "<badge>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/badge'}

    self.send_notification(nh, tags)

def send_template_notification(self, properties, tags=""):
    nh = Notification("template", properties)
    self.send_notification(nh, tags)
```

Bu yöntemler, bildirim hub'ınıza, bildirim göndermek üzere üst bilgileri ve doğru gövdesi /messages uç noktasına bir HTTP POST isteği gönderin.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Ayrıntılı günlük kaydını etkinleştirmek için hata ayıklama özelliğini kullanarak

HTTP isteğiyle ilgili ayrıntılı günlük kaydı bilgileri bildirim hub'ı başlatılırken hata ayıklama özelliği etkinleştirme yazar ve sonucu yanıt döküm yanı sıra ayrıntılı bir bildirim iletisi gönderin.
[Bildirim hub'ları TestSend özelliği](https://docs.microsoft.com/previous-versions/azure/reference/dn495827(v=azure.100)) bildirim gönderme sonucunu hakkında ayrıntılı bilgi verir.
Bunu kullanın - aşağıdaki kodu kullanarak başlatmak için:

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

Bildirim hub'ı gönderme isteği HTTP URL'si sonuç olarak bir "test" sorgu dizesi eklenmiş.

## <a name="complete-tutorial"></a>Öğreticiyi Tamamla

Şimdi bir Python arka ucunuzdan bildirim göndererek Başlarken Öğreticisi tamamlayabilirsiniz.

Bildirim hub'ları istemcinizi başlatır (bağlantı dizenizi ve hub adı belirtildiği gibi alternatif [Başlangıç eğitmeni]):

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName")
```

Ardından, hedef mobil platforma bağlı olarak gönderme kodu ekleyin. Bu örnek, platform, örneğin, windows için send_windows_notification göre gönderen bildirimlerini etkinleştirmek için üst düzey yöntemleri de ekler; send_apple_notification (için apple) vs.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store ve Windows Phone 8.1 (Silverlight olmayan)

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight

```python
hub.send_mpns_notification(toast)
```

### <a name="ios"></a>iOS

```python
alert_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_apple_notification(alert_payload)
```

### <a name="android"></a>Android

```python
fcm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_fcm_notification(fcm_payload)
```

### <a name="kindle-fire"></a>Kindle Fire

```python
adm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_adm_notification(adm_payload)
```

### <a name="baidu"></a>Baidu

```python
baidu_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_baidu_notification(baidu_payload)
```

Python kodunuzu çalıştıran hedef Cihazınızda görünen bir bildirim üretir.

## <a name="examples"></a>Örnekler

### <a name="enabling-the-debug-property"></a>Etkinleştirme `debug` özelliği

NotificationHub başlatılırken hata ayıklama bayrağı etkinleştirmek NotificationOutcome yanı sıra ayrıntılı HTTP istek ve yanıt döküm aşağıdaki gibi burada hangi HTTP üstbilgileri istek geçirilir ve hangi HTTP yanıtı anlayabilir görürsünüz Bildirim Hub'ından alınan:

![][1]

Gördüğünüz gibi ayrıntılı bildirim hub'ı sonucu.

- ne zaman ileti başarıyla anında iletilen bildirim servisi için gönderilir.
    ```xml
    <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
    ```
- Hedef için herhangi bir anında iletme bildirimi bulunamadı varsa, daha sonra büyük olasılıkla (gösteren bazı kayıtları eşleşmeyen olduğundan bildirim muhtemelen sunmak için bulunan herhangi bir kayıt olan yanıt aşağıdaki çıktıyı görmek için oluşturacağınız Etiketler)
    ```xml
    '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="https://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'
    ```

### <a name="broadcast-toast-notification-to-windows"></a>Windows için bildirimi yayınlayın

Windows İstemcisi için bir yayın bildirimi gönderirken gönderilen üstbilgileri dikkat edin.

```python
hub.send_windows_notification(wns_payload)
```

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Bir etiketi (veya etiket ifadesi) belirten bir bildirim gönderin

HTTP isteğine eklenen etiketler HTTP üstbilgisini dikkat edin (aşağıdaki örnekte, bildirim yalnızca 'Spor' yüküyle kayıtları gönderilir)

```python
hub.send_windows_notification(wns_payload, "sports")
```

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Birden çok etiket belirtme bildirim gönder

Birden çok etiket gönderildiğinde etiketleri HTTP üst bilgisi nasıl değiştiğine dikkat edin.

```python
tags = {'sports', 'politics'}
hub.send_windows_notification(wns_payload, tags)
```

![][4]

### <a name="templated-notification"></a>Şablonlu bildirimi

Biçim HTTP üst bilgisi değiştiğine dikkat edin ve yükü gövdesi HTTP isteği gövdesinin bir parçası olarak gönderilir:

**İstemci tarafı - kayıtlı şablonu:**

```python
var template = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
```

**Sunucu tarafı - yükü gönderme:**

```python
template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
hub.send_template_notification(template_payload)
```

![][5]

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede Python REST istemci bildirim hub'ları için nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

- Tam indirme [Python REST sarmalayıcı örneği], bu makaledeki tüm kodunu içerir.
- Bildirim hub'ları etiketleme özelliğini hakkında daha fazla bilgi [Yeni haber Öğreticisi]
- Bildirim hub'ları şablonları özelliği hakkında daha fazla bilgi [Haber öğretici yerelleştirme]

<!-- URLs -->
[Python REST sarmalayıcı örneği]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Başlangıç eğitmeni]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Yeni haber Öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Haber öğretici yerelleştirme]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
