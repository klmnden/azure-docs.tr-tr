---
title: Kayıt Yönetimi
description: Bu konuda, anında iletme bildirimleri almak için notification hubs ile cihazları kaydetmek açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: ''
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: af5738ac96bd2afacee493765453567f7f13c9e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="registration-management"></a>Kayıt yönetimi
## <a name="overview"></a>Genel Bakış
Bu konuda, anında iletme bildirimleri almak için notification hubs ile cihazları kaydetmek açıklanmaktadır. Konu kayıtlar yüksek düzeyde açıklanır ve cihazlar kaydettirmek için iki ana modeli sunar: bildirim hub'ına doğrudan CİHAZDAN kaydetme ve bir uygulama arka ucu kaydediliyor. 

## <a name="what-is-device-registration"></a>Cihaz kaydı nedir
Bildirim hub'ı ile cihaz kaydı kullanarak gerçekleştirilir bir **kayıt** veya **yükleme**.

#### <a name="registrations"></a>Kayıtlar
Bir kaydı bir aygıt için Platform bildirim hizmeti (PNS) tanıtıcı etiketleri ve büyük olasılıkla bir şablon ile ilişkilendirir. PNS tanıtıcısını ChannelURI, cihaz belirteci veya GCM kayıt kimliği olabilir. Etiketler cihaz tanıtıcılarını doğru kümesine bildirimleri yönlendirmek için kullanılır. Daha fazla bilgi için bkz: [Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md). Şablonları kayıt başına dönüştürme uygulamak için kullanılır. Daha fazla bilgi için bkz: [şablonları](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Yüklemeleri
Yüklemesi geliştirilmiş olabilir itme paketi içeren kayıt ilgili özellikler. Bunu aygıtlarınızı kaydetme en son ve en iyi yaklaşımdır. Ancak, istemci tarafı .NET SDK'sı tarafından desteklenmiyor ([Notification Hub SDK'sı arka uç işlemleri için](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) henüz dağıtmadığı.  Bu istemci CİHAZDAN kaydediyorsunuz varsa kullanılacak anlamına gelir [Notification Hubs REST API'si](https://msdn.microsoft.com/library/mt621153.aspx) yaklaşım yüklemelerini desteklemek için. Arka uç hizmeti kullanıyorsanız, kullanabilmek için olmalısınız [Notification Hub SDK'sı arka uç işlemleri için](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Yüklemeleri kullanmanın önemli avantajlarından bazıları şunlardır:

* Oluşturma veya bir yüklemeyi güncelleştirme ıdempotent tam değil. Bu nedenle tüm yinelenen kayıtları endişeniz olmadan deneyebilirsiniz.
* Yükleme modeli tek tek iter - belirli bir aygıtı hedefleme yapılacağı kolaylaştırır. A system tag **"$InstallationId:[installationId]"** is automatically added with each installation based registration. Bu nedenle hiçbir ek kodlama yapmak zorunda kalmadan belirli bir aygıt hedeflemek için bu etikete gönderme çağırabilirsiniz.
* Yüklemeleri kullanarak, kısmi kayıt güncelleştirmeler yapmak de sağlar. Bir düzeltme eki yöntemi kullanılarak ile kısmi güncelleştirme yüklemesinin istenilen [JSON düzeltme eki standart](https://tools.ietf.org/html/rfc6902). Kayıt etiketlerini güncelleştirmek istediğinizde özellikle yararlıdır. Tüm kayıt çekmek ve önceki tüm etiketleri yeniden yeniden gerekmez.

Bir yükleme içerebilir aşağıdaki özellikleri. Yükleme özellikleri bakın, tam bir listesi için [oluşturmak veya bir yükleme ile REST API üzerine](https://msdn.microsoft.com/library/azure/mt621153.aspx) veya [yükleme özellikleri](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx).

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



Kayıtlar ve varsayılan olarak yüklemeleri artık dolacağını dikkate almak önemlidir.

Kayıtlar ve yüklemeleri her cihaz/kanal için geçerli bir PNS tanıtıcısını içermelidir. PNS tanıtıcılarını yalnızca aygıttaki istemci uygulamasında elde edilebilir olduğundan, istemci uygulaması ile bir cihazda doğrudan kaydetmek için bir düzen yöneliktir. Diğer taraftan, güvenlikle ilgili önemli noktalar ve etiketler için ilgili iş mantığı, uygulama arka ucunu cihaz kaydında yönetmenizi gerektirebilir. 

#### <a name="templates"></a>Şablonlar
Kullanmak istiyorsanız, [şablonları](notification-hubs-templates-cross-platform-push-messages.md), aygıt yüklemesi de JSON bu aygıt ile ilişkili tüm şablonları tutun (Yukarıdaki örnek bakın) biçimlendirin. Şablon adları hedef farklı şablonları aynı aygıt için yardımcı olur.

Her bir şablon adı bir şablon gövde ve isteğe bağlı bir dizi etiketi unutmayın. Ayrıca, her platform ek şablon özelliklere sahip olabilir. Windows Mağazası'nın (WNS kullanarak) ve Windows Phone 8 (MPNS kullanarak) için ek bir üstbilgi kümesi şablonunun parçası olabilir. APNs söz konusu olduğunda, bir süre sonu özelliğini ya da veya şablon ifadesi ayarlayabilirsiniz. Yükleme özellikleri bakın, tam bir listesi için [oluşturmak veya bir yükleme ile REST üzerine](https://msdn.microsoft.com/library/azure/mt621153.aspx) konu.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Windows mağazası uygulamaları için ikincil döşeme
Windows mağazası istemci uygulamaları için ikincil döşeme bildirimleri gönderme birincil birine göndererek ile aynı olur. Bu ayrıca yüklemelerde desteklenir. İkincil döşeme istemci uygulamanızdan SDK şeffaf bir şekilde işler farklı bir ChannelUri gerektiğini unutmayın.

Windows mağazası uygulamanızı SecondaryTiles nesnesi oluşturmak için kullanılan aynı TileId SecondaryTiles sözlük kullanır.
Birincil ChannelUri gibi ile ikincil döşeme ChannelUris herhangi bir anda değiştirebilirsiniz. Güncelleştirilen bildirim hub'ı yüklemeleri tutmak için aygıt bunları ikincil döşeme geçerli ChannelUris ile yenilemeniz gerekir.

## <a name="registration-management-from-the-device"></a>Cihaz kayıt yönetimi
Cihaz kaydı istemci uygulamaları yönetirken, arka uç yalnızca bildirim göndermek için sorumludur. İstemci uygulamaları PNS tanıtıcılarını güncel tutun ve etiketleri kaydedin. Aşağıdaki resimde bu deseni gösterilmektedir.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Cihaz önce PNS PNS tanıtıcısını alır, sonra bildirim hub'ı ile doğrudan kaydeder. Kayıt başarılı olduktan sonra uygulama arka ucu, kayıt hedefleyen bir bildirim gönderebilirsiniz. Bildirimleri gönderme hakkında daha fazla bilgi için bkz: [Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md).
Bu durumda, kullanacağınız not yalnızca bildirim hub'larınız aygıttan erişim hakları dinler. Daha fazla bilgi için bkz: [güvenlik](notification-hubs-push-notification-security.md).

Aygıttan kaydetme basit yöntemidir, ancak bazı kısıtlamaları vardır.
İlk dezavantajı, uygulama etkinken bir istemci uygulaması yalnızca kendi etiketleri güncelleştirebilirsiniz olmasıdır. Bir kullanıcının ilk aygıt bir ek etiketi (örneğin, takımınız) kaydettiğinde Spor takıma ilgili etiketleri kaydetme iki cihazlar varsa, ikinci cihaza uygulamanın kadar Örneğin, ikinci bir cihaz takımınız hakkında bildirimler almaz ikinci kez yürütüldü. Etiketler birden çok aygıt tarafından etkilenir, daha genel olarak, etiketleri arka ucundan yönetme bir arzu seçenektir.
Kayıt Yönetimi istemci uygulamasından ikinci dezavantajı uygulamaları ele olduğundan, kayıt belirli etiketleri için güvenli hale getirme ek bakım, "etiketi düzeyi güvenlik." bölümünde açıklandığı gibi gerektirmesidir

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Bildirim hub'ı bir yükleme seçeneğini kullanarak bir aygıt ile kaydetmek için örnek kod
Şu anda yalnızca kullanarak destekleniyorsa [Notification Hubs REST API'si](https://msdn.microsoft.com/library/mt621153.aspx).

Düzeltme eki yöntemini kullanarak da kullanabilirsiniz [JSON düzeltme eki standart](https://tools.ietf.org/html/rfc6902) yükleme güncelleştirmek için.

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

    // If we have not stored a installation id in application data, create and store as application data.
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



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Bildirim hub'ı kullanarak bir kaydı bir aygıttan ile kaydetmek için örnek kod
Bu yöntemler oluşturun veya bunlar üzerinde adlandırılır cihaz için bir kayıt güncelleştirin. Başka bir deyişle, tanıtıcı veya etiketleri güncelleştirmek için tüm kayıt üzerine yazmalıdır. Her zaman belirli bir aygıt gereklidir geçerli etiketler güvenilir deposuyla gerekir böylece kayıtlar geçici olduğunu unutmayın.

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


## <a name="registration-management-from-a-backend"></a>Kayıt Yönetimi'nden bir arka uç
Kayıtlar arka ucundan yönetme ek kod yazma gerektirir. Uygulama aygıttan güncelleştirilmiş PNS arka ucuna uygulama (etiketleri ve şablonlar ile birlikte) her başlatılışında işlemek ve arka uç bu tanıtıcıyı bildirim hub'ındaki güncelleştirmelisiniz sağlamanız gerekir. Aşağıdaki resimde bu tasarım gösterilmektedir.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Kayıtlar arka ucundan yönetme avantajlar cihaza karşılık gelen uygulamanın devre dışı olduğunda bile kayıtlar etiketleri değiştirebilir ve kendi kaydı için bir etiket eklemeden önce istemci uygulamanın kimlik doğrulamasını olanağı vardır.

#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Bir bildirim hub'ından bir yükleme seçeneğini kullanarak bir arka uç ile kaydetmek için örnek kod
İstemci aygıtı hala önce ilgili yüklemeyi özellikleri olarak ve PNS tanıtıcısını alır ve kaydı gerçekleştirmek ve etiketleri vb. yetkilendirmek arka uç özel bir API çağırır. Arka uç yararlanabilirsiniz [Notification Hub SDK'sı arka uç işlemleri için](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Düzeltme eki yöntemini kullanarak da kullanabilirsiniz [JSON düzeltme eki standart](https://tools.ietf.org/html/rfc6902) yükleme güncelleştirmek için.

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
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
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


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Bildirim hub'ı bir kayıt kimliği kullanılarak bir aygıttan ile kaydetmek için örnek kod
Uygulama arka ucunuzdan, kayıtlar üzerinde temel CRUDS işlemler gerçekleştirebilirsiniz. Örneğin:

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


Arka uç kaydı güncelleştirmeleri arasındaki eşzamanlılık işlemelidir. Hizmet veri yolu iyimser eşzamanlılık denetimini kayıt yönetimi sunar. HTTP düzeyde, bu, kayıt yönetimi işlemleri ETag kullanılmasına uygulanır. Bu özellik, saydam bir güncelleştirme eşzamanlılık nedenlerle reddedilirse, bir özel durum SDKs tarafından kullanılır. Uygulama arka ucu, bu özel durumları işleme ve gerekirse güncelleştirme yeniden deneniyor sorumludur.

