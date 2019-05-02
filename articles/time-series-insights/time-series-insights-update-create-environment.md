---
title: 'Öğretici: Bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama | Microsoft Docs'
description: Azure zaman serisi öngörüleri önizlemesinde ortamınızı ayarlama konusunda bilgi edinin.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: f83063a88207f51f9d481447923fd8a8498692a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713903"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Öğretici: Bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama

Bu öğreticide bir Azure zaman serisi öngörüleri önizlemesi Kullandıkça Öde (PAYG) ortamı oluşturma işleminde size kılavuzluk eder. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Azure zaman serisi öngörüleri Önizleme ortamı oluşturun.
* Azure zaman serisi öngörüleri Önizleme ortamı, Azure Event Hubs bir olay hub'ına bağlanın.
* Bir çözüm Hızlandırıcı örnek akışı verilerini Azure zaman serisi öngörüleri önizlemesi ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlamak ve örneklerinizin ile ilişkilendirebilirsiniz.

# <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Bu bölümde, bir IOT hub'ına veri gönderecek üç sanal cihazlar oluşturun.

1. Git [Azure IOT Çözüm Hızlandırıcıları sayfasında](https://www.azureiotsolutions.com/Accelerators). Sayfanın birkaç önceden oluşturulmuş örnekler görüntülenmektedir. Azure hesabınızı kullanarak oturum açın. Ardından, **cihaz benzetimi**.

   [![Azure IOT Çözüm Hızlandırıcıları sayfası](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Seçin **şimdi deneyin**.

1. **Cihaz Benzetimi çözümü oluşturun** sayfasına gerekli parametreleri girin:

   | Parametre | Açıklama |
   | --- | --- |
   | **Çözüm adı** |    Yeni bir kaynak grubu oluşturmak için benzersiz bir değer girin. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır. |
   | **Abonelik** | Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin. |
   | **Bölge** |   Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölge belirtin. |
   | **İsteğe bağlı Azure Kaynaklarını dağıtma**    | Sanal cihazlar bağlanmak ve veri akışı için kullanacağınız için IOT Hub'ı seçili bırakın. |

   Ardından, **çözüm oluşturma**. Dağıtılacak çözümünüzü 10-15 dakika bekleyin.

   [![Cihaz benzetimi çözüm sayfası oluşturma](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. Çözüm Hızlandırıcı panosunda seçin **başlatma** düğmesi:

   [![Cihaz benzetimi çözümü başlatma](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. İçin yönlendirilirsiniz **Microsoft Azure IOT cihaz benzetimi** sayfası. Seçin **+ yeni benzetimi** sayfanın sağ üst.

   [![Azure IOT simülasyon sayfası](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Gerekli parametreleri aşağıdaki gibi doldurun:

    [![Parametreleri doldurun](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    |||
    | --- | --- |
    | **Ad** | Simülatör için benzersiz bir ad girin. |
    | **Açıklama** | Bir tanım girin. |
    | **Benzetim süresi** | Kümesine **süresiz olarak çalıştırmak**. |
    | **Cihaz modeli** | **Ad**: `Chiller` yazın. </br>**Miktar**: `3` yazın. |
    | **Hedef IoT Hub'ı** | Kümesine **kullanın, IOT hub'ı önceden sağlanan**. |

    Ardından, **benzetimi Başlat**.

1. Cihaz benzetimi panosunda görünür **etkin cihazlar** ve **saniye başına ileti**.

    [![Azure IOT simülasyon Panosu](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Liste cihaz benzetimi özellikleri

Azure zaman serisi görüşleri ortamı oluşturmadan önce IOT hub, abonelik ve kaynak grubu adları gerekir.

1. Çözüm Hızlandırıcı panosuna gidin ve aynı Azure aboneliği hesabını kullanarak oturum açın. Önceki adımlarda oluşturduğunuz cihaz benzetimi bulun.

1. Cihaz simülatör'ü seçip **başlatma**. Seçin **Azure Yönetim Portalı** işlecin sağ tarafındaki bağlantı.

    [![Simülatör listesi](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. IOT hub, abonelik ve kaynak grubu adlarını not alın.

    [![Azure portalı](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Zaman serisi öngörüleri Önizleme PAYG ortamı oluşturma

Bu bölümde kullanarak Azure zaman serisi öngörüleri Önizleme ortamı oluşturmayı açıklar [Azure portalında](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum açın.

1. Seçin **kaynak Oluştur**.

1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**.

   [![Nesnelerin interneti'ni seçin ve ardından Time Series Insights'ı seçin](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | ---|
   | **Ortam adı** | Azure zaman serisi öngörüleri Önizleme ortamı için benzersiz bir ad seçin. |
   | **Abonelik** | Azure zaman serisi öngörüleri Önizleme ortamı oluşturmak istediğiniz aboneliğinizi girin. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı abonelik kullanmak için en iyi bir uygulamadır. |
   | **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin ya da Azure zaman serisi öngörüleri Önizleme ortamı kaynak için yeni bir tane oluşturun. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı kaynak grubunu kullanmak için en iyi bir uygulamadır. |
   | **Konum** | Azure zaman serisi öngörüleri önizlemesi ortamınız için bir veri merkezi bölgesini seçin. Ek bant genişliği maliyetlerini ve gecikme süresini önlemek için diğer IOT kaynaklarıyla aynı bölgede Azure zaman serisi öngörüleri Önizleme ortamı tutmak en iyisidir. |
   | **Katmanı** |  Seçin **PAYG**, Kullandıkça Öde için anlamına gelir. Azure zaman serisi öngörüleri önizlemesi ürün için SKU budur. |
   | **özellik kimliği** | Zaman serisi, benzersiz olarak tanımlayan bir şey girin. Bu alan sabittir ve daha sonra değiştirilemez olduğunu unutmayın. Bu öğretici için kullanmak `iothub-connection-device-id`. Zaman serisi kimliği hakkında daha fazla bilgi edinmek için [bir zaman serisi kimliği seçme](./time-series-insights-update-how-to-id.md). |
   | **Depolama hesabı adı** | Oluşturulacak yeni bir depolama hesabı için genel olarak benzersiz bir ad girin. |

   Ardından, **sonraki: Olay kaynağı**.

   [![Zaman serisi görüşleri ortamı oluşturma sayfası](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Olay kaynağı sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | --- |
   | **Olay kaynağı oluşturma?** | `Yes` yazın.|
   | **Ad** | Olay kaynağı adı için kullanılan benzersiz bir değer girin.|
   | **Kaynak türü** | Seçin **IOT hub'ı**. |
   | **Bir hub seçilsin mi?** | Seçin **var olanı Seç**. |
   | **Abonelik** | Cihaz simülatörü kullanılan aboneliği seçin. |
   | **IOT hub'ı adı** | Cihaz simülatörü oluşturduğunuz IOT hub adını seçin. |
   | **IOT hub'ı erişim ilkesini** | Seçin **iothubowner**. |
   | **IOT Hub tüketici grubu** | Azure zaman serisi öngörüleri önizlemesi için benzersiz bir tüketici grubu ihtiyacınız var. Seçin **yeni**benzersiz bir ad girin ve ardından **Ekle**. |
   | **Zaman damgası özelliği** | Bu alan zaman damgası özelliği, gelen telemetri verilerini tanımlamak için kullanılır. Bu öğreticide, alanını doldurun yok. Bu simülatörü Time Series Insights'ın varsayılan olarak IOT Hub'ından gelen zaman damgası kullanır.|

   Ardından, **gözden geçir + Oluştur**.

   [![Olay kaynağını yapılandırma](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Gözden geçir sayfasında tüm alanları gözden geçirin ve seçin **Oluştur**.

   [![Gözden geçir + oluştur düğmesi ile oluşturma sayfası](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

1. Dağıtımınızın durumunu görebilirsiniz.

   [![Dağıtım tamamlandıktan bildirimi](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Kiracı sahipseniz Azure zaman serisi öngörüleri Önizleme ortamınıza erişimi almanız gerekir. Erişiminiz olduğundan emin olmak için:

   a. Kaynak grubunuz için arama yapın ve Azure zaman serisi öngörüleri önizlemesi ortamınızı seçin:

      [![Seçili ortam](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   b. Azure zaman serisi öngörüleri önizleme sayfasında Git **veri erişimi ilkeleri**.

     [![Veri erişimi ilkeleri](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   c. Kimlik bilgilerinizi listelendiğini doğrulayın.

     [![Listelenen kimlik bilgileri](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Kimlik bilgilerinizi listelenmemiş, kendiniz ortama erişim izni vermeniz gerekir. İzinleri ayarlama hakkında daha fazla bilgi edinmek için [veri erişim ver](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, temel analiz zaman serisi verilerinizi kullanarak gerçekleştirdiğiniz [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

1. Git kaynak sayfasından URL seçerek Azure zaman serisi öngörüleri önizlemesi gezgininizde [Azure portalında](https://portal.azure.com/).

   [![Zaman serisi öngörüleri Önizleme explorer URL'si](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Gezgini'nde seçin **zaman serisi örnekleri** ortamdaki tüm Azure zaman serisi öngörüleri Önizleme örneklerini görmek için düğümü.

   [![Ana öğesiz örneklerinin listesi](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Zaman serisi içinde gösterilen ilk örnek seçin. Ardından, **Göster ortalama baskısı**.

   [![Ortalama baskısı göstermek için menü komutu ile seçilen örneği](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

   Zaman serisi grafiği, sağ tarafta görüntülenmesi gerekir. Ayarlama **aralığı** için `15s`.

   [![Zaman serisi grafiği](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Yineleme **3. adım** diğer iki zaman serisi. Ardından, bu grafikte gösterildiği gibi tüm zaman damgaları görüntüleyebilirsiniz:

   [![Tüm zaman damgaları için grafik](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Son saat üzerinden zaman serisi eğilimleri görmek için zaman aralığını değiştirin.

   a. Seçin **zaman çerçevesini** seçenek kutusu:

      [![Zaman aralığı bir saate ayarlayın](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Tanımlayın ve bir modeli uygulayın

Bu bölümde, verilerinizin yapısı için bir model uygulayın. Model tamamlamak için türleri, hiyerarşileri ve örnekleri tanımlayın. Veri modelleme hakkında daha fazla bilgi için şuraya gidin [zaman serisi modeli](./time-series-insights-update-tsm.md).

1. Gezgini'nde seçin **modeli** sekmesinde:

   [![Explorer'da modeli sekmesi](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Seçin **+ Ekle** bir türü eklemek için. Sağ tarafta bir türü Düzenleyicisi açılır.

   [![Türleri için Ekle düğmesine](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Üç tür değişkenleri tanımlayın: baskı, sıcaklık ve nem oranı. Aşağıdaki bilgileri girin:

   | | |
   | --- | ---|
   | **Ad** | `Chiller` yazın. |
   | **Açıklama** | `This is a type definition of Chiller` yazın. |

   * Üç değişkenleriyle baskısı tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | `Avg Pressure` yazın. |
      | **Değer** | Seçin **baskısı (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **ortalama**. |

      [![Basınç tanımlama seçimleri](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Seçin **+ değişkeni Ekle** sonraki değişkeni eklemek için.

   * Sıcaklık tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | `Avg Temperature` yazın. |
      | **Değer** | Seçin **sıcaklık (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **ortalama**.|

      [![Sıcaklık tanımlama seçimleri](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

   * Nem tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin `Max Humidity` |
      | **Değer** | Seçin **nem (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **MAX**.|

      [![Sıcaklık tanımlama seçimleri](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

   Ardından **Oluştur**’u seçin.

1. Eklenen türünüz görebilirsiniz:

   [![Eklenen türü hakkında bilgi](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. Sonraki adım, bir hiyerarşi eklemektir. İçinde **hiyerarşileri** bölümünden **+ Ekle**:

   [![Hiyerarşiler Sekme Ekle düğmesi](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Hiyerarşi tanımlayın. Alanları aşağıdaki gibi doldurun:

   | | |
   | --- | ---|
   | **Ad** | `Location Hierarchy` yazın. |
   | **Düzey 1** | `Country` yazın. |
   | **Düzey 2** | `City` yazın. |
   | **3. düzey** | `Building` yazın. |

   Ardından **Oluştur**’u seçin.

   [![Hiyerarşi alanları oluştur düğmesi](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

1. Oluşturduğunuz hiyerarşi görebilirsiniz:

   [![Hiyerarşisi hakkında bilgi](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Seçin **örnekleri** soldaki. Örnekleri görünür, ilk örneğini seçin ve ardından seçin sonra **Düzenle**:

   [![Bir örneği için Düzenle düğmesini seçme](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. Bir metin düzenleyicisi sağ tarafta görüntülenir. Aşağıdaki bilgileri ekleyin:

   | | |
   | --- | --- |
   | **Tür** | Seçin **Soğutucu**. |
   | **Açıklama** | `Instance for Chiller-01.1` yazın. |
   | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
   | **Ülke** | `USA` yazın. |
   | **Şehir** | `Seattle` yazın. |
   | **Yapı** | `Space Needle` yazın. |

    Ardından **Kaydet**’i seçin.

   [![Kaydet düğmesi ile örnek alanları](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

1. Başka bir algılayıcı için önceki adımı yineleyin. Aşağıdaki alanları kullanın:

   * Soğutucu 01.2 için:

     | | |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | `Instance for Chiller-01.2` yazın. |
     | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
     | **Ülke** | `USA` yazın. |
     | **Şehir** | `Seattle` yazın. |
     | **Yapı** | `Pacific Science Center` yazın. |

   * Soğutucu 01.3 için:

     | | |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | `Instance for Chiller-01.3` yazın. |
     | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
     | **Ülke** | `USA` yazın. |
     | **Şehir** | `New York` yazın. |
     | **Yapı** | `Empire State Building` yazın. |

1. Git **Çözümle** sekmesini ve sayfayı yenileyin. Zaman serisi bulmak için tüm hiyerarşi düzeyleri genişletin.

   [![Analiz sekmesi](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Zaman serisi son saat boyunca keşfetmek için değiştirme **hızlı süreler** için **son bir saat**:

    [![Hızlı süreler kutusuyla seçilmiş son bir saat](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Zaman serisi altında seçin **Pasifik bilimi Merkezi** seçip **Göster en yüksek nem**.

    [![En yüksek nem Göster menüsü seçimi ile seçilen zaman serisi](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. Zaman serisi için **en yüksek nem** bir aralığı boyutu ile **1 dakika** açılır. Aralık filtre uygulamak için bir bölge seçin. Ardından, sağ tıklayıp **yakınlaştırma** zaman çerçevesi içinde olayları analiz etmek için:

   [![Yakınlaştırma komutu bir kısayol menüsünde Seçili aralık](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Ayrıca bir bölge seçin ve ardından olay ayrıntılarını görmek için sağ tıklayın:

   [![Ayrıntılı olayların listesi](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

> [!div class="checklist"]
> * Oluşturun ve cihaz benzetimi Hızlandırıcı kullanın.
> * Azure zaman serisi öngörüleri Önizleme PAYG ortamı oluşturun.
> * Azure zaman serisi öngörüleri Önizleme ortamı, olay hub'ına bağlanın.
> * Azure zaman serisi öngörüleri Önizleme ortamı için veri akışı için bir çözüm Hızlandırıcı örneği çalıştırın.
> * Verilerin temel bir analiz gerçekleştirin.
> * Zaman serisi modeli türü ve hiyerarşi tanımlayın ve bunları örneklerinizin ile ilişkilendirin.

Kendi Azure zaman serisi öngörüleri Önizleme ortamı oluşturma işlemini öğrendiğinize göre Azure zaman serisi görüşleri temel kavramlar hakkında daha fazla bilgi edinebilirsiniz.

Azure zaman serisi görüşleri depolama yapılandırması hakkında okuyun:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Zaman serisi modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme veri modelleme](./time-series-insights-update-tsm.md)