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
ms.openlocfilehash: 9b9e28f18208674609d0842b0e3a54e3fc661c9f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188805"
---
## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızın gönderilen telemetriyi görüntüleyebilirsiniz **Device Explorer** çözümdeki sayfası.

1. Şirket cihaz listesinde sağladığınız cihazı seçin **Device Explorer** sayfası. Bir panel Cihazınızı bir çizim cihaz telemetri dahil olmak üzere ilgili bilgileri görüntüler:

    ![Cihaz ayrıntıları bakın](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Seçin **baskısı** telemetri görünümünü değiştirmek için:

    ![Basınç telemetrisini görüntüleme](media/iot-suite-visualize-connecting/devicespressure.png)

1. Cihazınız hakkında tanılama bilgilerini görüntülemek için aşağı kaydırın **tanılama**:

    ![Cihaz tanılamayı görüntüle](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda hareket

Cihazlarınızda yöntem çağırmak için **Device Explorer** Uzaktan izleme çözümünde sayfası. Örneğin, Uzaktan izleme çözüm içinde **Soğutucu** cihazları uygulayan bir **yeniden** yöntemi.

1. Seçin **cihazları** gitmek için **Device Explorer** çözümdeki sayfası.

1. Şirket cihaz listesinde sağladığınız cihazı seçin **Device Explorer** sayfası:

    ![Gerçek Cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda çağırabilirsiniz yöntemlerin listesini görüntülemek için seçin **işleri**, ardından **yöntemleri**. Birden fazla cihazda çalıştırılacak bir iş zamanlamak için listede birden çok cihaz seçebilirsiniz. **İşleri** paneli yöntemi türleri, seçtiğiniz tüm cihazlar için ortak gösterir.

1. Seçin **yeniden**, iş adı kümesine **RebootPhysicalChiller** seçip **Uygula**:

    ![Üretici yazılımı güncelleştirme zamanla](media/iot-suite-visualize-connecting/deviceschedule.png)

1. İletiler dizisini sanal cihazı yöntemi işlerken cihazınızın kodunu çalıştıran konsolda görüntüler.

> [!NOTE]
> İş çözümdeki durumunu izlemek için seçin **iş durumunu görüntüleme**.

## <a name="next-steps"></a>Sonraki adımlar

Makaleyi [Uzaktan izleme çözüm Hızlandırıcısını özelleştirme](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) çözüm Hızlandırıcısını özelleştirmek için kullanabileceğiniz adımlar anlatılmaktadır.