---
title: "Kullanım Azure IOT Hub cihaz sağlama hizmeti yük arasında cihazları sağlamak için IOT hub'ları dengeli | Microsoft Docs"
description: "IOT hub'ları Azure Portalı'nda dengeli dağıtım noktaları arasında yük otomatik cihaz sağlama"
services: iot-dps
keywords: 
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4842944cd0d980fb7e817165da23b9c3c4037e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Yük dengeli IOT hub'ları aygıtlarda sağlama

Bu öğretici için birden çok, cihazlara sağlamak nasıl gösterir cihaz sağlama hizmeti (DPS) kullanarak yük dengeli IOT hub'ları. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İkinci bir IOT hub'ına ikinci bir cihaz sağlamak için Azure portalını kullanma 
> * İkinci bir cihaz için bir kayıt listesi Girişi Ekle
> * DPS ayırma ilkesini ayarlamak **bile dağıtım**
> * Yeni IOT hub'ı dağıtım noktaları için bağlantı

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin önceki üzerinde derlemeler [sağlama aygıt hub'ına](tutorial-provision-device-to-hub.md) Öğreticisi.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>İkinci bir IOT hub'ına ikinci bir cihaz sağlamak için Azure portalını kullanma

Adımları [sağlama aygıt hub'ına](tutorial-provision-device-to-hub.md) ikinci bir cihaz başka bir IOT hub'ına sağlayacak öğretici.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>İkinci bir cihaz için bir kayıt listesi Girişi Ekle

Kayıt listesi, hangi yöntemi kanıt (cihaz kimliğini onaylayan yöntemi) aygıtıyla kullanıyor DPS söyler. Sonraki adım, ikinci bir cihaz için bir kayıt listesi giriş eklemektir. 

1. Dağıtım noktaları sayfasında tıklatın **kayıtlarını yönetme**. **Kayıt liste girdisi eklemek** sayfası görüntülenir. 
2. Sayfanın üstündeki **Ekle**.
2. Alanları doldurun ve ardından **kaydetmek**.

## <a name="set-the-dps-allocation-policy"></a>DPS ayırma ilkesi ayarlama

Ayırma ilkesi aygıtları bir IOT hub'ına nasıl atanacağını belirleyen DPS bir ayardır. Üç desteklenen ayırma ilkeleri vardır: 

1. **En düşük gecikme süresine**: cihazları cihaz için en düşük gecikme süresine sahip hub'ına bağlı bir IOT hub'ına sağlanan.
2. **Dağıtım'eşit ağırlıklı** (varsayılan): bağlantılı IOT hub'ları için sağlaması yapılan aygıtlar eşit şekilde etkileyebilir. Bu varsayılan ayardır. Yalnızca bir IOT hub'ına aygıtları sağlıyorsanız, bu ayarı tutabilirsiniz. 
3. **Kayıt listesi aracılığıyla statik Yapılandırması**: İstenen IOT hub'ı kayıt listesinde belirtimi DPS düzeyi ayırma ilkesine göre öncelik alır.

Ayırma İlkesi ayarlamak için aşağıdaki adımları izleyin:

1. Ayırma ilkesini ayarlamak için dağıtım noktaları sayfasında tıklayın **ayırma ilkesini yönetmek**.
2. Ayırma ilkesini ayarlamak **eşit, dağıtım ağırlıklı**.
3. **Kaydet** düğmesine tıklayın.

## <a name="link-the-new-iot-hub-to-dps"></a>Yeni IOT hub'ı dağıtım noktaları için bağlantı

Dağıtım noktaları, hub'ına cihazlarını kaydedebilir DPS ve IOT hub bağlayın.

1. İçinde **tüm kaynakları** sayfasında, daha önce oluşturduğunuz dağıtım noktaları'ı tıklatın.
2. Dağıtım noktaları sayfasında tıklatın **bağlantılı IOT hub'ları**.
3. **Ekle**'ye tıklayın.
4. İçinde **IOT hub'ına Bağlantı Ekle** sayfasında, bağlı IOT hub'ı geçerli abonelik ya da farklı bir abonelik bulunup bulunmadığını belirtmek için radyo düğmelerini kullanın. Ardından, IOT hub'ından adını seçin **IOT hub'ı** kutusu.
5. **Kaydet** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İkinci bir IOT hub'ına ikinci bir cihaz sağlamak için Azure portalını kullanma 
> * İkinci bir cihaz için bir kayıt listesi Girişi Ekle
> * DPS ayırma ilkesini ayarlamak **bile dağıtım**
> * Yeni IOT hub'ı dağıtım noktaları için bağlantı

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
