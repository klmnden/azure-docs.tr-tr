---
title: Azure IOT merkezi uygulamanızda aygıtları yönetme | Microsoft Docs
description: Bir operatör olarak, Azure IOT merkezi uygulamanızda cihazların nasıl yönetileceğini öğrenin.
services: iot-central
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: b56e352a1f39dbd536347f6c7e83a6aabfe32ecc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IOT merkezi uygulamanızı aygıtları yönetme

Bu makalede, Microsoft Azure IOT merkezi uygulamanızda cihazları yönetmek için bir işleç olarak nasıl. Bir operatör olarak, şunları yapabilirsiniz:

- Kullanım **Explorer** sayfasını görüntülemek, eklemek ve Azure IOT merkezi uygulamanıza bağlı olan aygıtları silmek için.
- Aygıtlarınızı güncel bir stoğunu koruyun.
- Cihaz meta verilerini aygıt özelliklerinde saklanan değerleri değiştirerek güncelliğini.
- Belirli bir CİHAZDAN bir ayar güncelleştirerek aygıtlarınızı davranışını denetleyen **ayarları** sayfası.

## <a name="view-your-devices"></a>Aygıtlarınızı görüntüleyin

Tek bir cihaza görüntülemek için:

1. Seçin **Explorer** sol gezinti menüsünde. Burada bir listesini görürsünüz, [aygıt şablonları](howto-set-up-template.md).

1. Seçin bir **cihaz şablonu** sol bölmedeki.

1. Sağ taraftaki bölmede, bu cihaz şablondan oluşturulmuş cihazların bir listesini görürsünüz. Görmek için tek bir cihaza seçin **cihaz ayrıntıları** sayfa bu cihaz için:

    ![Cihaz Ayrıntıları sayfası](./media/howto-manage-devices/image1.png)

## <a name="add-a-device"></a>Cihaz ekleme

Azure IOT merkezi uygulamanıza bir cihaz eklemek için:

1. Seçin **Explorer** sol gezinti menüsünde.

1. Bir cihaz oluşturmak istediğiniz cihaz şablonu seçin.

1. Seçin + **yeni**.

1. Seçin **gerçek** veya **benzetimli**. Azure IOT merkezi uygulamanıza bağlanan bir fiziksel aygıt gerçek bir cihazı içindir. Bir sanal cihaz sizin için Azure IOT Merkezi tarafından oluşturulan örnek veriler var. Bu örnek, gerçek cihaz kullanır. Seçin **gerçek** gitmek için **cihaz ayrıntıları** yeni cihazınız için sayfa.

## <a name="delete-a-device"></a>Bir aygıtı silme

Azure IOT merkezi uygulamanızdan ya da gerçek veya sanal cihazı silmek için:

1. Seçin **Explorer** Gezinti menüsünde.

1. Cihaz şablonu silmek istediğiniz cihazı seçin.

1. Silme için aygıt yanındaki kutuyu işaretleyin.

1. Seçin **silmek**.

## <a name="change-a-device-setting"></a>Bir aygıt ayarı değiştirme

Ayarları bir cihaz davranışını denetler. Diğer bir deyişle, bunlar aygıtınıza girişleri yapın olanak sağlar. Görüntüleyebilir ve aygıt ayarlarını güncelleştirmede **cihaz ayrıntıları** sayfası.

1. Seçin **Explorer** Gezinti menüsünde.

1. Cihaz şablon ayarlarını değiştirmek istediğiniz cihazı seçin.

1. Seçin **ayarları** sekmesi. Burada, Cihazınızı sahip tüm ayarlar ve geçerli değerlerini görürsünüz. Her ayar için cihaz yine eşitleniyor görebilirsiniz.

1. İstenen değerleri için ayarları değiştirin. Aynı anda birden çok ayarlarını değiştirin ve bunları tümü aynı anda güncelleştirin.

1. Seçin **güncelleştirme**. Değerleri aygıtınıza gönderilir. Aygıt ayar değişikliğini kabul ettikten sonra ayarın durumu geri gider **eşitlenen**.

## <a name="change-a-property"></a>Bir özellik değiştirme

Şehir ve seri numarası gibi cihaz ile ilişkili aygıt meta verileri özelliklerdir. Görüntüleyin ve güncelleştirme özellikleri **cihaz ayrıntıları** sayfası.

1. Seçin **Explorer** Gezinti menüsünde.

1. Özelliklerini değiştirmek istediğiniz aygıtın aygıt şablonunu seçin.

1. Seçin **özellikleri** sekmesi, tüm özellikler gördüğünüz.

1. İstenen değerleri için özelliklerini değiştirin. Aynı anda birden çok özelliklerini değiştirin ve bunları tüm aynı anda güncelleştirin.

1. Seçin **güncelleştirme**.

> [!NOTE]
> Değeri değiştiremezsiniz _cihaz özellikleri_. Cihaz özellikleri aygıt tarafından ayarlanır ve Azure IOT merkezi uygulamasında salt okunurdur.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda cihazların nasıl yönetileceğini öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz kümelerini kullanma](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->