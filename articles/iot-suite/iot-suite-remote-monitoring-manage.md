---
title: "Uzaktan izleme çözümünde - Azure cihaz Yönetimi | Microsoft Docs"
description: "Bu öğretici, Uzaktan izleme çözümüne bağlı cihazların nasıl yönetileceğini gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 84c2eaaab2dfc09c93fbfeac3fe2bfcc7066a411
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="manage-and-configure-your-devices"></a>Yönetmek ve cihazlarınızı yapılandırmak

Bu öğretici cihaza Uzaktan izleme çözümü yönetim özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Contoso bir çıktı artırmak için kendi olanaklarının genişletmek için yeni makineler sıralı. Teslim edilecek yeni makineler için beklerken çözümünüzün davranışını doğrulamak için bir benzetimi çalıştırmak istediğiniz. Bir operatör olarak, yönetmek ve cihazlar Uzaktan izleme çözümünde yapılandırmak istiyorsunuz.

Yönetmek ve cihazları yapılandırmak için genişletilebilir bir yol sağlamak için Uzaktan izleme çözümü IOT Hub özellikleri gibi kullanır, [işleri](../iot-hub/iot-hub-devguide-jobs.md) ve [doğrudan yöntemleri](../iot-hub/iot-hub-devguide-direct-methods.md). Bir aygıt Geliştirici yöntemleri fiziksel cihazda nasıl uyguladığını bilgi edinmek için [önceden yapılandırılmış Uzaktan izleme çözümü özelleştirme](iot-suite-remote-monitoring-customize.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz sağlayın.
> * Sanal cihaz sınayın.
> * Aygıt yöntemlerini çözümden çağırın.
> * Bir aygıt yeniden yapılandırın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

## <a name="provision-a-simulated-device"></a>Bir sanal cihaz sağlama

Gidin **aygıtları** sayfasında çözümde ve ardından **sağlama**. İçinde **sağlama** paneli, seçin **benzetimli**:

![Bir sanal cihaz sağlama](media/iot-suite-remote-monitoring-manage/devicesprovision.png)

Kümesine sağlamak için aygıt sayısını bırakın **1**. Seçin **altyapısı** olarak **cihaz modeli**ve ardından **Uygula** sanal cihazı oluşturmak için:

![Benzetimli altyapısı cihazı hazırlama](media/iot-suite-remote-monitoring-manage/devicesprovisionengine.png)

Sağlama konusunda bilgi almak için bir *fiziksel* aygıtı bkz [Cihazınızı Uzaktan izleme önceden yapılandırılmış çözümü arasında bağlantı](iot-suite-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Sanal cihaz test

Yeni sanal cihazınız ayrıntılarını görüntülemek için aygıtlar listesinde seçin **aygıtları** sayfası. Cihazınızla ilgili bilgi görüntüler **aygıt ayrıntı** paneli:

![Yeni sanal altyapısı aygıtı görüntüleme](media/iot-suite-remote-monitoring-manage/devicesviewnew.png)

İçinde **aygıt ayrıntı**, yeni aygıtınızı telemetri göndermeyi doğrulayın. Cihazınızı farklı telemetri akışlardan görüntülemek için bir telemetri adı gibi seçin **titreşimi**:

![Bir telemetri akışı görüntülemek için seçin](media/iot-suite-remote-monitoring-manage/devicesvibration.png)

**Aygıt ayrıntı** Masası etiket değerleri, desteklediği yöntemleri ve cihaz tarafından raporlanan özellikler gibi cihaz hakkında diğer bilgileri görüntüler.

Ayrıntılı tanılama görüntülemek için kaydırma görmek üzere **tanılama**.

## <a name="act-on-a-device"></a>Bir cihazda hareket

Bir cihaz üzerinde çalışmak için aygıtlar listesinde seçin ve ardından **zamanlama**. **Altyapısı** cihaz modeli belirtir dört yöntem bir aygıtı desteklemesi gerekir:

![Altyapısı yöntemleri](media/iot-suite-remote-monitoring-manage/devicesmethods.png)

Seçin **yeniden**, iş adı ayarlamak **RestartEngine**ve ardından **Uygula**:

![Zamanlama yeniden başlatma yöntemi](media/iot-suite-remote-monitoring-manage/devicesrestartengine.png)

İşin durumunu izlemek için **Bakım** sayfasında, **sistem durumu**:

![Zamanlamalar iş izleme](media/iot-suite-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Diğer cihazlar yöntemleri

Farklı sanal cihaz türleri için araştırırken, diğer cihaz türleri farklı yöntemi destekler bakın. Fiziksel aygıtların bir dağıtımda cihaz modeli cihaz desteklemelidir yöntemlerini belirtir. Genellikle, aygıt Geliştirici hareket aygıt yanıt yöntem çağrısı olarak yapar kodu geliştirmek için sorumludur.

Birden fazla cihazda çalıştırmak için bir yöntem zamanlamak için birden çok aygıt listesinde üzerinde seçebilirsiniz **aygıtları** sayfası. **Zamanlama** paneli yöntemi türleri seçilen tüm aygıtlarda ortak gösterir.

## <a name="reconfigure-a-device"></a>Bir aygıt yeniden yapılandırın

Bir aygıt yapılandırmasını değiştirmek için aygıt listesinde seçin **aygıtları** sayfasında ve ardından **yeniden**. RECONFIGURE paneli değiştirebileceğiniz seçili cihaz için özellik değerlerini gösterir:

![Bir aygıt yeniden yapılandırın](media/iot-suite-remote-monitoring-manage/devicesreconfigure.png)

Değişiklik yapmak için iş için bir ad eklemek, özellik değerlerini güncelleştirin ve seçin **Uygula**:

![Bir cihaz özelliği değeri güncelleştirin](media/iot-suite-remote-monitoring-manage/devicesreconfigurephysical.png)

İşin durumunu izlemek için **Bakım** sayfasında, **sistem durumu**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz sağlayın.
> * Sanal cihaz sınayın.
> * Aygıt yöntemlerini çözümden çağırın.
> * Bir aygıt yeniden yapılandırın.

Cihazlarınızı yönetmek öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Aygıt sorunlarını gidermek ve](iot-suite-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](iot-suite-remote-monitoring-test.md).
* [Cihazınızı Uzaktan izleme önceden yapılandırılmış çözümü arasında bağlantı](iot-suite-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->