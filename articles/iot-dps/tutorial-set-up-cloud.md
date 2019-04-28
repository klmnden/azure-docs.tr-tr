---
title: Portalda Azure IoT Hub Cihazı Sağlama Hizmeti için bulutu ayarlama | Microsoft Docs
description: Azure Portal’da IoT Hub otomatik cihaz sağlama
author: wesmc7777
ms.author: wesmc
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 8f06d3f033a2bf5907dc2ee324359bef0eb247d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953539"
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihazı Sağlama Hizmeti ile cihaz sağlama için bulut kaynaklarını yapılandırma

Bu öğretici, IoT Hub Cihazı Sağlama Hizmeti kullanılarak otomatik cihaz sağlama için bulutun nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IoT Hub Cihazı Sağlama Hizmeti oluşturmak ve kimlik kapsamını almak için Azure portalını kullanma
> * IoT hub oluşturma
> * IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama
> * Cihaz Sağlama Hizmeti’nde ayırma ilkesini ayarlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Cihaz Sağlama Hizmeti örneği oluşturma ve kimlik kapsamını alma

Yeni bir Cihaz Sağlama Hizmeti örneği oluşturmak için şu adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. Arama kutusuna **cihaz sağlama** yazın. 

3. **IoT Hub Cihazı Sağlama Hizmeti**’ne tıklayın.

4. **IoT Hub Cihazı Sağlama Hizmeti**’ni aşağıdaki bilgilerle doldurun:
    
   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Ad** | Herhangi bir benzersiz ad | -- | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |   

   ![Portalda Cihaz Sağlama hizmeti ile ilgili temel bilgileri girin](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. **Oluştur**’a tıklayın. Birkaç dakika sonra Cihaz Sağlama Hizmeti örneği oluşturulur ve **Genel bakış** sayfası görüntülenir.

6. Yeni hizmet örneğinin **Genel bakış** sayfasındaki **Kimlik kapsamı** değerini daha sonra kullanmak üzere kopyalayın. Bu değer, kayıt kimliklerini belirlemek için kullanılır ve kayıt kimliğinin benzersiz olduğuna dair bir garanti sağlar.

7. **Hizmet uç noktası** değerini de daha sonra kullanmak üzere kopyalayın. 

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Cihaz Sağlama Hizmeti’ni bir IoT hub’a bağlama

Sonraki adım, IoT Hub Cihazı Sağlama Hizmeti’nin cihazları söz konusu hub’a kaydedebilmesi için Cihaz Sağlama Hizmeti ile IoT hub’ın bağlanmasıdır. Hizmet yalnızca Cihaz Sağlama Hizmeti’ne bağlanmış olan IoT hub’lara cihazları sağlayabilir. Şu adımları izleyin.

1. **Tüm kaynaklar** sayfasında, önceden oluşturduğunuz Cihaz Sağlama Hizmeti örneğine tıklayın.

2. Cihaz Sağlama Hizmeti sayfasında **Bağlı IoT hub’lar** seçeneğine tıklayın.

3. **Ekle**'ye tıklayın.

4. **IoT hub'ına bağlantı ekleme** sayfasına aşağıdaki bilgileri girin ve **Kaydet**'e tıklayın:

    * **Abonelik:** IOT hub'ı içeren aboneliği seçili olduğundan emin olun. Farklı bir abonelikte bulunan IoT hub'a bağlantı verebilirsiniz.

    * **IOT hub:** Bu cihaz sağlama hizmeti örneğine bağlamak istediğiniz IOT hub'ı adını seçin.

    * **Erişim İlkesi:** Seçin **iothubowner** IOT hub'ına bağlantı kurmak için kullanılacak kimlik bilgilerini olarak.

   ![Portalda Cihaz Sağlama Hizmeti'ne bağlanmak için hub adını bağlayın](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Cihaz Sağlama Hizmeti’nde ayırma ilkesini ayarlama

Ayırma ilkesini cihazların bir IOT hub'ına nasıl atandığını belirleyen bir IOT Hub cihazı sağlama hizmeti ayarıdır. Desteklenen üç ayırma ilkesi vardır: 

1. **En düşük gecikme**: Cihazlar hub'ında cihaz için en düşük gecikme ile temel bir IOT hub sağlanır.

2. **Eşit ağırlıklı dağılım** (varsayılan): Bağlı IOT hub'lara cihaz sağlanma olasılığı. Bu varsayılan ayardır. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz. 

3. **Kayıt listesi aracılığıyla statik yapılandırma**: Kayıt listesindeki istenen IOT hub'ın belirtimi, cihaz sağlama hizmeti düzeyindeki ayırma ilkesinden önceliklidir önceliklidir.

Ayırma ilkesini ayarlamak için Cihaz Sağlama Hizmeti sayfasında **Ayırma ilkesini yönetme** seçeneğine tıklayın. Ayırma ilkesinin **Eşit ağırlıklı dağılım** (varsayılan) olarak ayarlandığından emin olun. Herhangi bir değişiklik yaparsanız işiniz bittiğinde **Kaydet**’e tıklayın.

![Ayırma ilkesini yönetme](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer öğreticiler, bu öğreticiyi temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu öğreticide oluşturulan kaynakları silmeyin. Devam etmeyi planlamıyorsanız Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra IoT Hub Cihazı Sağlama Hizmeti örneğinizi seçin. **Tüm kaynaklar** sayfasının en üstündeki **Sil** seçeneğine tıklayın.  

2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** sayfasının en üstündeki **Sil** seçeneğine tıklayın.
 
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * IoT Hub Cihazı Sağlama Hizmeti oluşturmak ve kimlik kapsamını almak için Azure portalını kullanma
> * IoT hub oluşturma
> * IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama
> * Cihaz Sağlama Hizmeti’nde ayırma ilkesini ayarlama

Sağlama için cihazınızın nasıl ayarlanacağını öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sağlama için cihazı ayarlama](tutorial-set-up-device.md)
