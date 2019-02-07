---
title: Azure IoT Central'da cihazlarınızı izleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızı cihazlarınızı izlemek için kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 02/01/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 9480565643887b5a9a4d644ba3173b365eaea29c
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767782"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Öğretici: Azure IoT Central kullanarak cihazlarınızı izleme

Bu öğreticide, bir operatör olarak cihazlarınızı izlemek ve ayarlarınızı değiştirmek için Microsoft Azure IoT Central uygulamasını nasıl kullanacağınız gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmaya yönelik üç oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Operatörün görünümlerini özelleştirme](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir klima cihazındaki sıcaklık belirli bir eşiği aştığında bildirim gönderecek şekilde bir kural eklemiştir. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

[Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) öğreticisinin sonunda aldığınız e-posta iletisini açın. E-postada **Cihazınızı açmak için buraya tıklayın**’ı seçin:

![Uyarı bildirim e-postası](media/tutorial-monitor-devices-experimental/email.png)

Önceki öğreticilerde oluşturduğunuz **Bağlı Klima-1** sanal cihazının **Cihaz** sayfası tarayıcıda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/tutorial-monitor-devices-experimental/sourcedevice.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Operatör olarak, bir cihaza ilişkin bilgileri **Ölçümler**, **Ayarlar**, **Özellikler**, **Kurallar** ve **Pano** sayfalarında görüntüleyebilirsiniz. Oluşturucu, **Pano**’yu bağlı bir klima cihazına ilişkin önemli bilgileri gösterecek şekilde özelleştirmiştir.

Cihaza ilişkin bilgileri görmek için **Pano** görünümünü seçin.

![Cihaz panosu](media/tutorial-monitor-devices-experimental/initial_screen.png)

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihazın geçerli hedef sıcaklığını da **Ayarlanmış hedef sıcaklık** kutucuğunda görebilirsiniz. Hedef sıcaklığın çok yüksek olmasına karar verdiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazın hedef sıcaklığını değiştirmek için **Ayarlar** sayfasını kullanın:

1. **Ayarlar**'ı seçin. **Sıcaklığı Ayarla**’yı 75 olarak değiştirin. Yeni hedef sıcaklığı cihaza göndermek için **Güncelleştir**’i seçin. Cihaz ayarlarını değiştir onaylarsa, bir ayar durumu değişir **eşitlenen**:

    ![Ayarları güncelleştirme](media/tutorial-monitor-devices-experimental/change_settings.png)

2. **Pano**’yu seçin ve yeni ayar değerini doğrulayın:

    ![Güncelleştirilmiş cihaz panosu](media/tutorial-monitor-devices-experimental/new_settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="nextstepaction"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

Artık cihazınızı nasıl izleyeceğinizi bildiğinize göre [Cihaz ekleme](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) adımını uygulamanız önerilir.