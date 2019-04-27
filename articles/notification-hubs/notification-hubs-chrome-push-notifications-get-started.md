---
title: Azure Notification Hubs ile Chrome uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bir Chrome Uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenin.
services: notification-hubs
keywords: mobil anında iletme bildirimleri,anında iletme bildirimleri,anında iletme bildirimi,chrome anında iletme bildirimleri
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 03374f63345bd6c9e4f2b603443a1448493e1cdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60875970"
---
# <a name="tutorial-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Öğretici: Azure Notification Hubs ile Chrome uygulamalarına anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğreticide, [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) kullanılarak bir bildirim hub’ı oluşturma ve anında iletme bildirimlerini örnek bir Google Chrome uygulamasına gönderme adımları açıklanmaktadır. Chrome uygulaması bir Google Chrome tarayıcısı bağlamında çalıştırılır ve bildirim hub’ına kaydolur.

> [!NOTE]
> Chrome uygulaması anında iletme bildirimleri, genel tarayıcı içi bildirimler olmayıp tarayıcı genişletilebilirlik modeline özgüdür (ayrıntılar için bkz. [Chrome Uygulamalarına Genel Bakış]). Chrome uygulamaları, masaüstü tarayıcısının yanı sıra Apache Cordova aracılığıyla mobil cihazlarda (Android ve iOS) da çalıştırılır. Daha fazla bilgi için bkz. [Mobil Cihazlarda Chrome Uygulamaları].

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * [Google Cloud Messaging'i etkinleştirme](#register)
> * [Bildirim hub'ınızı yapılandırma](#configure-hub)
> * [Chrome Uygulamanızı bildirim hub'ına bağlama](#connect-app)
> * [Chrome Uygulamanıza anında iletme bildirimi gönderme](#send)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a id="register"></a>Google Cloud Messaging'i etkinleştirme

1. [Google Cloud Console](https://console.cloud.google.com/cloud-resource-manager) web sitesine gidin ve Google hesabı kimlik bilgilerinizle oturum açın
2. Araç çubuğunda **Proje Oluştur**’u seçin.

    ![Proje Oluştur düğmesi](media/notification-hubs-chrome-get-started/create-project-button.png)
3. Uygun bir **Project Name** (Proje Adı) sağlayın ve ardından **Create** (Oluştur) düğmesine tıklayın.
4. Araç çubuğundaki bildirim simgesini (Zil) seçin ve **Proje Oluştur** iletisini seçin.

    ![Proje oluşturma bildirimi](media/notification-hubs-chrome-get-started/project-creation-notification.png)
5. Oluşturduğunuz proje için **Projects** (Projeler) sayfasındaki **Project Number**'ı (Proje Numarası) not edin. Proje numarasını Chrome Uygulamasında GCM'ye kaydı için **GCM Sender ID** (GCM Gönderen Kimliği) olarak kullanırsınız.

    ![Google Cloud Console - Proje Numarası](media/notification-hubs-chrome-get-started/gcm-project-number.png)
6. Panoda **API’lerin genel bakışına git** seçeneğini belirleyin.

    ![API’lerin genel bakışına git düğmesi](media/notification-hubs-chrome-get-started/go-to-apis-overview-button.png)
7. API ve Hizmetler sayfasında **API’leri ve Hizmetleri Etkinleştir**’i seçin.

    ![API’leri ve Hizmetleri Etkinleştir](media/notification-hubs-chrome-get-started/enable-apis-and-services.png)
8. **Bulut mesajlaşması** anahtar sözcüğüyle listede arama yapın. Filtrelenen listede **Google Cloud Messaging**’i seçin.

    ![Google cloud messaging API’si](media/notification-hubs-chrome-get-started/google-cloud-messaging-api.png)
9. **Google Cloud Messaging** sayfasında **Etkinleştir**’i seçin.

    ![GCM’yi etkinleştir](media/notification-hubs-chrome-get-started/enable-gcm.png)
10. **API ve Hizmetler** sayfasında, **Kimlik Bilgileri** sekmesine geçin. **Kimlik bilgilerini oluşturun**’u ve sonra **API anahtarı**’nı seçin.

    ![API anahtarı oluştur düğmesi](media/notification-hubs-chrome-get-started/create-api-key-button.png)
11. **API anahtarı oluşturuldu** iletişim kutusunda kopyala düğmesini seçerek anahtarı panoya kopyalayın. Başka bir yere kaydedin. Bir sonraki bölümde bildirim hub’ını GCM’ye anında iletme bildirimleri gönderecek şekilde yapılandırmak için bu değeri kullanırsınız.

    ![API sayfası](media/notification-hubs-chrome-get-started/api-created-page.png)
12. **API anahtarları** listesinde API anahtarınızı seçin. API anahtarı sayfasında **IP adresleri (web sunucuları, sıralanmış işler vb.)** seçeneğini belirleyin ve IP adresi için **0.0.0.0/0** girip Kaydet’e tıklayın.

    ![IP adreslerini etkinleştirme](media/notification-hubs-chrome-get-started/enable-ip-addresses.png)

## <a id="configure-hub"></a>Bildirim hub’ınızı oluşturma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. **BİLDİRİM AYARLARI** kategorisinde **Google (GCM)** seçeneğini belirleyin, GCM projesi için **API anahtarını** girin ve **Kaydet**’e tıklayın.

&emsp;&emsp;&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](media/notification-hubs-chrome-get-started/configure-gcm-api-key.png)

## <a id="connect-app"></a>Chrome Uygulamanızı bildirim hub'ına bağlama

Bildirim hub'ınız şimdi GCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve göndermesi için kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz.

### <a name="create-a-new-chrome-app"></a>Yeni bir Chrome Uygulaması oluşturma

Aşağıdaki örnek, [Chrome Uygulaması GCM Örneği]'ni temel alır ve Chrome Uygulaması oluşturmak için önerilen yöntemi kullanır. Bu bölümde, Azure Notification Hubs’a özgü adımlar vurgulanır.

> [!NOTE]
> Bu Chrome Uygulamasının kaynağını [Chrome Uygulaması Bildirim Hub'ı Örneği]'nden indirmenizi öneririz.

Chrome Uygulaması JavaScript aracılığıyla oluşturulur ve bunu oluşturmak için tercih ettiğiniz herhangi bir sözcük düzenleyicisini kullanabilirsiniz. Aşağıdaki resimde Chrome Uygulamasının nasıl göründüğü gösterilmiştir:

![Google Chrome Uygulaması][15]

1. Bir klasör oluşturun ve `ChromePushApp` olarak adlandırın. Bu rastgele bir addır. Klasörü farklı adlandırırsanız gerekli kod kesimlerinde yolu değiştirdiğinizden emin olun.
2. İkinci adımda oluşturduğunuz klasöre [crypto-js kitaplığı]'nı indirin. Bu kitaplık klasörü iki alt klasör içerir: `components` ve `rollups`.
3. Bir `manifest.json` dosyası oluşturun. Tüm Chrome Uygulamaları, uygulama meta verilerini ve en önemlisi, kullanıcı yüklediğinde uygulamaya verilen tüm izinleri içeren bir bildirim dosyası tarafından desteklenir.

    ```json
    {
        "name": "NH-GCM Notifications",
        "description": "Chrome platform app.",
        "manifest_version": 2,
        "version": "0.1",
        "app": {
        "background": {
            "scripts": ["background.js"]
        }
        },
        "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
        "icons": { "128": "gcm_128.png" }
    }
    ```

    Bu Chrome Uygulamasının GCM'den anında iletme bildirimleri almasına izin verildiğini belirten `permissions` öğesine dikkat edin. Örnek uygulama ayrıca özgün GCM örneğinden yeniden kullanılan kaynakta bulabileceğiniz bir simge dosyası (`gcm_128.png`) kullanır. Bunu, [simge ölçütleri](https://developer.chrome.com/apps/manifest/icons)'ne uygun herhangi bir görüntü ile değiştirebilirsiniz.
4. Aşağıda kod ile `background.js` adlı bir dosya oluşturun:

    ```javascript
    // Returns a new notification ID used in the notification.
    function getNotificationId() {
        var id = Math.floor(Math.random() * 9007199254740992) + 1;
        return id.toString();
    }

    function messageReceived(message) {
        // A message is an object with a data property that
        // consists of key-value pairs.

        // Concatenate all key-value pairs to form a display string.
        var messageString = "";
        for (var key in message.data) {
        if (messageString != "")
            messageString += ", "
        messageString += key + ":" + message.data[key];
        }
        console.log("Message received: " + messageString);

        // Pop up a notification to show the GCM message.
        chrome.notifications.create(getNotificationId(), {
        title: 'GCM Message',
        iconUrl: 'gcm_128.png',
        type: 'basic',
        message: messageString
        }, function() {});
    }

    var registerWindowCreated = false;

    function firstTimeRegistration() {
        chrome.storage.local.get("registered", function(result) {

        registerWindowCreated = true;
        chrome.app.window.create(
            "register.html",
            {  width: 520,
                height: 500,
                frame: 'chrome'
            },
            function(appWin) {}
        );
        });
    }

    // Set up a listener for GCM message event.
    chrome.gcm.onMessage.addListener(messageReceived);

    // Set up listeners to trigger the first-time registration.
    chrome.runtime.onInstalled.addListener(firstTimeRegistration);
    chrome.runtime.onStartup.addListener(firstTimeRegistration);
    ```

    Bu dosya Chrome uygulamasının pencere HTML'sini (`register.html`) ve ayrıca işleyicisini tanımlar `messageReceived` gelen anında iletme bildirimini işlemek için.
5. `register.html` adlı, Chrome Uygulamasının kullanıcı arabirimini tanımlayan bir dosya oluşturun.

   > [!NOTE]
   > Bu örnekte **CryptoJS v3.1.2** kullanılır. Kitaplığın başka bir sürümünü indirdiyseniz `src` yolunda sürümü düzgün şekilde değiştirdiğinizden emin olun.

    ```html
    <html>
        <head>
            <title>GCM Registration</title>
            <script src="register.js"></script>
            <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
            <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
        <body>
            Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
            <button id="registerWithGCM">Register with GCM</button>
            <br/>
            <br/>
            <br/>

            Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
            Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

            <br/>
            <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
            <br/>
            <br/>

            <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
        </body>
    </html>
    ```
6. Bu adımdaki kod ile `register.js` adlı bir dosya oluşturun. Bu dosya `register.html` arkasındaki betiği belirtir. Chrome Uygulamaları satır içi yürütmeye izin vermez. Bu nedenle kullanıcı arabiriminiz için ayrı bir destek betiği oluşturmanız gerekir.

    ```javascript
    var registrationId = "";
    var hubName        = "", connectionString = "";
    var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

    window.onload = function() {
        document.getElementById("registerWithGCM").onclick = registerWithGCM;  
        document.getElementById("registerWithNH").onclick = registerWithNH;
        updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
    }

    function updateLog(status) {
        currentStatus = document.getElementById("console").innerHTML;
        if (currentStatus != "") {
        currentStatus = currentStatus + "\n\n";
        }

        document.getElementById("console").innerHTML = currentStatus  + status;
    }

    function registerWithGCM() {
        var senderId = document.getElementById("senderId").value.trim();
        chrome.gcm.register([senderId], registerCallback);

        // Prevent register button from being clicked again before the registration finishes.
        document.getElementById("registerWithGCM").disabled = true;
    }

    function registerCallback(regId) {
        registrationId = regId;
        document.getElementById("registerWithGCM").disabled = false;

        if (chrome.runtime.lastError) {
        // When the registration fails, handle the error and retry the
        // registration later.
        updateLog("Registration failed: " + chrome.runtime.lastError.message);
        return;
        }

        updateLog("Registration with GCM succeeded.");
        document.getElementById("registerWithNH").disabled = false;

        // Mark that the first-time registration is done.
        chrome.storage.local.set({registered: true});
    }

    function registerWithNH() {
        hubName = document.getElementById("hubName").value.trim();
        connectionString = document.getElementById("connectionString").value.trim();
        splitConnectionString();
        generateSaSToken();
        sendNHRegistrationRequest();
    }

    // From https://msdn.microsoft.com/library/dn495627.aspx
    function splitConnectionString()
    {
        var parts = connectionString.split(';');
        if (parts.length != 3)
        throw "Error parsing connection string";

        parts.forEach(function(part) {
        if (part.indexOf('Endpoint') == 0) {
        endpoint = 'https' + part.substring(11);
        } else if (part.indexOf('SharedAccessKeyName') == 0) {
        sasKeyName = part.substring(20);
        } else if (part.indexOf('SharedAccessKey') == 0) {
        sasKeyValue = part.substring(16);
        }
        });

        originalUri = endpoint + hubName;
    }

    function generateSaSToken()
    {
        targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
        var expiresInMins = 10; // 10 minute expiration

        // Set expiration in seconds.
        var expireOnDate = new Date();
        expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
        var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
        .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
        .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
        .getUTCSeconds()) / 1000;
        var tosign = targetUri + '\n' + expires;

        // Using CryptoJS.
        var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
        var base64signature = signature.toString(CryptoJS.enc.Base64);
        var base64UriEncoded = encodeURIComponent(base64signature);

        // Construct authorization string.
        sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
    }

    function sendNHRegistrationRequest()
    {
        var registrationPayload =
        "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
        "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
            "<content type=\"application/xml\">" +
                "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                    "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                "</GcmRegistrationDescription>" +
            "</content>" +
        "</entry>";

        // Update the payload with the registration ID obtained earlier.
        registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

        var url = originalUri + "/registrations/?api-version=2014-09";
        var client = new XMLHttpRequest();

        client.onload = function () {
        if (client.readyState == 4) {
            if (client.status == 200) {
            updateLog("Notification Hub Registration successful!");
            updateLog(client.responseText);
            } else {
            updateLog("Notification Hub Registration did not succeed!");
            updateLog("HTTP Status: " + client.status + " : " + client.statusText);
            updateLog("HTTP Response: " + "\n" + client.responseText);
            }
        }
        };

        client.onerror = function () {
            updateLog("ERROR - Notification Hub Registration did not succeed!");
        }

        client.open("POST", url, true);
        client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
        client.setRequestHeader("Authorization", sasToken);
        client.setRequestHeader("x-ms-version", "2014-09");

        try {
            client.send(registrationPayload);
        }
        catch(err) {
            updateLog(err.message);
        }
    }
    ```

    Betik aşağıdaki anahtar parametrelere sahiptir:

   * `window.onload` Kullanıcı Arabirimi üzerindeki iki düğmenin düğme tıklama olaylarını tanımlar. İlk düğme tıklama olay işleyicisi GCM'ye kaydedilir. Diğeri, Azure Notification Hubs'a kaydedilmek üzere GCM'ye kayıttan sonra döndürülen kayıt kimliğini kullanır.
   * `updateLog` kod günlüğü bilgilerine imkan tanıyan işlevdir.
   * `registerWithGCM` yapan ilk düğme tıklama işleyicisidir `chrome.gcm.register` geçerli Chrome uygulaması örneğini kaydetmek için GCM'ye çağırın.
   * `registerCallback` GCM kayıt çağrısı döndürüldüğünde çağrılan geri çağırma işlevidir.
   * `registerWithNH` Notification Hubs ile kaydeden ikinci düğme tıklama işleyicisidir. `hubName` ve `connectionString` öğelerini (kullanıcı tarafından belirtilen) alır ve Notification Hubs Kayıt REST API'si çağrısını işler.
   * `splitConnectionString` ve `generateSaSToken` tüm REST API çağrılarında kullanılması gereken SaS belirteci oluşturma işleminin JavaScript uygulamasını temsil eden yardımcılardır. Daha fazla bilgi için bkz. [Ortak Kavramlar](https://msdn.microsoft.com/library/dn495627.aspx).
   * `sendNHRegistrationRequest` Azure Notification hubs'ı bir HTTP REST çağrısı yapan işlevdir.
   * `registrationPayload` Kayıt XML yükünü tanımlar. Daha fazla bilgi için bkz. [Kayıt NH REST API’si oluşturma]. Kayıt kimliğini GCM'den alınan değerle güncelleştirin.
   * `client` bir örneği olan `XMLHttpRequest` uygulamanın HTTP POST isteği yapmak için kullanır. `Authorization` üst bilgisini `sasToken` ile güncelleştirin. Bu çağrının başarıyla tamamlanması, bu Chrome Uygulaması örneğinin Azure Notification Hubs'a kaydedilmesini sağlar.

     Bu proje için genel klasör yapısı aşağıdaki yapısı gibi olmalıdır: ![Google Chrome uygulaması - klasör yapısı][21]

### <a name="set-up-and-test-your-chrome-app"></a>Chrome Uygulamanızı ayarlama ve test etme

1. Chrome tarayıcınızı açın. **Chrome uzantıları**'nı açın ve **Geliştirici modunu** etkinleştirin.

    ![Google Chrome - Geliştirici Modu'nu Etkinleştirme][16]
2. **Paketi açılmış uzantı yükle**'ye tıklayın ve dosyaları oluşturduğunuz klasöre gidin. İsteğe bağlı olarak **Chrome Apps & Extensions Developer Tool** (Chrome Uygulamaları ve Uzantıları Geliştirici Aracı) da kullanabilirsiniz. Bu araç kendi içinde bir Chrome Uygulamasıdır (Chrome Web Mağazası'ndan yüklenir) ve Chrome Uygulaması geliştirmeniz için gelişmiş hata ayıklama özellikleri sağlar.

    ![Google Chrome - Paketi Açılmış Uzantı Yükleme][17]
3. Chrome Uygulaması hatasız şekilde oluşturulduysa Chrome Uygulamanızın görüntülendiğini görürsünüz.

    ![Google Chrome - Chrome Uygulaması Ekranı][18]
4. Daha önce **Google Cloud Console**'dan aldığınız **Project Number (Proje Numarası)** değerini Gönderen Kimliği olarak girin ve **Register with GCM (GCM'ye Kaydet)** seçeneğine tıklayın. **Registration with GCM succeeded (GCM kaydı başarılı)** iletisini görmeniz gerekir.

    ![Google Chrome - Chrome Uygulamasını Özelleştirme][19]
5. **Notification Hub Name (Bildirim Hub'ı Adı)** değerini ve daha önce portaldan edindiğiniz **DefaultListenSharedAccessSignature** dizesini girin, ardından **Register with Azure Notification Hub (Azure Notification Hub'ına kaydet)** seçeneğine tıklayın. **Notification Hub Registration successful! (Notification Hub'ı Kaydı başarılı!)** iletisini ve Azure Notification Hubs kayıt kimliğini içeren kayıt yanıtının ayrıntılarını görmeniz gerekir.

    ![Google Chrome - Notification Hub'ı Ayrıntılarını Belirtme][20]  

## <a name="send"></a>Chrome Uygulamanıza bildirim gönderme

Test amacıyla, bir .NET konsol uygulaması kullanarak Chrome anında iletme bildirimleri gönderin.

> [!NOTE]
> Notification Hubs ile herhangi bir arka uçtan genel [REST arabirimi](https://msdn.microsoft.com/library/windowsazure/dn223264.aspx) aracılığıyla anında iletme bildirimleri gönderebilirsiniz. Daha fazla platformlar arası örnek için [belge portalına](https://azure.microsoft.com/documentation/services/notification-hubs/) göz atın.

1. Visual Studio'da, **Dosya** menüsünden **Yeni**'yi ve ardından **Proje**'yi seçin. **Visual C#** altında, **Windows** ve **Konsol Uygulaması**'na, ardından **Tamam**'a tıklayın.  Bu adım, yeni bir konsol uygulama projesi oluşturur.
2. **Araçlar** menüsünden **NuGet Paket Yöneticisi**’ne ve ardından **Paket Yöneticisi Konsolu**’na tıklayın. Alttaki pencerede Paket Yöneticisi Konsolu görüntülenir.
3. Konsol penceresinde aşağıdaki komutu yürütün:

    ```shell
    Install-Package Microsoft.Azure.NotificationHubs
    ```

   Azure Service Bus SDK'sıyla başvuru [WindowsAzure.ServiceBus NuGet paketini otomatik olarak projeye eklenir](https://nuget.org/packages/WindowsAzure.ServiceBus/).
4. `Program.cs` öğesini açın ve aşağıdaki `using` deyimini ekleyin:

    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```
5. `Program` sınıfında aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async void SendNotificationAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
        String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
        await hub.SendGcmNativeNotificationAsync(message);
    }
    ```

    `<hub name>` yer tutucusunu, Bildirim Hub’ı sayfanızdaki [portalda](https://portal.azure.com) görünen bildirim hub’ının adıyla değiştirdiğinizden emin olun. Ayrıca, bağlantı dizesi yer tutucusunu, bildirim hub'ı yapılandırma bölümünde edindiğiniz `DefaultFullSharedAccessSignature` adlı bağlantı dizesi ile değiştirin.

    > [!NOTE]
    > Bağlantı dizesini **Dinleme** erişimi ile değil, **Tam** erişim ile kullandığınızdan emin olun. **Dinleme** erişimi bağlantı dizesi, anında iletme bildirimlerinin gönderilmesi için izin vermez.

6. Aşağıdaki çağrıları `Main` yönteminde ekleyin:

    ```csharp
    SendNotificationAsync();
    Console.ReadLine();
    ```

7. Chrome'un çalıştığından emin olun ve konsol uygulamasını çalıştırın.
8. Masaüstünüzde aşağıdaki bildirim açılır penceresini görmeniz gerekir.

    ![Google Chrome - Bildirim][13]
9. Ayrıca Chrome çalışırken, görev çubuğunda (Windows'ta) Chrome Bildirimler penceresini kullanarak da tüm bildirimlerinizi görebilirsiniz.

    ![Google Chrome - Bildirimler Listesi](./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png)

> [!NOTE]
> Tarayıcıda Chrome Uygulamasının çalışıyor veya açık olması gerekmez (Ancak Chrome tarayıcısının çalışıyor olması gerekir). Tüm bildirimlerinizin birleştirilmiş görünümünü de Chrome Bildirimler penceresinde görebilirsiniz.

## <a name="next-steps"> </a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm istemcilere yayın bildirimleri gönderdiniz. Belirli cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome Uygulaması Bildirim Hub'ı Örneği]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Notification Hubs Overview]: notification-hubs-push-notification-overview.md
[Chrome Uygulamalarına Genel Bakış]: https://developer.chrome.com/apps/about_apps
[Chrome Uygulaması GCM Örneği]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Mobil Cihazlarda Chrome Uygulamaları]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Kayıt NH REST API’si oluşturma]: https://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js kitaplığı]: https://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
