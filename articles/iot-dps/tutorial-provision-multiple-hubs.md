---
title: Yük dengeli IoT Hub'larında cihazları sağlamak için Azure IoT Hub Cihazı Sağlama Hizmeti'ni kullanma | Microsoft Docs
description: Azure Portalı'nda yük dengeli IoT Hub'larında DPS otomatik cihaz sağlama
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: d0a3720fe729d5e260bbe5b0902460c8c7cfc7cb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629635"
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Yük dengeli IoT Hub'larında cihazları sağlama

Bu öğreticide, Cihaz Sağlama Hizmeti (DPS) kullanılarak birden çok yük dengeli IoT Hub'ı için cihazları sağlama işlemi gösterilir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama 
> * İkinci cihaza bir kayıt listesi girdisi ekleme
> * DPS ayırma ilkesi olarak **eşit dağılım**'ı ayarlama
> * Yeni IoT Hub'ını DPS'ye ayarlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, önceki [Hub'a cihaz sağlama](tutorial-provision-device-to-hub.md) öğreticisinin üzerine kurulmuştur.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama

Başka bir IoT Hub'ına ikinci cihazı sağlamak için, [Hub'a cihaz sağlama](tutorial-provision-device-to-hub.md) öğreticisindeki adımları izleyin.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>İkinci cihaza bir kayıt listesi girdisi ekleme

Kayıt listesi DPS'ye cihazla hangi kanıtlama yöntemini (cihazın kimliğini onaylama yöntemi) kullandığını bildirir. Sonraki adım, ikinci cihaz için bir kayıt listesi girdisi eklemektir. 

1. DPS'nizin sayfasında **Kayıtları yönetme**'ye tıklayın. **Kayıt listesi girdisi ekleme** sayfası görüntülenir. 
2. Sayfanın en üstündeki **Ekle**'ye tıklayın.
2. Alanları doldurun ve ardından **Kaydet**'e tıklayın.

## <a name="set-the-dps-allocation-policy"></a>DPS ayırma ilkesini ayarlama

Ayırma ilkesi, bir IoT Hub’ına cihazların nasıl atandığını belirleyen bir DPS ayarıdır. Desteklenen üç ayırma ilkesi vardır: 

1. **En düşük gecikme**: Cihaza yönelik en düşük gecikme ile hub’a dayalı bir IoT hub’a cihazlar sağlanabilir.
2. **Eşit ağırlıklı dağılım** (varsayılan): Bağlı IoT hub’lara cihaz sağlanma olasılığı eşittir. Bu varsayılan ayardır. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz. 
3. **Kayıt listesi aracılığıyla statik yapılandırma**: Kayıt listesindeki istenen IoT hub’ın belirtimi, DPS düzeyindeki ayırma ilkesinden önceliklidir.

Ayırma ilkesini ayarlamak için şu adımları izleyin:

1. Ayırma ilkesini ayarlamak için DPS sayfasında **Ayırma ilkesini yönetme** seçeneğine tıklayın.
2. Ayırma ilkesi olarak **Eşit ağırlıklı dağılım**'ı ayarlayın.
3. **Kaydet**’e tıklayın.

## <a name="link-the-new-iot-hub-to-dps"></a>Yeni IoT Hub'ını DPS'ye bağlama

DPS'nin cihazları bu hub'a kaydedebilmesi için DPS'yi IoT Hub'ına bağlayın.

1. **Tüm kaynaklar** sayfasında, daha önce oluşturduğunuz DPS'ye tıklayın.
2. DPS sayfasında **IoT hub'larına bağlanıldı** öğesine tıklayın.
3. **Ekle**'ye tıklayın.
4. **IoT hub'a bağlantı ekleme** sayfasında, bağlı IoT hub’ın geçerli abonelikte mi yoksa farklı bir abonelikte mi bulunduğunu belirtmek için radyo düğmelerini kullanın. Sonra **IoT hub** kutusunda IoT hub’ın adını seçin.
5. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama 
> * İkinci cihaza bir kayıt listesi girdisi ekleme
> * DPS ayırma ilkesi olarak **eşit dağılım**'ı ayarlama
> * Yeni IoT Hub'ını DPS'ye bağlama

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
