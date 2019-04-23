---
title: Time Series Insights - Uzaktan izleme ile tümleştirerek Azure | Microsoft Docs
description: Bu nasıl yapılır makalesinde Time Series Insights zaten içermeyen mevcut bir uzaktan izleme çözüm Time Series Insights'ı yapılandırmak öğreneceksiniz.
author: aditidugar
manager: timlt
ms.author: adugar
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 4cc9b0051eaa12eee07f067352126ad159107a83
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60007483"
---
# <a name="integrate-azure-time-series-insights-with-remote-monitoring"></a>Azure Time Series Insights’ı Uzaktan İzleme ile tümleştirme

Azure Time Series Insights, bulutta IoT ölçekli zaman serisi verilerinin yönetimi için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Time Series Insights, depolamak ve zaman serisi verileri yönetmek, keşfedin ve aynı anda olaylar görselleştirin, kök neden analizi gerçekleştirin ve birden çok siteyi ve varlığı karşılaştırın için kullanabilirsiniz.

Uzaktan izleme çözüm Hızlandırıcısını artık otomatik dağıtım ve Time Series Insights ile tümleştirme sağlar. Bu nasıl yapılır zaman serisi görüşleri Time Series Insights zaten içermeyen mevcut bir uzaktan izleme çözüm yapılandırma konusunda bilgi edinin.

> [!NOTE]
> Zaman serisi görüşleri Azure Çin Bulutu şu anda kullanılabilir değil. Yeni Uzaktan izleme çözüm Hızlandırıcı dağıtımlarda Azure Çin Bulutu, Cosmos DB için tüm depolama kullanın.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır tamamlamak için bir uzaktan izleme çözümü dağıtmış olmanız gerekir:

* [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluşturun

Time Series Insights veri akışı için kullanılmak üzere IOT hub'ınızdaki ayrılmış bir tüketici grubu oluşturun.

> [!NOTE]
> Tüketici grupları, Azure IOT Hub'ından veri çekmek için uygulamalar tarafından kullanılır. Her bir tüketici grubu en fazla beş çıkış tüketiciler sağlar. 32 adede kadar tüketici grubu oluşturabilirsiniz ve her beş havuzlarını çıktısı, yeni bir tüketici grubu oluşturmanız gerekir.

1. Azure portalında Cloud Shell düğmesine tıklayın.

1. Yeni bir tüketici grubu oluşturmak için aşağıdaki komutu yürütün. Kaynak grubu adı IOT hub'ı Uzaktan izleme, dağıtımınızdaki adı ve Uzaktan izleme dağıtımınızı adını kullanın:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="deploy-time-series-insights"></a>Time Series Insights'ı dağıtma

Ardından, zaman serisi görüşleri, Uzaktan izleme çözümünüze ek bir kaynak dağıtma ve IOT hub'ına bağlanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Seçin **kaynak Oluştur** > **nesnelerin interneti** > **Time Series Insights**.

    ![Yeni zaman serisi görüşleri](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights.png)

1. Time Series Insights ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Value |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran adı kullanan **contorosrmtsi**. Bu adımı tamamladığınızda, kendi benzersiz bir ad seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **Var olanı kullan**. Varolan Uzaktan izleme kaynak grubu adını seçin. |
    | Location | Kullanıyoruz **Doğu ABD**. Ortamınızı mümkünse Uzaktan izleme çözümünüzü aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Zaman serisi görüşleri oluşturma](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights-create.png)

1. **Oluştur**’a tıklayın. Uygulamanın oluşturulması için ortamı için biraz sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IOT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturulan tüketici grubu kullandığınızdan emin olun. Zaman serisi görüşleri her hizmeti, ayrılmış bir tüketici grubu başka bir hizmet tarafından kullanımda olması gerekir.

1. Yeni zaman serisi görüşleri ortamınıza gidin.

1. Sol tarafta, seçin **olay kaynakları**.

    ![Olay kaynakları görüntüle](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle**'ye tıklayın.

    ![Olay kaynağı ekleme](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources-add.png)

1. IOT hub'ınıza yeni bir olay kaynağı yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Value |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran adı kullanan **contosorm IOT hub**. Bu adımı tamamladığınızda, kendi benzersiz bir ad kullanın. |
    | Kaynak | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Mevcut aboneliklerden IOT hub'ı kullanın** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | Iot hub'ı adı | **contosorma57a6**. Uzaktan izleme çözümünüzden IOT hub'ınızın adını kullanın. |
    | Iot hub'ı ilke adı | **iothubowner** kullanılan ilkeyi sahibi ilke olduğundan emin olun. |
    | Iot hub'ı ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | Iot hub'ı tüketici grubu | **timeseriesinsights** |
    | Olay serileştirme biçimi | **JSON**     | 
    | Zaman damgası özellik adı | Boş bırakın |

    ![Olay kaynağı oluşturma](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur**’a tıklayın.

## <a name="configure-the-data-access-policy"></a>Veri erişim ilkesini yapılandırma

Uzaktan izleme çözümünüzü erişimi olan tüm kullanıcılar Time Series Insights gezgininin verileri incelemek için emin olmak için Azure portalında uygulama ve veri erişimi ilkeleri kapsamındaki kullanıcıların ekleyin. 

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Seçin **ContosoRM** kaynak grubu.

1. Seçin **contosormtsi** Azure kaynaklarını listesinde.

1. Seçin **veri erişimi ilkeleri** rol atamaları geçerli listesini görmek için.

1. Seçin **Ekle** açmak için **kullanıcı kuralı seçin** bölmesi.

   Rol atama izinleri yoksa, görmüyorsanız **Ekle** seçeneği.

1. İçinde **rol** aşağı açılan listesinde, bir rol gibi seçin **okuyucu** ve **katkıda bulunan**.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin. Birkaç dakika sonra güvenlik sorumlusu veri erişimi ilkeleri rolde atanır.

> [!NOTE]
> Ek kullanıcılar için Time Series Insights gezgininin erişim gerekiyorsa, bu adımlara kullanabileceğiniz [veri erişim izni verme](../time-series-insights/time-series-insights-data-access.md#grant-data-access).

## <a name="configure-azure-stream-analytics"></a>Azure Stream Analytics yapılandırın 

Sonraki adım, Cosmos DB'ye gönderme kesmek için Azure Stream Analytics Yöneticisi mikro hizmet yapılandırın ve yalnızca zaman serisi Öngörülerinde depolamaya sağlamaktır. Cosmos DB'de iletilerinizi çoğaltmak istiyorsanız bu adımı atlayın.

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Seçin **ContosoRM** kaynak grubu.

1. Akış işi kaynakları listesinden Azure Stream Analytics (ASA) bulun. Kaynak adı ile başlayan **streamingjobs -**.

1. En üstünde, ASA işleri akış durdurmak için düğmeye tıklayın.

1. ASA sorguyu düzenlemek ve kaldırma **seçin**, **INTO**, ve **FROM** iletileri noktası yan tümceleri Cosmos DB'de akış. Bu yan tümceleri, sorgu alt kısmında olması ve aşağıdaki örneğe:

    ```sql
    SELECT
            CONCAT(T.IoTHub.ConnectionDeviceId, ';', CAST(DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) AS nvarchar(max))) as id,
            1 as [doc.schemaVersion],
            'd2cmessage' as [doc.schema],
            T.IoTHub.ConnectionDeviceId as [device.id],
            'device-sensors;v1' as [device.msg.schema],
            'StreamingJobs' as [data.schema],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', System.Timestamp) as [device.msg.created],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) as [device.msg.received],
            udf.removeUnusedProperties(T) as Data
    INTO
        Messages
    FROM
        DeviceTelemetry T PARTITION BY PartitionId TIMESTAMP BY T.EventEnqueuedUtcTime
    ```

6. Azure Stream Analytics akış işleri yeniden başlatın.

7. En son değişiklikleri, Azure Stream Analytics Yöneticisi mikro hizmet için komut istemine aşağıdaki komutu yazarak çekin:

.NET: 

```cmd/sh
docker pull azureiotpcs/asa-manager-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/asa-manager-java:1.0.2
```

## <a name="configure-the-telemetry-microservice"></a>Telemetri mikro hizmet yapılandırma

En son Telemetri mikro hizmet, komut istemine aşağıdaki komutu yazarak çekin:

.NET:

```cmd/sh
docker pull azureiotpcs/telemetry-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/telemetry-java:1.0.2
```

## <a name="optional-configure-the-web-ui-to-link-to-the-time-series-insights-explorer"></a>*[İsteğe bağlı]*  Web kullanıcı Arabirimi için Time Series Insights gezgininin bağlanmak için yapılandırın

Kolayca Time Series Insights explorer'ın verilerinizi görüntülemek için ortama kolayca bağlamak için kullanıcı arabirimini özelleştirme öneririz. Bunu yapmak için aşağıdaki komutu kullanarak Web Arabirimine en son değişiklikleri çekin:

```cmd/sh
docker pull azureiotpcs/pcs-remote-monitoring-webui:1.0.2
```

## <a name="configure-the-environment-variables"></a>Ortam değişkenlerini yapılandırma

Time Series Insights tümleştirmesi tamamlamak için güncelleştirilmiş mikro hizmetlere yönelik dağıtım ortamı yapılandırmanız gerekir.

### <a name="basic-deployments"></a>Temel dağıtımlar

Ortamını yapılandırmak `basic` güncelleştirilmiş mikro hizmetlere yönelik dağıtım.

1. Azure portalında tıklayarak **Azure Active Directory** sol panelde sekmesi.

1. Tıklayarak **uygulama kayıtları**.

1. Arayın ve'a tıklayın, **ContosoRM** uygulama.

1. Gidin **ayarları** > **anahtarları** ve uygulamanız için yeni bir anahtar oluşturun. Anahtar değeri güvenli bir konuma kopyalamak emin olun.

1. Çekme [en son docker compose yaml dosyası](https://github.com/Azure/pcs-cli/tree/5a9b4e0dbe313172eff19236e54a4d461d4f3e51/solutions/remotemonitoring/single-vm) son etiketini kullanarak GitHub deposundan. 

1. Şirket özetlenen adımları izleyerek VM içine SSH [nasıl SSH anahtarları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

1. Bağlantı kurulduktan sonra yazın `cd /app`.

1. Her mikro hizmet docker'da aşağıdaki ortam değişkenleri yaml dosyası oluşturma ve `env-setup` VM'de betik:

    ```sh
    PCS_TELEMETRY_STORAGE_TYPE=tsi
    PCS_TSI_FQDN={TSI Data Access FQDN}
    PCS_AAD_TENANT={AAD Tenant Id}
    PCS_AAD_APPID={AAD application Id}
    PCS_AAD_APPSECRET={AAD application key}
    ```

1. Gidin **telemetri hizmeti** ve ayrıca düzenleme docker compose dosyası aynı ortam değişkenlerini yukarıdaki ekleyerek.

1. Gidin **ASA Yöneticisi hizmeti** ve düzenleme docker compose dosyası ekleyerek `PCS_TELEMETRY_STORAGE_TYPE`.

1. Docker kapsayıcıları kullanarak yeniden `sudo ./start.sh` VM'den.

> [!NOTE]
> Ortam değişkenlerini Yukarıdaki yapılandırma 1.0.2 önce Uzaktan izleme sürümleri için geçerlidir

### <a name="standard-deployments"></a>Standart dağıtım

Ortamını yapılandırmak `standard` yukarıdaki güncelleştirilmiş mikro hizmetlerin dağıtımı

1. Komut satırında çalıştırın `kubectl proxy`. Daha fazla bilgi için [Kubernetes API'sine erişim](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/#using-kubectl-to-start-a-proxy-server).

1. Kubernetes Yönetim Konsolu'nu açın.

1. TSI için aşağıdaki yeni ortam değişkenleri eklemek için yapılandırma eşlemesi bulun:

    ```yaml
    telemetry.storage.type: "tsi"
    telemetry.tsi.fqdn: "{TSI Data Access FQDN}"
    security.auth.serviceprincipal.secret: "{AAD application service principal secret}"
    ```

4. Telemetri hizmeti pod şablon yaml dosyası Düzenle:

    ```yaml
    - name: PCS_AAD_TENANT
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.tenant
    - name: PCS_AAD_APPID
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.audience
    - name: PCS_AAD_APPSECRET
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.serviceprincipal.secret
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    - name: PCS_TSI_FQDN
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.tsi.fqdn
    ```

5. ASA Yöneticisi hizmeti pod şablon yaml dosyası Düzenle:

    ```yaml
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Verilerinizi keşfetme ve bir uyarı Time Series Insights Gezgininde Tanılama hakkında bilgi edinmek için öğreticimize bakın [yürütülmesi bir kök neden analizi](iot-accelerators-remote-monitoring-root-cause-analysis.md).

* Keşfetme ve Time Series Insights gezgininin verilerde sorgulama hakkında bilgi edinmek için ilgili belgelere bakın [Azure Time Series Insights gezgininin](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
