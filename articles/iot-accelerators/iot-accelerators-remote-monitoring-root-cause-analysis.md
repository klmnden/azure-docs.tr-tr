---
title: Bir uyarıda kök neden analizi yürütme - Azure | Microsoft Docs
description: Bu öğreticide Azure Time Series Insights kullanarak bir uyarıda kök neden analizinin nasıl yürütüleceğini öğreneceksiniz.
author: aditidugar
ms.author: adugar
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 70d29359d4a4bcf9f5badbbf0c553d7bed88a02b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61444768"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Öğretici: Bir uyarı üzerine kök neden analizi yürütme

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözümü hızlandırıcısının nasıl kullanılacağını öğreneceksiniz. Uzaktan İzleme çözümü panosunda bir uyarı tetiklendiğini görecek ve sonra kök nedeni araştırmak için Azure Time Series Insights gezginini kullanacaksınız.

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

Bir filtre uyguladığınızda, filtre koşulları karşılayan cihazların Haritası ve telemetriyi panelinde görüntülenir **Pano**. **truck-02** dahil olmak üzere, çözüm hızlandırıcısına iki tırın bağlı olduğunu görebilirsiniz.

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

Yazarak cihazları Filtrele **teslim kamyon** seçin ve filtre kutusuna **sıcaklık** olarak **ölçü** sol panelde:

[![TSI Gezgini tır sıcaklığı](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Uzaktan izleme Panoda gördüğünüz aynı görünümde görürsünüz. Ayrıca, içinden uyarının başlatıldığı zaman çerçevesi için artık daha yakın içinde yakınlaştırabilirsiniz:

[![TSI Gezgini yakınlaştırma](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Tırlardan gelen diğer telemetri akışlarına da ekleme yapabilirsiniz. Tıklayın **Ekle** sol üst köşesindeki düğme. Yeni bir bölme görüntülenir:

[![Yeni bölme ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Seçin **yüksekliği** olarak **ölçü** ve **iothub-bağlantı-cihaz-ID** olarak **bölünmüş tarafından** yüksekliği telemetrisine eklenecek değer görünümü:

[![Sıcaklık ve rakım ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Uyarıyı tanılama

Geçerli görünümdeki akışları baktığınızda, iki kamyon için yüksekliği profilleri farklı olduğunu görebilirsiniz. Ayrıca tır yüksek bir rakıma ulaştığında **delivery-truck-02** tırında sıcaklık düşüşü gerçekleşir. Tırlar aynı rotayı izleyecek şekilde zamanlanmış olduğundan bu bulgu sizi şaşırtabilir.

Turların farklı yolculuk yolları izlediğine dair şüphenizi onaylamak için, **Ekle** düğmesini kullanarak yan panele başka bir bölme ekleyin. Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Boylam telemetri verilerini görünümünüze eklemek için **Ölçü** olarak **boylam** ve **Bölme ölçütü:** değeri olarak **iothub-connection-device-id** seçeneğini belirleyin. **Boylam** akışları arasındaki farka bakarak tırların farklı yolculuklar yaptığını görebilirsiniz:

[![Sıcaklık, rakım ve boylam ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Kamyon yollar genellikle önceden getirilmiş olsa da trafik düzenlerini, hava durumu, diğer öngörülemeyen olayları gecikmelere neden ve üzerinde en iyi kendi mantığınızı tabanlı kamyon sürücüleri için son dakika yönlendirme kararları bırakın olduğunu unutmayın. Ancak, varlıklarınızı araç içinde Sıcaklığın kritik olduğundan, Uzaktan izleme çözümünde ek bir kural oluşturmanız gerekir. Bu kural, ortalama yüksekliği 1 dakikalık aralık boyunca 350 ayak olursa bir uyarı alırsınız sağlamaktır:

[![Uzaktan İzleme kuralları sekmesi rakım kuralı ayarlama](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Kurallar oluşturma ve kuralları düzenleme hakkında bilgi edinmek için, [cihaz sorunlarını algılama](iot-accelerators-remote-monitoring-automate.md) bölümünde yer alan önceki öğreticiye göz atın.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözümü hızlandırıcısı ile Time Series Insights gezgininin nasıl kullanılacağı gösterildi. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları tanımlama ve düzeltme amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları tanımlamak ve düzeltmek için cihaz uyarılarını kullanma](iot-accelerators-remote-monitoring-maintain.md)
