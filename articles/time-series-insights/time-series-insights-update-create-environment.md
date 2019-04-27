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
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 75ab3baf8638a387defd0e367b4a61c3746794f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60544646"
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

   ![Azure IOT Çözüm Hızlandırıcıları sayfası][1]

   Seçin **şimdi deneyin**.

1. **Cihaz Benzetimi çözümü oluşturun** sayfasına gerekli parametreleri girin:

   | Parametre | Açıklama |
   | --- | --- |
   | **Çözüm adı** |    Yeni bir kaynak grubu oluşturmak için benzersiz bir değer girin. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır. |
   | **Abonelik** | Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin. |
   | **Bölge** |   Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölge belirtin. |
   | **İsteğe bağlı Azure Kaynaklarını dağıtma**    | Sanal cihazlar bağlanmak ve veri akışı için kullanacağınız için IOT Hub'ı seçili bırakın. |

   Ardından, **çözüm oluşturma**. Dağıtılacak çözümünüzü 10-15 dakika bekleyin.

   ![Cihaz benzetimi çözüm sayfası oluşturma][2]

1. Çözüm Hızlandırıcı panosunda seçin **başlatma** düğmesi:

   ![Cihaz benzetimi çözümü başlatma][3]

1. İçin yönlendirilirsiniz **Microsoft Azure IOT cihaz benzetimi** sayfası. Seçin **+ yeni benzetimi** sayfanın sağ üst.

   ![Azure IOT simülasyon sayfası][4]

1.  Gerekli parametreleri aşağıdaki gibi doldurun:

    ![Parametreleri doldurun][5]

    |||
    | --- | --- |
    | **Ad** | Simülatör için benzersiz bir ad girin. |
    | **Açıklama** | Bir tanım girin. |
    | **Benzetim süresi** | Kümesine **süresiz olarak çalıştırmak**. |
    | **Cihaz modeli** | **Ad**: Girin **Soğutucu**. </br>**Miktar**: Girin **3**. |
    | **Hedef IoT Hub'ı** | Kümesine **kullanın, IOT hub'ı önceden sağlanan**. |

    Ardından, **benzetimi Başlat**.

1. Cihaz benzetimi panosunda görünür **etkin cihazlar** ve **saniye başına ileti**.

    ![Azure IOT simülasyon Panosu][6]

## <a name="list-device-simulation-properties"></a>Liste cihaz benzetimi özellikleri

Azure zaman serisi görüşleri ortamı oluşturmadan önce IOT hub, abonelik ve kaynak grubu adları gerekir.

1. Çözüm Hızlandırıcı panosuna gidin ve aynı Azure aboneliği hesabını kullanarak oturum açın. Önceki adımlarda oluşturduğunuz cihaz benzetimi bulun.

1. Cihaz simülatör'ü seçip **başlatma**. Seçin **Azure Yönetim Portalı** işlecin sağ tarafındaki bağlantı.

    ![Simülatör listesi][7]

1. IOT hub, abonelik ve kaynak grubu adlarını not alın.

    ![Azure portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Zaman serisi öngörüleri Önizleme PAYG ortamı oluşturma

Bu bölümde kullanarak Azure zaman serisi öngörüleri Önizleme ortamı oluşturmayı açıklar [Azure portalında](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum açın.

1. Seçin **kaynak Oluştur**.

1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**.

   ![Nesnelerin interneti'ni seçin ve ardından Time Series Insights'ı seçin][9]

1. Sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | ---|
   | **Ortam adı** | Azure zaman serisi öngörüleri Önizleme ortamı için benzersiz bir ad seçin. |
   | **Abonelik** | Azure zaman serisi öngörüleri Önizleme ortamı oluşturmak istediğiniz aboneliğinizi girin. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı abonelik kullanmak için en iyi bir uygulamadır. |
   | **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin ya da Azure zaman serisi öngörüleri Önizleme ortamı kaynak için yeni bir tane oluşturun. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı kaynak grubunu kullanmak için en iyi bir uygulamadır. |
   | **Konum** | Azure zaman serisi öngörüleri önizlemesi ortamınız için bir veri merkezi bölgesini seçin. Ek bant genişliği maliyetlerini ve gecikme süresini önlemek için diğer IOT kaynaklarıyla aynı bölgede Azure zaman serisi öngörüleri Önizleme ortamı tutmak en iyisidir. |
   | **Katmanı** |  Seçin **PAYG**, Kullandıkça Öde için anlamına gelir. Azure zaman serisi öngörüleri önizlemesi ürün için SKU budur. |
   | **özellik kimliği** | Zaman serisi, benzersiz olarak tanımlayan bir şey girin. Bu alan sabittir ve daha sonra değiştirilemez olduğunu unutmayın. Bu öğretici için kullanmak **iothub-bağlantı-cihaz-ID**. Zaman serisi kimliği hakkında daha fazla bilgi edinmek için [bir zaman serisi kimliği seçme](./time-series-insights-update-how-to-id.md). |
   | **Depolama hesabı adı** | Oluşturulacak yeni bir depolama hesabı için genel olarak benzersiz bir ad girin. |

   Ardından, **sonraki: Olay kaynağı**.

   ![Zaman serisi görüşleri ortamı oluşturma sayfası][10]

1. Olay kaynağı sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | --- |
   | **Olay kaynağı oluşturma?** | Girin **Evet**.|
   | **Ad** | Olay kaynağı adı için kullanılan benzersiz bir değer girin.|
   | **Kaynak türü** | Girin **IOT hub'ı**. |
   | **Bir hub seçilsin mi?** | Girin **var olanı Seç**. |
   | **Abonelik** | Cihaz simülatörü kullanılan abonelik girin. |
   | **IOT hub'ı adı** | Cihaz simülatörü oluşturduğunuz IOT hub adını girin. |
   | **IOT hub'ı erişim ilkesini** | Girin **iothubowner**. |
   | **IOT Hub tüketici grubu** | Azure zaman serisi öngörüleri önizlemesi için benzersiz bir tüketici grubu ihtiyacınız var. Seçin **yeni**benzersiz bir ad girin ve ardından **Ekle**. |
   | **Zaman damgası özelliği** | Bu alan zaman damgası özelliği, gelen telemetri verilerini tanımlamak için kullanılır. Bu öğreticide, alanını doldurun yok. Bu simülatörü Time Series Insights'ın varsayılan olarak IOT Hub'ından gelen zaman damgası kullanır.|

   Ardından, **gözden geçir + Oluştur**.

   ![Bir olay kaynağını ayarlama sayfası][13]

1. Gözden geçir sayfasında tüm alanları gözden geçirin ve seçin **Oluştur**.

   ![Gözden geçir + oluştur düğmesi ile oluşturma sayfası][14]

1. Dağıtımınızın durumunu görebilirsiniz.

   ![Dağıtım tamamlandıktan bildirimi][15]

1. Kiracı sahipseniz Azure zaman serisi öngörüleri Önizleme ortamınıza erişimi almanız gerekir. Erişiminiz olduğundan emin olmak için:

   a. Kaynak grubunuz için arama yapın ve Azure zaman serisi öngörüleri önizlemesi ortamınızı seçin:

      ![Seçili ortam][16]

   b. Azure zaman serisi öngörüleri önizleme sayfasında Git **veri erişimi ilkeleri**.

     ![Veri erişimi ilkeleri][17]

   c. Kimlik bilgilerinizi listelendiğini doğrulayın.

     ![Listelenen kimlik bilgileri][18]

   Kimlik bilgilerinizi listelenmemiş, kendiniz ortama erişim izni vermeniz gerekir. İzinleri ayarlama hakkında daha fazla bilgi edinmek için [veri erişim ver](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, temel analiz zaman serisi verilerinizi kullanarak gerçekleştirdiğiniz [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

1. Git kaynak sayfasından URL seçerek Azure zaman serisi öngörüleri önizlemesi gezgininizde [Azure portalında](https://portal.azure.com/).

   ![Zaman serisi öngörüleri Önizleme explorer URL'si][19]

1. Gezgini'nde seçin **ana öğesiz örnekleri** ortamdaki tüm Azure zaman serisi öngörüleri Önizleme örneklerini görmek için düğümü.

   ![Ana öğesiz örneklerinin listesi][20]

1. Zaman serisi içinde gösterilen ilk örnek seçin. Ardından, **Göster ortalama baskısı**.

   ![Ortalama baskısı göstermek için menü komutu ile seçilen örneği][21]

   Zaman serisi grafiği, sağ tarafta görünmelidir:

   ![Zaman serisi grafiği][22]

1. Yineleme 3. adım diğer iki ile zaman serisi. Ardından, bu grafikte gösterildiği gibi tüm zaman damgaları görüntüleyebilirsiniz:

   ![Tüm zaman damgaları için grafik][23]

1. Son saat üzerinden zaman serisi eğilimleri görmek için zaman aralığını değiştirin. 

   a. Seçin **gelen** seçenek kutusu:

      ![İlk seçenek kutusu][24]

   b. Son bir saat olayları görüntülemek için kutusunda değişiklik zamanı:

      ![Saat ayarlamaları][25]

1. Basınç son saat boyunca üç cihazlara karşılaştırın:

   ![Üç cihazlar arasında karşılaştırma][26]

## <a name="define-and-apply-a-model"></a>Tanımlayın ve bir modeli uygulayın

Bu bölümde, verilerinizin yapısı için bir model uygulayın. Model tamamlamak için türleri, hiyerarşileri ve örnekleri tanımlayın. Veri modelleme hakkında daha fazla bilgi için şuraya gidin [zaman serisi modeli](./time-series-insights-update-tsm.md).

1. Gezgini'nde seçin **modeli** sekmesinde:

   ![Explorer'da modeli sekmesi][27]

1. Seçin **+ Ekle** bir türü eklemek için. Sağ tarafta bir türü Düzenleyicisi açılır.

   ![Türleri için Ekle düğmesine][28]

1. Üç tür değişkenleri tanımlayın: baskı, sıcaklık ve nem oranı. Aşağıdaki bilgileri girin:

   | | |
   | --- | ---|
   | **Ad** | Girin **Soğutucu**. |
   | **Açıklama** | Girin **Soğutucu bir tür tanımı budur**. |

   * Üç değişkenleriyle baskısı tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin **ortalama baskısı**. |
      | **Değer** | Seçin **baskısı (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **ortalama**. |

      ![Basınç tanımlama seçimleri][29]

      Seçin **+ değişkeni Ekle** sonraki değişkeni eklemek için.

   * Sıcaklık tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin **ortalama sıcaklık**. |
      | **Değer** | Seçin **sıcaklık (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **ortalama**.|

      ![Sıcaklık tanımlama seçimleri][30]

   * Nem tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin **Maksimum nem**. |
      | **Değer** | Seçin **nem (çift)**. Bu alan Azure zaman serisi öngörüleri önizlemesi olayları almaya başladıktan sonra doldurulması için birkaç dakika sürebileceğini unutmayın. |
      | **Toplama işlemi** | Seçin **MAX**.|

      ![Sıcaklık tanımlama seçimleri][31]

   Ardından **Oluştur**’u seçin.

1. Eklenen türünüz görebilirsiniz:

   ![Eklenen türü hakkında bilgi][32]

1. Sonraki adım, bir hiyerarşi eklemektir. İçinde **hiyerarşileri** bölümünden **+ Ekle**:

   ![Hiyerarşiler Sekme Ekle düğmesi][33]

1. Hiyerarşi tanımlayın. Alanları aşağıdaki gibi doldurun:

   | | |
   | --- | ---|
   | **Ad** | Girin **konumu hiyerarşi**. |
   | **Düzey 1** | Girin **Ülke**. |
   | **Düzey 2** | Girin **Şehir**. |
   | **3. düzey** | Girin **yapı**. |

   Ardından **Oluştur**’u seçin.

   ![Hiyerarşi alanları oluştur düğmesi][34]

1. Oluşturduğunuz hiyerarşi görebilirsiniz:

   ![Hiyerarşisi hakkında bilgi][35]

1. Seçin **örnekleri** soldaki. Örnekleri görünür, ilk örneğini seçin ve ardından seçin sonra **Düzenle**:

   ![Bir örneği için Düzenle düğmesini seçme][36]

1. Bir metin düzenleyicisi sağ tarafta görüntülenir. Aşağıdaki bilgileri ekleyin:

   | | |
   | --- | --- |
   | **Tür** | Seçin **Soğutucu**. |
   | **Açıklama** | Girin **Soğutucu 01.1 örneği**. |
   | **Hiyerarşiler** | Etkinleştirme **konumu hiyerarşi**. |
   | **Ülke** | Girin **ABD**. |
   | **Şehir** | Girin **Seattle**. |
   | **Yapı** | Girin **boşluk iğne**. |

    Ardından **Kaydet**’i seçin.

   ![Kaydet düğmesi ile örnek alanları][37]

1. Başka bir algılayıcı için önceki adımı yineleyin. Aşağıdaki alanları kullanın:

   * Soğutucu 01.2 için:

     | | |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | Girin **Soğutucu 01.2 örneği**. |
     | **Hiyerarşiler** | Etkinleştirme **konumu hiyerarşi**. |
     | **Ülke** | Girin **ABD**. |
     | **Şehir** | Girin **Seattle**. |
     | **Yapı** | Girin **Pasifik bilimi Merkezi**. |

   * Soğutucu 01.3 için:

     | | |
     | --- | --- |
     | **Tür** | Seçin **Soğutucu**. |
     | **Açıklama** | Girin **Soğutucu 01.1 örneği**. |
     | **Hiyerarşiler** | Etkinleştirme **konumu hiyerarşi**. |
     | **Ülke** | Girin **ABD**. |
     | **Şehir** | Girin **New York**. |
     | **Yapı** | Girin **Empire durumu yapı**. |

1. Git **Çözümle** sekmesini ve sayfayı yenileyin. Zaman serisi bulmak için tüm hiyerarşi düzeyleri genişletin.

   ![Analiz sekmesi][38]

1. Zaman serisi son saat boyunca keşfetmek için değiştirme **hızlı süreler** için **son bir saat**:

   ![Hızlı süreler kutusuyla seçilmiş son bir saat][39]

1. Zaman serisi altında seçin **Pasifik bilimi Merkezi** seçip **Göster en yüksek nem**.

   ![En yüksek nem Göster menüsü seçimi ile seçilen zaman serisi][40]

1. Zaman serisi için **en yüksek nem** boyutu 1 dakikalık bir zaman aralığı ile açılır. Aralık filtre uygulamak için bir bölge seçin. Ardından, sağ tıklayıp **yakınlaştırma** zaman çerçevesi içinde olayları analiz etmek için:

   ![Yakınlaştırma komutu bir kısayol menüsünde Seçili aralık][41]

1. Ayrıca bir bölge seçin ve ardından olay ayrıntılarını görmek için sağ tıklayın:

   ![Ayrıntılı olayların listesi][44]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

* Oluşturun ve cihaz benzetimi Hızlandırıcı kullanın.
* Azure zaman serisi öngörüleri Önizleme PAYG ortamı oluşturun.
* Azure zaman serisi öngörüleri Önizleme ortamı, olay hub'ına bağlanın.
* Azure zaman serisi öngörüleri Önizleme ortamı için veri akışı için bir çözüm Hızlandırıcı örneği çalıştırın.
* Verilerin temel bir analiz gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlayın ve bunları örneklerinizin ile ilişkilendirin.

Kendi Azure zaman serisi öngörüleri Önizleme ortamı oluşturma işlemini öğrendiğinize göre Azure zaman serisi görüşleri temel kavramlar hakkında daha fazla bilgi edinebilirsiniz.

Azure zaman serisi görüşleri depolama yapılandırması hakkında okuyun:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Zaman serisi modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme veri modelleme](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-seven-dashboard.png
[7]: media/v2-update-provision/device-six-listings.png
[8]: media/v2-update-provision/device-eight-portal.png

[9]: media/v2-update-provision/payg-one-azure.png
[10]: media/v2-update-provision/payg-two-create.png
[11]: media/v2-update-provision/payg-three-new.png
[12]: media/v2-update-provision/payg-four-add.png
[13]: media/v2-update-provision/payg-five-event-source.png
[14]: media/v2-update-provision/payg-six-review.png
[15]: media/v2-update-provision/payg-seven-deploy.png
[16]: media/v2-update-provision/payg-eight-environment.png
[17]: media/v2-update-provision/payg-nine-data-access.png
[18]: media/v2-update-provision/payg-ten-verify.png

[19]: media/v2-update-provision/analyze-one-portal.png
[20]: media/v2-update-provision/analyze-two-unparented.png
[21]: media/v2-update-provision/analyze-three-show-pressure.png
[22]: media/v2-update-provision/analyze-four-chart.png
[23]: media/v2-update-provision/analyze-five-chart.png
[24]: media/v2-update-provision/analyze-six-from.png
[25]: media/v2-update-provision/analyze-seven-change-from.png
[26]: media/v2-update-provision/analyze-eight-all.png

[27]: media/v2-update-provision/define-one-model.png
[28]: media/v2-update-provision/define-two-add.png
[29]: media/v2-update-provision/define-three-variable.png
[30]: media/v2-update-provision/define-four-avg.png
[31]: media/v2-update-provision/define-five-humidity.png
[32]: media/v2-update-provision/define-six-type.png
[33]: media/v2-update-provision/define-seven-hierarchy.png
[34]: media/v2-update-provision/define-eight-add-hierarchy.png
[35]: media/v2-update-provision/define-nine-created.png
[36]: media/v2-update-provision/define-ten-edit.png
[37]: media/v2-update-provision/define-eleven-chiller.png
[38]: media/v2-update-provision/define-twelve.png
[39]: media/v2-update-provision/define-thirteen-explore.png
[40]: media/v2-update-provision/define-fourteen-show-max.png
[41]: media/v2-update-provision/define-fifteen-filter.png
[42]: media/v2-update-provision/define-sixteen.png
[43]: media/v2-update-provision/define-seventeen.png
[44]: media/v2-update-provision/define-eighteen.png