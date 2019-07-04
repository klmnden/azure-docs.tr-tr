---
title: Kayıt Yönetimi
description: Bu konuda, anında iletme bildirimleri almak için notification hubs ile cihazları kaydetmeniz açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.author: jowargo
ms.date: 04/08/2019
ms.openlocfilehash: fffa6784702f239e0af0e9e88a4b9937d20b86ed
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67488639"
---
# <a name="registration-management"></a>Kayıt yönetimi

## <a name="overview"></a>Genel Bakış

Bu konuda, anında iletme bildirimleri almak için notification hubs ile cihazları kaydetmeniz açıklanmaktadır. Konu kayıtları yüksek düzeyde açıklanır ve cihazlar kaydetmek için iki ana desen tanıtır: bildirim hub'ına doğrudan CİHAZDAN kaydetme ve bir uygulama arka ucu kaydediliyor.

## <a name="what-is-device-registration"></a>Cihaz kaydı nedir

Bildirim hub'ı ile cihaz kaydı kullanılarak gerçekleştirilir bir **kayıt** veya **yükleme**.

### <a name="registrations"></a>Kayıtlar

Bir kayıt Platform bildirim sistemi (PNS) tanıtıcı bir cihaz için etiketleri ve büyük olasılıkla bir şablon ile ilişkilendirir. PNS tanıtıcısının Channelurı, cihaz belirtecini ve FCM kayıt kimliği olabilir. Etiketler, cihaz tanıtıcılarını doğru kümesine bildirimleri yönlendirmek için kullanılır. Daha fazla bilgi için [Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md). Şablonları kayıt başına dönüştürme uygulamak için kullanılır. Daha fazla bilgi için bkz. [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md).

> [!NOTE]
> Azure Notification Hubs, cihaz başına 60 etiketler en fazla destekler.

### <a name="installations"></a>Yüklemeleri

Bir yükleme geliştirilmiş olan bir anında iletme paketi içeren bir kaydı ilgili özellikler. Bunu, cihazları kaydetme en son ve en iyi yaklaşımdır. Ancak, istemci tarafı .NET SDK'sı tarafından desteklenmez ([arka uç işlemleri için bildirim hub'ı SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) henüz dağıtmadığı.  Bu istemci CİHAZDAN kaydediyorsanız, haritamın kullanmak anlamına gelir [Notification Hubs REST API'si](https://docs.microsoft.com/rest/api/notificationhubs/create-overwrite-installation) yüklemelerini desteklemek için bir yaklaşım. Arka uç hizmetini kullanıyorsanız, kullanılacak erişebileceğinizi [arka uç işlemleri için bildirim hub'ı SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Yüklemeleri kullanmanın bazı temel avantajları şunlardır:

- Oluşturma veya güncelleştirme yüklemesi tam olarak etkilidir. Bu nedenle tüm yinelenen kayıtları DB'dir olmadan deneyebilirsiniz.
- Özel Etiket biçimi yükleme modelini destekler (`$InstallationId:{INSTALLATION_ID}`) söz konusu cihaz için doğrudan bildirim göndererek sağlar. Örneğin, bir yükleme kimliği, uygulamanın kodunu ayarlar `joe93developer` özel bu cihaz için bir geliştirici bu cihaz için bir bildirim gönderirken hedefleyebilirsiniz `$InstallationId:{joe93developer}` etiketi. Bu, ek bir kodlama yapmak zorunda kalmadan belirli bir cihazı hedeflemeniz sağlar.
- Yüklemelerini kullanarak, kısmi kayıt güncelleştirmeler yapmak de sağlar. PATCH kullanılarak yöntemi ile kısmi güncelleştirme yüklemesinin istenen [JSON-Patch standart](https://tools.ietf.org/html/rfc6902). Kayıt etiketleri güncelleştirmek istediğiniz durumlarda kullanışlıdır. Tüm kayıt çekin ve ardından önceki tüm etiketleri yeniden yeniden gerekmez.

Yüklemesi aşağıdaki özellikleri içerebilir. Yükleme özellikleri hakkında tam listesi için bkz. [oluşturun veya REST API ile yüklemesi üzerine](https://docs.microsoft.com/rest/api/notificationhubs/create-overwrite-installation) veya [yükleme özellikleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.installation).

```json
// Example installation format to show some supported properties
{
    installationId: "",
    expirationTime: "",
    tags: [],
    platform: "",
    pushChannel: "",
    ………
    templates: {
        "templateName1" : {
            body: "",
            tags: [] },
        "templateName2" : {
            body: "",
            // Headers are for Windows Store only
            headers: {
                "X-WNS-Type": "wns/tile" }
            tags: [] }
    },
    secondaryTiles: {
        "tileId1": {
            pushChannel: "",
            tags: [],
            templates: {
                "otherTemplate": {
                    bodyTemplate: "",
                    headers: {
                        ... }
                    tags: [] }
            }
        }
    }
}
```

> [!NOTE]
> Varsayılan olarak, kayıtları ve yüklemelerini dolmaz.

Kayıtları ve yüklemelerini her cihaz/kanal için geçerli bir PNS tanıtıcısını içermelidir. PNS tanıtıcılarını kaydetmekten, cihazda bir istemci uygulamasında yalnızca alınabilir olduğundan, bir istemci uygulaması ile bir cihazda doğrudan kayıt modelidir. Öte yandan, güvenlik hususlarının yanı sıra etiketlerle ilgili iş mantığı uygulama arka ucu, cihaz kaydını Yönet gerektirebilir.

> [!NOTE]
> (Kayıtları API yapmasına rağmen) yüklemeleri API Baidu hizmet desteklemez. 

### <a name="templates"></a>Şablonlar

Kullanmak istiyorsanız [şablonları](notification-hubs-templates-cross-platform-push-messages.md), cihaz yüklemesi de bu cihaz bir JSON ile ilişkili tüm şablonları tutan biçimlendirmek (Yukarıdaki örnek bakın). Şablon adları, aynı cihaza için hedef farklı şablonlar yardımcı olur.

Her bir şablon adı, şablon gövdesi ve isteğe bağlı bir etiket kümesine eşler. Ayrıca, her platformun ek şablon özelliklere sahip olabilir. Windows Store (WNS kullanarak) ve Windows Phone 8 (MPNS kullanarak) için ek bir üst kümesi şablonunun bir parçası olabilir. APNs söz konusu olduğunda, bir sabit veya şablon ifadesi bir sona erme özelliğini ayarlayabilirsiniz. Yükleme özellikleri bakın tam listesi için [oluşturmak veya bir yükleme ile REST üzerine](https://docs.microsoft.com/rest/api/notificationhubs/create-overwrite-installation) konu.

### <a name="secondary-tiles-for-windows-store-apps"></a>Windows Store uygulamaları için ikincil kutucuk

Windows Store istemci uygulamalar için ikincil kutucuk bildirimleri göndermek için birincil olanı göndererek ile aynı olur. Bu ayrıca yüklemelerde desteklenir. İkincil kutucuk üzerindeki istemci uygulamanızı SDK şeffaf bir şekilde işleyen farklı bir Channelurı vardır.

Windows Store uygulamanızda SecondaryTiles nesne oluşturmak için kullanılan aynı TileId SecondaryTiles sözlük kullanır. Birincil Channelurı gibi ile ikincil kutucuk ChannelUris her an değiştirebilirsiniz. Güncelleştirilen bildirim hub'ında yüklemeleri tutulabilmesi için cihaz bunları ikincil kutucuk geçerli ChannelUris ile yenilemeniz gerekir.

## <a name="registration-management-from-the-device"></a>Cihaz kayıt yönetimi

Cihaz kaydı istemci uygulamalardan yönetirken, arka uç yalnızca bildirimleri göndermekten sorumludur. İstemci uygulamaları PNS tanıtıcılarını kaydetmekten güncel tutun ve etiketleri kaydedin. Aşağıdaki resimde, bu düzen gösterilmiştir.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Cihaz ilk PNS tanıtıcısının PNS alır ve sonra bildirim hub'ı doğrudan kaydeder. Kayıt başarılı olduktan sonra uygulama arka ucu, kayıt hedefleyen bir bildirim gönderebilirsiniz. Bildirimleri gönderme hakkında daha fazla bilgi için bkz. [Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md).

Bu durumda, notification hubs'ı CİHAZDAN erişmek için yalnızca dinleme haklarına kullanın. Daha fazla bilgi için [güvenlik](notification-hubs-push-notification-security.md).

CİHAZDAN kaydetme en basit yöntemdir, ancak bazı kısıtlamaları vardır:

- Uygulama etkin olduğunda, bir istemci uygulaması yalnızca kendi etiketleri güncelleştirebilirsiniz. Bir kullanıcının etiketleri Spor takımlar için ek bir etiket (örneğin, Seahawks) için ilk cihaz kaydederken ilgili kaydetme iki cihazı varsa, uygulamanın ikinci bir cihaz üzerinde olana kadar gibi ikinci bir cihaz Seahawks hakkında bildirimler almaz ikinci kez yürütüldü. Etiketler, birden çok cihaz tarafından etkilenir, daha genel olarak, etiketleri arka ucundan yönetme bir arzu seçenektir.
- Uygulamaları uğrayabileceği olduğundan, kayıt için belirli etiketlere güvenli hale getirme çok dikkatli "etiketi düzeyi güvenliği" bölümünde açıklandığı gibi gerektirir

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Bir bildirim hub'ından yükleme kullanarak bir cihazı kaydetmek için örnek kod

Şu anda bu yalnızca kullanarak desteklenir [Notification Hubs REST API'si](https://docs.microsoft.com/rest/api/notificationhubs/create-overwrite-installation).

PATCH kullanılarak yöntemi de kullanabilirsiniz [JSON-Patch standart](https://tools.ietf.org/html/rfc6902) yükleme güncelleştirmek için.

```
class DeviceInstallation
{
    public string installationId { get; set; }
    public string platform { get; set; }
    public string pushChannel { get; set; }
    public string[] tags { get; set; }
}

private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
        string hubName, string listenConnectionString)
{
    if (deviceInstallation.installationId == null)
        return HttpStatusCode.BadRequest;

    // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
    ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
    string hubResource = "installations/" + deviceInstallation.installationId + "?";
    string apiVersion = "api-version=2015-04";

    // Determine the targetUri that we will sign
    string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

    //=== Generate SaS Security Token for Authorization header ===
    // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
    string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

    using (var httpClient = new HttpClient())
    {
        string json = JsonConvert.SerializeObject(deviceInstallation);

        httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

        var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
        return response.StatusCode;
    }
}

var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

string installationId = null;
var settings = ApplicationData.Current.LocalSettings.Values;

// If we have not stored an installation id in application data, create and store as application data.
if (!settings.ContainsKey("__NHInstallationId"))
{
    installationId = Guid.NewGuid().ToString();
    settings.Add("__NHInstallationId", installationId);
}

installationId = (string)settings["__NHInstallationId"];

var deviceInstallation = new DeviceInstallation
{
    installationId = installationId,
    platform = "wns",
    pushChannel = channel.Uri,
    //tags = tags.ToArray<string>()
};

var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                    "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

if (statusCode != HttpStatusCode.Accepted)
{
    var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
    dialog.Commands.Add(new UICommand("OK"));
    await dialog.ShowAsync();
}
else
{
    var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
    dialog.Commands.Add(new UICommand("OK"));
    await dialog.ShowAsync();
}
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Bir bildirim hub'ından bir kaydı'nı kullanarak bir cihazı kaydetmek için örnek kod

Bu yöntemler veya bir kayıt üzerinde çağrılır cihaz için güncelleştirilemiyor. Başka bir deyişle, tanıtıcı veya etiketleri güncelleştirmek için tüm kayıt üzerine yazmalıdır. Her zaman belirli bir cihazın geçerli etiketlere sahip güvenilir bir depo olmalıdır böylece kayıtları geçici olduğunu unutmayın.

```
// Initialize the Notification Hub
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

// The Device id from the PNS
var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

// If you are registering from the client itself, then store this registration id in device
// storage. Then when the app starts, you can check if a registration id already exists or not before
// creating.
var settings = ApplicationData.Current.LocalSettings.Values;

// If we have not stored a registration id in application data, store in application data.
if (!settings.ContainsKey("__NHRegistrationId"))
{
    // make sure there are no existing registrations for this push handle (used for iOS and Android)    
    string newRegistrationId = null;
    var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
    foreach (RegistrationDescription registration in registrations)
    {
        if (newRegistrationId == null)
        {
            newRegistrationId = registration.RegistrationId;
        }
        else
        {
            await hub.DeleteRegistrationAsync(registration);
        }
    }

    newRegistrationId = await hub.CreateRegistrationIdAsync();

    settings.Add("__NHRegistrationId", newRegistrationId);
}

string regId = (string)settings["__NHRegistrationId"];

RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
registration.RegistrationId = regId;
registration.Tags = new HashSet<string>(YourTags);

try
{
    await hub.CreateOrUpdateRegistrationAsync(registration);
}
catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
{
    settings.Remove("__NHRegistrationId");
}
```

## <a name="registration-management-from-a-backend"></a>Kayıt Yönetimi'nden bir arka uç

Arka ucunuzdan kayıtları yönetme, ek kod yazmaya gerektirir. Cihaz uygulamasından güncelleştirilmiş PNS (etiketler ve şablonlar ile birlikte) uygulamayı başlatır her zaman arka ucuna işlemek ve arka uç, bildirim hub'ı Bu tutamacı güncelleştirmelisiniz sağlamanız gerekir. Bu tasarım aşağıdaki resimde gösterilmektedir.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Kayıtları yönetme arka ucundan avantajları karşılık gelen uygulama cihaz üzerinde etkin olsa bile kayıtları etiketleri değiştirmek ve kayıt için bir etiket eklemeden önce istemci uygulaması kimlik doğrulaması için mevcuttur.

### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Bir bildirim hub'ından yükleme kullanarak bir arka uç ile kaydetmek için örnek kod

İstemci cihaz yine de önce ilgili yüklemeyi özellikler olarak ve PNS tanıtıcısını alır ve bir özel API kaydını gerçekleştirme ve etiketleri vb. yetkilendirmek arka uçta çağırır. Arka uç yararlanabilir [arka uç işlemleri için bildirim hub'ı SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

PATCH kullanılarak yöntemi de kullanabilirsiniz [JSON-Patch standart](https://tools.ietf.org/html/rfc6902) yükleme güncelleştirmek için.

```
// Initialize the Notification Hub
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

// Custom API on the backend
public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
{

    Installation installation = new Installation();
    installation.InstallationId = deviceUpdate.InstallationId;
    installation.PushChannel = deviceUpdate.Handle;
    installation.Tags = deviceUpdate.Tags;

    switch (deviceUpdate.Platform)
    {
        case "mpns":
            installation.Platform = NotificationPlatform.Mpns;
            break;
        case "wns":
            installation.Platform = NotificationPlatform.Wns;
            break;
        case "apns":
            installation.Platform = NotificationPlatform.Apns;
            break;
        case "fcm":
            installation.Platform = NotificationPlatform.Fcm;
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }


    // In the backend we can control if a user is allowed to add tags
    //installation.Tags = new List<string>(deviceUpdate.Tags);
    //installation.Tags.Add("username:" + username);

    await hub.CreateOrUpdateInstallationAsync(installation);

    return Request.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Bir bildirim hub'ından kayıt Kimliğini kullanarak bir cihazı kaydetmek için örnek kod

Uygulama arka ucunuzu kayıtlardaki temel CRUDS işlemleri gerçekleştirebilirsiniz. Örneğin:

```
var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

// create a registration description object of the correct type, e.g.
var reg = new WindowsRegistrationDescription(channelUri, tags);

// Create
await hub.CreateRegistrationAsync(reg);

// Get by id
var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

// update
r.Tags.Add("myTag");

// update on hub
await hub.UpdateRegistrationAsync(r);

// delete
await hub.DeleteRegistrationAsync(r);
```

Arka uç kaydı güncelleştirmeleri arasında eşzamanlılık işlemesi gerekir. Service Bus kaydı yönetimi için iyimser eşzamanlılık denetimi sunar. HTTP düzeyinde bu ETag kullanımıyla ilişkili kayıt yönetim işlemleri üzerinde uygulanır. Bu özellik, bir güncelleştirme eşzamanlılık nedeniyle reddedilmesi durumunda, bir özel durum SDKs tarafından şeffaf bir şekilde kullanılır. Uygulama arka ucu, bu özel durumları işleme ve gerekirse güncelleştirmeyi yeniden denemeden sorumludur.
