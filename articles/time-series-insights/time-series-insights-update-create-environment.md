---
title: Bir Azure zaman serisi öngörüleri Önizleme ortamı öğreticiyi ayarlama | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesinde ortamınızı ayarlama konusunda bilgi edinin.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: 20cec1305f84bd1ff7e01f2e1d38f374aa17bc6f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106694"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Öğretici: bir Azure zaman serisi öngörüleri Önizleme ortamı ayarlama

Bu öğreticide sanal cihazlardan veri ile doldurulan bir Azure zaman serisi öngörüleri Önizleme ortamı oluşturma işleminde size rehberlik eder. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Bir zaman serisi öngörüleri Önizleme ortamı oluşturun.
* Zaman serisi öngörüleri Önizleme ortamı, Azure Event Hubs bir olay hub'ına bağlanın.
* Rüzgar grubu benzetimi için veri akışı zaman serisi öngörüleri Önizleme ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlamak ve örneklerinizin ile ilişkilendirin.

## <a name="create-a-time-series-insights-preview-environment"></a>Bir zaman serisi öngörüleri Önizleme ortamı oluşturma

Bu bölümde bir zaman serisi öngörüleri Önizleme ortamı kullanarak oluşturmayı açıklar [Azure portalında](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum açın.

1. Seçin **kaynak Oluştur**.

1. Seçin **nesnelerin interneti** kategori tıklayın ve ardından **Time Series Insights**.

  ![Oluşturma bir kaynak seçin ardından nesnelerin interneti'ni seçin ve Time Series Insights'ı seçin][1]

1. Üzerinde **Temelleri** sekmesinde gerekli parametreler girin ve ardından **sonraki: olay kaynağı**

  ![Zaman serisi görüşleri ortamı temelleri sekmesi ve sonraki: olay kaynağı düğmesi][2]

1. Üzerinde **olay kaynağı** sekmesinde gerekli parametreler girin ve ardından **gözden geçir + Oluştur**.

  ![Olay kaynağı sekmesi ve gözden geçir + oluştur düğmesi][3]

1. Üzerinde **özeti** sekmesinde tüm ayrıntılarını gözden geçirin ve ardından **Oluştur** ortamınızı sağlamaya başlamak için.

  ![Özet sekmesi ve Oluştur düğmesi][4]

1. Dağıtım başarılı olduğunda bir bildirim görüntülenir.

  ![Dağıtım başarılı bildirimi][5]

## <a name="send-events-to-your-time-series-insights-environment"></a>Olayları zaman serisi görüşleri ortamınıza gönderme

Bu bölümde, olayları zaman serisi görüşleri ortamınıza bir olay hub'ı aracılığıyla göndermek için Yeldeğirmeni cihaz simülatörü kullanın.

  1. Azure portalında, olay hub'ı kaynağınıza gidin ve zaman serisi görüşleri ortamınıza bağlanın. Bilgi edinmek için bkz [kaynağınızın mevcut bir olay hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Olay hub'ı kaynak sayfasında Git **paylaşılan erişim ilkeleri** > **RootManageSharedAccessKey**. Değeri kopyalamak **bağlantı dizesi-birincil anahtar**.

      ![Birincil anahtar bağlantı dizesi değerini kopyalayın][6]

  1. [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html) kısmına gidin. Bu web uygulamasının URL'sinde Yeldeğirmeni cihazların benzetimini yapar.

  1. İçinde **olay hub'ı bağlantı dizesi** kutusunda Web sayfasında, önceki adımda kopyaladığınız bağlantı dizesini yapıştırın.

      ![Olay hub'ı bağlantı dizesi kutusunda birincil anahtar bağlantı dizesini yapıştırın][7]

  1. Seçin **başlatmak için tıklatın** olay hub'ınıza olayları göndermek için. Adlı bir dosya *instances.json* bilgisayarınıza indirilir. Bu dosyayı daha sonra kullanmak üzere kaydedin.

  1. Azure portalında event hub'ınıza geri dönün. Olay hub'ında **genel bakış** sayfa, olay hub'ı tarafından alınan yeni olaylar gösterilir.

     ![Ölçümleri olay hub'ı gösteren bir olay hub'ı genel bakış sayfası][8]

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, zaman serisi öngörüleri'ni kullanarak veri serisi explorer güncelleştirme sürenizi temel analiz gerçekleştirin.

  1. Azure portalında kaynak sayfasından URL tıklayarak Time Series Insights güncelleştirme gezgininizde gidin.

      ![Time Series Insights Gezgini URL'si][9]

  1. Gezginde, altında **fiziksel hiyerarşi**seçin **ana öğesiz örnekleri** ortamdaki tüm zaman serisi örnekleri görmek için düğümleri.

     ![Fiziksel hiyerarşi bölmesinde ana öğesiz örneklerinin listesi][10]

  1. Bu öğreticide, geçtiğimiz gün içinde gönderilen verileri analiz ediyoruz. Seçin **hızlı süreler**ve ardından **son 24 saat**.

     ![Hızlı süreler açılan kutusunda, son 24 saat seçin][11]

  1. Seçin **Sensor_0**ve ardından **ortalama değeri Göster** bu zaman serisi görüşleri örneğinden gönderilen verileri görselleştirmek için.

     ![Sensor_0 show ortalama değer seçin][12]

  1. Benzer şekilde, temel analiz gerçekleştirmek için diğer zaman serisi görüşleri örneklerinden gelen verilerle çizebilirsiniz.

     ![Time Series Insights veri çizimi][13]

## <a name="define-a-type-and-hierarchy"></a>Bir tür ve hiyerarşi tanımlama 

Bu bölümde, tür ve hiyerarşi yazar ve türü ve hiyerarşi'i Time Series Insights örneklerinizin ile ilişkilendirebilirsiniz. Daha fazla bilgi edinebilirsiniz [zaman serisi modelleri](./time-series-insights-update-tsm.md).

  1. Gezgini'nde seçin **modeli** sekmesi.

     ![Explorer menüsünde modeli sekmesi][14]

  1. İçinde **türleri** bölümünden **Ekle** yeni bir zaman serisi modeli türü oluşturun.

     ![Türleri sayfasındaki Ekle düğmesi][15]

  1. Türü Düzenleyicisi'nde için değerleri girin **adı** ve **açıklama**. Değişkenleri oluşturma **ortalama**, **Min**, ve **Max** aşağıdaki şekilde gösterildiği gibi değerleri. Seçin **Oluştur** türünü kaydetmek için.

     ![Bir tür Ekle bölmesi ve Oluştur düğmesi][16]

     ![Yeldeğirmeni örnek türleri][17]

  1. İçinde **hiyerarşileri** bölümünden **Ekle** yeni bir zaman serisi modeli hiyerarşi oluşturmak için.

     ![Hiyerarşiler sayfasında Ekle düğmesi][18]

  1. Hiyerarşi Düzenleyici'de bir değer girin **adı** ve hiyerarşi düzeyleri ekleyin. Seçin **Oluştur** hiyerarşisine kaydetmek için.

     ![Bir hiyerarşiye Ekle bölmesi ve Oluştur düğmesi][19]

     ![Fiziksel hiyerarşi kutusu][20]

  1. İçinde **örnekleri** bölümünde bir örneğini seçin ve ardından **Düzenle** türü ve hiyerarşi Bu örnekle ilişkilendirilecek.

     ![Liste Örnekleri][21]

  1. Örnek Düzenleyicisi'nde, 3. ve 5. adımda tanımladığınız hiyerarşi ve türünü seçin.

     ![Bir örneği bölmesinde Düzenle][22]

  1. Alternatif olarak, tek seferde tüm örnekleri için hiyerarşi ve türünü seçmek için düzenleyebileceğiniz *instances.json* daha önce indirilen dosya. Bu dosyada, tüm değiştirmek **TypeID** ve **HierarchyId** alanları kimliği ile 3. ve 5. adımda elde edilen.

  1. İçinde **örnekleri** bölümünden **karşıya JSON** ve düzenlenmiş karşıya *instances.json* dosya.

     ![JSON Karşıya Yükle düğmesi][23]

  1. Seçin **Analytics** sekmesini ve tarayıcınızı yenileyin. Tanımladığınız hiyerarşi ve türü ile ilişkilendirilmiş tüm örnekler görüntülenmesi gerekir.

     ![Time Series Insights veri çizimi][24]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

* Bir zaman serisi öngörüleri Önizleme ortamı oluşturun.
* Zaman serisi öngörüleri Önizleme ortamı, olay hub'ına bağlanın.
* Zaman serisi öngörüleri Önizleme ortamı için veri akışı bir Rüzgar grubu simülasyonu çalıştırın.
* Verilerin temel bir analiz gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlayın ve bunları örneklerinizin ile ilişkilendirin.

Kendi Time Series Insights güncelleştirme ortamınızı oluşturma işlemini öğrendiğinize göre zaman serisi görüşleri temel kavramlar hakkında daha fazla bilgi edinebilirsiniz.

Zaman serisi görüşleri depolama yapılandırması hakkında okuyun:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Zaman serisi modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme veri modelleme](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/tutorial-one.png
[2]: media/v2-update-provision/tutorial-two.png
[3]: media/v2-update-provision/tutorial-three.png
[4]: media/v2-update-provision/tutorial-four.png
[5]: media/v2-update-provision/tutorial-five.png
[6]: media/v2-update-provision/tutorial-six.png
[7]: media/v2-update-provision/tutorial-seven.png
[8]: media/v2-update-provision/tutorial-eight.png
[9]: media/v2-update-provision/tutorial-nine.png
[10]: media/v2-update-provision/tutorial-ten.png
[11]: media/v2-update-provision/tutorial-eleven.png
[12]: media/v2-update-provision/tutorial-twelve.png
[13]: media/v2-update-provision/tutorial-thirteen.png
[14]: media/v2-update-provision/tutorial-fourteen.png
[15]: media/v2-update-provision/tutorial-fifteen.png
[16]: media/v2-update-provision/tutorial-sixteen.png
[17]: media/v2-update-provision/tutorial-seventeen.png
[18]: media/v2-update-provision/tutorial-eighteen.png
[19]: media/v2-update-provision/tutorial-nineteen.png
[20]: media/v2-update-provision/tutorial-twenty.png
[21]: media/v2-update-provision/tutorial-twenty-one.png
[22]: media/v2-update-provision/tutorial-twenty-two.png
[23]: media/v2-update-provision/tutorial-twenty-three.png
[24]: media/v2-update-provision/tutorial-twenty-four.png