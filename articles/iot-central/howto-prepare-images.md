---
title: Görüntüleri karşıya yüklemek için Azure IOT Central uygulamasına | Microsoft Docs
description: Bir oluşturucu hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme hakkında bilgi edinin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 18c44a3d91a4964d054c8e142394da7d69772ed0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960710"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme

Bu makalede, nasıl bir oluşturucu, Microsoft Azure IOT Central uygulamasına özel görüntüleri karşıya yükleyerek özelleştirebileceğiniz açıklanmaktadır. Örneğin, cihazın resmi bir cihaz panosunu özelleştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT Central bir uygulamadır. Daha fazla bilgi için [bir uygulaması hızlı başlangıç oluşturma](quick-deploy-iot-central.md).
1. Ölçeklendirme ve resim dosyalarını yeniden boyutlandırma için bir araç.

## <a name="choose-where-to-use-custom-images"></a>Özel görüntüler kullanmak hedef konumu seçin

Bir Azure IOT Central uygulamasına aşağıdaki konumlarda özel görüntüleri ekleyebilirsiniz:

* **Uygulama Yöneticisi** sayfası

    ![Uygulama Yöneticisi sayfasında görüntüsü](media/howto-prepare-images/applicationmanager.png)

* Giriş sayfası

    ![Giriş sayfasında görüntüsü](media/howto-prepare-images/homepage.png)

* Bir cihaz şablonu

    ![Görüntüye cihaz şablonu](media/howto-prepare-images/devicetemplate.png)

* Bir cihaz Panoda bir kutucuğu

    ![Cihaz kutucuğundaki görüntüsü](media/howto-prepare-images/devicetile.png)

* Bir cihaz kümesi Panoda bir kutucuğu

    ![Cihaz kümesi kutucuğundaki görüntüsü](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Görüntüleri hazırlama

Tüm dört konumlarında PNG, GIF veya JPEG görüntülerini kullanabilirsiniz.

Aşağıdaki tabloda kullanabileceğiniz resim boyutları özetlenmektedir:

| Konum | Boyutlar |
| -------- | ------ |
| **Uygulama Yöneticisi** | 268 x 160 piksel |
| Cihaz şablonu | 64 x 64 piksel |
| Giriş sayfası ve Pano kutucukları | En küçük boyutlu 200 x 200 kutucuğudur piksel, daha büyük kutucukları küçük kutucuk kare veya dikdörtgen katları olabilir. Örneğin 200 x 400 px, 400 x 200 piksel veya 400 x 400 px |

Uygulamada en iyi görüntü için yukarıdaki tabloda gösterilen boyutları eşleşen görüntüleri oluşturmanız gerekir.

## <a name="upload-the-images"></a>Görüntüleri karşıya yükleme

Aşağıdaki bölümlerde farklı konumlarda kullanmak üzere istediğiniz görüntüyü karşıya yükleme açıklanmaktadır:

### <a name="application-manager"></a>Uygulama Yöneticisi

Üzerinde kullanılacak bir görüntü karşıya yüklemek için **Uygulama Yöneticisi**, gitmek **uygulama ayarları** sayfasını **Yönetim** bölümü. Bu görevi tamamlamak için yönetici olmanız gerekir:

![Uygulama görüntüsünü karşıya yükleme](media/howto-prepare-images/uploadapplicationmanager.png)

Karşıya yükleme görüntüsüne tıklayın ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.

### <a name="home-page"></a>Giriş sayfası

Giriş sayfasında kullanılacak bir görüntü karşıya yüklemek için gidin **giriş sayfası** uygulamanızın ve üzerinde Tasarım modunu değiştir. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Giriş sayfası görüntüsünü karşıya yükleme](media/howto-prepare-images/uploadhomepage.png)

Karşıya yükleme görüntüsüne tıklayın ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntüyü karşıya yükler, sonra Tasarım modu açık durumdayken yeniden boyutlandırabilirsiniz.

### <a name="device-template"></a>Cihaz şablonu

Bir cihaz şablonunda kullanılacak bir görüntü karşıya yüklemek için gidin **Device Explorer**, cihaz şablonu ve bir cihaz seçin ve tasarım moduna geçin. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Cihaz şablon görüntüsünü karşıya yükleme](media/howto-prepare-images/uploaddevicetemplate.png)

Karşıya yükleme görüntüsüne tıklayın ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.

### <a name="device-dashboard"></a>Cihaz panosu

Cihaz panosunda kullanılacak bir görüntü karşıya yüklemek için gidin **Device Explorer**, cihaz şablonu ve bir cihaz seçin. Ardından **Pano** sayfası ve anahtar tasarım modu. Bu görevi tamamlamak için bir oluşturucu olmalıdır:

![Cihaz Pano görüntüsünü karşıya yükleme](media/howto-prepare-images/uploaddevicedashboard.png)

Karşıya yükleme görüntüsüne tıklayın ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntü yükledikten sonra yeniden boyutlandırabilir ve sırasında yeniden konumlandırdığınız **tasarım modu** açık.

### <a name="device-set-dashboard"></a>Cihaz Pano Ayarla

Bir cihaz kümesi Panoda kullanılacak bir görüntü karşıya yüklemek için gidin **cihaz kümeleri** cihaz kümesi ve bir cihaz seçin. Ardından **Pano** sayfası ve anahtar **tasarım modu** üzerinde:

![Cihaz kümesi Panosu resmi karşıya yükle](media/howto-prepare-images/uploaddevicesetdashboard.png)

Karşıya yükleme görüntüsüne tıklayın ve sonra yerel makinenizden karşıya yüklenecek dosyayı seçin.

Görüntü yükledikten sonra yeniden boyutlandırabilir ve tasarım modu açık durumdayken yeniden konumlandırma.

## <a name="next-steps"></a>Sonraki adımlar

Hazırlama ve Azure IOT Central uygulamanıza görüntüleri karşıya yükleme gerçekleştirmeyi öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Azure IOT Central uygulamanızdaki cihazları yönetme](howto-manage-devices.md)