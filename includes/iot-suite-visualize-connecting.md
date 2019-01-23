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
ms.openlocfilehash: 73ba80878615f04e1755a4d12014691c5ae2a077
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453129"
---
## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızın gönderilen telemetriyi görüntüleyebilirsiniz **cihazları** çözümdeki sayfası.

1. Şirket cihaz listesinde sağladığınız cihazı seçin **cihazları** sayfası. Bir panel Cihazınızı bir çizim cihaz telemetri dahil olmak üzere ilgili bilgileri görüntüler:

    ![Cihaz ayrıntıları bakın](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Seçin **baskısı** telemetri görünümünü değiştirmek için:

    ![Basınç telemetrisini görüntüleme](media/iot-suite-visualize-connecting/devicespressure.png)

1. Cihazınız hakkında tanılama bilgilerini görüntülemek için aşağı kaydırın **tanılama**:

    ![Cihaz tanılamayı görüntüle](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda hareket

Cihazlarınızda yöntem çağırmak için **cihazları** Uzaktan izleme çözümünde sayfası. Örneğin, Uzaktan izleme çözüm içinde **Soğutucu** cihazları uygulayan bir **FirmwareUpdate** yöntemi.

1. Seçin **cihazları** gitmek için **cihazları** çözümdeki sayfası.

1. Şirket cihaz listesinde sağladığınız cihazı seçin **cihazları** sayfası:

    ![Gerçek Cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda çağırabilirsiniz yöntemlerin listesini görüntülemek için seçin **işleri**, ardından **Run yöntemi**. Birden fazla cihazda çalıştırılacak bir iş zamanlamak için listede birden çok cihaz seçebilirsiniz. **İşleri** paneli yöntemi türleri, seçtiğiniz tüm cihazlar için ortak gösterir.

1. Seçin **FirmwareUpdate**, iş adı kümesine **UpdatePhysicalChiller**. Ayarlama **üretici yazılımı sürümü** için **2.0.0**ayarlayın **bellenim URI** için **http://contoso.com/updates/firmware.bin**ve ardından **Uygula**:

    ![Üretici yazılımı güncelleştirme zamanla](media/iot-suite-visualize-connecting/deviceschedule.png)

1. İletiler dizisini sanal cihazı yöntemi işlerken cihazınızın kodunu çalıştıran konsolda görüntüler.

1. Güncelleştirme tamamlandığında, yeni bellenim sürümünü görüntüler **cihazları** sayfası:

    ![Güncelleştirme tamamlandı](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> İş çözümdeki durumunu izlemek için seçin **görünümü**.

## <a name="next-steps"></a>Sonraki adımlar

Makaleyi [Uzaktan izleme çözüm Hızlandırıcısını özelleştirme](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) çözüm Hızlandırıcısını özelleştirmek için kullanabileceğiniz adımlar anlatılmaktadır.