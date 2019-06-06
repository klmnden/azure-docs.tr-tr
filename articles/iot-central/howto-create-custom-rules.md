---
title: Özel kurallar ve bildirimleri ile Azure IOT Central genişletin | Microsoft Docs
description: Bir çözüm geliştirici olarak, bir cihazın telemetri göndermesini durdurur, e-posta bildirimleri göndermek için bir IOT Central uygulamasına yapılandırın. Bu çözüm, Azure Stream Analytics ve Azure işlevleri kullanır.
author: dominicbetts
ms.author: dobett
ms.date: 05/14/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 5248b9546ffe931b72123778d0d23574e5238405
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742408"
---
# <a name="extend-azure-iot-central-with-custom-rules-that-send-notifications"></a>Azure IOT Central bildirim gönderen özel kurallar ile genişletme

Bu nasıl yapılır kılavuzunda, bir çözüm geliştirici olarak gösteren özel kurallar ve bildirimler ile IOT Central uygulamanızı genişletme. Bu örnek, bir cihazın telemetri göndermesini durdurur, operatörün bildirim göndererek gösterir. Çözüm bir [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) bir cihazın telemetri göndermesini durduğunda algılamak için sorgu. Stream Analytics kullanan iş [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/) bildirim göndermek için e-kullanarak [SendGrid](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/).

Bu nasıl yapılır kılavuzunda IOT Central zaten yerleşik kurallar ve Eylemler ile getirebileceği ötesine genişletmek gösterilmektedir.

Nasıl yapılır bu kılavuzda şunların nasıl yapılır:

* Stream kullanarak bir IOT Central uygulama telemetri *verileri sürekli dışarı aktarma*.
* Veri gönderen bir cihaza durdurduğunda algılar bir Stream Analytics sorgu oluşturun.
* SendGrid Hizmetleri ve Azure işlevleri'ni kullanarak bir e-posta bildirimi gönderin.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliği gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

### <a name="iot-central-application"></a>IOT Central uygulamasına

Bir IOT Central uygulamasından oluşturma [Azure IOT Central - uygulamalarım](https://aka.ms/iotcentral) sayfası aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Ödeme planı | Kullandıkça Öde |
| Uygulama şablonu | Contoso Örneği |
| Uygulama adı | Varsayılan değeri kabul edin veya kendi ad seçin |
| URL'si | Varsayılan değeri kabul edin veya kendi benzersiz URL ön eki seçin |
| Dizin | Azure Active Directory kiracınızın |
| Azure aboneliği | Azure aboneliğiniz |
| Bölge | Doğu ABD |

Örnekler ve ekran görüntüleri bu makaleyi kullanın **Doğu ABD** bölge. Size yakın bir konum seçin ve tüm kaynaklarınız aynı bölgede oluşturduğunuzdan emin olun.

### <a name="resource-group"></a>Kaynak grubu

Kullanım [bir kaynak grubu oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.ResourceGroup) adlı **DetectStoppedDevices** oluşturduğunuz diğer kaynakları içerecek. Azure kaynaklarınızı IOT Central uygulamanız ile aynı konumda oluşturun.

### <a name="event-hubs-namespace"></a>Event Hubs ad alanı

Kullanım [bir Event Hubs ad alanı oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.EventHub) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Ad    | Ad alanı adı seçin |
| Fiyatlandırma katmanı | Temel |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| Location | Doğu ABD |
| İşleme Birimleri | 1 |

### <a name="stream-analytics-job"></a>Stream Analytics işi

Kullanım [bir Stream Analytics işi oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Ad    | İşinizin adı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| Location | Doğu ABD |
| Barındırma ortamı | Bulut |
| Akış birimleri | 3 |

### <a name="function-app"></a>İşlev uygulaması

Kullanım [bir işlev uygulaması oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.FunctionApp) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Uygulama adı    | İşlev uygulamanızın adı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| İşletim Sistemi | Windows |
| Barındırma planı | Tüketim Planı |
| Location | Doğu ABD |
| Çalışma zamanı yığını | .NET |
| Depolama | Yeni oluştur |

### <a name="sendgrid-account"></a>SendGrid hesabı

Kullanım [SendGrid hesabı oluşturmak için Azure portalını](https://portal.azure.com/#create/Sendgrid.sendgrid) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Ad    | SendGrid hesabınızın adını seçin |
| Parola | Bir parola oluşturun |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| Fiyatlandırma katmanı | F1 Ücretsiz |
| İletişim bilgileri | Gerekli bilgileri doldurun |

Tüm gerekli kaynakları oluşturduğunuz zaman, **DetectStoppedDevices** kaynak grubu, aşağıdaki ekran görüntüsüne benzer görünür:

![Durdurulan cihazları kaynak grubu algılayın](media/howto-create-custom-rules/resource-group.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Sürekli olarak olay hub'ına telemetri dışarı aktarmak için bir IOT Central uygulamasına yapılandırabilirsiniz. Bu bölümde, IOT Central uygulamanızdan telemetri almak için bir olay hub'ı oluşturun. Olay hub'ı işleme için Stream Analytics işinizi telemetri gönderir.

1. Azure portalında Event Hubs ad alanınıza gidin ve seçin **+ olay hub'ı**.
1. Olay hub'ınızı adlandırın **centralexport**seçip **Oluştur**.

Event Hubs ad alanınız şu ekran görüntüsüne benzer görünür:

![Event Hubs ad alanı](media/howto-create-custom-rules/event-hubs-namespace.png)

## <a name="get-sendgrid-api-key"></a>SendGrid API anahtarı alma

İşlev uygulamanız, e-posta iletileri göndermek için SendGrid API anahtarı gerekir. SendGrid API anahtarı oluşturmak için:

1. Azure portalında, SendGrid hesabınıza gidin. Ardından **Yönet** SendGrid hesabınıza erişmek için.
1. SendGrid hesabınızı seçin **ayarları**, ardından **API anahtarları**. Seçin **API anahtarı oluşturma**:

    ![SendGrid API anahtarı oluşturma](media/howto-create-custom-rules/sendgrid-api-keys.png)

1. Üzerinde **API anahtarı oluştur** adlı bir anahtar oluşturun, sayfa **AzureFunctionAccess** ile **tam erişim** izinleri.
1. API anahtarını not edin, işlev uygulamanızı yapılandırırken gerekir.

## <a name="define-the-function"></a>Fonksiyon tanımlayın

Bu çözüm, Stream Analytics işi durdurulmuş bir cihaz algıladığında e-posta bildirimi göndermek için bir Azure işlev uygulaması kullanır. İşlev uygulamanızı oluşturmak için:

1. Azure portalında gidin **App Service** örneğini **DetectStoppedDevices** kaynak grubu.
1. Seçin **+** yeni bir işlev oluşturmak için.
1. Üzerinde **bir geliştirme ORTAMI seçin** sayfasında **portal** seçip **devam**.
1. Üzerinde **bir işlev oluşturma** sayfasında **Web kancası + API** seçip **Oluştur**.

Çağrılan bir varsayılan işlev portalın oluşturduğu **HttpTrigger1**:

![Varsayılan HTTP tetikleyici işlevi](media/howto-create-custom-rules/default-function.png)

### <a name="configure-function-bindings"></a>İşlev bağlamaları yapılandırma

SendGrid ile e-posta göndermek için işlevinizi bağlamalarını şu şekilde yapılandırmak gerekir:

1. Seçin **tümleştir**, çıktı seçin **HTTP ($return)** ve ardından **Sil**.
1. Seçin **+ yeni çıkış**, ardından **SendGrid**ve ardından **seçin**. Seçin **yükleme** SendGrid uzantıyı yüklemek için.
1. Yükleme tamamlandığında seçin **işlev dönüş değeri kullan**. Geçerli bir ekleme **adresine** e-posta bildirimleri almak için.  Geçerli bir ekleme **adresinden** e-posta gönderen olarak kullanılacak.
1. Seçin **yeni** yanındaki **SendGrid API anahtarı uygulama ayarı**. Girin **SendGridAPIKey** key ve not ettiğiniz daha önce değeri olarak SendGrid API anahtarı. Ardından **Oluştur**’u seçin.
1. Seçin **Kaydet** işleviniz için SendGrid bağlamaları kaydetmek için.

Tümleştirme ayarları, aşağıdaki ekran görüntüsüne benzer görünür:

![İşlev uygulaması tümleştirmeleri](media/howto-create-custom-rules/function-integrate.png)

### <a name="add-the-function-code"></a>İşlev kodunu ekleyin

İşlevinizi uygulamak için ekleme C# gelen HTTP istek ayrıştırılamıyor ve şu şekilde e-postaları göndermek için kod:

1. Seçin **HttpTrigger1** değiştirin ve işlev uygulamanızda işlevi C# kodu aşağıdaki kodla:

    ```csharp
    #r "Newtonsoft.Json"
    #r "..\bin\SendGrid.dll"

    using System;
    using SendGrid.Helpers.Mail;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static SendGridMessage Run(HttpRequest req, ILogger log)
    {
        string requestBody = new StreamReader(req.Body).ReadToEnd();
        log.LogInformation(requestBody);
        var notifications = JsonConvert.DeserializeObject<IList<Notification>>(requestBody);

        SendGridMessage message = new SendGridMessage();
        message.Subject = "Contoso device notification";

        var content = "The following device(s) have stopped sending telemetry:<br/><br/><table><tr><th>Device ID</th><th>Time</th></tr>";
        foreach(var notification in notifications) {
            log.LogInformation($"No message received - Device: {notification.deviceid}, Time: {notification.time}");
            content += $"<tr><td>{notification.deviceid}</td><td>{notification.time}</td></tr>";
        }
        content += "</table>";
        message.AddContent("text/html", content);

        return message;
    }

    public class Notification
    {
        public string deviceid { get; set; }
        public string time { get; set; }
    }
    ```

    Yeni kod kaydedene kadar bir hata iletisi görebilirsiniz.

1. Seçin **Kaydet** işlevini kaydetmek için.

### <a name="test-the-function-works"></a>İşlevin çalıştığını test

Portalda işlevi test etmek için öncelikle seçin **günlükleri** altındaki Kod Düzenleyicisi. Ardından **Test** sağ tarafındaki Kod Düzenleyicisi. Aşağıdaki JSON olarak kullanmak **istek gövdesi**:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

İşlev günlük iletilerini görünür **günlükleri** paneli:

![Günlük çıktısını işlevi](media/howto-create-custom-rules/function-app-logs.png)

Birkaç dakika sonra **için** e-posta adresi aşağıdaki içeriğe sahip bir e-posta alır:

```txt
The following device(s) have stopped sending telemetry:

Device ID   Time
test-device-1   2019-05-02T14:23:39.527Z
test-device-2   2019-05-02T14:23:50.717Z
test-device-3   2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Stream Analytics sorgusu ekleme

Bu çözüm, 120 saniye boyunca telemetri gönderen bir cihaza durduğunda algılamak için bir Stream Analytics sorgusu kullanır. Sorgu, giriş olarak olay hub'ından telemetriyi kullanır. İş, sorgu sonuçları işlev uygulamasına gönderir. Bu bölümde, Stream Analytics işi yapılandırın:

1. Azure portalında, Stream analytics işi altında gidin **işleri topolojisi** seçin **girişleri**, seçin **+ akış Girişi Ekle**ve ardından **olay Hub**.
1. Daha önce oluşturduğunuz olay hub'ı kullanarak giriş yapılandırmak için aşağıdaki tablodaki bilgileri kullanın ve ardından **Kaydet**:

    | Ayar | Değer |
    | ------- | ----- |
    | Girdi diğer adı | centraltelemetry |
    | Abonelik | Aboneliğiniz |
    | Olay hub'ı ad alanı | Olay hub'ı ad alanı |
    | Olay Hub'ı adı | Var olanı - kullan **centralexport** |

1. Altında **işleri topolojisi**seçin **çıkışları**, seçin **+ Ekle**ve ardından **Azure işlevi**.
1. Çıkış yapılandırın ve ardından aşağıdaki tablodaki bilgileri kullanın **Kaydet**:

    | Ayar | Değer |
    | ------- | ----- |
    | Çıktı diğer adı | EmailNotification |
    | Abonelik | Aboneliğiniz |
    | İşlev uygulaması | İşlev uygulamanızı |
    | İşlev  | HttpTrigger1 |

1. Altında **işleri topolojisi**seçin **sorgu** ve varolan bir sorguyu aşağıdaki SQL ile değiştirin:

    ```sql
    with
    LeftSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid1, 
        EventEnqueuedUtcTime AS time1
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    ),
    RightSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid2, 
        EventEnqueuedUtcTime AS time2
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    )

    SELECT
        LeftSide.deviceid1 as deviceid,
        LeftSide.time1 as time
    INTO
        [emailnotification]
    FROM
        LeftSide
        LEFT OUTER JOIN
        RightSide 
        ON
        LeftSide.deviceid1=RightSide.deviceid2 AND DATEDIFF(second,LeftSide,RightSide) BETWEEN 1 AND 120
        where
        -- Find records where a device didn't send a message 120 seconds
        RightSide.deviceid2 is NULL
    ```

1. **Kaydet**’i seçin.
1. Stream Analytics işi başlatmak için **genel bakış**, ardından **Başlat**, ardından **artık**, ardından **Başlat**:

    ![Stream Analytics](media/howto-create-custom-rules/stream-analytics.png)

## <a name="configure-export-in-iot-central"></a>IOT Central dışa aktarma yapılandırma

Gidin [IOT Central uygulamasına](https://aka.ms/iotcentral) Contoso şablondan bir şablondan oluşturulmuştur. Bu bölümde, kendi benzetilmiş aygıtlardan olay hub'ınıza telemetri akışı için uygulamayı yapılandırma. Dışarı aktarma yapılandırmak için:

1. Gidin **verileri sürekli dışarı aktarma** sayfasında **+ yeni**, ardından **Azure Event Hubs**.
1. Dışarı aktarma yapılandırın, ardından seçmek için aşağıdaki Ayaları kullanın **Kaydet**:

    | Ayar | Değer |
    | ------- | ----- |
    | Görünen ad | Event Hubs'a Aktar |
    | Enabled | Açık |
    | Event Hubs ad alanı | Event Hubs ad alanınızın adı |
    | Olay hub'ı | centralexport |
    | Ölçümler | Açık |
    | Cihazlar | Kapalı |
    | Cihaz şablonları | Kapalı |

![Verileri sürekli dışarı aktarma yapılandırması](media/howto-create-custom-rules/cde-configuration.png)

Dışarı aktarma durumunu tamamlanana kadar bekleyin **çalıştıran** devam etmeden önce.

## <a name="test"></a>Test etme

Çözümü test etmek için IOT Central'ndan verileri sürekli dışarı aktarma durdurulmuş sanal cihazlar için devre dışı bırakabilirsiniz:

1. IOT Central uygulamanıza gidin **verileri sürekli dışarı aktarma** sayfasından seçim yapıp **dışarı aktarmak için Event Hubs** yapılandırmasını dışarı aktarma.
1. Ayarlama **etkin** için **kapalı** ve **Kaydet**.
1. En az iki dakika sonra **için** e-posta adresi içerik aşağıdaki gibi görünen bir veya daha fazla e-postaları alır:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID   Time
    7b169aee-c843-4d41-9f25-7a02671ee659    2019-05-09T14:28:59.954Z
    ```

## <a name="tidy-up"></a>Çatalınızdan

Bu nasıl yapılır sonra çatalınızdan ve gereksiz maliyetleri önlemek için silin **DetectStoppedDevices** Azure portalında kaynak grubu.

IOT Central uygulamadan silebilirsiniz **Yönetim** uygulama içinde sayfa.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda öğrendiğiniz nasıl yapılır:

* Stream kullanarak bir IOT Central uygulama telemetri *verileri sürekli dışarı aktarma*.
* Veri gönderen bir cihaza durdurduğunda algılar bir Stream Analytics sorgu oluşturun.
* SendGrid Hizmetleri ve Azure işlevleri'ni kullanarak bir e-posta bildirimi gönderin.

Özel kurallar ve bildirimleri oluşturmak nasıl artık bildiğinize göre önerilen sonraki adıma öğrenmektir nasıl [genişletmek Azure IOT Central özel analizlerle](howto-create-custom-analytics.md).
