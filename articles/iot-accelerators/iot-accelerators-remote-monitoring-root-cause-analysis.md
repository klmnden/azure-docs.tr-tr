---
title: Bir uyarıda kök neden analizi yürütme - Azure | Microsoft Docs
description: Bu öğreticide Azure Time Series Insights kullanarak bir uyarıda kök neden analizinin nasıl yürütüleceğini öğreneceksiniz.
author: aditidugar
ms.author: adugar
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/11/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 8258c8f34b4b9a1b216d9d497dcdf7d3b8db1373
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369520"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Öğretici: Bir uyarıda kök neden analizi yürütme

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözüm hızlandırıcısının nasıl kullanılacağını öğreneceksiniz. Uzaktan İzleme çözüm panosunda bir uyarı tetiklendiğini görecek ve sonra kök nedeni araştırmak için Azure Time Series Insights gezginini kullanacaksınız.

Öğreticide konum, rakım, hız ve yük sıcaklığı telemetri verilerini gönderen iki sanal tır cihazı kullanılmaktadır. Tırlar, Contoso adlı bir kuruluş tarafından yönetilmektedir ve Uzaktan İzleme çözümü hızlandırıcısına bağlıdır. Contoso operatörü olarak, tırlarınızdan birinin (delivery-truck-02) neden düşük sıcaklık uyarısı kaydettiğini anlamanız gerekir.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Panodaki cihazları filtreleme
> * Gerçek zamanlı telemetri verilerini görüntüleme
> * Time Series Insights gezgininde verileri keşfetme
> * Kök neden analizi yürütme
> * Öğrendiklerinize dayanarak yeni bir kural oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Görüntülenecek cihazları seçme

**Dashboard** (Pano) sayfasında görüntülenecek bağlı cihazları seçmek için filtreleri kullanın. Yalnızca **Truck** (Tır) cihazlarını görüntülemek için filtre açılan menüsünden yerleşik **Trucks** (Tırlar) filtresini seçin:

[![Panoda trucks (tır) filtresi](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Bir filtre uyguladığınızda **Dashboard** (Pano) sayfasındaki haritada ve telemetri panelinde yalnızca filtre koşullarıyla eşleşen cihazlar görüntülenir. **truck-02** dahil olmak üzere, çözüm hızlandırıcısına iki tırın bağlı olduğunu görebilirsiniz.

## <a name="view-real-time-telemetry"></a>Gerçek zamanlı telemetri verilerini görüntüleme

Çözüm hızlandırıcısı, **Dashboard** (Pano) sayfasındaki grafiğe gerçek zamanlı telemetri verilerini çizer. Varsayılan olarak grafik, zaman içinde değişiklik gösteren rakım telemetri verilerini göstermektedir:

[![Tır için rakım telemetri verileri çizimi](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Tırların sıcaklık telemetri verilerini görüntülemek için **Telemetri paneli**’nde **Sıcaklık** seçeneğine tıklayın. Son 15 dakika boyunca her iki tırın da sıcaklığının nasıl değişiklik gösterdiğini görebilirsiniz. Uyarılar bölmesinde delivery-truck-02 için düşük sıcaklık uyarısının tetiklendiğini de görebilirsiniz.

[![Düşük sıcaklık uyarısı ile RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Verileri keşfetme

Düşük sıcaklık alarmının nedenini belirlemek için, Time Series Insights gezgininde tır telemetri verilerini açın. Panodaki **Time Series Insights’ta Keşfet** bağlantılarından herhangi birine tıklayın:

[![TSI bağlantıları vurgulanmış şekilde RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Gezgin başlatıldığında tüm cihazların listelendiğini görürsünüz:

[![TSI Gezgini başlangıç görünümü](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtre kutusuna **delivery-truck** yazarak cihazları filtreleyin ve sol panelde **Ölçü** olarak **sıcaklık** seçeneğini belirleyin:

[![TSI Gezgini tır sıcaklığı](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Uzaktan İzleme panosunda gördüğünüz aynı görünümü görürsünüz ve uyarının tetiklendiği zaman çerçevesinin yakınına yakınlaştırma yapabilirsiniz:

[![TSI Gezgini yakınlaştırma](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Tırlardan gelen diğer telemetri akışlarına da ekleme yapabilirsiniz. Sol üst köşedeki **Ekle** düğmesine tıklayın. Yeni bir bölme görüntülenir:

[![Yeni bölme ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Rakım telemetri verilerini görünümünüze eklemek için **Ölçü** olarak **rakım** ve **Bölme ölçütü:** değeri olarak **iothub-connection-device-id** seçeneğini belirleyin:

[![Sıcaklık ve rakım ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Uyarıyı tanılama

Geçerli görünümdeki akışlara baktığınızda, iki tır için rakım profillerinin çok farklı olduğunu görebilirsiniz. Ayrıca tır yüksek bir rakıma ulaştığında **delivery-truck-02** tırında sıcaklık düşüşü gerçekleşir. Tırlar aynı rotayı izleyecek şekilde zamanlanmış olduğundan bu bulgu sizi şaşırtabilir.

Turların farklı yolculuk yolları izlediğine dair şüphenizi onaylamak için, **Ekle** düğmesini kullanarak yan panele başka bir bölme ekleyin. Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Boylam telemetri verilerini görünümünüze eklemek için **Ölçü** olarak **boylam** ve **Bölme ölçütü:** değeri olarak **iothub-connection-device-id** seçeneğini belirleyin. **Boylam** akışları arasındaki farka bakarak tırların farklı yolculuklar yaptığını görebilirsiniz:

[![Sıcaklık, rakım ve boylam ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Tır rotaları genellikle önceden iyileştirilmiş olsa da, trafik desenleri, hava durumu ve diğer öngörülemez olayların gecikmelere neden olabileceğini fark eder ve son dakika rota kararlarını, tır sürücülerinin takdirine bırakırsınız. Ancak, taşıt içindeki varlıklarınızın sıcaklığı kritik önem taşısa da, 1 dakikalık aralıklarda ortalama rakımın 350 ft üzerine çıkması durumunda bir uyarı aldığınızdan emin olmak için Uzaktan İzleme çözümünüzde ek bir kural oluşturmanız gerekir:

[![Uzaktan İzleme kuralları sekmesi rakım kuralı ayarlama](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Kurallar oluşturma ve kuralları düzenleme hakkında bilgi edinmek için, [cihaz sorunlarını algılama](iot-accelerators-remote-monitoring-automate.md) bölümünde yer alan önceki öğreticiye göz atın.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözüm hızlandırıcısı ile Time Series Insights gezgininin nasıl kullanılacağı gösterildi. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları tanımlama ve düzeltme amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları tanımlamak ve düzeltmek için cihaz uyarılarını kullanma](iot-accelerators-remote-monitoring-maintain.md)
