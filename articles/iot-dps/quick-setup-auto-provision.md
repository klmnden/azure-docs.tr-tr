---
title: Azure portalında Cihaz Sağlamayı ayarlama | Microsoft Belgeleri
description: Azure Hızlı Başlangıcı -Azure Portal'da Azure IoT Hub Cihazı Sağlama hizmetini ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 07/12/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: ce1586e472e1d1ea5ddd9ca5a426b1bea2b5b931
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42022575"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Azure portalı ile IoT Hub Cihazı Sağlama Hizmetini ayarlama

Bu adımlar, cihazlarınızı sağlamak için Azure bulut kaynaklarını ayarlamayı gösterir. Bu makalede şunlarla ilgili adımlar yer alır: IoT hub'ınızı oluşturma, yeni IoT Hub Cihazı Sağlama Hizmetini oluşturma ve iki hizmeti birbirine bağlama. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>IoT Hub’ı cihaz sağlama hizmeti için yeni bir örnek oluşturun

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Cihaz sağlama hizmeti** için *Mağazada arama yapın*. **IoT Hub Cihazı Sağlama Hizmeti**’ni seçin ve **Oluştur** düğmesine tıklayın. 

3. Yeni Cihaz Sağlama hizmeti örneğiniz için aşağıdaki bilgileri sağlayın ve **Oluştur**'a tıklayın.

    * **Ad:** Yeni Cihaz Sağlama hizmeti örneğiniz için benzersiz bir ad girin. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.
    * **Abonelik**: Bu Cihaz Sağlama hizmeti örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin.
    * **Kaynak grubu:** Bu alan yeni örneği içerecek yeni kaynak grubunu oluşturmanızı veya mevcut bir kaynak grubunu seçmenizi sağlar. Yukarıda oluşturduğunuz IoT hub'ını içeren kaynak grubunu seçin; örneğin, **TestResources**. İlgili tüm kaynakları aynı gruba birlikte koyarak, bunları birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-portal.md).
    * **Konum**: Cihazlarınıza en yakın konumu seçin.
    * **Panoya sabitle:** Örneği panonuza sabitleyip daha kolay bulabilmek için bu seçeneği belirtin.

    ![Portal dikey penceresinde Cihaz Sağlama hizmeti örneğiniz ile ilgili temel bilgileri girin](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Hizmet başarıyla dağıtıldıktan sonra özet dikey penceresi otomatik olarak açılır.


## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>IoT hub’ını ve Cihaz Sağlama Hizmetinizi bağlayın

Bu bölümde, Cihaz Sağlama Hizmeti örneğine yapılandırma ekleyeceksiniz. Bu yapılandırma, cihazların sağlanacağı IOT hub'ı ayarlar.

1. Azure portalının sol taraftaki menüsünden **Tüm kaynaklar** düğmesine tıklayın. Önceki bölümde oluşturduğunuz Cihaz Sağlama hizmeti örneğini seçin.  

2. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Bağlantılı IOT hub'ları**’nı seçin. Üstte görünen **+ Ekle** düğmesine tıklayın. 

3. **IoT hub'a bağlantı ekle** sayfasında, yeni Cihaz Sağlama hizmeti örneğinizi IoT hub'a bağlamak için aşağıdaki bilgileri sağlayın. Ardından **Kaydet**'e tıklayın. 

    * **Abonelik:** Yeni Cihaz Sağlama hizmeti örneğinizle bağlamak istediğiniz IoT hub'ı içeren aboneliği seçin.
    * **Iot hub'ı:** Yeni Cihaz Sağlama hizmeti örneğinize bağlanacak IoT hub'ı seçin.
    * **Erişim İlkesi:** IoT hub'ı ile bağlantı oluşturmak için kimlik bilgileri olarak **iothubowner** öğesini seçin.  

    ![Portal dikey penceresinde Cihaz Sağlama hizmeti örneğine bağlanmak için hub adını bağlayın](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Seçili hub’ı **Bağlantılı IoT hub'ları** dikey penceresinde görürsünüz. **Bağlı IoT hub’larını** görüntülemek için **Yenile**’ye tıklamanız gerekebilir.



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Sonraki Hızlı Başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu Hızlı Başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT hub'ını ve bir Cihaz Sağlama hizmeti örneği dağıttınız ve iki kaynağı birbirine bağladınız. Bu yöntemi bir sanal cihaz sağlamak üzere kullanmayı öğrenmek için sanal cihaz oluşturma Hızlı Başlangıcına geçin.

> [!div class="nextstepaction"]
> [Sanal cihaz oluşturmak için hızlı başlangıç](./quick-create-simulated-device.md)
