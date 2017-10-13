---
title: "Azure Notification Hubs ile Chrome uygulamalarına anında iletme bildirimleri gönderme | Microsoft Belgeleri"
description: "Bir Chrome Uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenin."
services: notification-hubs
keywords: "mobil anında iletme bildirimleri,anında iletme bildirimleri,anında iletme bildirimi,chrome anında iletme bildirimleri"
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 600b1b7e5f3987c9a0acc33b7049f7118442b931
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Azure Notification Hubs ile Chrome uygulamalarına anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu konu başlığı altında, Google Chrome tarayıcısı bağlamında görüntülenen bir Chrome Uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağı gösterilir. Bu öğreticide, [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) kullanarak anında iletme bildirimleri alan bir Chrome uygulaması oluşturacağız. 

> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

Bu öğretici, anında iletme bildirimlerini etkinleştirmek için şu temel adımlarda size yol gösterir:

* [Google Cloud Messaging'i etkinleştirme](#register)
* [Bildirim hub'ınızı yapılandırma](#configure-hub)
* [Chrome Uygulamanızı bildirim hub'ına bağlama](#connect-app)
* [Chrome Uygulamanıza anında iletme bildirimi gönderme](#send)
* [Ek işlevler ve özellikler](#next-steps)

> [!NOTE]
> Chrome uygulaması anında iletme bildirimleri, genel tarayıcı içi bildirimler olmayıp tarayıcı genişletilebilirlik modeline özgüdür (ayrıntılar için bkz. [Chrome Uygulamalarına Genel Bakış]). Chrome uygulamaları, masaüstü tarayıcısının yanı sıra Apache Cordova aracılığıyla mobil cihazlarda (Android ve iOS) da çalıştırılır. Daha fazla bilgi için bkz. [Mobil Cihazlarda Chrome Uygulamaları].
> 
> 

[Google Cloud Messaging for Chrome]'u kullanım dışı bırakıldığı ve aynı GCM artık hem Android cihazları hem Chrome örneklerini desteklediği için GCM ve Azure Notification Hubs yapılandırması Android için yapılandırma ile aynıdır.

## <a id="register"></a>Google Cloud Messaging'i etkinleştirme
1. [Google Cloud Console] web sitesine gidin, Google hesabı kimlik bilgilerinizle oturum açın ve ardından **Create Project** (Proje Oluştur) düğmesine tıklayın. Uygun bir **Project Name** (Proje Adı) sağlayın ve ardından **Create** (Oluştur) düğmesine tıklayın.
   
       ![Google Cloud Console - Create Project][1]
2. Oluşturduğunuz proje için **Projects** (Projeler) sayfasındaki **Project Number**'ı (Proje Numarası) not edin. Bunu, Chrome Uygulamasında GCM'ye kayıt için **GCM Sender ID** (GCM Gönderen Kimliği) olarak kullanacaksınız.
   
       ![Google Cloud Console - Project Number][2]
3. Sol bölmede **APIs & auth** (API'ler ve kimlik doğrulama) seçeneğine tıklayın, ardından aşağı kaydırın ve **Google Cloud Messaging for Android**'i etkinleştirmek için iki durumlu düğmeye tıklayın. **Google Cloud Messaging for Chrome**'u etkinleştirmeniz gerekmez.
   
       ![Google Cloud Console - Server Key][3]
4. Sol bölmede **Credentials (Kimlik Bilgileri)**  > **Create New Key (Yeni Anahtar Oluştur)**  > **Server Key (Sunucu Anahtarı)**  > **Create (Oluştur)** seçeneklerine tıklayın.
   
       ![Google Cloud Console - Credentials][4]
5. Sunucu **API Key**'ini (API Anahtarı) not edin. Daha sonra, bunu bildirim hub'ınızda yapılandırarak GCM'ye anında iletme bildirimleri göndermek üzere etkinleştirebilirsiniz.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>Bildirim hub'ınızı yapılandırma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   **Ayarlar** dikey penceresinde, **Bildirim Hizmetleri**'ni ve ardından **Google (GCM)** seçeneğini belirleyin. API anahtarını girin ve kaydedin.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Chrome Uygulamanızı bildirim hub'ına bağlama
Bildirim hub'ınız şimdi GCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve göndermesi için kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz. LK

### <a name="create-a-new-chrome-app"></a>Yeni bir Chrome Uygulaması oluşturma
Aşağıdaki örnek, [Chrome Uygulaması GCM Örneği]'ni temel alır ve Chrome Uygulaması oluşturmak için önerilen yöntemi kullanır. Özellikle Azure Notification Hubs ile ilgili olan adımları vurgulayacağız. 

> [!NOTE]
> Bu Chrome Uygulamasının kaynağını [Chrome Uygulaması Bildirim Hub'ı Örneği]'nden indirmenizi öneririz.
> 
> 

Chrome Uygulaması JavaScript aracılığıyla oluşturulur ve bunu oluşturmak için tercih ettiğiniz herhangi bir sözcük düzenleyicisini kullanabilirsiniz. Aşağıda, bu Chrome Uygulamasının nasıl görüneceği gösterilmektedir.

![Google Chrome Uygulaması][15]

1. Bir klasör oluşturun ve `ChromePushApp` olarak adlandırın. Elbette, bu rastgele bir addır. Klasörü farklı adlandırırsanız gerekli kod kesimlerinde yolu değiştirdiğinizden emin olun.
2. İkinci adımda oluşturduğunuz klasöre [crypto-js kitaplığı]'nı indirin. Bu kitaplık klasörü iki alt klasör içerir: `components` ve `rollups`.
3. Bir `manifest.json` dosyası oluşturun. Tüm Chrome Uygulamaları, uygulama meta verilerini ve en önemlisi, kullanıcı yüklediğinde uygulamaya verilen tüm izinleri içeren bir bildirim dosyası tarafından desteklenir.
   
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
   
    Bu Chrome Uygulamasının GCM'den anında iletme bildirimleri alabileceğini belirten `permissions` öğesine dikkat edin. Bunun aynı zamanda, Chrome Uygulamasının kaydetmek için REST çağrısı yapacağı Azure Notification Hubs URI'sini belirtmesi de gerekir.
    Bizim örnek uygulamamız da özgün GCM örneğinden yeniden kullanılan kaynakta bulabileceğiniz bir simge dosyası (`gcm_128.png`) kullanır. Bunu, [simge ölçütleri](https://developer.chrome.com/apps/manifest/icons)'ne uygun herhangi bir görüntü ile değiştirebilirsiniz.
4. Aşağıda kod ile `background.js` adlı bir dosya oluşturun:
   
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
   
    Bu dosya, Chrome Uygulama penceresi HTML'sini açar (**register.html**) ve aynı zamanda gelen anında iletme bildirimini işlemek için **messageReceived** işleyicisini tanımlar.
5. `register.html` adlı bir dosya oluşturun. Bu, Chrome Uygulamasının kullanıcı arabirimini tanımlar. 
   
   > [!NOTE]
   > Bu örnekte **CryptoJS v3.1.2** kullanılır. Kitaplığın başka bir sürümünü indirdiyseniz `src` yolunda sürümü düzgün şekilde değiştirdiğinizden emin olun.
   > 
   > 
   
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
6. Aşağıda kod ile `register.js` adlı bir dosya oluşturun. Bu dosya `register.html` arkasındaki betiği belirtir. Chrome Uygulamaları satır içi yürütmeye izin vermez. Bu nedenle kullanıcı arabiriminiz için ayrı bir destek betiği oluşturmanız gerekir.
   
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
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
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
                updateLog("Notification Hub Registration succesful!");
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
   
    Yukarıdaki betik aşağıdaki anahtar parametrelere sahiptir:
   
   * **window.onLoad**, kullanıcı arabirimi üzerindeki iki düğmenin düğme tıklama olaylarını tanımlar. Biri GCM'ye kaydedilir. Diğeri, Azure Notification Hubs'a kaydedilmek üzere GCM'ye kayıttan sonra döndürülen kayıt kimliğini kullanır.
   * **updateLog** basit günlüğe kaydetme özelliklerini kullanmamızı sağlayan işlevdir.
   * **registerWithGCM** geçerli Chrome Uygulaması örneğini kaydetmek için GCM'ye `chrome.gcm.register` çağrısı yapan ilk düğme tıklama işleyicisidir.
   * **registerCallback**, GCM kayıt çağrısı döndürüldüğünde çağrılan geri çağırma işlevidir.
   * **registerWithNH**, Notification Hubs'a kaydedilen ikinci düğme tıklama işleyicisidir. `hubName` ve `connectionString` öğelerini (kullanıcı tarafından belirtilen) alır ve Notification Hubs Kayıt REST API'si çağrısını işler.
   * **splitConnectionString** ve **generateSaSToken**, tüm REST API çağrılarında kullanılması gereken SaS belirteci oluşturma işleminin JavaScript uygulamasını temsil eden yardımcılardır. Daha fazla bilgi için bkz. [Ortak Kavramlar](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest**, Azure Notification Hubs'a HTTP REST çağrısı yapan işlevdir.
   * **registrationPayload** kayıt XML yükünü tanımlar. Daha fazla bilgi için bkz. [Kayıt NH REST API’si oluşturma]. Burada, GCM'den aldıklarımız ile kayıt kimliğini güncelleştiririz.
   * **client**, HTTP POST isteği yapmak için kullandığımız bir **XMLHttpRequest** örneğidir. `Authorization` üst bilgisini `sasToken` ile güncelleştirdiğimizi unutmayın. Bu çağrının başarıyla tamamlanması, bu Chrome Uygulaması örneğinin Azure Notification Hubs'a kaydedilmesini sağlar.

Bu proje için genel klasör yapısı şuna benzemelidir: ![Google Chrome Uygulaması - Klasör Yapısı][21]

### <a name="set-up-and-test-your-chrome-app"></a>Chrome Uygulamanızı ayarlama ve test etme
1. Chrome tarayıcınızı açın. **Chrome uzantıları**'nı açın ve **Geliştirici modunu** etkinleştirin.
   
       ![Google Chrome - Enable Developer Mode][16]
2. **Paketi açılmış uzantı yükle**'ye tıklayın ve dosyaları oluşturduğunuz klasöre gidin. İsteğe bağlı olarak **Chrome Apps & Extensions Developer Tool** (Chrome Uygulamaları ve Uzantıları Geliştirici Aracı) da kullanabilirsiniz. Bu araç kendi içinde bir Chrome Uygulamasıdır (Chrome Web Mağazası'ndan yüklenir) ve Chrome Uygulaması geliştirmeniz için gelişmiş hata ayıklama özellikleri sağlar.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Chrome Uygulaması hatasız şekilde oluşturulduysa Chrome Uygulamanızın gösterildiğini göreceksiniz.
   
       ![Google Chrome - Chrome App Display][18]
4. Daha önce **Google Cloud Console**'dan aldığınız **Project Number (Proje Numarası)** değerini Gönderen Kimliği olarak girin ve **Register with GCM (GCM'ye Kaydet)** seçeneğine tıklayın. **Registration with GCM succeeded (GCM kaydı başarılı)** iletisini görmeniz gerekir.
   
       ![Google Chrome - Chrome App Customization][19]
5. **Notification Hub Name (Bildirim Hub'ı Adı)** değerini ve daha önce portaldan edindiğiniz **DefaultListenSharedAccessSignature** dizesini girin, ardından **Register with Azure Notification Hub (Azure Notification Hub'ına kaydet)** seçeneğine tıklayın. **Notification Hub Registration successful! (Notification Hub'ı Kaydı başarılı!)** iletisini ve Azure Notification Hubs kayıt kimliğini içeren kayıt yanıtının ayrıntılarını görmeniz gerekir.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Chrome Uygulamanıza bildirim gönderme
Test amacıyla, bir .NET konsol uygulaması kullanarak size Chrome anında iletme bildirimleri göndereceğiz. 

> [!NOTE]
> Notification Hubs ile Ortak <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST arabirimimizi</a> kullanarak herhangi bir arka uçtan anında iletme bildirimleri gönderebilirsiniz. Daha fazla platformlar arası örnek için [belge portalımıza](https://azure.microsoft.com/documentation/services/notification-hubs/) göz atın.
> 
> 

1. Visual Studio'da, **Dosya** menüsünden **Yeni**'yi ve ardından **Proje**'yi seçin. **Visual C#** altında, **Windows** ve **Konsol Uygulaması**'na, ardından **Tamam**'a tıklayın.  Bu, yeni bir konsol uygulama projesi oluşturur.
2. **Araçlar** menüsünde, **Kitaplık Paket Yöneticisi**'ne ve ardından **Paket Yöneticisi Konsolu**'na tıklayın. Bu, Paket Yöneticisi Konsolu'nu görüntüler.
3. Konsol penceresinde aşağıdaki komutu yürütün:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference to the Azure Service Bus SDK with the <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. `Program.cs` öğesini açın ve aşağıdaki `using` deyimini ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
5. `Program` sınıfında aşağıdaki yöntemi ekleyin:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace the connection string placeholder with the connection string called `DefaultFullSharedAccessSignature` that you obtained in the notification hub configuration section.
   
   > [!NOTE]
   > Bağlantı dizesini **Dinleme** erişimi ile değil, **Tam** erişim ile kullandığınızdan emin olun. **Dinleme** erişimi bağlantı dizesi, anında iletme bildirimlerinin gönderilmesi için izin vermez.
   > 
   > 
6. Aşağıdaki çağrıları `Main` yönteminde ekleyin:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Chrome'un çalıştığından emin olun ve konsol uygulamasını çalıştırın.
8. Masaüstünüzde aşağıdaki bildirim açılır penceresini görmeniz gerekir.
   
       ![Google Chrome - Notification][13]
9. Ayrıca Chrome çalışırken, görev çubuğunda (Windows'ta) Chrome Bildirimler penceresini kullanarak da tüm bildirimlerinizi görebilirsiniz.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Tarayıcıda Chrome Uygulamasının çalışıyor veya açık olması gerekmez (Ancak Chrome tarayıcısının çalışıyor olması gerekir). Tüm bildirimlerinizin birleştirilmiş görünümünü de Chrome Bildirimler penceresinde görebilirsiniz.
> 
> 

## <a name="next-steps"> </a>Sonraki adımlar
[Notification Hubs'a Genel Bakış]'ta Notification Hubs hakkında daha fazla bilgi edinin.

Belirli kullanıcıları hedeflemek için [Azure Notification Hubs Kullanıcılara Bildirme] öğreticisine bakın. 

Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Azure Notification Hubs son dakika haberleri] öğreticisini izleyebilirsiniz.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
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
[Google Cloud Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs'a Genel Bakış]: notification-hubs-push-notification-overview.md
[Chrome Uygulamalarına Genel Bakış]: https://developer.chrome.com/apps/about_apps
[Chrome Uygulaması GCM Örneği]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Mobil Cihazlarda Chrome Uygulamaları]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Kayıt NH REST API’si oluşturma]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js kitaplığı]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Kullanıcılara Bildirme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs son dakika haberleri]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
