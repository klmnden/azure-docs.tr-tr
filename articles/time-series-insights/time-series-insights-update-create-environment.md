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
ms.openlocfilehash: 965fd4b0ca5defc2b1bf633af72ac250942d838f
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554818"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Öğretici: Bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama

Bu öğreticide bir Azure Time Series Insights Kullandıkça Öde (PAYG) önizleme ortamı oluşturma işleminde size kılavuzluk eder. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Azure zaman serisi öngörüleri Önizleme ortamı oluşturun.
* Azure zaman serisi öngörüleri Önizleme ortamı, Azure Event Hubs bir olay hub'ına bağlanın.
* Bir Rüzgar grubu benzetim akışı verilerini Azure zaman serisi öngörüleri önizlemesi ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlamak ve örneklerinizin ile ilişkilendirin.

# <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Bu bölümde, bir IOT Hub'ına veri gönderecek üç sanal cihazlar oluşturacaksınız.

1. Git [Azure IOT Çözüm Hızlandırıcıları giriş sayfasında](https://www.azureiotsolutions.com/Accelerators). Azure IOT Çözüm Hızlandırıcıları giriş sayfasında birden fazla önceden oluşturulmuş örnekler görüntülenmektedir. Azure hesabınızı kullanarak oturum açın. Ardından, **cihaz benzetimi**.

   ![Azure IOT Çözüm Hızlandırıcıları giriş sayfası][1]

   Son olarak, tıklayın **şimdi deneyin**.

1. Gerekli Parametreler girin **oluşturmak cihaz benzetimi** çözüm sayfasında:

   | Parametre | Açıklama |
   | --- | --- |
   | Çözüm adı |    Yeni bir kaynak grubu oluşturmak için kullanılan benzersiz değer. Listelenen Azure kaynakları | oluşturulur ve kaynak grubuna atanır. |
   | Abonelik | TSI ortamınızı oluşturmak için kullanılan aynı abonelik belirtin |
   | Bölge |   TSI oluşturulması için kullanılan aynı bölge belirtin. |
   | İsteğe bağlı Azure Kaynaklarını dağıtma    | Sanal cihazlar/veri akışı bağlanmak için kullanacağı şekilde bırakın IOT hub'ı kullanıma. |

   Gerekli parametreleri girdikten sonra tıklayarak **çözüm oluşturma**. Dağıtılacak çözümünüz için yaklaşık 10-15 dakika bekleyin.

   ![Cihaz benzetimi çözümü oluşturun][2]

1. İçinde **çözüm Hızlandırıcı Panosu**, tıklayın **başlatma** düğmesi:

   ![Cihaz benzetimi çözümü başlatma][3]

1. İçin yönlendirilirsiniz **Microsoft Azure IOT cihaz benzetimi** sayfası. Tıklayın **+ yeni benzetimi** için ekranın sağ üst kısımdaki bulunur.

   ![Azure IOT simülasyon sayfası][4]

1.  Gerekli parametreleri aşağıdaki gibi doldurun:

    ![Parametreleri doldurun][5]

    |||
    | --- | --- |
    | **Ad** | Simülatör için benzersiz bir ad girin |
    | **Açıklama** | Bir tanım girin |
    | **Benzetim süresi** | Ayarlayın `Run indefinitely` |
    | **Cihaz modeli** | **Ad**: Girin `Chiller` **tutarı**: Girin `3` |
    | **Hedef IoT Hub'ı** | Ayarlayın `Use pre-provisioned IoT Hub` |

    Gerekli parametreleri doldurduktan sonra tıklayarak **benzetimi Başlat**.

1. Cihaz benzetimi panosunda görünür **etkin cihazlar** ve **saniye başına ileti**.

    ![Azure IOT simülasyon Panosu][6]

## <a name="list-device-simulation-properties"></a>Liste cihaz benzetimi özellikleri

Azure zaman serisi görüşleri ortamı oluşturmadan önce IOT Hub, abonelik ve kaynak grubu adı adları gerekir.

1. Git **çözüm Hızlandırıcı Panosu** aynı Azure aboneliği hesabını kullanarak oturum açın. Önceki adımlarda oluşturduğunuz cihaz benzetimi bulun.

1. Üzerinde cihaz simülatörü ve tıklayın **başlatma**. Tıklayarak **Azure Yönetim Portalı** bağlantıyı sağ tarafta görüntülenir.

    ![Simülatör listesi][7]

1. IOT Hub, abonelik ve kaynak grubu adlarını not alın.

    ![Azure portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Zaman serisi öngörüleri Önizleme PAYG ortamı oluşturma

Bu bölümde kullanarak Azure zaman serisi öngörüleri Önizleme ortamı oluşturmayı açıklar [Azure portalında](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum açın.

1. Seçin **kaynak Oluştur**.

1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**.

   ![Oluşturma bir kaynak seçin ardından nesnelerin interneti'ni seçin ve Time Series Insights'ı seçin][9]

1. Sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | ---|
   | **Ortam adı** | Azure zaman serisi öngörüleri Önizleme ortamı için benzersiz bir ad seçin. |
   | **Abonelik** | Azure zaman serisi öngörüleri Önizleme ortamı oluşturmak istediğiniz aboneliğinizi girin. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı abonelik kullanmak için en iyi bir uygulamadır. |
   | **Kaynak Grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Mevcut bir kaynak grubu seçin ya da Azure zaman serisi öngörüleri Önizleme ortamı kaynak için yeni bir tane oluşturun. Cihaz simülatörü tarafından oluşturulan IOT kaynaklarınızı geri kalanı gibi aynı kaynak grubunu kullanmak için en iyi bir uygulamadır. |
   | **Konum** | Azure zaman serisi öngörüleri önizlemesi ortamınız için bir veri merkezi bölgesini seçin. Ek bant genişliği maliyetlerini ve gecikme süresini önlemek için diğer IOT kaynaklarıyla aynı bölgede Azure zaman serisi öngörüleri Önizleme ortamı tutmak en iyisidir. |
   | **Katmanı** |  Seçin `PAYG` Kullandıkça Öde için anlamına gelir. Azure zaman serisi öngörüleri Önizleme ürünü için SKU budur. |
   | **özellik kimliği** | Zaman serisi, benzersiz olarak tanımlar. Bu alan sabittir ve daha sonra değiştirilemez olduğunu unutmayın. Bu öğreticide alanın ayarlanacağı için `iothub-connection-device-id`. Zaman serisi kimliği hakkında daha fazla bilgi edinmek için [bir zaman serisi kimliği seçme](./time-series-insights-update-how-to-id.md). |
   | **Depolama hesabı adı** | Oluşturulacak yeni bir depolama hesabı için genel benzersiz bir ad girin. |

   Yukarıdaki alanları doldurduktan sonra tıklatın **sonraki: Olay kaynağı**.

   ![İleri'ye tıklayın: Olay Kaynağı][10]

1. Sayfasında alanları aşağıdaki gibi doldurun:

   | | |
   | --- | --- |
   | **Olay kaynağı oluşturma?** | Girin `Yes`|
   | **Ad** | Olay kaynağını adlandırmak için kullanılan benzersiz bir değer gerektirir.|
   | **Kaynak türü** | Girin `IoT Hub` |
   | **Bir Hub seçilsin mi?** | Girin `Select Existing` |
   | **Abonelik** | Cihaz simülatörü kullanılan abonelik girin. |
   | **IOT hub'ı adı** | Cihaz simülatörü için oluşturduğunuz IOT hub adını girin. |
   | **IOT hub'ı erişim ilkesini** | Girin `iothubowner` |
   | **IOT Hub tüketici grubu** | Azure zaman serisi öngörüleri önizlemesi için benzersiz bir tüketici grubu ihtiyacınız var. |
   | **Zaman damgası** | Bu alan zaman damgası özelliği, gelen telemetri verilerini tanımlamak için kullanılır. Bu öğretici için alanın doldurmayın. Bu simülatörü Time Series Insights'ın varsayılan olarak IOT Hub'ından gelen zaman damgası kullanır.|

   Benzersiz bir tüketici grubu oluşturmak için:

   1. Tıklayın **yeni** yanındaki **IOT Hub tüketici grubu** alan:

      ![İleri'ye tıklayın: Olay Kaynağı][11]

   1. Tüketici grubu benzersiz bir ad verin ve tıklayın **Ekle**:

      ![Ekle'ye tıklayın.][12]

   Yukarıdaki alanları doldurduktan sonra tıklatın **gözden geçir + Oluştur**.

      ![Gözden geçirme ve oluşturma][13]

1. İnceleme sayfasında tüm alanları gözden geçirin ve tıklayın **Oluştur**.

   ![Oluştur][14]

1. Dağıtımınızın durumunu görebilirsiniz.

   ![Dağıtım tamamlandı][15]

1. Kiracı sahipseniz, zaman serisi ortamına erişim almanız gerekir. Erişiminiz olduğundan emin olmak için:

   * Yeni oluşturulan Azure zaman serisi öngörüleri Önizleme ortamınıza gidin. Kaynak grubunuz için arama yaparak bunu yapabilirsiniz. Ardından, zaman serisi ortamınıza tıklayın:

      ![Dağıtım tamamlandı][16]

   * Azure zaman serisi öngörüleri önizleme sayfasında gidin **veri erişimi ilkeleri**.

     ![Veri erişimi ilkeleri][17]

   * Kimlik bilgilerinizi listelendiğini doğrulayın.

     ![Kimlik bilgilerinizi doğrulayın][18]

   Kimlik bilgilerinizi listelenmiyorsa, kendiniz ortama erişim izni vermek gerekecektir. Okuma [veri erişim verme](./time-series-insights-data-access.md) izinleri ayarlama hakkında daha fazla bilgi edinmek için.

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, temel analiz zaman serisi verilerinizi kullanarak gerçekleştirdiğiniz [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).

1. Git kaynak sayfasından URL tıklayarak Azure zaman serisi öngörüleri önizlemesi gezgininizde [Azure portalında](https://portal.azure.com/).

   ![Time Series Insights Gezgini URL'si][19]

1. Gezgini'nde seçin **ana öğesiz örnekleri** tüm Azure zaman serisi öngörüleri Önizleme ortamında görmek için düğümleri.

   ![Ana öğesiz örneklerinin listesi][20]

1. İlk örnekte gösterilen zaman serisinde tıklayın. Ardından **Göster ortalama baskısı**.

   ![Ortalama baskısı Göster][21]

1. Zaman serisi grafiği, sağ tarafta görünmelidir:

   ![Zaman serisi grafiği][22]

1. Yineleme **3. adım** diğer iki zaman serisi. Tüm zaman damgaları aşağıda gösterildiği gibi görüntülenebilir:

   ![Tüm zaman serisi grafiği][23]

1. Değiştirme **zaman aralığı** son saat üzerinden zaman serisi eğilimleri görmek için. Seçin **gelen** aşağıda gösterildiği gibi seçenek kutusu:

   ![From seçeneğini belirleyin][24]

1. Zaman içinde **gelen** son bir saat olayları görüntülemek için bu seçeneği kutusunu:

   ![From seçeneğini belirleyin][25]

1. Basınç son saat boyunca üç cihazlara karşılaştırın:

   ![From seçeneğini belirleyin][26]

## <a name="define-and-apply-a-model"></a>Tanımlayın ve bir modeli uygulayın

Bu bölümde, verilerinizin yapısı için bir model uygulanır. Model tamamlamak için türleri, hiyerarşileri ve örnekleri tanımlayacaksınız. Veri modelleme hakkında daha fazla bilgi için şuraya gidin [zaman serisi modelleri](./time-series-insights-update-tsm.md).

1. Gezgini'nde seçin **modeli** sekmesinde:

   ![Model sekmesini seçin][27]

1. Ardından, tıklayarak **+ Ekle** bir türü eklemek için. Sağ tarafta bir türü Düzenleyicisi açılır.

   ![Ekle'ye tıklayın.][28]

1. Ardından, üç değişkenleri tanımlayın: Baskı, sıcaklık ve nem bir türde. Aşağıdaki alanları girin:

   | | |
   | --- | ---|
   | **Ad** | Girin `Chiller` |
   | **Açıklama** | Girin `This is a type definition of Chiller` |

   * Şimdi, baskısı üç değişkenleri tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin `Avg Pressure` |
      | **Değer** | Seçin **baskısı (çift)**. Not: Azure Time Series Insights olayları almaya başladıktan sonra doldurmak Bu alan için birkaç dakika sürebilir |
      | **Toplama işlemi** | `AVG` seçeneğini belirleyin |

      ![Değişken Ekle][29]

      Tıklayarak **+ değişkeni** sonraki değişkeni eklemek için.

   * Şimdi, sıcaklık tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin `Avg Temperature` |
      | **Değer** | Seçin **sıcaklık (çift)**. Not: Azure Time Series Insights olayları almaya başladıktan sonra doldurmak Bu alan için birkaç dakika sürebilir |
      | **Toplama işlemi** | `AVG` seçeneğini belirleyin|

      ![Sıcaklık tanımlayın][30]

   * Şimdi, nem tanımlayın:

      | | |
      | --- | ---|
      | **Ad** | Girin `Max Humidity` |
      | **Değer** | Seçin **nem (çift)**. Not: Azure Time Series Insights olayları almaya başladıktan sonra doldurmak Bu alan için birkaç dakika sürebilir |
      | **Toplama işlemi** | `MAX` seçeneğini belirleyin|

      ![Sıcaklık tanımlayın][31]

   Değişkenleri tanımladıktan sonra tıklayın **Oluştur**.

1. Eklenen türünüz görebilirsiniz:

   ![Eklenen türü bakın][32]

1. Sonraki adım, bir hiyerarşi eklemektir. İçinde **hiyerarşileri** bölümünden **+ Ekle** yeni bir hiyerarşi oluşturmak için:

   ![Hiyerarşi Ekle][33]

1. Hiyerarşi tanımlayın. Alanları aşağıdaki gibi girin:

   | | |
   | --- | ---|
   | **Ad** | Girin `Location Hierarchy` |
   | **Düzey 1** | Girin `Country` |
   | **Düzey 2** | Girin `City` |
   | **3. düzey** | Girin `Building` |

   Yukarıdaki alanları doldurduktan sonra tıklayarak **Oluştur**.

   ![Hiyerarşi tanımlama][34]

1. Oluşturulan hiyerarşi görebilirsiniz:

   ![Hiyerarşiniz bakın][35]

1. Hiyerarşiniz tanımladıktan sonra tıklayın **örnekleri** soldaki. Örnekleri görünür, ilk örneği tıklayıp seçin sonra **Düzenle**:

   ![Bir örneği Düzenle][36]

1. Bir metin düzenleyicisi sağ tarafta görüntülenir. Aşağıdaki alanları ekleyin:

   | | |
   | --- | --- |
   | **Tür** | `Chiller` seçeneğini belirleyin |
   | **Açıklama** | Girin `Instance for Chiller-01.1` |
   | **Hiyerarşiler** | Etkinleştirme `Location Hierarchy` |
   | **Ülke** | Girin `USA` |
   | **Şehir** | Girin `Seattle` |
   | **Yapı** | Girin `Space Needle` |

    Yukarıdaki alanları doldurduktan sonra tıklatın **Kaydet**.

   ![Bir Soğutucu Kaydet][37]

1. Başka bir algılayıcı için önceki adımı yineleyin. Aşağıdaki alanları kullanın:

   * Soğutucu 01.2 için:

     | | |
     | --- | --- |
     | **Tür** | `Chiller` seçeneğini belirleyin |
     | **Açıklama** | Girin `Instance for Chiller-01.2` |
     | **Hiyerarşiler** | Etkinleştirme `Location Hierarchy` |
     | **Ülke** | Girin `USA` |
     | **Şehir** | Girin `Seattle` |
     | **Yapı** | Girin `Pacific Science Center` |

   * Soğutucu 01.3 için:

     | | |
     | --- | --- |
     | **Tür** | `Chiller` seçeneğini belirleyin |
     | **Açıklama** | Girin `Instance for Chiller-01.1` |
     | **Hiyerarşiler** | Etkinleştirme `Location Hierarchy` |
     | **Ülke** | Girin `USA` |
     | **Şehir** | Girin `New York` |
     | **Yapı** | Girin `Empire State Building` |

1. Git **Çözümle** sekmesini ve sayfayı yenileyin. Zaman serisi bulmak için tüm hiyerarşi düzeyleri genişletin.

   ![Çözümleme sekmesini görüntüleyin][38]

1. Zaman serisi son saat boyunca keşfetmek için değiştirme **hızlı süreler** son bir saat için:

   ![Son bir saat keşfedin][39]

1. Zamanını üzerinde altında serisi **Pasifik bilimi Merkezi** tıklatıp **Göster en yüksek nem**.

   ![En yüksek nem Göster][40]

1. Zaman serisi için **en yüksek nem** boyutu 1 dakikalık bir zaman aralığı ile açılır. Aralık filtre uygulamak için bir bölge sol. Ardından, sağ tıklayın ve zaman dilimi içinde olayları çözümlemek için Yakınlaştır:

   ![Görüntüleme, filtreleme ve yakınlaştırma][41]

   ![Görüntüleme, filtreleme ve yakınlaştırma][42]

1. Ayrıca bir bölge sol ve olay ayrıntılarını görmek için sağ tıklayın:

   ![Görüntüleme, filtreleme ve yakınlaştırma][43]

   ![Görüntüleme, filtreleme ve yakınlaştırma][44]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

* Oluşturun ve cihaz benzetimi Hızlandırıcı kullanın.
* Azure zaman serisi öngörüleri Önizleme PAYG ortamı oluşturun.
* Azure zaman serisi öngörüleri Önizleme ortamı, olay hub'ına bağlanın.
* Azure zaman serisi öngörüleri Önizleme ortamı için veri akışı bir Rüzgar grubu simülasyonu çalıştırın.
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
[6]: media/v2-update-provision/device-six-listings.png
[7]: media/v2-update-provision/device-seven-dashboard.png
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