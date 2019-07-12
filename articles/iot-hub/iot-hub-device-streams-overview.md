---
title: Azure IOT Hub cihaz akışları (Önizleme) | Microsoft Docs
description: IOT Hub cihaz akışları genel bakış
author: robinsh
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: robinsh
ms.openlocfilehash: 86f8c6e527f58a7c4cfca6233165f0ab05426409
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672564"
---
# <a name="iot-hub-device-streams-preview"></a>IOT Hub cihaz akışları (Önizleme)

Azure IOT hub'ı *cihaz akışları* çeşitli bulut-cihaz iletişimi senaryoları için güvenli çift yönlü TCP tünel oluşturmayı kolaylaştırır. Bir cihaz akışını, bir IOT Hub tarafından cout *akış uç noktası* hangi cihaz ve hizmet uç noktaları arasında bir proxy olarak görev yapar. Cihazlar bir ağ güvenlik duvarının arkasında olan veya özel bir ağın içinde bulunan aşağıdaki diyagramda bu Kurulum, özellikle yararlı olur. Bu nedenle, IOT Hub cihaz akışları adresi müşterilerin ihtiyacını bir güvenlik duvarı uyumlu şekilde ve daha geniş kapsamda gelen veya giden ağ güvenlik duvarı bağlantı noktalarını açmak zorunda kalmadan IOT cihazlarına erişmek için Yardım.

!["IOT Hub cihaz akışları genel bakış"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png )

IOT Hub cihaz akışlarını kullanarak cihazları, güvenli kalmasını ve yalnızca bağlantı noktası 443 üzerinden giden TCP bağlantıları için IOT hub'ın akış uç noktasını açmanız gerekir. Bir akış kurulduktan sonra hizmet ve cihaz tarafı uygulamalar her birbirine ham bayt gönderip WebSocket istemci nesnesi programlı erişim gerekir. Güvenilirlik ve bu tünel tarafından sağlanan garantileri sıralama, TCP özellik değildir.

## <a name="benefits"></a>Avantajlar

IOT Hub cihaz akışları, aşağıdaki avantajları sağlar:

* **Güvenlik Duvarı uyumlu güvenli bağlantı:** IOT cihazları (yalnızca giden bağlantı IOT hub'ına bağlantı noktası 443 üzerinden yapılması gerekiyor) cihazı veya ağ taşınmasıyla, gelen güvenlik duvarı bağlantı noktasının açmadan hizmet uç noktalarından erişilebilir.

* **Kimlik doğrulaması:** IOT Hub ile ilgili kimlik bilgilerini kullanarak kimlik doğrulaması tünelinin iki tarafı hem cihaz hem de hizmet gerekir.

* **Şifreleme:** Varsayılan olarak, IOT Hub cihaz akışları TLS etkin bağlantıları kullanın. Bu, olup uygulama şifreleme veya kullanıp kullanmadığına bakılmaksızın, trafik her zaman şifrelenir sağlar.

* **Basitlik bağlantısı:** Çoğu durumda, cihaz akışları kullanımını IOT cihazları bağlantıyı etkinleştirmek için sanal özel ağlar karmaşık Kurulumu gereksinimini ortadan kaldırır.

* **TCP/IP yığını ile uyumluluğu:** IOT Hub cihaz akışları TCP/IP'yi uygulama trafiği barındırabilir. Başka bir deyişle, çok çeşitli özel olarak standartlara dayanan protokoller bu özelliği kullanabilir.

* **Özel ağ kurulumlarında kullanım kolaylığı:** Hizmet, cihazın IP adresi yerine cihaz Kimliğine başvuruda bulunarak bir cihazla iletişim kuramıyor. Bu, burada bir cihaz özel bir ağ içinde bulunur ve özel bir IP adresi veya IP adresini dinamik olarak atanır ve hizmet tarafına bilinmiyor durumlarda kullanışlıdır.

## <a name="device-stream-workflows"></a>Cihaz akışı iş akışları

Cihaz kimliğini sağlayarak bir cihaza bağlanmak için hizmet istediğinde, cihaz akışını başlatılır Bu iş akışı özellikle SSH ve RDP, burada bir SSH veya RDP istemci programı'nı kullanarak cihaz üzerinde çalışan, SSH veya RDP sunucusuna uzaktan bağlanmak için bir kullanıcı düşünüyor dahil olmak üzere bir istemci/sunucu iletişim modelini içine sığar.

Cihaz akışı oluşturma işlemi, cihaz, hizmeti, IOT hub'ın ana ve akış uç noktaları arasında bir anlaşma içerir. IOT hub'ın ana uç cihaz akışını oluşturulmasını düzenleyen olsa da hizmet ve cihaz arasında akan trafik akış uç noktasını işler.

### <a name="device-stream-creation-flow"></a>Cihaz akışı oluşturma akışı

SDK'sını kullanarak bir cihaz akışını programlı oluşturulmasını da aşağıdaki şekilde açıklanmamıştır aşağıdaki adımları içerir:

!["Cihaz stream anlaşma işlemi"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png)

1. Cihaz uygulama cihaza yeni bir cihaz akışı başlatıldığında önceden haberdar bir geri çağırma kaydeder. Cihaz önyükleme ve IOT Hub'ına bağlandığında, bu adım genellikle gerçekleşir.

2. Hizmet tarafı program cihaz kimliği sağlayarak gerektiğinde cihaz akışını başlatır (_değil_ IP adresi).

3. IOT hub cihaz tarafı program 1. adımda kaydettiğiniz geri çağırarak bildirir. Cihaz kabul edin veya akış başlatma isteği reddedebilir. Bu mantıksal uygulama senaryonuzu belirli olabilir. Akış isteği cihaz tarafından reddedilirse, IOT Hub hizmeti buna uygun olarak bildirir; Aksi takdirde, aşağıdaki adımları izleyin.

4. Cihaz, akış uç noktasını güvenli giden TCP bağlantı bağlantı noktası 443 üzerinden oluşturur ve bağlantı için bir WebSocket yükseltir. Kimlik doğrulaması için kullanılacak kimlik bilgilerinin yanı sıra, akış uç noktasının URL'sini hem cihaza tarafından sağlanan IOT hub'ı adım 3'te gönderilen isteğin bir parçası olarak.

5. Hizmet cihaz akış kabul sonucunu bildirilir ve kendi WebSocket istemcisi akış uç noktası oluşturmaya devam eder. Benzer şekilde, IOT Hub'ından akış uç noktası URL'si ve kimlik doğrulama bilgilerini alır.

Yukarıdaki el sıkışması işlemde:

* Anlaşma sürecinin 60 saniye (Adım 2-5) içinde tamamlamanız gerekir, aksi takdirde bir zaman aşımı el sıkışması başarısız olur ve hizmet Bilgilendirilecek.

* Yukarıdaki akış oluşturma akışı tamamlandıktan sonra akış uç noktasının bir proxy olarak görev yapacak ve trafik, ilgili WebSockets üzerinden cihaz ile hizmet arasında aktarır.

* Cihaz ve hizmet bağlantı noktası 443 üzerinden giden bağlantı akış uç noktasını yanı sıra IOT Hub'ın ana uç nokta gerekir. Bu uç nokta URL'sini kullanılabilir *genel bakış* IOT Hub'ın portalında sekmesi.

* Güvenilirlik garanti kurulan bir akışın sıralama olduğundan TCP içermektedir.

* IOT hub'ı ve akış uç noktasına yönelik tüm bağlantılar, TLS kullanın ve şifrelenir.

### <a name="termination-flow"></a>Akışı sonlandırma

(Hizmet veya cihaz) ya da ağ geçidi TCP bağlantıları kesilir yerleşik bir akışı sonlandırır. Cihaz veya hizmet programları veya involuntarily ağ bağlantısı zaman aşımı veya işlemin başarısız olması durumunda WebSocket kapatarak bu yer gönüllü olarak alabilir. Cihaz veya akış uç noktası hizmetin bağlantı sonlandırılması, bağlı bir TCP bağlantısı (zorla) de sonlandırılacak ve hizmet ve cihaz gerekirse akışı yeniden oluşturmak için sorumludur.

## <a name="connectivity-requirements"></a>Bağlantı gereksinimleri

Hem cihaz hem de cihaz akışını hizmet tarafında, IOT hub'ı ve kendi akış uç noktasını TLS etkin bağlantı kurabilen olması gerekir. Bu, bu uç noktalarına bağlantı noktası 443 üzerinden giden bağlantı gerektirir. Bu uç noktaları ile ilişkilendirilmiş konak adı bulunabilir *genel bakış* IOT Hub aşağıdaki çizimde gösterildiği gibi sekmesinde:

!["Cihaz akış uç noktaları"](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

Alternatif olarak, uç bilgi hub'ının özellikler bölümü altında özellikle, Azure CLI kullanarak alınabilir `property.hostname` ve `property.deviceStreams` anahtarları.

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

Çıktı, hub'ın cihaz ve hizmet cihaz akışını kurulabilmesi bağlanmak için gereken tüm uç noktaların bir JSON nesnesidir.

```json
{
  "streamingEndpoints": [
    "https://<YourIoTHubName>.<region-stamp>.streams.azure-devices.net"
  ]
}
```

> [!NOTE]
> Azure CLI Sürüm 2.0.57 yüklediğinizden emin olun ya da daha yeni. En son sürümü karşıdan yükleyebileceğiniz [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sayfası.
>

## <a name="allow-outbound-connectivity-to-the-device-streaming-endpoints"></a>Giden bağlantı akış uç noktaları cihaza izin ver

Bu makalenin başında belirtildiği gibi cihazınızın IOT Hub ile akış uç noktası cihaz akışları başlatma işlemi sırasında bir giden bağlantı oluşturur. Cihaz veya alt ağdaki güvenlik duvarlarınızdan giden akış ağ geçidi bağlantı noktası 443 üzerinden (iletişim TLS kullanılarak şifrelenmiş bir WebSocket bağlantısı üzerinden gerçekleşir. Not) bağlanmaya izin vermelidir.

Cihaz akış uç noktası ana bilgisayar adı, Azure IOT hub'ı portalındaki genel bakış sekmesinin altında bulunabilir. !["Cihaz akış uç noktaları"](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

Alternatif olarak, Azure CLI kullanarak bu bilgileri bulabilirsiniz:

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

> [!NOTE]
> Azure CLI Sürüm 2.0.57 yüklediğinizden emin olun ya da daha yeni. En son sürümü karşıdan yükleyebileceğiniz [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sayfası.
>

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Etkinlik günlükleri cihaz akışları sorunlarını giderme

IOT hub'ına cihaz akışları etkinlik günlüğü toplamak için Azure İzleyici günlüklerini ayarlayabilirsiniz. Bu sorun giderme senaryoları çok yararlı olabilir.

IOT Hub'ınızın cihaz akış etkinlikleri için Azure İzleyici günlüklerine yapılandırmak için aşağıdaki adımları izleyin:

1. Gidin *tanılama ayarları* sekmesinde IOT hub'ına ve tıklayarak *tanılamayı Aç* bağlantı.

   !["Tanılama günlüklerini etkinleştirme"](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings-pane.png)

2. Tanılama ayarlarınızı için bir ad girin ve seçin *Log Analytics'e gönderme* seçeneği. Mevcut bir Log Analytics çalışma alanı kaynağı seçin veya yeni bir tane oluşturmak için yönlendirilecektir. Ayrıca, kontrol *DeviceStreams* listeden.

    !["Cihaz akış günlüklerini etkinleştirme"](./media/iot-hub-device-streams-overview/device-streams-configure-diagnostics.png)

3. Cihaz akış günlüklerinizi altında artık erişebilirsiniz *günlükleri* IOT Hub'ınızın portalında sekmesi. Cihaz akış etkinlik günlüklerini görünür `AzureDiagnostics` tablo ve sahip `Category=DeviceStreams`.

   Aşağıda gösterildiği gibi hedef cihaz ve işlemin sonucu kimliğini de günlüklerde kullanılabilir.

   !["Cihaz akış günlüklerine erişme"](./media/iot-hub-device-streams-overview/device-streams-view-logs.png)

## <a name="regional-availability"></a>Bölgesel Kullanılabilirlik

Genel Önizleme sırasında IOT Hub cihaz akışları Orta ABD ve orta ABD EUAP bölgelerinde kullanılabilir. Lütfen şu bölgelerden birinde hub'ınıza oluşturduğunuzdan emin olun.

## <a name="sdk-availability"></a>SDK kullanılabilirlik

(Cihaz ve Hizmet tarafı üzerinde) her akış iki tarafının tüneli oluşturmak için IOT Hub SDK'sını kullanın. Genel Önizleme boyunca, müşteriler aşağıdaki SDK dillerden birini seçebilirsiniz:

* C ve C# SDK'ın cihaz tarafında cihaz akışlarını destekler.

* NodeJS ve C# SDK hizmet tarafında cihaz akışlarını destekler.

## <a name="iot-hub-device-stream-samples"></a>IOT Hub cihaz stream örnekleri

İki [hızlı başlangıç örnekleri](/azure/iot-hub) IOT hub'ı sayfasında kullanılabilir. Bunlar, uygulamalar tarafından cihaz akışları kullanımını gösterir.

* *Yankı* örnek (doğrudan, SDK'sı API'nin çağırarak) cihaz akışları programlı kullanımını gösterir.

* *Yerel Ara* örnek gösterir (örneğin, SSH, RDP veya web) kullanıma hazır istemci/sunucu uygulama trafiği tüneli cihaz akışları.

Bu örnekler, aşağıda daha ayrıntılı olarak ele alınmaktadır.

### <a name="echo-sample"></a>Yankı örnek

Yankı örnek gönderme ve alma bayt arasındaki hizmet ve cihaz uygulamalarınız için cihaz akışları programlı kullanımını gösterir. Farklı dillerde hizmet ve cihaz programları kullanmanız gerektiğini unutmayın. Örneğin, C cihaz programıyla kullanabileceğiniz C# hizmet programı.

Yankı örnekleri şunlardır:

* [C#Hizmet ve hizmet programı](quickstart-device-streams-echo-csharp.md)

* [Node.js hizmet programı](quickstart-device-streams-echo-nodejs.md)

* [C cihaz programı](quickstart-device-streams-echo-c.md)

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>(İçin SSH veya RDP) yerel proxy örneği

Yerel bir ara sunucu örneği, istemci ve sunucu programı arasındaki iletişimi kapsar mevcut uygulamanın trafiği tüneli etkinleştirmek için bir yol gösterir. Bu ayar istemci/sunucu protokollerine için çalışır, SSH ve burada Hizmet tarafı (istemci programları SSH veya RDP çalıştıran) bir istemci olarak davranır ve aygıt tarafı (RDP sunucu program veya SSH arka plan programı çalışıyor) sunucusu olarak davranan RDP gibi.

Bu bölümde, SSH kullanıcı bir cihaz için cihaz akışları (RDP veya başka bir istemci/sunucu uygulama durum benzer protokolün karşılık gelen bağlantı noktası kullanarak) üzerinden etkinleştirmek için cihaz akışları kullanımını açıklar.

Kurulum iki yararlanır *yerel Ara* aşağıdaki şekilde, yani gösterilen programlar *cihaz yerel proxy* ve *hizmeti-yerel proxy*. Yerel ara program gerçekleştirilmesinden sorumlu olduğunu [cihaz akış başlatma el sıkışması](#device-stream-creation-flow) IOT Hub ve SSH istemcisi ve normal istemci/sunucu yuva kullanarak SSH arka plan programı ile etkileşim kurma.

!["Cihaz stream proxy Kurulumu SSH/RDP için"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png)

1. Kullanıcı, cihaza bir cihaz akışını başlatmak için hizmet yerel ara sunucu çalıştırır.

2. Cihaz yerel proxy akış başlatma isteği kabul eder ve tünel (yukarıda açıklandığı gibi) için IOT Hub'ın akış uç noktası oluşturulur.

3. Cihaz yerel proxy cihazda 22 numaralı bağlantı noktasında dinleme SSH arka plan programı uç noktasına bağlanır.

4. Hizmet yerel proxy kullanıcıdan yeni SSH bağlantıları bekleyen atanan bir bağlantı noktasında dinler (örnek, ancak bu kullanılan 2222 numaralı bağlantı noktasına başka kullanılabilir bağlantı noktasına yapılandırılabilir). Kullanıcı SSH İstemcisi hizmeti-yerel proxy bağlantı noktası localhost üzerinde işaret eder.

### <a name="notes"></a>Notlar

* SSH arka plan programı (solda) SSH istemciye (sağdaki) arasında bir uçtan uca tünel yukarıdaki adımları tamamlayın. Bu uçtan uca bağlantı parçası, IOT Hub cihaz akış üzerinden trafik göndermeye içerir.

* Şekil okları bağlantı uç noktaları arasında kurulan yönü belirtir. Özellikle, (Bu genellikle bir güvenlik duvarı tarafından engellenip engellenmediğini) cihaz gidip herhangi bir gelen bağlantı olduğunu unutmayın.

* Hizmet yerel proxy 2222 numaralı bağlantı noktasına kullanma seçimi rastgele bir seçimdir. Proxy, kullanılabilir herhangi bir bağlantı kullanmak için yapılandırılabilir.

* 22 numaralı bağlantı noktasının bu durumda Protokolü bağlı ve özel SSH seçimdir. RDP çalışması için bağlantı noktası 3389 kullanılması gerekir. Sağlanan örnek program yapılandırılabilir.

Seçtiğiniz dilde yerel ara programları çalıştırmak yönergeler için aşağıdaki bağlantıları kullanın. Benzer şekilde [Yankı örnek](#echo-sample), tam olarak birlikte çalışabilir olduğundan, cihaz ve hizmet yerel proxy programlar farklı dillerde çalıştırabilirsiniz.

* [C#Hizmet ve hizmet programı](quickstart-device-streams-proxy-csharp.md)

* [Node.js hizmet programı](quickstart-device-streams-proxy-nodejs.md)

* [C cihaz programı](quickstart-device-streams-proxy-c.md)

## <a name="next-steps"></a>Sonraki adımlar

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

> [!div class="nextstepaction"]
> [IOT cihaz akışların (kanal 9) gösterir.](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
