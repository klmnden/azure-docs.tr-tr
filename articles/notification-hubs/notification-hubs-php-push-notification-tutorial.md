---
title: PHP ile Notification hubs'ı kullanma
description: Bir PHP arka ucundan Azure Notification hubs'ı kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 054edaf321d90015840fd84e1697fca742fd7e1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60872198"
---
# <a name="how-to-use-notification-hubs-from-php"></a>Php'den Notification hubs'ı kullanma

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

MSDN konu başlığı altında açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak bir Java/PHP/Ruby arka tüm Notification Hubs özelliklerine erişim [Notification Hubs REST API'leri](https://msdn.microsoft.com/library/dn223264.aspx).

Bu konudaki göstereceğiz nasıl yapılır:

* Php'de Notification Hubs özellikleri için bir REST istemcisi oluşturmak;
* İzleyin [başlangıç Öğreticisi](notification-hubs-ios-apple-push-notification-apns-get-started.md) için seçtiğiniz mobil platformda, arka uç bölümü PHP'de uygulama.

## <a name="client-interface"></a>İstemci arabirimi

Ana istemci arabirimi kullanılabilir yöntemleri aynı sağlayabilir [.NET Notification Hubs SDK'sı](https://msdn.microsoft.com/library/jj933431.aspx), öğreticileri ve örnekleri şu anda bu sitedeki tüm doğrudan çevrilecek sağlar ve katkıda bulunan internet üzerindeki bir topluluk.

Kullanılabilir tüm kod bulabilirsiniz [PHP REST sarmalayıcı örneği].

Örneğin, bir istemci oluşturmak için şunu yazın:

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Bir iOS yerel bildirimi göndermek için:

    ```php
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);
    ```

## <a name="implementation"></a>Uygulama

Henüz oluşturmadıysanız, izleyin [Başlangıç eğitmeni] bölümünde arka uç uygulamak için sahip olduğu son yedekleme.
Ayrıca isterseniz kodu kullanabilirsiniz [PHP REST sarmalayıcı örneği] ve doğrudan [öğreticiye](#complete-tutorial) bölümü.

Tam bir REST sarmalayıcı uygulamak için tüm ayrıntıları bulunabilir [MSDN](https://msdn.microsoft.com/library/dn530746.aspx). Bu bölümde, biz PHP uygulaması bildirim hub'ları REST uç noktalarına erişmesi için gereken ana adımlar açıklanmaktadır:

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluştur
3. HTTP çağrısı gerçekleştirin

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

İstemci, bağlantı dizesini işlediğinde, Oluşturucusu uygulama ana sınıfı şöyledir:

    ```php
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
    ```

### <a name="create-a-security-token"></a>Bir güvenlik belirteci oluşturma

Hakkında bilgi için Azure belgelerine başvurun [bir SAS güvenlik belirteci oluşturma](https://docs.microsoft.com/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token).

Ekleme `generateSasToken` yönteme `NotificationHub` geçerli istek ve kimlik bilgileri bağlantı dizesinden ayıklanan URI'sini temel belirteci oluşturmak için sınıf.

    ```php
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
    ```

### <a name="send-a-notification"></a>Bildirim gönderme

İlk olarak, bize bir bildirim temsil eden bir sınıf tanımlar.

    ```php
    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "fcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }
    ```

Bu sınıf, bir yerel bildirimi gövdesi veya kümesi üzerinde çalışması şablon bildirim özelliklerini ve format (yerel platform veya şablonu) ve (Apple sona erme özelliğini ve WNS gibi platforma özgü özellikler içeren üst bilgiler, bir dizi için bir kapsayıcı üst bilgiler).

Başvurmak [Notification Hubs REST API belgeleri](https://msdn.microsoft.com/library/dn495827.aspx) ve tüm seçenekler için belirli bildirim platformları biçimleri.

Bu sınıf ile kullanarak Şimdi Gönder içinde bildirim yöntemlerini yazabiliriz `NotificationHub` sınıfı:

    ```php
    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "fcm"])) {
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
            throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 
    ```

Yukarıdaki yöntemi bir HTTP POST isteği gönderin `/messages` notification hub, bildirim göndermek üzere üst bilgileri ve doğru gövde ile uç nokta.

## <a name="complete-tutorial"></a>Öğreticiyi Tamamla

Şimdi PHP arka ucundan bildirim göndererek Başlarken Öğreticisi tamamlayabilirsiniz.

Bildirim hub'ları istemcinizi başlatır (bağlantı dizenizi ve hub adı belirtildiği gibi alternatif [Başlangıç eğitmeni]):

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Ardından, hedef mobil platforma bağlı olarak gönderme kodu ekleyin.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store ve Windows Phone 8.1 (Silverlight olmayan)

    ```php
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);
    ```

### <a name="ios"></a>iOS

    ```php
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);
    ```

### <a name="android"></a>Android

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("fcm", $message);
    $hub->sendNotification($notification, null);
    ```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight

    ```php
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
    ```

### <a name="kindle-fire"></a>Kindle Fire

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);
    ```

PHP kodunuzu çalıştıran, artık hedef Cihazınızda görünen bir bildirim üretir.

## <a name="next-steps"></a>Sonraki Adımlar

Bu konu başlığında, biz basit bir Java REST istemci bildirim hub'ları için nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

* Tam indirme [PHP REST sarmalayıcı örneği], yukarıdaki kod içerir.
* Bildirim hub'ları etiketleme özelliğini [bozucu haber öğreticide] hakkında daha fazla bilgi
* [Kullanıcılara bildirme öğreticide] bireysel kullanıcılara bildirimler gönderme hakkında bilgi edinin

Daha fazla bilgi için Ayrıca bkz: [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[PHP REST sarmalayıcı örneği]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Başlangıç eğitmeni]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
