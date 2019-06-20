---
title: 'Öğretici: Bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama | Microsoft Docs'
description: Azure zaman serisi öngörüleri önizlemesinde ortamınızı ayarlama konusunda bilgi edinin.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 06/18/2019
ms.custom: seodec18
ms.openlocfilehash: 824d24b97f192583a42192b3bb90eb1818e1aa18
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273021"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Öğretici: Bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama

Bu öğreticide bir Azure zaman serisi öngörüleri önizlemesi Kullandıkça Öde (PAYG) ortamı oluşturma işleminde size kılavuzluk eder. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Azure zaman serisi öngörüleri Önizleme ortamı oluşturun.
* Azure zaman serisi öngörüleri Önizleme ortamı, Azure Event Hubs bir olay hub'ına bağlanın.
* Bir çözüm Hızlandırıcı örnek akışı verilerini Azure zaman serisi öngörüleri önizlemesi ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlamak ve örneklerinizin ile ilişkilendirebilirsiniz.

>[!TIP]
> [IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators) özel IOT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan kullanabileceğiniz kurumsal sınıf, önceden yapılandırılmış çözümler sunar.

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Bu bölümde, veri göndermek için Azure IOT Hub örneği üç sanal cihazlar oluşturun.

1. Git [Azure IOT Çözüm Hızlandırıcıları sayfasında](https://www.azureiotsolutions.com/Accelerators). Sayfanın birkaç önceden oluşturulmuş örnekler görüntülenmektedir. Azure hesabınızı kullanarak oturum açın. Ardından, **cihaz benzetimi**.

   [![Azure IOT Çözüm Hızlandırıcıları sayfası](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Seçin **şimdi deneyin**.

1. Üzerinde **cihaz benzetimi oluşturma çözümü** sayfasında, aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | --- |
    | **Dağıtım adı** | Yeni bir kaynak grubu için benzersiz bir değer girin. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır. |
    | **Azure aboneliği** | Time Series Insights ortamınızı oluşturmak için kullanılan aboneliği seçin. |
    | **Azure konum** | Time Series Insights ortamınızı oluşturmak için kullanılan bir bölge seçin. |
    | **Dağıtım seçenekleri** | Seçin **yeni IOT hub'ı sağlama**. |
 
    Seçin **çözüm oluşturma**. Bu çözüm dağıtmayı tamamlamak 20 dakika kadar sürebilir.

    [![Cihaz benzetimi çözüm sayfası oluşturma](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Zaman serisi öngörüleri Önizleme PAYG ortamı oluşturma

Bu bölümde, Azure zaman serisi öngörüleri Önizleme ortamı oluşturun ve IOT Çözüm Hızlandırıcısı kullanarak oluşturduğunuz IOT hub'ına bağlamak açıklar [Azure portalında](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum açın.

1. Seçin **kaynak Oluştur** > **nesnelerin interneti** > **Time Series Insights**.

   [![Nesnelerin interneti'ni seçin ve ardından Time Series Insights'ı seçin](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. İçinde **zaman serisi görüşleri oluşturma ortam** bölmesinde, **Temelleri** sekmesinde, aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | ---|
    | **Ortam adı** | Azure zaman serisi öngörüleri Önizleme ortamı için benzersiz bir ad girin. |
    | **Abonelik** | Azure zaman serisi öngörüleri Önizleme ortamı oluşturmak istediğiniz abonelik girin. Cihaz simülatörü tarafından oluşturulan IOT kaynakları geri kalanı gibi aynı abonelik kullanmak iyi bir uygulamadır. |
    | **Kaynak grubu** | Mevcut bir kaynak grubunu seçin veya Azure zaman serisi öngörüleri Önizleme ortamı kaynak için yeni bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Cihaz simülatörü tarafından oluşturulan bir IOT kaynakları olarak aynı kaynak grubunu kullanmak iyi bir uygulamadır. |
    | **Location** | Azure zaman serisi öngörüleri önizlemesi ortamınız için bir veri merkezi bölgesini seçin. Ek gecikme önlemek için bir IOT kaynaklarınızı aynı bölgede Azure zaman serisi öngörüleri önizlemesi ortamınızı oluşturmak idealdir. |
    | **Katmanı** |  Seçin **PAYG** (*Kullandıkça Öde*). Azure zaman serisi öngörüleri önizlemesi ürün için SKU budur. |
    | **özellik kimliği** | Zaman serisi örneğinizin benzersiz olarak tanımlayan bir değer girin. Girdiğiniz değer **özellik kimliği** kutusudur değişmez. Daha sonra değiştiremezsiniz. Bu öğretici için girin **iothub-bağlantı-cihaz-ID**. Zaman serisi kimliği hakkında daha fazla bilgi için bkz: [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md). |
    | **Depolama hesabı adı** | Oluşturulacak yeni bir depolama hesabı için genel olarak benzersiz bir ad girin. |
   
   Seçin **sonraki: Olay kaynağı**.

   [![Zaman serisi görüşleri ortamı oluşturma bölmesi](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Üzerinde **olay kaynağı** sekmesinde, aşağıdaki parametreleri ayarlayın:

   | Parametre | Eylem |
   | --- | --- |
   | **Olay kaynağı oluşturma?** | Seçin **Evet**.|
   | **Ad** | Bir benzersiz değer için olay kaynağı adını girin. |
   | **Kaynak türü** | Seçin **IOT hub'ı**. |
   | **Bir hub'ını seçin** | Seçin **var olanı Seç**. |
   | **Abonelik** | Cihaz simülatörü kullanılan aboneliği seçin. |
   | **IOT hub'ı adı** | Cihaz simülatörü oluşturduğunuz IOT hub adını seçin. |
   | **IOT hub'ı erişim ilkesini** | Seçin **iothubowner**. |
   | **IOT Hub tüketici grubu** | Seçin **yeni**benzersiz bir ad girin ve ardından **Ekle**. Tüketici grubu, Azure zaman serisi öngörüleri önizlemesinde benzersiz bir değer olmalıdır. |
   | **Zaman damgası özelliği** | Bu değer tanımlamak için kullanılan **zaman damgası** , gelen telemetri verilerini bir özellik. Bu öğretici için bu kutuyu boş bırakın. Bu simülatörü Time Series Insights'ın varsayılan olarak IOT Hub'ından gelen zaman damgası kullanır. |

   **İncele ve oluştur**’u seçin.

   [![Olay kaynağını yapılandırma](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Üzerinde **gözden geçir + Oluştur** sekmesini inceleyin seçimleri ve ardından **Oluştur**.

    [![Gözden geçir + oluştur düğmesi ile oluşturma sayfası](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

    Dağıtımınızın durumunu görebilirsiniz:

    [![Dağıtım tamamlandıktan bildirimi](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Kiracı sahipseniz Azure zaman serisi öngörüleri Önizleme ortamınıza erişebilirsiniz. Erişiminiz olduğundan emin olmak için:

   1. Kaynak grubunuz için arama yapın ve ardından Azure zaman serisi öngörüleri önizlemesi ortamınızı seçin:

      [![Seçili ortam](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   1. Azure zaman serisi öngörüleri önizleme sayfasında **veri erişimi ilkeleri**:

      [![Veri erişimi ilkeleri](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   1. Kimlik bilgilerinizi listelendiğini doğrulayın:

      [![Listelenen kimlik bilgileri](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Kimlik bilgilerinizi listelenmemiş, kendiniz ortama erişim izni vermeniz gerekir. İzinleri ayarlama hakkında daha fazla bilgi edinmek için [veri erişim ver](./time-series-insights-data-access.md).

## <a name="stream-data-into-your-environment"></a>Ortamınıza Stream veri

1. Geri gidin [Azure IOT Çözüm Hızlandırıcıları sayfasında](https://www.azureiotsolutions.com/Accelerators). Çözümünüzü çözüm Hızlandırıcı Panonuzda bulun. Ardından, **başlatma**:

    [![Cihaz benzetimi çözümü başlatma](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. İçin yönlendirilirsiniz **Microsoft Azure IOT cihaz benzetimi** sayfası. Sayfanın sağ üst köşesinde bulunan seçin **yeni benzetimi**.

    [![Azure IOT simülasyon sayfası](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. İçinde **benzetimi Kurulum** bölmesinde, aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | --- |
    | **Ad** | Simülatör için benzersiz bir ad girin. |
    | **Açıklama** | Bir tanım girin. |
    | **Benzetim süresi** | Kümesine **süresiz olarak çalıştırmak**. |
    | **Cihaz modeli** | **Ad**: Girin **Soğutucu**. <br />**Miktar**: Girin **3**. |
    | **Hedef IoT Hub'ı** | Kümesine **kullanın, IOT hub'ı önceden sağlanan**. |

    [![Parametrelerini ayarlamak için](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    Seçin **benzetimi Başlat**.

    Cihaz benzetimi Panoda gösterilen bilgiler unutmayın **etkin cihazlar** ve **saniye başına ileti**.

    [![Azure IOT simülasyon Panosu](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, temel analiz zaman serisi verilerinizi kullanarak gerçekleştirdiğiniz [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

1. Git kaynak sayfasından URL seçerek Azure zaman serisi öngörüleri önizlemesi gezgininizde [Azure portalında](https://portal.azure.com/).

    [![Zaman serisi öngörüleri Önizleme explorer URL'si](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Gezgini'nde seçin **zaman serisi örnekleri** ortamdaki tüm Azure zaman serisi öngörüleri Önizleme örneklerini görmek için düğümü.

    [![Ana öğesiz örneklerinin listesi](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. İlk zaman serisi örnek seçin. Ardından, **Göster baskısı**.

    [![Ortalama baskısı göstermek için menü komutu ile zaman serisi örnek seçildi](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

    Zaman serisi grafiği görüntülenir. Değişiklik **aralığı** için **15 sn**.

    [![Zaman serisi grafiği](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Yineleme 3. adım diğer iki ile zaman serisi örnekleri. Tüm zaman serisi örnekleri, bu grafikte gösterildiği gibi görüntüleyebilirsiniz:

    [![Tüm zaman damgaları için grafik](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. İçinde **zaman çerçevesini** kutusu seçeneği, son bir saat içinde zaman serisi eğilimleri görmek için zaman aralığını Değiştir:

    [![Zaman aralığı bir saate ayarlayın](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Tanımlayın ve bir modeli uygulayın

Bu bölümde, verilerinizin yapısı için bir model uygulayın. Model tamamlamak için türleri, hiyerarşileri ve örnekleri tanımlayın. Veri modelleme hakkında daha fazla bilgi için bkz: [zaman serisi modeli](./time-series-insights-update-tsm.md).

1. Gezgini'nde seçin **modeli** sekmesinde:

   [![Explorer'da modeli sekmesi](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Seçin **Ekle** bir türü eklemek için:

   [![Türleri için Ekle düğmesine](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Ardından, üç değişken türü için tanımladığınız: *baskısı*, *sıcaklık*, ve *nem*. İçinde **bir tür eklemek** bölmesinde, aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | ---|
    | **Ad** | Girin **Soğutucu**. |
    | **Açıklama** | Girin **Soğutucu bir tür tanımı budur**. |

   * Tanımlamak için *baskısı*altında **değişkenleri**, aşağıdaki parametreleri ayarlayın:

     | Parametre | Eylem |
     | --- | ---|
     | **Ad** | Girin **ortalama baskısı**. |
     | **Değer** | Seçin **baskısı (çift)** . Bu işlem birkaç dakika sürebilir **değer** Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra otomatik olarak doldurulacak. |
     | **Toplama işlemi** | Seçin **ortalama**. |

      [![Basınç tanımlama seçimleri](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Sonraki değişkeni eklemek için seçin **değişkeni Ekle**.

   * Tanımlama *sıcaklık*:

     | Parametre | Eylem |
     | --- | ---|
     | **Ad** | Girin **ortalama sıcaklık**. |
     | **Değer** | Seçin **sıcaklık (çift)** . Bu işlem birkaç dakika sürebilir **değer** Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra otomatik olarak doldurulacak. |
     | **Toplama işlemi** | Seçin **ortalama**.|

      [![Sıcaklık tanımlama seçimleri](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

      Sonraki değişkeni eklemek için seçin **değişkeni Ekle**.

   * Tanımlama *nem*:

      | | |
      | --- | ---|
      | **Ad** | Girin **Maksimum nem oranı** |
      | **Değer** | Seçin **nem (çift)** . Bu işlem birkaç dakika sürebilir **değer** Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra otomatik olarak doldurulacak. |
      | **Toplama işlemi** | Seçin **MAX**.|

      [![Sıcaklık tanımlama seçimleri](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

    **Oluştur**’u seçin.

    Eklediğiniz türü görebilirsiniz:

    [![Eklenen türü hakkında bilgi](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. Sonraki adım, bir hiyerarşi eklemektir. Altında **hiyerarşileri**seçin **Ekle**:

    [![Hiyerarşiler Sekme Ekle düğmesi](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. İçinde **Düzenle hiyerarşi** bölmesinde, aşağıdaki parametreleri ayarlayın:

   | Parametre | Eylem |
   | --- | ---|
   | **Ad** | Girin **konumu hiyerarşi**. |
   | **Düzey 1** | Girin **Ülke**. |
   | **Düzey 2** | Girin **Şehir**. |
   | **3. düzey** | Girin **yapı**. |

   **Kaydet**’i seçin.

    [![Hiyerarşi alanları oluştur düğmesi](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

   Oluşturduğunuz hiyerarşi görebilirsiniz:

    [![Hiyerarşisi hakkında bilgi](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Seçin **örnekleri**. İlk örneğini seçin ve ardından **Düzenle**:

    [![Bir örneği için Düzenle düğmesini seçme](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. İçinde **Düzenle örnekleri** bölmesinde, aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | --- |
    | **Tür** | Seçin **Soğutucu**. |
    | **Açıklama** | Girin **Soğutucu 01.1 örneği**. |
    | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
    | **Ülke** | Girin **ABD**. |
    | **Şehir** | Girin **Seattle**. |
    | **Yapı** | Girin **boşluk iğne**. |

    [![Örnek alanları Kaydet düğmesi](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

   **Kaydet**’i seçin.

1. Başka bir algılayıcı için önceki adımı yineleyin. Aşağıdaki değerleri güncelleştirin:

   * Soğutucu 01.2 için:

     | Parametre | Eylem |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | Girin **Soğutucu 01.2 örneği**. |
     | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
     | **Ülke** | Girin **ABD**. |
     | **Şehir** | Girin **Seattle**. |
     | **Yapı** | Girin **Pasifik bilimi Merkezi**. |

   * Soğutucu 01.3 için:

     | Parametre | Eylem |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | Girin **Soğutucu 01.3 örneği**. |
     | **Hiyerarşiler** | Seçin **konumu hiyerarşi**. |
     | **Ülke** | Girin **ABD**. |
     | **Şehir** | Girin **New York**. |
     | **Yapı** | Girin **Empire durumu yapı**. |

1. Seçin **Çözümle** sekmesini ve sonra sayfayı yenileyin. Altında **konumu hiyerarşi**, zaman serisi örnekleri görüntülemek için tüm hiyerarşi düzeyleri genişletin:

   [![Analiz sekmesi](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Zaman serisi örnekleri son saat boyunca keşfetmek için değiştirme **hızlı süreler** için **son bir saat**:

    [![Hızlı süreler kutusuyla seçilmiş son bir saat](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Altında **Pasifik bilimi Merkezi**zaman serisi örneğini seçin ve ardından **Göster en yüksek nem**.

    [![Seçilen zaman serisi örnek ve en yüksek nem Göster menüsü seçimi](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. Zaman serisi için **en yüksek nem** bir aralığı boyutu ile **1 dakika** açılır. Aralık filtre uygulamak için bir bölge seçin. Zaman dilimi olayları çözümlemek için grafiğe sağ tıklayın ve ardından **yakınlaştırma**:

   [![Yakınlaştırma komutu bir kısayol menüsünde Seçili aralık](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Olay ayrıntılarını görmek için bir bölge seçin ve ardından grafiği sağ tıklayın:

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
