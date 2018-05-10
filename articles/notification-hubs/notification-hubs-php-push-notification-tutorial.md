---
title: PHP ile bildirim hub'ları kullanma
description: Bir PHP arka ucunu Azure bildirim hub'ları kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 930da7cca312ac6233b337dd7ddac478c3bbee7b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-use-notification-hubs-from-php"></a>Php'den Notification Hubs kullanma
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bir PHP/Java/Ruby MSDN konusunda açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak uç tüm bildirim hub'ları özellikleri erişebileceğiniz [bildirim hub'ları REST API'leri](http://msdn.microsoft.com/library/dn223264.aspx).

Bu konudaki gösteriyoruz nasıl yapılır:

* PHP Notification Hubs özellikleri için bir REST istemci oluşturun;
* İzleyin [Get öğreticisinde](notification-hubs-ios-apple-push-notification-apns-get-started.md) seçim için mobil platformda, arka uç bölümü PHP ile uygulama.

## <a name="client-interface"></a>İstemci arabirimi
Ana istemci arabirimi bulunan aynı yöntemleri sağlayabilir [.NET Notification Hubs SDK'sı](http://msdn.microsoft.com/library/jj933431.aspx), hangi öğreticiler ve bu site şu anda kullanılabilir örnekleri doğrudan çevirmenizi sağlar ve katkıda bulunan internet üzerindeki bir topluluk.

Kullanılabilir tüm kod Bul [PHP REST sarmalayıcı örnek].

Örneğin, bir istemci oluşturmak için şunu yazın:

    $hub = new NotificationHub("connection string", "hubname");    

Bir iOS yerel bildirim göndermek için:

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Uygulama
Henüz kaldırdıysanız, izleyin [Get öğreticisinde] son uç uygulamak için sahip olduğu bölüm yukarı.
Ayrıca, isterseniz koddan kullanabilirsiniz [PHP REST sarmalayıcı örnek] ve doğrudan gitmek [öğreticiye](#complete-tutorial) bölümü.

Tam bir REST sarmalayıcı uygulamak için tüm ayrıntıları bulunabilir [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Bu bölümde, PHP uygulaması bildirim hub'ları REST uç noktalarını erişmek için gerekli ana adımlar açıklanmaktadır:

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluştur
3. HTTP çağrısı yapmak

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma
İstemci, bağlantı dizesini ayrıştırmak için olan Oluşturucusu uygulama ana sınıfı şöyledir:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Güvenlik belirteci oluşturma
Güvenlik belirteci oluşturma ayrıntılarını kullanılabilir [burada](http://msdn.microsoft.com/library/dn495627.aspx).
Eklenecek aşağıdaki yöntemi sahip **NotificationHub** geçerli istek ve kimlik bilgileri bağlantı dizesinden ayıklanan URI'sini temel belirteç oluşturmak için sınıfı.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Bildirim gönderme
İlk olarak, bir bildirim temsil eden bir sınıf bize tanımlayın.

    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }

Bu sınıf bir yerel bildirim gövdesi veya biçimi (yerel platform veya şablon) ve (örneğin, Apple sona erme özelliği ve WNS platforma özgü özellikleri içeren bir dizi üst bilgiler ve bir şablon bildirim durumunun özellikleri kümesi için bir kapsayıcıdır üst bilgiler).

Başvurmak [bildirim hub'ları REST API belgelerine](http://msdn.microsoft.com/library/dn495827.aspx) ve kullanabileceğiniz tüm seçenekler için belirli bildirim platformları biçimleri.

Bu sınıfla çağrılmak, biz Şimdi Gönder içinde bildirim yöntemleri yazabilirsiniz **NotificationHub** sınıfı.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Yukarıdaki yöntemler, bildirim hub ' ınızı doğru gövde ve bildirim göndermek için üstbilgiler /messages uç noktası bir HTTP POST isteği gönderin.

## <a name="complete-tutorial"></a>Öğreticiyi Tamamla
Şimdi bir PHP arka ucunu bildirim göndererek Başlarken Öğreticisi tamamlayabilirsiniz.

Bildirim hub'ları istemciniz başlatılamıyor (belirtildiği gibi bağlantı dizenizi ve hub adı yerine [Get öğreticisinde]):

    $hub = new NotificationHub("connection string", "hubname");    

Ardından, hedef mobil platforma bağlı olarak gönderme kodu ekleyin.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows mağazası ve Windows Phone 8.1 (Silverlight olmayan)
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

PHP kodunuzu çalıştırmaya şimdi, hedef cihazda görünen bir bildirim üretir.

## <a name="next-steps"></a>Sonraki Adımlar
Bu konuda, biz basit bir Java REST istemci bildirim hub'ları için nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

* Tam karşıdan [PHP REST sarmalayıcı örnek], yukarıdaki tüm kodunu içerir.
* Bildirim hub'ları özelliği [çiğnemekten haber öğreticide] etiketleme hakkında bilgi almaya devam etmek
* Bildirimleri bireysel kullanıcılara [kullanıcılara bildirme öğreticide] İtme hakkında bilgi edinin

Daha fazla bilgi için Ayrıca bkz. [PHP Geliştirici Merkezi](/develop/php/).

[PHP REST sarmalayıcı örnek]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get öğreticisinde]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

