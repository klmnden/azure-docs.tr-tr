---
title: Görüntüleri karşıya yüklemek için Azure IOT Central uygulamasına | Microsoft Docs
description: Bir oluşturucu hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme hakkında bilgi edinin.
author: dominicbetts
ms.author: dobett
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a20662c2fc9b416fefce89a6ebe706307ee71bb7
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236473"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme

Bu makalede, nasıl bir oluşturucu, Azure IOT Central uygulamasına özel görüntüleri karşıya yükleyerek özelleştirebileceğiniz açıklanmaktadır. Örneğin, cihazın resmi bir cihaz panosunu özelleştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT Central bir uygulamadır. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
1. Ölçeklendirme ve resim dosyalarını yeniden boyutlandırma için bir araç.

## <a name="choose-where-to-use-custom-images"></a>Özel görüntüler kullanmak hedef konumu seçin

Bir Azure IOT Central uygulamasına aşağıdaki konumlarda özel görüntüleri ekleyebilirsiniz:

* **Uygulamalarım** sayfası

    ![Uygulama Yöneticisi sayfasında görüntüsü](media/howto-prepare-images/applicationmanager.png)

* Uygulama Panosu

    ![Uygulama Panoda resim](media/howto-prepare-images/homepage.png)

* Bir cihaz şablonu

    ![Görüntüye cihaz şablonu](media/howto-prepare-images/devicetemplate.png)

* Bir cihaz Panoda bir kutucuğu

    ![Cihaz kutucuğundaki görüntüsü](media/howto-prepare-images/devicetile.png)

* Bir cihaz kümesi Panoda bir kutucuğu

    ![Cihaz kümesi kutucuğundaki görüntüsü](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Görüntüleri hazırlama

Tüm dört konumlarında PNG, GIF veya JPEG görüntülerini kullanabilirsiniz.

Aşağıdaki tabloda kullanabileceğiniz resim boyutları özetlenmektedir:

| Location | Boyutlar |
| -------- | ------ |
| Uygulama Yöneticisi | 268 x 160 piksel |
| Cihaz şablonu | 64 x 64 piksel |
| Pano kutucukları | En küçük boyutlu 200 x 200 kutucuğudur piksel, daha büyük kutucukları küçük kutucuk kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 px, 400 x 200 piksel veya 400 x 400 px |

Uygulamada en iyi görüntü için yukarıdaki tabloda gösterilen boyutları eşleşen görüntüleri oluşturmanız gerekir.

## <a name="upload-the-images"></a>Görüntüleri karşıya yükleme

Aşağıdaki bölümlerde farklı konumlarda görüntüleri karşıya yükleme açıklanmaktadır:

### <a name="application-manager"></a>Uygulama Yöneticisi

Üzerinde kullanılacak bir görüntü karşıya yüklemek için **uygulamalarım** sayfasında, gitmek **uygulama ayarları** sayfasını **Yönetim** bölümü. Bu görevi tamamlamak için yönetici olmanız gerekir:

![Uygulama görüntüsünü karşıya yükleme](media/howto-prepare-images/uploadapplicationmanager.png)

Seçin **uygulama görüntüsü** kutucuğuna bir görüntüyü karşıya yükleme (268 x 160 piksel) yerel makinenizden.

### <a name="application-dashboard"></a>Uygulama panosu

Uygulama Panosu üzerinde bir görüntüyü karşıya yükleme için gidin **Pano** seçin ve uygulamanın sayfasını **Düzenle**. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Pano görüntüsünü karşıya yükleme](media/howto-prepare-images/uploadhomepage.png)

Altında **yapılandırma görüntü**seçin **görüntü** görüntüyü yerel makinenizden karşıya yüklenecek bir kutucuk. En küçük boyutlu 200 x 200 kutucuğudur piksel, daha büyük kutucukları küçük kutucuk kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 px, 400 x 200 piksel veya 400 x 400 px.

**Kaydet** karşıya yüklenen görüntüyü. Düzenleme modundayken yeniden boyutlandırabilirsiniz. Seçin **Bitti** bittiğinde.

### <a name="device-template"></a>Cihaz şablonu

Bir cihaz şablonu bir görüntüyü karşıya yükleme için gidin **cihaz şablonları** ve cihaz şablonu seçin. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Cihaz şablon görüntüsünü karşıya yükleme](media/howto-prepare-images/uploaddevicetemplate.png)

Bir görüntüyü karşıya yükleme için görüntü kutucuğunu seçin (64 x 64 piksel) yerel makinenizden.

### <a name="device-dashboard"></a>Cihaz panosu

Cihaz panosunda bir görüntüyü karşıya yükleme için gidin **cihaz şablonları** ve cihaz şablonu seçin. Ardından **Pano** sekmesi. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Cihaz Pano görüntüsünü karşıya yükleme](media/howto-prepare-images/uploaddevicedashboard.png)

Altında **yapılandırma görüntü**seçin **görüntü** kutucuğuna ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin. En küçük boyutlu 200 x 200 kutucuğudur piksel, daha büyük kutucukları küçük kutucuk kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 px, 400 x 200 piksel veya 400 x 400 px.

**Kaydet** karşıya yüklenen görüntüyü. Yeniden boyutlandırma ve düzenleme modunda yeniden konumlandırdığınız. Seçin **Bitti** bittiğinde.

### <a name="device-set-dashboard"></a>Cihaz Pano Ayarla

Bir cihaz kümesi Panoda bir görüntüyü karşıya yükleme için gidin **cihaz kümeleri** cihaz kümesi ve bir cihaz seçin. Ardından **Pano** sayfasından seçim yapıp **Düzenle**:

![Cihaz kümesi Panosu resmi karşıya yükle](media/howto-prepare-images/uploaddevicesetdashboard.png)

Altında **yapılandırma görüntü**seçin **görüntü** görüntüyü yerel makinenizden karşıya yüklenecek bir kutucuk. En küçük boyutlu 200 x 200 kutucuğudur piksel, daha büyük kutucukları küçük kutucuk kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 px, 400 x 200 piksel veya 400 x 400 px.

**Kaydet** karşıya yüklenen görüntüyü. Yeniden boyutlandırma ve düzenleme modunda yeniden konumlandırdığınız. Seçin **Bitti** bittiğinde.

## <a name="next-steps"></a>Sonraki adımlar

Hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme gerçekleştirmeyi öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Central kullanıcı Arabirimi özelleştirme](./howto-customize-ui.md)
* [Uygulama Panosu yapılandırın](./howto-configure-homepage.md)
* [Azure IOT Central uygulamanızdaki cihazları yönetme](howto-manage-devices.md)