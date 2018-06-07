---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 5702c6e9c9d75c6cccb82f1c57684ef7b9898c34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666033"
---
## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızı gönderilen telemetriyi görüntüleyebilir **aygıtları** çözümdeki sayfası.

1. Üzerinde aygıtlar listesinde sağladığınız cihazı seçin **aygıtları** sayfası. Bölmenin çizim cihaz telemetri dahil olmak üzere Cihazınızı hakkında bilgileri görüntüler:

    ![Cihaz ayrıntıları](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Seçin **baskısı** telemetri görünümünü değiştirmek için:

    ![Görünüm baskısı telemetri](media/iot-suite-visualize-connecting/devicespressure.png)

1. Cihazınızla ilgili tanılama bilgilerini görüntülemek için ekranı aşağı kaydırarak **tanılama**:

    ![Görünüm aygıt tanılama](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda hareket

Cihazlarınızda yöntemleri çağırmak için **aygıtları** Uzaktan izleme çözümünde sayfası. Örneğin, Uzaktan izleme çözüm içinde **Soğutucu** aygıtları uygulayan bir **FirmwareUpdate** yöntemi.

1. Seçin **aygıtları** gitmek için **aygıtları** çözümdeki sayfası.

1. Üzerinde aygıtlar listesinde sağladığınız cihazı seçin **aygıtları** sayfa:

    ![Fiziksel Cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda çağırabilir yöntemlerinin listesini görüntülemek için seçin **işleri**, ardından **Run yöntemi**. Birden fazla cihazda işi zamanlamak için listede birden çok aygıt seçebilirsiniz. **İşleri** paneli yöntemi türlerini seçtiğiniz tüm cihazların ortak gösterir.

1. Seçin **FirmwareUpdate**, iş adı ayarlamak **UpdatePhysicalChiller**. Ayarlama **bellenim sürümü** için **2.0.0**ayarlayın **bellenim URI** için **http://contoso.com/updates/firmware.bin**ve ardından **Uygula**:

    ![Zamanlama bellenimi güncelleştirme](media/iot-suite-visualize-connecting/deviceschedule.png)

1. İleti sırası, sanal cihaz yöntemi işlerken aygıt kodunuzu çalıştırmaya görüntülemez.

1. Güncelleştirme tamamlandığında, yeni bellenim sürümü görüntüler **aygıtları** sayfa:

    ![Güncelleştirme tamamlandı](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Çözümdeki işinin durumunu izlemek için tercih **Görünüm**.

## <a name="next-steps"></a>Sonraki adımlar

Makaleyi [Uzaktan izleme Çözüm Hızlandırıcısı özelleştirme](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) Çözüm Hızlandırıcısı özelleştirmek için bazı yöntemleri açıklar.