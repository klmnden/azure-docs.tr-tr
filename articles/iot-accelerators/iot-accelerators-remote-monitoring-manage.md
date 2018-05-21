---
title: Uzaktan izleme çözümünde - Azure cihaz Yönetimi | Microsoft Docs
description: Bu öğretici, Uzaktan izleme çözümüne bağlı cihazların nasıl yönetileceğini gösterir.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3ad6de2a0ebcd257ca90ea3c5c69988d4c1afd7a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="manage-and-configure-your-devices"></a>Yönetmek ve cihazlarınızı yapılandırmak

Bu öğretici cihaza Uzaktan izleme çözümü yönetim özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Contoso bir çıktı artırmak için kendi olanaklarının genişletmek için yeni makineler sıralı. Teslim edilecek yeni makineler için beklerken çözümünüzün davranışını doğrulamak için bir benzetimi çalıştırmak istediğiniz. Bir operatör olarak, yönetmek ve cihazlar Uzaktan izleme çözümünde yapılandırmak istiyorsunuz.

Yönetmek ve cihazları yapılandırmak için genişletilebilir bir yol sağlamak için Uzaktan izleme çözümü IOT Hub özellikleri gibi kullanır, [işleri](../iot-hub/iot-hub-devguide-jobs.md) ve [doğrudan yöntemleri](../iot-hub/iot-hub-devguide-direct-methods.md). Bir aygıt Geliştirici yöntemleri fiziksel cihazda nasıl uyguladığını bilgi edinmek için [Uzaktan izleme Çözüm Hızlandırıcısı özelleştirme](iot-accelerators-remote-monitoring-customize.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz sağlayın.
> * Sanal cihaz sınayın.
> * Aygıt yöntemlerini çözümden çağırın.
> * Bir aygıt yeniden yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-accelerators-remote-monitoring-deploy.md) Öğreticisi.

## <a name="add-a-simulated-device"></a>Bir sanal cihaz ekleme

Gidin **aygıtları** sayfasında çözümde ve ardından **+ yeni cihaz**. İçinde **yeni cihaz** paneli, seçin **benzetimli**:

![Bir sanal cihaz sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovision.png)

Kümesine sağlamak için aygıt sayısını bırakın **1**. Seçin **hatalı altyapısı** cihaz modeli ve ardından **Uygula** sanal cihazı oluşturmak için:

![Benzetimli altyapısı cihazı hazırlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine.png)

Sağlama konusunda bilgi almak için bir *fiziksel* aygıtı bkz [Cihazınızı Uzaktan izleme Çözüm Hızlandırıcısı bağlama](iot-accelerators-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Sanal cihaz test

Yeni sanal cihazınız ayrıntılarını görüntülemek için aygıtlar listesinde seçin **aygıtları** sayfası. Cihazınızla ilgili bilgi görüntüler **aygıt ayrıntı** paneli:

![Yeni sanal altyapısı aygıtı görüntüleme](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew.png)

İçinde **aygıt ayrıntı**, yeni aygıtınızı telemetri göndermeyi doğrulayın. Cihazınızı farklı telemetri akışlardan görüntülemek için bir telemetri adı gibi seçin **titreşimi**:

![Bir telemetri akışı görüntülemek için seçin](./media/iot-accelerators-remote-monitoring-manage/devicesvibration.png)

**Aygıt ayrıntı** Masası etiket değerleri, desteklediği yöntemleri ve cihaz tarafından raporlanan özellikler gibi cihaz hakkında diğer bilgileri görüntüler.

Ayrıntılı tanılama görüntülemek için kaydırma görmek üzere **tanılama**.

## <a name="act-on-a-device"></a>Bir cihazda hareket

Bir veya daha fazla aygıtlar üzerinde çalışmak için aygıt listesinden seçin ve ardından **işleri**. **Altyapısı** cihaz modeli belirtir üç yöntem bir aygıtı desteklemesi gerekir:

![Altyapısı yöntemleri](./media/iot-accelerators-remote-monitoring-manage/devicesmethods.png)

Seçin **FillTank**, iş adı ayarlamak **FillEngineTank**ve ardından **Uygula**:

![Zamanlama yeniden başlatma yöntemi](./media/iot-accelerators-remote-monitoring-manage/devicesrestartengine.png)

İşin durumunu izlemek için **Bakım** sayfasında, **işleri**:

![Zamanlamalar iş izleme](./media/iot-accelerators-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Diğer cihazlar yöntemleri

Farklı sanal cihaz türleri için araştırırken, diğer cihaz türleri farklı yöntemi destekler bakın. Fiziksel aygıtların bir dağıtımda cihaz modeli cihaz desteklemelidir yöntemlerini belirtir. Genellikle, aygıt Geliştirici hareket aygıt yanıt yöntem çağrısı olarak yapar kodu geliştirmek için sorumludur.

Birden fazla cihazda çalıştırmak için bir yöntem zamanlamak için birden çok aygıt listesinde üzerinde seçebilirsiniz **aygıtları** sayfası. **İşleri** paneli yöntemi türleri seçilen tüm aygıtlarda ortak gösterir.

## <a name="reconfigure-a-device"></a>Bir aygıt yeniden yapılandırın

Bir aygıt yapılandırmasını değiştirmek için aygıt listesinde seçin **aygıtları** sayfa'ı seçin **işleri**ve ardından **yeniden**. İşlerini paneli değiştirebileceğiniz seçili cihaz için özellik değerlerini gösterir:

![Bir aygıt yeniden yapılandırın](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure.png)

Değişiklik yapmak için iş için bir ad eklemek, özellik değerlerini güncelleştirin ve seçin **Uygula**:

![Bir cihaz özelliği değeri güncelleştirin](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical.png)

İşin durumunu izlemek için **Bakım** sayfasında, **işleri**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz sağlayın.
> * Sanal cihaz sınayın.
> * Aygıt yöntemlerini çözümden çağırın.
> * Bir aygıt yeniden yapılandırın.

Cihazlarınızı yönetmek öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Aygıt sorunlarını gidermek ve](iot-accelerators-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](iot-accelerators-remote-monitoring-test.md).
* [Uzaktan izleme Çözüm Hızlandırıcısı Cihazınızı bağlama](iot-accelerators-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->