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
ms.openlocfilehash: 0b16c6da0066ac4e919c1bef031d3206a359aae6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126469"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Azure portalı ile IoT Hub Cihazı Sağlama Hizmetini ayarlama

Bu adımlar, cihazlarınızı sağlamak için Azure bulut kaynaklarını ayarlamayı gösterir. Bu makalede şunlarla ilgili adımlar yer alır: IoT hub'ınızı oluşturma, yeni IoT Hub Cihazı Sağlama Hizmetini oluşturma ve iki hizmeti birbirine bağlama. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>IoT Hub’ı cihaz sağlama hizmeti için yeni bir örnek oluşturun

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Cihaz sağlama hizmeti** için *Mağazada arama yapın*. **IoT Hub Cihazı Sağlama Hizmeti**’ni seçin ve **Oluştur** düğmesine tıklayın. 

3. Yeni Cihaz Sağlama hizmeti örneğiniz için aşağıdaki bilgileri sağlayın ve **Oluştur**'a tıklayın.

    * **Adı:** Yeni cihaz sağlama hizmeti örneği için benzersiz bir ad sağlayın. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.
    * **Abonelik:** Bu cihaz sağlama hizmeti örneği oluşturmak için kullanmak istediğiniz aboneliği seçin.
    * **Kaynak grubu:** Bu alanı, yeni bir kaynak grubu oluşturun veya mevcut bir yeni örneği içeren sağlar. Yukarıda oluşturduğunuz IoT hub'ını içeren kaynak grubunu seçin; örneğin, **TestResources**. İlgili tüm kaynakları aynı gruba birlikte koyarak, bunları birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir. Daha fazla bilgi için [yönetme Azure Resource Manager kaynak grupları](../azure-resource-manager/manage-resource-groups-portal.md).
    * **Konum:** Cihazlarınıza en yakın konumu seçin.

      ![Portal dikey penceresinde Cihaz Sağlama hizmeti örneğiniz ile ilgili temel bilgileri girin](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Kaynak örneğinin oluşturma ilerlemesini izlemek için bildirim düğmesine tıklayın. Hizmet başarıyla dağıtıldıktan sonra **Panoya sabitle**'ye ve **Kaynağa git**'e tıklayın.

    ![Dağıtımı izleme bildirimi](./media/quick-setup-auto-provision/pin-to-dashboard.png)

## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>IoT hub’ını ve Cihaz Sağlama Hizmetinizi bağlayın

Bu bölümde, Cihaz Sağlama Hizmeti örneğine yapılandırma ekleyeceksiniz. Bu yapılandırma, cihazların sağlanacağı IOT hub'ı ayarlar.

1. Azure portalının sol taraftaki menüsünden **Tüm kaynaklar** düğmesine tıklayın. Önceki bölümde oluşturduğunuz Cihaz Sağlama hizmeti örneğini seçin.  

2. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Bağlantılı IOT hub'ları**’nı seçin. Üstte görünen **+ Ekle** düğmesine tıklayın. 

3. **IoT hub'a bağlantı ekle** sayfasında, yeni Cihaz Sağlama hizmeti örneğinizi IoT hub'a bağlamak için aşağıdaki bilgileri sağlayın. Ardından **Kaydet**'e tıklayın. 

    * **Abonelik:** Yeni cihaz sağlama hizmeti Örneğinize bağlanmak istediğiniz IOT hub'ı içeren aboneliği seçin.
    * **IOT hub:** Yeni cihaz sağlama hizmeti Örneğinize bağlanmak için IOT hub'ı seçin.
    * **Erişim İlkesi:** Seçin **iothubowner** olarak IOT hub ile bağlantı kurmak için kimlik bilgileri.  

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
