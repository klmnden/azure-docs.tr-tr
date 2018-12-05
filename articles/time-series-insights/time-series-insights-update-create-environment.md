---
title: Azure Time Series Insights (Önizleme) öğreticisi | Microsoft Docs
description: Azure Time Series Insights (Önizleme) hakkında bilgi edinin
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: ed25d03f7c592476b9284790ac12f9954661a42b
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872314"
---
# <a name="azure-time-series-insights-preview-tutorial"></a>Azure Time Series Insights (Önizleme) öğreticisi

Bu öğreticide, sanal cihazlar verilerle doldurulmuş bir Azure zaman serisi öngörüleri (TSI) önizleme ortamı oluşturma işlemi boyunca size yol gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* TSI (Önizleme) ortamı oluşturun.
* Olay Hub'ına TSI (Önizleme) ortama bağlanın.
* Rüzgar grubu benzetimi için veri akışı TSI Önizleme ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Zaman serisi modeli türü ve hiyerarşi tanımlamak ve örneklerinizin ile ilişkilendirin.

## <a name="create-a-time-series-insights-preview-environment"></a>Time Series Insights (Önizleme) ortamı oluşturma

Bu bölümde bir Azure TSI (Önizleme) kullanarak ortam oluşturmayı açıklar [Azure portalı](https://portal.azure.com/).

1. Azure portalında abonelik hesabınızı kullanarak oturum
1. Üstteki menüden **+ Kaynak oluştur**'u seçin.
1. **Nesnelerin İnterneti** kategorisini, ardından **Time Series Insights**’ı seçin.

  ![öğretici-bir][1]

1. Zaman serisi görüşleri ortamı sayfadaki gerekli parametrelerini doldurun ve tıklayarak **sonraki: olay kaynağı**

  ![öğretici-iki][2]

1. Üzerinde **olay kaynağı** sayfasında gerekli parametreleri doldurun ve tıklayarak **gözden geçir + Oluştur**.

  ![öğretici-üç][3]

1. Tüm ayrıntılarını gözden geçirin ve tıklayın **Oluştur** ortamınızı sağlamaya başlamak için.

  ![öğretici-dört][4]

1. Dağıtım başarıyla tamamlandıktan sonra bir bildirim alırsınız.

  ![öğretici-beş][5]

## <a name="send-events-to-your-tsi-environment"></a>TSI ortamınıza olayları gönderme

Bu bölümde, TSI ortamınıza bir olay hub'ı üzerinden olayları göndermek için Yeldeğirmeni cihaz simülatörü kullanır.

  1. Azure portalında, olay hub'ı kaynağınıza gidin ve TSI ortamınıza bağlanın. Bilgi [kaynağınızın mevcut bir olay Hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Olay hub'ı kaynak sayfasında Git **paylaşılan erişim ilkeleri** ardından **RootManageSharedAccessKey**. Kopyalama **bağlantı dizesi-birincil anahtar** burada görüntülenir.

      ![öğretici-altı][6]

  1. [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html) kısmına gidin. Bu web uygulaması Yeldeğirmeni cihazların benzetimini yapar.
  1. Bağlantı dizesi kopyalamıştır içinde 3. adımdaki Yapıştır **olay hub'ı bağlantı dizesi**

      ![öğretici-yedi][7]

  1. Tıklayarak **için Başlat'ı tıklatın** olayları olay Hub'ına gönderme. Bu aşamada, adlı bir dosya `instances.json` makinenize indirilir. Biz bunu daha sonra ihtiyacınız olacak şekilde bu dosyayı kaydedin.

  1. Olay hub'ına dönün. Artık hub.d tarafından alınan yeni olayların görünmesi

     ![öğretici-sekiz][8]

## <a name="analyze-data-in-your-environment"></a>Ortamınızı verileri analiz etme

Bu bölümde, Time Series Insights'ı kullanarak serisi Veri Gezgini güncelleştirme sürenizi temel analiz gerçekleştirir.

  1. Time Series Insights güncelleştirme gezgininizde URL'sini Azure Portal'da kaynak sayfasından tıklayarak gezinin.

      ![öğretici-dokuz][9]

  1. Explorer'ın tıklayarak **ana öğesiz örnekleri** ortamdaki tüm zaman serisi örnekleri görmek için düğümleri.

     ![öğretici-on][10]

  1. Bu öğreticide, son gün içinde gönderilen verileri çözümlemek için ekleyeceğiz. Bunu yapmak için tıklayın **hızlı süreler** seçip **son 24 saat** seçeneği.

     ![öğretici-on][11]

  1. Seçin **Sensor_0** ve **ortalama değeri Göster** bu zaman serisi örneğinden gönderilen verileri görselleştirmek için.

     ![öğretici-on][12]

  1. Benzer şekilde, temel analiz gerçekleştirmek için diğer zaman serisi örneklerinden gelen verileri çizebilirsiniz.

     ![öğretici-On üç][13]

## <a name="define-a-type--hierarchy"></a>Türü & hiyerarşisini tanımlayın 

Bu bölümde, bir tür hiyerarşisi, yazar ve bunları, zaman serisi örnekleri ile ilişkilendirin. Daha fazla bilgi edinin [zaman serisi modelleri](./time-series-insights-update-tsm.md).

  1. Explorer'ın tıklayarak **modeli** uygulama çubuğunda sekme.

     ![öğretici-on dört][14]

  1. Türleri bölümüne tıklayarak **+ Ekle**. Bu, yeni bir zaman serisi modeli türü oluşturmanızı sağlar.

     ![öğretici-on beş][15]

  1. Türü Düzenleyicisi'nde girin bir **adı**, **açıklama**ve ardından değişkenlerinin ilgili daha fazla bilgi için **ortalama**, **Min**, ve **Max** aşağıda gösterildiği gibi değerleri. Tıklayarak **Oluştur** türünü kaydetmek için.

     ![öğretici-on altı][16]

     ![öğretici-on yedi][17]

  1. İçinde **hiyerarşileri** bölümünde, tıklayarak **+ Ekle**. Bu, yeni bir zaman serisi modeli hiyerarşisi oluşturmanızı sağlar.

     ![öğretici-On sekiz][18]

  1. Hiyerarşi düzenleyiciye bir **adı** ve hiyerarşi düzeyleri aşağıda gösterildiği gibi ekleyin. Tıklayarak **Oluştur** hiyerarşisine kaydetmek için.

     ![öğretici-on dokuz][19]

     ![öğretici-yirmi][20]

  1. İçinde **örnekleri** bölümünde bir örneğini seçin ve tıklayın **Düzenle**. Bu, bir tür ve hiyerarşi Bu örnekle ilişkili olanak tanır.

     ![öğretici-yirmi-bir][21]

  1. Örneği Düzenleyici'de gösterildiği gibi adım 3, 5 yukarıda tanımlanan hiyerarşi ve türünü seçin.

     ![öğretici yirmi iki][22]

  1. Alternatif olarak, tüm örnekleri için aynı anda bunun için düzenleyebileceğiniz `instances.json` daha önce indirilen dosya. Bu dosyada, tüm değiştirmek **TypeID** ve **HierarchyId** Kimliğine sahip alanlar elde adımları 3, yukarıdaki 5.

  1. İçinde **örnekleri** bölümünde, tıklayın **karşıya JSON** ve düzenlenmiş karşıya `instances.json` dosya aşağıda gösterildiği gibi.

     ![öğretici yirmi üç][23]

  1. Gidin **Analytics** sekmesini ve tarayıcınızı yenileyin. Artık, tür ve yukarıda tanımlanan hiyerarşi ile ilişkilendirilmiş tüm örnekler görmeniz gerekir.

     ![öğretici yirmi dört][24]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

* TSI (Önizleme) ortamı oluşturun.
* Olay Hub'ına TSI (Önizleme) ortama bağlanın.
* Bir Rüzgar grubu simülasyonu için veri akışı (Önizleme) TSI ortamına çalıştırın.
* Temel analiz verileri gerçekleştirin.
* Hiyerarşi, zaman serisi modeli tür tanımlama ve örneklerinizin ile ilişkilendirin.

TSI güncelleştirme ortamınızı oluşturma işlemini öğrendiğinize göre TSI temel kavramlar hakkında daha fazla bilgi edinebilirsiniz.

TSI depolama yapılandırması hakkında okuyun:

> [!div class="nextstepaction"]
> [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Zaman serisi modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure TSI (Önizleme) verileri modelleme](./time-series-insights-update-tsm.md)

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