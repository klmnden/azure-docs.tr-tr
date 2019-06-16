---
title: Dağıtılmış izleme (Önizleme) ile IOT iletileri için bağıntı kimlikleri ekleyin
description: ''
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: jlian
ms.openlocfilehash: 302c382a7e19e9dcc4c979d31ddc0768655a1465
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400849"
---
# <a name="trace-azure-iot-device-to-cloud-messages-with-distributed-tracing-preview"></a>Azure IOT cihaz-bulut iletileri dağıtılmış izleme (Önizleme) ile izleme

Microsoft Azure IOT hub'ı şu anda dağıtılmış izleme olarak destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IOT hub'ı dağıtılmış izlemeyi desteklemek için ilk Azure hizmetlerinden biridir. Diğer Azure Hizmetleri, dağıtılmış izleme desteği gibi çözümünüzde ilgili Azure Hizmetleri genelinde mümkün izleme IOT iletileri olacaktır. Dağıtılmış izlemeyi bir arka plan için bkz: [dağıtılmış izleme](../azure-monitor/app/distributed-tracing.md).

IOT hub'ı için Dağıtılmış izlemeyi etkinleştirme olanağı sağlar:

- Tam olarak kullanarak IOT hub'ı aracılığıyla her ileti akışını izlemek [izleme bağlamı](https://github.com/w3c/trace-context). Bu izleme bağlamı bağıntı kimlikleri bir bileşenden olaylar bileşenlerindeki olayları ilişkilendirmenize olanak tanıyan içerir. Tüm IOT cihaz iletileri kullanarak veya bir alt kümesi için uygulanabilir [cihaz ikizi](iot-hub-devguide-device-twins.md).
- İzleme bağlamına otomatik olarak oturum [Azure İzleyici tanılama günlükleri](iot-hub-monitor-resource-health.md).
- Ölçün ve ileti akışı ve gecikme süresini cihazlardan IOT hub'ı ve yönlendirme uç noktaları anlayın.
- Azure olmayan hizmetler için Dağıtılmış izleme IOT çözümünüzü uygulamak istediğiniz nasıl ele başlatın.

Bu makalede, kullandığınız [C için Azure IOT cihaz SDK'sını](./iot-hub-device-sdk-c-intro.md) dağıtılmış izleme. Dağıtılmış izleme desteği yine de diğer SDK'ları için devam ediyor.

## <a name="prerequisites"></a>Önkoşullar

- Dağıtılmış izleme şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

  - **Kuzey Avrupa**
  - **Güneydoğu Asya**
  - **Batı ABD 2**

- Bu makalede, IOT hub'ına telemetri gönderme ile ilgili bilgi sahibi olduğunuz varsayılır. Tamamladığınızdan emin olun [C hızlı telemetri gönderme](./quickstart-send-telemetry-c.md).

- Bir cihaz IOT hub'ı (adımlar her bir Hızlı Başlangıç) ve not edin, bağlantı dizesini kaydedin.

- En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

## <a name="configure-iot-hub"></a>IOT hub'ı yapılandırma

Bu bölümde, dağıtılmış izleme öznitelikler (bağıntı kimlikleri ve zaman damgaları) oturum için IOT hub'ı yapılandırın.

1. IOT hub'ına gidin [Azure portalında](https://portal.azure.com/).

1. IOT hub'ınız için sol bölmede aşağı kaydırarak **izleme** tıklayın ve bölüm **tanılama ayarları**.

1. Tanılama ayarları zaten açık olmayan ise tıklayın **tanılamayı Aç**. Tanılama ayarları zaten etkinleştirdiyseniz, tıklayın **tanılama ayarı ekleme**.

1. İçinde **adı** yeni bir tanılama ayarı için bir ad girin. Örneğin, **DistributedTracingSettings**.

1. Bir veya daha fazla günlük gönderileceği belirlemek aşağıdaki seçeneklerden birini seçin:

    - **Bir depolama hesabında arşivle**: Günlük bilgilerini içeren bir depolama hesabı yapılandırın.
    - **Olay hub'ına Stream**: Günlük bilgilerini içeren bir olay hub'ı yapılandırın.
    - **Log Analytics'e gönderme**: Bir log analytics çalışma alanı günlük bilgilerini içerecek şekilde yapılandırın.

1. İçinde **günlük** bölümünde, günlük bilgileri için istediğiniz işlemleri seçin.

    Eklediğinizden emin olun **DistributedTracing**ve yapılandırma bir **bekletme** kaç gün boyunca bekletilir günlüğünü istiyor. Günlük tutma depolama maliyetleri etkiler.

    ![DistributedTracing kategorisi için tanılama ayarları IOT olduğu gösteren ekran görüntüsü](./media/iot-hub-distributed-tracing/diag-logs.png)

1. Tıklayın **Kaydet** yeni ayar için.

1. (İsteğe bağlı) Akış için farklı yerlerde, ayarlanmış iletilerini görmek için [en az iki farklı uç noktalarına yönlendirme kuralları](iot-hub-devguide-messages-d2c.md).

Günlük etkinleştirildikten sonra geçerli izleme özelliklerini içeren bir ileti aşağıdaki durumlarda hiçbirinde karşılaşıldığında IOT hub'ı günlüğe kaydeder:

- İletileri IOT Hub'ın veri ağ geçidi ulaşır.
- İleti, IOT Hub tarafından işlenir.
- İletinin özel uç noktalara yönlendirilir. Yönlendirme etkinleştirilmelidir.

Bu günlükler ve kendi şemaları hakkında daha fazla bilgi için bkz: [dağıtılmış izleme IOT hub'ı tanılama günlüklerinde](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

## <a name="set-up-device"></a>Cihaz ayarlama

Bu bölümde, bir geliştirme ortamı ile kullanmak için hazırlama [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c). Ardından, cihazınızın telemetri iletilerini dağıtılmış izlemeyi etkinleştirmek için örneklerden birini değiştirin.

Windows üzerinde örnek oluşturmak için bu yönergeleri verilmiştir. Diğer ortamlar için bkz: [C SDK'sını derleme](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compile) veya [önceden paketlenmiş C SDK'sı için belirli Platform geliştirme](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#prepackaged-c-sdk-for-platform-specific-development).

### <a name="clone-the-source-code-and-initialize"></a>Kaynak kodu kopyalayın ve başlatma

1. Yükleme ["C++ ile masaüstü geliştirme" iş yükü](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2017) Visual Studio 2015 veya 2017.

1. Yükleme [CMake](https://cmake.org/). Olduğundan emin olun, içinde `PATH` yazarak `cmake -version` bir komut isteminden.

1. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin.

    ```cmd
    cd azure-iot-sdk-c    
    mkdir cmake
    cd cmake
    cmake ..
    ```

    Varsa `cmake` C++ derleyicinizi bulunamıyor yukarıdaki komutu çalıştırırken derleme hataları alabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

### <a name="edit-the-send-telemetry-sample-to-enable-distributed-tracing"></a>Dağıtılmış izlemeyi etkinleştirmek için send telemetri örnek Düzenle

1. Açmak için bir düzenleyici kullanın `azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c` kaynak dosyası.

1. `connectionString` sabitinin bildirimini bulun:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=2)]

    Değiştirin `connectionString` Not içinde yapılan cihaz bağlantı dizesiyle sabit [cihaz kaydetme](./quickstart-send-telemetry-c.md#register-a-device) bölümünü [C hızlı telemetri gönderme](./quickstart-send-telemetry-c.md).

1. Değişiklik `MESSAGE_COUNT` tanımlamak `5000`:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=3)]

1. Çağıran kod satırı bulun `IoTHubDeviceClient_LL_SetConnectionStatusCallback` Gönder ileti döngüsünden önce bir bağlantı durumu geri çağırma işlevini kaydetmek için. Bu satırın altındaki kodu çağırmak için aşağıda gösterildiği gibi ekleyin `IoTHubDeviceClient_LL_EnablePolicyConfiguration` dağıtılmış izlemeyi cihaz için etkinleştirme:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_tracing&highlight=5)]

    `IoTHubDeviceClient_LL_EnablePolicyConfiguration` İşlevi sağlayan ilkeleri aracılığıyla yapılandırılan belirli IoTHub özellik [cihaz ikizlerini](./iot-hub-devguide-device-twins.md). Bir kez `POLICY_CONFIGURATION_DISTRIBUTED_TRACING` etkin Yukarıdaki kod satırıyla, cihaz izleme davranışı üzerinde cihaz ikizini dağıtılmış izleme değişikliği yansıtır.

1. Tüm kotanızı kullanmadan çalışan örnek uygulama tutmak için bir saniyelik gecikme Gönder ileti döngüsü sonuna ekleyin:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_sleep&highlight=8)]

### <a name="compile-and-run"></a>Derleyin ve çalıştırın

1. Gidin *iothub_ll_telemetry_sample* CMake dizinden proje dizinine (`azure-iot-sdk-c/cmake`) daha önce oluşturduğunuz ve örnek derleme:

    ```cmd
    cd iothub_client/samples/iothub_ll_telemetry_sample
    cmake --build . --target iothub_ll_telemetry_sample --config Debug
    ```

1. Uygulamayı çalıştırın. Cihaz, dağıtılmış bir izleme telemetri gönderir.

    ```cmd
    Debug/iothub_ll_telemetry_sample.exe
    ```

1. Uygulamayı çalıştıran tutun. İsteğe bağlı olarak konsol penceresinin bakarak IOT Hub'ına gönderilen ileti gözlemleyin.

<!-- For a client app that can receive sampling decisions from the cloud, check out [this sample](https://aka.ms/iottracingCsample).  -->

### <a name="workaround-for-third-party-clients"></a>Üçüncü taraf istemciler için geçici çözüm

Sahip **değil Önemsiz** C SDK'sını kullanarak dağıtılmış izleme özelliğini önizlemek için. Bu nedenle, bu yaklaşım önerilmez.

İleti geliştirme Kılavuzu izleyerek tüm IOT Hub protokol temelleri ilk olarak, uygulamalıdır [oluşturun ve IOT Hub iletilerini okuma](iot-hub-devguide-messages-construct.md). Ardından, eklemek için MQTT/AMQP iletileri Protokolü özelliklerini Düzenle `tracestate` olarak **sistem özelliği**. Daha ayrıntılı belirtmek gerekirse:

* MQTT için ekleme `%24.tracestate=timestamp%3d1539243209` ileti konusuna burada `1539243209` unix zaman damgası biçimi iletisinde oluşturma saati ile değiştirilmelidir. Örnek olarak, uygulamaya başvurmak [C SDK](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/iothubtransport_mqtt_common.c#L761)
* AMQP için ekleme `key("tracestate")` ve `value("timestamp=1539243209")` ek açıklama iletisi. Bir başvuru uygulaması için bkz: [burada](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/uamqp_messaging.c#L527).

Bu özellik içeren iletileri yüzdesi denetlemek için ikizi güncelleştirmeleri gibi bulut tarafından başlatılan olayları dinlemek için mantığı uygulayın.

## <a name="update-sampling-options"></a>Güncelleştirme örnekleme seçenekleri 

Buluttan izlenecek iletileri yüzdesini değiştirmek için cihaz ikizi güncelleştirmeniz gerekir. JSON düzenleyicisini portalı ve IOT Hub hizmeti SDK'sını dahil olmak üzere bu birden çok yol gerçekleştirebilirsiniz. Aşağıdaki alt bölümleri örnekler sunar.

### <a name="update-using-the-portal"></a>Portalı kullanarak güncelleştirme

1. IOT hub'ına gidin [Azure portalında](https://portal.azure.com/), ardından **IOT cihazları**.

1. Cihazınızı tıklayın.

1. Aranacak **dağıtılmış izleme (Önizleme) etkinleştir**, ardından **etkinleştirme**.

    ![Azure portalında dağıtılmış izlemeyi etkinleştirme](./media/iot-hub-distributed-tracing/azure-portal.png)

1. Seçin bir **örnekleme hızı** %0 ile % 100 arasında.

1. **Kaydet**’e tıklayın.

1. Birkaç saniye bekleyin ve isabet **Yenile**, cihaz tarafından başarıyla kabul ettiyseniz, bir onay işareti ile bir eşitleme simgesi görüntülenir.

1. Telemetri ileti uygulaması için konsol penceresine geri dönün. İle gönderilen iletiler görürsünüz `tracestate` uygulama özellikleri.

    ![İzleme durumu](./media/iot-hub-distributed-tracing/MicrosoftTeams-image.png)

1. (İsteğe bağlı) Örnekleme hızını değiştirmek için farklı bir değer ve değişikliğin iletileri içeren sıklığı gözlemleyin `tracestate` uygulama özellikleri.

### <a name="update-using-azure-iot-hub-toolkit-for-vs-code"></a>VS Code için Azure IOT hub'ı Araç Seti kullanarak güncelleştirme

1. VS Code yükleyin ve ardından VS Code için Azure IOT hub'ı Araç Seti en son sürümünü yüklemek [burada](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

1. VS Code açın ve [IOT hub'ı bağlantı dizesi ayarlama](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit#user-content-prerequisites).

1. Cihaz genişletin ve Ara **dağıtılmış izlemeyi ayarlama (Önizleme)** . Seçeneğine tıklayın **güncelleştirme dağıtılmış izlemeyi ayarlama (Önizleme)** alt düğümü.

    ![Azure IOT hub'ı araç seti, dağıtılmış izlemeyi etkinleştirme](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-1.png)

1. Açılan pencerede seçin **etkinleştirme**, örnekleme Oranı 100 onaylamak için Enter tuşuna basın.

    ![Güncelleştirme örnekleme modu](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-2.png)

    ![Güncelleştirme örnekleme hızı](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-3.png)

### <a name="bulk-update-for-multiple-devices"></a>Birden çok cihaz için toplu güncelleştirme

Birden fazla cihaza dağıtılmış izleme örnekleme yapılandırmasını güncelleştirmek için [otomatik cihaz Yapılandırması](iot-hub-auto-device-config.md). Bu ikizi şema izlediğinizden emin olun:

```json
{
    "properties": {
        "desired": {
            "azureiot*com^dtracing^1": {
                "sampling_mode": 1,
                "sampling_rate": 100
            }
        }
    }
}
```

| Öğe adı | Gerekli | Tür | Açıklama |
|-----------------|----------|---------|-----------------------------------------------------|
| `sampling_mode` | Evet | Integer | Şu anda iki mod değerleri, örnekleme açıp kapatmak için desteklenir. `1` açık ve `2` kapalıdır. |
| `sampling_rate` | Evet | Integer | Bu değer bir yüzde değeri. Yalnızca değerlerini `0` için `100` (sınırlar dahil) izin verilir.  |

## <a name="query-and-visualize"></a>Sorgulayın ve görselleştirin

Bir IOT Hub tarafından günlüğe kaydedilen tüm izlemeler görmek için tanılama ayarlarında seçili günlüğü deposunun sorgulayın. Bu bölümde birkaç farklı seçenekler gösterilmektedir.

### <a name="query-using-log-analytics"></a>Log Analytics kullanarak sorgulama

Ayarladıysanız [tanılama günlükleri ile Log Analytics](../azure-monitor/platform/diagnostic-logs-stream-log-store.md), günlükler için bakarak sorgu `DistributedTracing` kategorisi. Örneğin, bu sorgu, günlüğe kaydedilen tüm izlemeler gösterir:

```Kusto
// All distributed traces 
AzureDiagnostics 
| where Category == "DistributedTracing" 
| project TimeGenerated, Category, OperationName, Level, CorrelationId, DurationMs, properties_s 
| order by TimeGenerated asc  
```

Log Analytics tarafından gösterildiği örnek günlükleri:

| TimeGenerated | OperationName | Kategori | Düzey | CorrelationId | süre (MS) | Özellikler |
|--------------------------|---------------|--------------------|---------------|---------------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-02-22T03:28:28.633Z | DiagnosticIoTHubD2C | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01 |  | {"DeviceID": "AZ3166", "messageSize": "96", "callerLocalTimeUtc": "2018-02-22T03:27:28.633Z","calleeLocalTimeUtc": "2018-02-22T03:27:28.687Z"} |
| 2018-02-22T03:28:38.633Z | DiagnosticIoTHubIngress | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 20 | {"isRoutingEnabled":"false","parentSpanId":"0144d2590aacd909"} |
| 2018-02-22T03:28:48.633Z | DiagnosticIoTHubEgress | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 23 | {"endpointType": "EventHub", "Uçnoktaadı": "myEventHub", "parentSpanId": "0144d2590aacd909"} |

Farklı günlük türlerini anlamak için bkz: [Azure IOT hub'ı tanılama günlükleri](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

### <a name="application-map"></a>Uygulama Eşlemesi

IOT iletileri akışını görselleştirmek için Uygulama Haritası örnek uygulamasını ayarlayın. Örnek uygulama için Dağıtılmış izleme günlükleri gönderir [Uygulama Haritası](../application-insights/app-insights-app-map.md) bir Azure işlevi ve bir olay hub'ı kullanarak.

> [!div class="button"]
> <a href="https://github.com/Azure-Samples/e2e-diagnostic-provision-cli" target="_blank">Github üzerinde örnek edinin</a>

Bu aşağıdaki resimde, üç yönlendirme uç noktaları ile uygulama eşlemesinde dağıtılmış izleme gösterir:

![Dağıtılmış IOT uygulama eşlemesinde izleme](./media/iot-hub-distributed-tracing/app-map.png)

## <a name="understand-azure-iot-distributed-tracing"></a>Azure IOT dağıtılmış izlemeyi anlama

### <a name="context"></a>Bağlam

Kendi dahil olmak üzere birçok IOT çözümleri [başvuru mimarisi](https://aka.ms/iotrefarchitecture) (yalnızca İngilizce), genellikle bir değişken izleyin [mikro hizmet mimarisi](https://docs.microsoft.com/azure/architecture/microservices/). Bir IOT çözümü daha karmaşık büyüdükçe, bir düzine ya da daha fazla mikro hizmetler kullanarak sonlandırın. Bu mikro olabilir veya Azure'dan olabilir. IOT iletileri bırakmayı veya yavaşlamasıdır Burada sunulan zor olabilir. Örneğin, 5 farklı Azure hizmetlerini ve etkin 1500 cihazları kullanan bir IOT çözümü vardır. Her cihaz 10 CİHAZDAN buluta iletiler/saniye (15.000 toplam ileti/saniye için) gönderir, ancak web uygulamanızı yalnızca 10.000 ileti/saniye görür dikkat edin. Sorun nerede? Sabah nasıl bulabilirim?

### <a name="distributed-tracing-pattern-in-microservice-architecture"></a>Dağıtılmış izleme modelinde mikro hizmet mimarisi

IOT ileti akışı farklı hizmetler arasında yeniden oluşturmak için her hizmetin yaymak bir *bağıntı kimliği* iletinin benzersiz olarak tanımlayan. Bir merkezi sistemde toplandığında, bağıntı kimlikleri, ileti akışı görmek etkinleştirin. Bu yöntem çağrılır [dağıtılmış izleme deseni](https://docs.microsoft.com/azure/architecture/microservices/logging-monitoring#distributed-tracing).

Dağıtılmış izleme için geniş benimseme desteklemek için Microsoft için katkıda bulunduğu [dağıtılmış izleme için W3C standart teklifi](https://w3c.github.io/trace-context/).

### <a name="iot-hub-support"></a>IOT hub'ı desteği

Etkinleştirildikten sonra IOT hub'ın dağıtılmış izleme desteği bu akışını izleyerek devam edeceğiz:

1. IOT cihaz üzerinde bir ileti oluşturulur.
1. IOT cihaz (yardımıyla bulut) Bu ileti bir izleme bağlamı ile atanmalıdır karar verir.
1. SDK'sını ekler bir `tracestate` ileti uygulama özelliğini içeren ileti oluşturma zaman damgası.
1. IOT cihaz IOT Hub'ına ileti gönderir.
1. IOT hub ağ geçidi bir ileti gelirse.
1. IOT Hub'ı arar `tracestate` ileti uygulama özellikleri ve doğru biçimde olup olmadığını denetler.
1. Bu nedenle, IOT hub'ı oluşturur ve günlükleri `trace-id` ve `span-id` kategorisi altında Azure İzleyici tanılama günlüklerine `DiagnosticIoTHubD2C`.
1. İleti işleme tamamlandıktan sonra IOT hub'ı başka bir oluşturur `span-id` ve varolan yanı sıra oturum `trace-id` kategorisi altında `DiagnosticIoTHubIngress`.
1. İleti için yönlendirme etkinse, IOT hub'ı özel uç noktaya yazar ve başka bir oturum `span-id` aynı `trace-id` kategorisi altında `DiagnosticIoTHubEgress`.
1. Yukarıdaki adımları üretilen her ileti için yinelenir.

## <a name="public-preview-limits-and-considerations"></a>Genel Önizleme sınırlamaları ve dikkat edilmesi gerekenler

- İzleme bağlamı W3C standart şu anda çalışma taslak teklifidir.
- Şu anda, istemci SDK'sı tarafından desteklenen tek geliştirme c bir dildir
- Bulut-cihaz ikizi özelliği için kullanılamaz [IOT hub'ı temel katman](iot-hub-scaling.md#basic-and-standard-tiers). Ancak, IOT hub'ı yine de düzgün oluşan izleme içerik üstbilgisinde görürse Azure İzleyici günlüğe kaydeder.
- Verimli çalışmasını sağlamak için IOT hub'ı bir dağıtılmış izleme bir parçası olarak ortaya çıkabilecek günlük fiyat bir kısıtlama dayatır.

## <a name="next-steps"></a>Sonraki adımlar

- Mikro hizmetler genel dağıtılmış izleme modelinde hakkında daha fazla bilgi için bkz: [mikro hizmet mimarisi Desen: izleme dağıtılmış](https://microservices.io/patterns/observability/distributed-tracing.html).
- Çok sayıda cihaz için Dağıtılmış izleme ayarları uygulamak için yapılandırmasını ayarlamak için bkz [yapılandırma ve izleme IOT cihazlarını uygun ölçekte](iot-hub-auto-device-config.md).
- Azure İzleyici hakkında daha fazla bilgi için bkz: [Azure İzleyici nedir?](../azure-monitor/overview.md).
