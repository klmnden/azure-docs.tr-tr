---
title: Azure portalında Cihaz Sağlamayı ayarlama | Microsoft Belgeleri
description: Azure Hızlı Başlangıcı -Azure Portal'da Azure IoT Hub Cihazı Sağlama hizmetini ayarlama
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 276bd33f5724db4d67da0cc31b16297915c9a417
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629448"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Azure portalı ile IoT Hub Cihazı Sağlama Hizmetini ayarlama

Bu adımlar, cihazlarınızı sağlamak için Azure bulut kaynaklarını ayarlamayı gösterir. Bu, IoT hub'ınızı oluşturma, yeni IoT Hub Cihazı Sağlama Hizmetini oluşturma ve iki hizmeti birbirine bağlamayı içerir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Nesnelerin İnterneti**’ni seçin, **IoT Hub**’ı seçin ve **Oluştur** düğmesine tıklayın. 

3. IoT Hub’ınıza **Ad** verin. Fiyatlandırma için kullanılabilir seçenekler arasından seçim yapın, [IoT Hub birimlerine](https://azure.microsoft.com/pricing/details/iot-hub/) girin, cihazdan buluta iletilerin bölüm sayısını seçin ve bu kaynak için kullanılacak aboneliği seçin. Yeni veya var olan kaynak grubunun adını girin ve konumu seçin. Tamamlandığında **Oluştur**’a tıklayın.

    ![Portal dikey penceresinde IoT hub’ınız ile ilgili temel bilgileri girin](./media/quick-setup-auto-provision/create-iot-hub-portal.png)  

4. IOT hub'ı başarıyla dağıtıldıktan sonra hub özeti dikey penceresi otomatik olarak açılır.


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>IoT Hub’ı cihaz sağlama hizmeti için yeni bir örnek oluşturun

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Cihaz sağlama hizmeti** için *Mağazada arama yapın*. **IoT Hub Cihazı Sağlama Hizmeti**’ni seçin ve **Oluştur** düğmesine tıklayın. 

3. Cihaz sağlama hizmet örneğinize bir **Ad** verin. Bu örnek için kullanılacak aboneliği seçin, ardından yeni veya mevcut bir kaynak grubunun adını girin. Konumu seçin. Tamamlandığında **Oluştur**’a tıklayın.

    ![Portal dikey penceresinde DPS örneğiniz ile ilgili temel bilgileri girin](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Hizmet başarıyla dağıtıldıktan sonra özet dikey penceresi otomatik olarak açılır.


## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>IoT hub’ını ve Cihaz Sağlama Hizmetinizi bağlayın

1. Azure portalının sol taraftaki menüsünden **Tüm kaynaklar** düğmesine tıklayın. Önceki bölümde oluşturduğunuz Cihaz Sağlama Hizmeti örneğini seçin.  

2. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Bağlantılı IOT hub'ları**’nı seçin. Üstte görünen **+ Ekle** düğmesine tıklayın. 

3. **IoT hub'ına bağlantı ekle** portal dikey penceresinde, geçerli aboneliği seçin veya başka bir abonelik için adı ve bağlantı dizesini girin. Açılan listeden hub adını seçin. İşlem tamamlandığında **Kaydet**’e tıklayın. 

    ![Portal dikey penceresinde DPS örneğine bağlanmak için hub adını bağlayın](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Seçili hub’ı **Bağlantılı IoT hub'ları** dikey penceresinde görürsünüz. **Bağlı IoT hub’larını** görüntülemek için **Yenile**’ye tıklamanız gerekebilir.



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Sonraki Hızlı Başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu Hızlı Başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT hub'ını ve bir Cihaz Sağlama Hizmeti örneği dağıttınız ve iki kaynağı birbirine bağladınız. Bu yöntemi bir sanal cihaz sağlamak üzere kullanmayı öğrenmek için sanal cihaz oluşturma Hızlı Başlangıcına geçin.

> [!div class="nextstepaction"]
> [Sanal cihaz oluşturmak için hızlı başlangıç](./quick-create-simulated-device.md)
