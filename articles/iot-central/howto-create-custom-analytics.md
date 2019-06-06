---
title: Azure IOT Central ile özel analiz genişletin | Microsoft Docs
description: Bir çözüm geliştirici olarak, bir IOT Central uygulamasına özel analiz ve görselleştirmeler yapmak için yapılandırın. Bu çözüm, Azure Databricks kullanır.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e039e2b8d9c183b5bfee1bee47e4addc4c873bf7
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743448"
---
# <a name="extend-azure-iot-central-with-custom-analytics"></a>Azure IOT Central ile özel analizi genişletme

Bu nasıl yapılır kılavuzunda, bir çözüm geliştirici olarak gösteren özel analiz ve görselleştirmeler ile IOT Central uygulamanızı genişletme. Örnekte bir [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) IOT Central telemetri akışını analiz ve görselleştirmeler gibi oluşturmak için çalışma alanı [kutusunda çizimleri](https://wikipedia.org/wiki/Box_plot).

Bu nasıl yapılır kılavuzunda IOT Central zaten ile getirebileceği ötesine genişletmek gösterilmektedir [yerleşik analiz araçları](howto-create-analytics.md).

Nasıl yapılır bu kılavuzda şunların nasıl yapılır:

* Stream kullanarak bir IOT Central uygulama telemetri *verileri sürekli dışarı aktarma*.
* Analiz etmek ve cihaz telemetrisi çizmek için bir Azure Databricks ortam oluşturun.

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

Kullanım [bir kaynak grubu oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.ResourceGroup) adlı **IoTCentralAnalysis** oluşturduğunuz diğer kaynakları içerecek. Azure kaynaklarınızı IOT Central uygulamanız ile aynı konumda oluşturun.

### <a name="event-hubs-namespace"></a>Event Hubs ad alanı

Kullanım [bir Event Hubs ad alanı oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.EventHub) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Ad    | Ad alanı adı seçin |
| Fiyatlandırma katmanı | Temel |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | IoTCentralAnalysis |
| Location | Doğu ABD |
| İşleme Birimleri | 1 |

### <a name="azure-databricks-workspace"></a>Azure Databricks çalışma alanı

Kullanım [Azure Databricks hizmeti oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.Databricks) aşağıdaki ayarlara sahip:

| Ayar | Değer |
| ------- | ----- |
| Çalışma alanı adı    | Çalışma alanınızın adının seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | IoTCentralAnalysis |
| Location | Doğu ABD |
| Fiyatlandırma Katmanı | Standart |

Gerekli kaynaklardan oluşturduğunuz zaman, **IoTCentralAnalysis** kaynak grubu, aşağıdaki ekran görüntüsüne benzer görünür:

![IOT Central analiz kaynak grubu](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Sürekli olarak olay hub'ına telemetri dışarı aktarmak için bir IOT Central uygulamasına yapılandırabilirsiniz. Bu bölümde, IOT Central uygulamanızdan telemetri almak için bir olay hub'ı oluşturun. Olay hub'ı işleme için Stream Analytics işinizi telemetri gönderir.

1. Azure portalında Event Hubs ad alanınıza gidin ve seçin **+ olay hub'ı**.
1. Olay hub'ınızı adlandırın **centralexport**seçip **Oluştur**.
1. Event hubs ad alanınızdaki listesinde seçin **centralexport**. Ardından **paylaşılan erişim ilkeleri**.
1. **+ Ekle** öğesini seçin. Adlı bir ilke oluşturun **dinleme** ile **dinleme** talep.
1. İlke hazır olduğunda, listeden seçin ve ardından kopyalama **bağlantı dizesi-birincil anahtar** değeri.
1. Bu bağlantı dizesini not edin, olay hub'ından okumak için Databricks not defteri yapılandırdığınızda, daha sonra kullanın.

Event Hubs ad alanınız şu ekran görüntüsüne benzer görünür:

![Event Hubs ad alanı](media/howto-create-custom-analytics/event-hubs-namespace.png)

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

![Verileri sürekli dışarı aktarma yapılandırması](media/howto-create-custom-analytics/cde-configuration.png)

Dışarı aktarma durumunu tamamlanana kadar bekleyin **çalıştıran** devam etmeden önce.

## <a name="configure-databricks-workspace"></a>Databricks çalışma alanını yapılandırma

Azure portalında, Azure Databricks hizmetinize gidin ve seçin **çalışma alanını Başlat**. Yeni bir sekme tarayıcınızda açılır ve çalışma alanınıza imzalar.

### <a name="create-a-cluster"></a>Küme oluşturma

Üzerinde **Azure Databricks** sayfasında, ortak görevler, seçim listesi altında **yeni kümeye**.

Bilgileri aşağıdaki tabloda, kümenizi oluşturmak için kullanın:

| Ayar | Değer |
| ------- | ----- |
| Küme Adı | centralanalysis |
| Küme modunu | Standart |
| Databricks çalışma zamanı sürümü | 5.3 (Scala 2.11, Spark 2.4.0) |
| Python sürümü | 3 |
| Otomatik Ölçeklendirmeyi Etkinleştirme | Hayır |
| Dakika işlem yapılmadıktan sonra Sonlandır | 30 |
| Çalışan türü | Standard_DS3_v2 |
| Çalışanlar | 1 |
| Sürücü türü | Çalışanla aynı |

Küme oluşturma birkaç dakika sürer, küme oluşturma devam etmeden önce tamamlanmasını bekleyin.

### <a name="install-libraries"></a>Kitaplıkları yükleme

Üzerinde **kümeleri** sayfasında, küme durumunu tamamlanana kadar bekleyin **çalıştıran**.

Aşağıdaki adımlar kümesine örneğinizi gereken kitaplığı içeri aktarma gösterir:

1. Üzerinde **kümeleri** sayfasında, durumu kadar bekleyin **centralanalysis** etkileşimli küme **çalıştıran**.

1. Kümeyi seçin ve ardından **kitaplıkları** sekmesi.

1. Üzerinde **kitaplıkları** sekmesini, **yükleme yeni**.

1. Üzerinde **kitaplık yükleme** sayfasında **Maven** kitaplık kaynağı.

1. İçinde **koordinatları** metin kutusuna şu değeri girin: `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Seçin **yükleme** kümede kitaplığını yüklemek için.

1. Kitaplık durumunu sunulmuştur **yüklü**:

    ![Yüklü kitaplığı](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Bir Databricks not defteri alma

Python kodunu analiz edin ve görselleştirin, IOT Central telemetri içeren bir Databricks not defteri içeri aktarmak için aşağıdaki adımları kullanın:

1. Gidin **çalışma** Databricks ortamınızdaki sayfası. Hesap adınızın yanındaki açılır menüyü seçin ve sonra **alma**.

1. Aşağıdaki adresi girin ve bir URL'den içeri aktarmak için seçin: [https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Not Defteri içeri aktarmayı tercih **alma**.

1. Seçin **çalışma** içeri aktarılan not defteri görüntülemek için:

    ![İçeri aktarılan not defteri](media/howto-create-custom-analytics/import-notebook.png)

1. Daha önce kaydettiğiniz Event Hubs bağlantı dizesi eklemek için ilk Python hücre kodunu düzenleyin:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Analizini Çalıştır

Analizi çalıştırmak için Not defterini kümeye eklemeniz gerekir:

1. Seçin **Detached** seçip **centralanalysis** kümesi.
1. Küme çalışmıyorsa başlatın.
1. Not defterini başlatmak için Çalıştır düğmesi seçin.

Son hücresinde bir hata görebilirsiniz. Bu durumda, önceki hücreleri çalışıp denetleyin, bazı verileri depolama alanına yazılacak bir dakika bekleyin ve ardından son hücreye yeniden çalıştırın.

### <a name="view-smoothed-data"></a>Düzleştirilmiş veri görüntüleme

Not defterinde hücre 14 çalışırken ortalama nem oranı cihaz türüne göre bir çizim görmek için aşağı kaydırın. Akış telemetri ulaştığında Bu çizim sürekli olarak güncelleştirir:

![Telemetri çizim düzleştirilmiş](media/howto-create-custom-analytics/telemetry-plot.png)

Not defterinde grafiği yeniden boyutlandırabilirsiniz.

### <a name="view-box-plots"></a>Görünüm kutusu çizer

Not defterinde hücre 20 görmek için aşağı kaydırın [kutusunda çizimleri](https://en.wikipedia.org/wiki/Box_plot). Bunları güncelleştirmek için hücre yeniden şekilde kutusu çizimleri statik verilere dayanır:

![Kutusu çizer](media/howto-create-custom-analytics/box-plots.png)

Not defterinde çizimleri yeniden boyutlandırabilirsiniz.

## <a name="tidy-up"></a>Çatalınızdan

Bu nasıl yapılır sonra çatalınızdan ve gereksiz maliyetleri önlemek için silin **IoTCentralAnalysis** Azure portalında kaynak grubu.

IOT Central uygulamadan silebilirsiniz **Yönetim** uygulama içinde sayfa.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda öğrendiğiniz nasıl yapılır:

* Stream kullanarak bir IOT Central uygulama telemetri *verileri sürekli dışarı aktarma*.
* Analiz etmek ve telemetri verilerini çizmek için bir Azure Databricks ortam oluşturun.

Size özel analytics oluşturmayı biliyorsanız, önerilen sonraki adıma öğrenmektir nasıl [Görselleştir ve Azure IOT Central bir Power BI panosunda çözümlemek](howto-connect-powerbi.md).
