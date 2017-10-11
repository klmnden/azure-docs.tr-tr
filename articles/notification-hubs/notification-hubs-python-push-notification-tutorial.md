---
title: "Bildirim hub'ları Python ile kullanma"
description: "Bir Python arka ucunu Azure bildirim hub'ları kullanmayı öğrenin."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9ceedb9940759427fc8cec74a1307e42472563a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-python"></a>Python'dan bildirim hub'ları kullanma
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bir Java/PHP/Python/Ruby MSDN konusunda açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak uç tüm bildirim hub'ları özellikleri erişebileceğiniz [bildirim hub'ları REST API'leri](http://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Bu bildirim gönderir Python içinde uygulamak için bir örnek başvuru uygulamasıdır ve resmi olarak desteklenen bildirim hub'ı Python SDK'sı değil.
> 
> Bu örnek, Python 3.4 kullanılarak yazılır.
> 
> 

Bu konudaki gösteriyoruz nasıl yapılır:

* REST istemcisi Python Notification Hubs özellikleri için oluşturun.
* Bildirim hub'ı REST API'lerine Python arabirimi kullanarak bildirimleri gönderin. 
* Bir HTTP REST istek/yanıt dökümü için hata ayıklama/eğitim amaçlı alın. 

İzleyebileceğiniz [Get öğreticisinde](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) seçim için mobil platformda, arka uç bölümü Python içinde uygulama.

> [!NOTE]
> Örnek kapsamını yalnızca bildirimleri göndermek için sınırlı ve herhangi bir kayıt yönetim yapmaz.
> 
> 

## <a name="client-interface"></a>İstemci arabirimi
Ana istemci arabirimi bulunan aynı yöntemleri sağlayabilir [.NET Notification Hubs SDK'sı](http://msdn.microsoft.com/library/jj933431.aspx). Bu öğreticiler ve bu site şu anda kullanılabilir örnekleri doğrudan Çevir olanak sağlar ve Internet üzerindeki topluluk tarafından katkısı.

Kullanılabilir tüm kod Bul [Python REST sarmalayıcı örnek].

Örneğin, bir istemci oluşturmak için şunu yazın:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Windows bildirim göndermek için:

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a>Uygulama
Zaten yaptıysanız Lütfen izleyin bizim [Get öğreticisinde] son uç uygulamak için sahip olduğu bölüm yukarı.

Tam bir REST sarmalayıcı uygulamak için tüm ayrıntıları bulunabilir [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Bu bölümde biz Python uygulaması bildirim hub'ları REST uç noktalarını erişmek ve bildirimleri göndermek için gerekli ana adımlar açıklanmıştır

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluştur
3. HTTP REST API kullanarak bildirim gönderme

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma
Oluşturucusu bağlantı dizesini ayrıştırmak için bir istemci uygulama ana sınıfı şöyledir:

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


### <a name="create-security-token"></a>Güvenlik belirteci oluşturma
Güvenlik belirteci oluşturma ayrıntılarını kullanılabilir [burada](http://msdn.microsoft.com/library/dn495627.aspx).
Aşağıdaki yöntemlerden eklenecek zorunda **NotificationHub** geçerli istek ve kimlik bilgileri bağlantı dizesinden ayıklanan URI'sini temel belirteç oluşturmak için sınıfı.

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

### <a name="send-a-notification-using-http-rest-api"></a>HTTP REST API kullanarak bildirim gönderme
Birincisi, let tanımlamak bir bildirim temsil eden sınıf.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Bu sınıf, bir yerel bildirim gövdesi veya bir şablon bildirim biçimi (yerel platform veya şablon) ve platforma özgü özellikleri (örneğin, Apple sona erme özelliği ve WNS üstbilgileri) içeren bir dizi üstbilgileri durumunda özellikler kümesi için bir kapsayıcıdır.

Lütfen [bildirim hub'ları REST API belgelerine](http://msdn.microsoft.com/library/dn495827.aspx) ve kullanabileceğiniz tüm seçenekler için belirli bildirim platformları biçimleri.

Bu sınıf ile biz Gönder içinde bildirim yöntemleri yazabilirsiniz artık **NotificationHub** sınıfı.

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

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

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

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
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

Yukarıdaki yöntemler, bildirim hub ' ınızı doğru gövde ve bildirim göndermek için üstbilgiler /messages uç noktası bir HTTP POST isteği gönderin.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Ayrıntılı günlük kaydını etkinleştirmek için hata ayıklama özelliğini kullanma
HTTP isteğiyle ilgili ayrıntılı günlük kaydı bilgileri bildirim hub'ı başlatılırken hata ayıklama özelliğini etkinleştirme yazılır ve ayrıntılı bir bildirim iletisi yanı sıra yanıt döküm sonucunu gönderin. Son olarak adlandırılan bu özellik eklediğimiz [bildirim hub'ları TestSend özelliği](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) bildirim gönderme sonucunu hakkında ayrıntılı bilgiler döndürür. -Kullanmak için aşağıdakileri kullanarak başlatın:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Bildirim hub'ı İsteği Gönder HTTP URL'si, "test" sorgu dizesi ile sonuç olarak eklenmiş. 

## <a name="complete-tutorial"></a>Öğreticiyi Tamamla
Şimdi bir Python arka ucunu bildirim göndererek Başlarken Öğreticisi tamamlayabilirsiniz.

Bildirim hub'ları istemciniz başlatılamıyor (belirtildiği gibi bağlantı dizenizi ve hub adı yerine [Get öğreticisinde]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Ardından, hedef mobil platforma bağlı olarak gönderme kodu ekleyin. Bu örnek ayrıca örneğin send_windows_notification Windows platform göre gönderen bildirimlerini etkinleştirmek için daha yüksek düzey yöntemleri ekler; (için apple) send_apple_notification vs. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows mağazası ve Windows Phone 8.1 (Silverlight olmayan)
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight
    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Python kodunuzu çalıştıran hedef aygıtınızda görünen bir bildirim üretir.

## <a name="examples"></a>Örnekler:
### <a name="enabling-debug-property"></a>Hata ayıklama özelliğini etkinleştirme
Hata ayıklama bayrağı hangi HTTP üstbilgileri istekte geçirilir ve hangi HTTP yanıtı bildirim Hub'ından alınan burada anlamak aşağıdaki gibi NotificationOutcome yanı sıra ayrıntılı HTTP isteği ve yanıt döküm görür NotificationHub başlatılırken etkinleştirdiğinizde:![][1]

Bildirim hub'ı sonuç ayrıntılı örneğin görürsünüz 

* ileti başarıyla için anında iletilen bildirim servisi gönderildiğinde. 
  
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
* Herhangi bir anında bildirim bulunan hiçbir hedef olsaydı sonra büyük olasılıkla aşağıdaki (hangi kayıtları eşleşmeyen bazı etiketler olduğundan bildirim büyük olasılıkla teslim etmek için bulunan herhangi bir kayıt olduğunu gösterir) yanıt görmek için olmaz
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Windows bildirim yayını
Windows İstemcisi için yayın bildirim gönderirken gönderilen üstbilgileri dikkat edin. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Bir etiketi (veya etiket ifadesi) belirterek bildirim gönder
HTTP isteği eklenen etiketleri HTTP üstbilgisi dikkat edin (aşağıdaki örnekte, biz bildirim yalnızca 'Spor' yükü kayıtlarla gönderme)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Birden çok etiket belirtme bildirim gönder
Birden çok etiket gönderildiğinde etiketleri HTTP üstbilgisi nasıl değiştiğine dikkat edin. 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Şablonlu bildirim
Biçim HTTP üstbilgisi değişiklikleri dikkat edin ve yük gövde HTTP isteği gövdesinin bir parçası olarak gönderilir:

**İstemci tarafı - kayıtlı şablonu**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

**Sunucu tarafı - yükü gönderme**

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a>Sonraki Adımlar
Bu konuda size bildirim hub'ları için basit bir Python REST istemcisi nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

* Tam karşıdan [Python REST sarmalayıcı örnek], yukarıdaki tüm kodunu içerir.
* Bildirim hub'ları özelliği etiketleme hakkında bilgi almaya devam etmek [çiğnemekten haber Öğreticisi]
* Bildirim hub'ları şablonları özelliği hakkında bilgi almaya devam etmek [yerelleştirme haber Öğreticisi]

<!-- URLs -->
[Python REST sarmalayıcı örnek]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get öğreticisinde]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[çiğnemekten haber Öğreticisi]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[yerelleştirme haber Öğreticisi]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

