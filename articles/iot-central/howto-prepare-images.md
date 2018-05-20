---
title: Azure IOT merkezi uygulamanıza resimler yükleyin | Microsoft Docs
description: Oluşturucu olarak hazırlamak ve görüntüleri Azure IOT merkezi uygulamanızı karşıya yükleme hakkında bilgi edinin.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: a43f2dd780604235ada1d8e3ae8a20563042fbaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Hazırlama ve Azure IOT merkezi uygulamanıza resimler yükleyin

Bu makalede, nasıl bir oluşturucu olarak Microsoft Azure IOT merkezi uygulamanızı özel resimler karşıya yükleyerek özelleştirebileceğiniz açıklanmaktadır. Örneğin, aygıt resmini içeren bir cihaz Pano özelleştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT merkezi bir uygulama. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
1. Ölçekleme ve resim dosyalarını yeniden boyutlandırma için bir araçtır.

## <a name="choose-where-to-use-custom-images"></a>Özel görüntüleri kullanmak konumu seçin

Azure IOT merkezi uygulamanın aşağıdaki konumlarda özel resimler ekleyebilirsiniz:

* **Uygulama Yöneticisi** sayfası

    ![Uygulama Yöneticisi sayfasında görüntüsü](media/howto-prepare-images/applicationmanager.png)

* Giriş sayfası

    ![Giriş sayfasında görüntüsü](media/howto-prepare-images/homepage.png)

* Bir cihaz şablonu

    ![Görüntüye cihaz şablonu](media/howto-prepare-images/devicetemplate.png)

* Bir aygıt Panoda bir kutucuğu

    ![Görüntüye cihaz döşeme](media/howto-prepare-images/devicetile.png)

* Bir cihaz kümesi Panoda bir kutucuğu

    ![Görüntüye cihaz kümesi döşeme](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Görüntüleri hazırlama

Tüm dört konumlarda PNG, GIF veya JPEG görüntüleri kullanabilirsiniz.

Aşağıdaki tabloda kullanabileceğiniz görüntü boyutları özetlenmektedir:

| Konum | Boyutlar |
| -------- | ------ |
| **Uygulama Yöneticisi** | 268 x 160 piksel |
| Cihaz şablonu | 64 x 64 piksel |
| Giriş sayfası ve Pano kutucukları | En küçük boyutlu döşeme 200 x 200 olan piksel, büyük döşeme küçük döşeme kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 piksel, 400 x 200 piksel veya 400 x 400 piksel |

Uygulamanın en iyi görüntülemek için önceki tabloda gösterilen boyutları eşleşen görüntüleri oluşturmanız gerekir.

## <a name="upload-the-images"></a>Resimler yükleyin

Aşağıdaki bölümlerde, farklı konumlarda kullanılacak görüntüleri karşıya açıklanmaktadır:

### <a name="application-manager"></a>Uygulama Yöneticisi

Kullanılacak görüntüyü karşıya yüklemek için **Uygulama Yöneticisi**, gitmek **uygulama ayarları** sayfasındaki **Yönetim** bölümü. Bu görevi tamamlamak için yönetici olmanız gerekir:

![Uygulama görüntüsü karşıya yükle](media/howto-prepare-images/uploadapplicationmanager.png)

Karşıya yükleme görüntüsünü tıklatın ve ardından yerel makinenizden karşıya yüklenecek dosyayı seçin.

### <a name="home-page"></a>Giriş sayfası

Giriş sayfasında kullanmak üzere bir görüntüyü karşıya yüklemek için gidin **giriş sayfası** uygulama ve anahtar tasarım modu. Bu görevi tamamlamak için bir oluşturucusu olmalıdır:

![Giriş sayfası görüntüsü karşıya yükle](media/howto-prepare-images/uploadhomepage.png)

Karşıya yükleme görüntüsünü tıklatın ve ardından yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntü gönderildikten sonra bu tasarım moduna geçiş sırasında boyutlandırabilirsiniz.

### <a name="device-template"></a>Cihaz şablonu

Bir aygıt şablonunda kullanılacak bir görüntü karşıya yüklemek için gidin **aygıt Explorer**cihaz şablonu ve ardından bir cihaz seçin ve tasarım moduna geç. Bu görevi tamamlamak için bir oluşturucusu olmalıdır:

![Cihaz şablon görüntüsü karşıya yükle](media/howto-prepare-images/uploaddevicetemplate.png)

Karşıya yükleme görüntüsünü tıklatın ve ardından yerel makinenizden karşıya yüklenecek dosyayı seçin.

### <a name="device-dashboard"></a>Cihaz Pano

Bir aygıt Panoda kullanmak üzere bir görüntüyü karşıya yüklemek için gidin **aygıt Explorer**, cihaz şablonu ve bir cihaz seçin. Ardından **Pano** sayfası ve anahtar Tasarım modunda. Bu görevi tamamlamak için bir oluşturucusu olmalıdır:

![Cihaz Pano görüntüsü karşıya yükle](media/howto-prepare-images/uploaddevicedashboard.png)

Karşıya yükleme görüntüsünü tıklatın ve ardından yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntü gönderildikten sonra yeniden boyutlandırma ve onu çalışırken yeniden konumlandırmak **Tasarım modunda** açık.

### <a name="device-set-dashboard"></a>Cihaz Pano ayarlama

Bir cihaz kümesi Panoda kullanmak üzere bir görüntüyü karşıya yüklemek için gidin **aygıt kümeleri** aygıt kümesi ve bir cihaz seçin. Ardından **Pano** sayfası ve anahtar **Tasarım modunda** üzerinde:

![Aygıt kümesi Pano resmini karşıya yükle](media/howto-prepare-images/uploaddevicesetdashboard.png)

Karşıya yükleme görüntüsünü tıklatın ve ardından yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntü gönderildikten sonra yeniden boyutlandırma ve tasarım moduna geçiş sırasında yeniden konumlandırmak.

## <a name="next-steps"></a>Sonraki adımlar

Hazırlama ve görüntüleri Azure IOT merkezi uygulamanızı karşıya yüklemek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Azure IOT merkezi uygulamanızı aygıtları yönetme](howto-manage-devices.md)