---
title: Portalda Azure IoT Hub Cihazı Sağlama Hizmeti için bulutu ayarlama | Microsoft Docs
description: Azure Portal’da IoT Hub otomatik cihaz sağlama
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e334ff0c8dec3a9611b60f64e565111064d10c18
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38619291"
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihazı Sağlama Hizmeti ile cihaz sağlama için bulut kaynaklarını yapılandırma

Bu öğretici, IoT Hub Cihazı Sağlama Hizmeti kullanılarak otomatik cihaz sağlama için bulutun nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IoT Hub Cihazı Sağlama Hizmeti oluşturmak ve kimlik kapsamını almak için Azure portalını kullanma
> * IoT hub oluşturma
> * IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama
> * Cihaz Sağlama Hizmeti’nde ayırma ilkesini ayarlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

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

   ![Portala DPS’niz ile ilgili temel bilgileri girin](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. **Oluştur**’a tıklayın.
6. *Kimlik kapsamı*, kayıt kimliklerini belirlemek için kullanılır ve kayıt kimliğinin benzersiz olduğuna dair bir garanti sağlar. Bu değeri almak için **Genel Bakış** seçeneğine tıklayıp Cihaz Sağlama Hizmeti’nin **Temel Parçalar** sayfasını açın. **Kimlik Kapsamı** değerini daha sonra kullanmak üzere geçici bir konuma kopyalayın.
7. Ayrıca **Hizmet uç noktası** değerini daha sonra kullanmak üzere not alın veya geçici bir konuma kopyalayın. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Cihaz Sağlama Hizmeti’ni bir IoT hub’a bağlama

Sonraki adım, IoT Hub Cihazı Sağlama Hizmeti’nin cihazları söz konusu hub’a kaydedebilmesi için Cihaz Sağlama Hizmeti ile IoT hub’ın bağlanmasıdır. Hizmet yalnızca Cihaz Sağlama Hizmeti’ne bağlanmış olan IoT hub’lara cihazları sağlayabilir. Şu adımları izleyin.

1. **Tüm kaynaklar** sayfasında, önceden oluşturduğunuz Cihaz Sağlama Hizmeti örneğine tıklayın.
2. Cihaz Sağlama Hizmeti sayfasında **Bağlı IoT hub’lar** seçeneğine tıklayın.
3. **Ekle**'ye tıklayın.
4. **IoT hub'a bağlantı ekleme** sayfasında, bağlı IoT hub’ın geçerli abonelikte mi yoksa farklı bir abonelikte mi bulunduğunu belirtmek için radyo düğmelerini kullanın. Sonra **IoT hub** kutusunda IoT hub’ın adını seçin.
5. **Kaydet**’e tıklayın.

   ![Hub adını portaldaki DPS’ye bağlama](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Cihaz Sağlama Hizmeti’nde ayırma ilkesini ayarlama

Ayırma ilkesi, bir IoT hub’a cihazların nasıl atandığını belirleyen bir IoT Hub Cihazı Sağlama Hizmeti ayarıdır. Desteklenen üç ayırma ilkesi vardır: 

1. **En düşük gecikme**: Cihaza yönelik en düşük gecikme ile hub’a dayalı bir IoT hub’a cihazlar sağlanabilir.
2. **Eşit ağırlıklı dağılım** (varsayılan): Bağlı IoT hub’lara cihaz sağlanma olasılığı eşittir. Bu varsayılan ayardır. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz. 
3. **Kayıt listesi aracılığıyla statik yapılandırma**: Kayıt listesindeki istenen IoT hub’ın belirtimi, Cihaz Sağlama Hizmeti düzeyindeki ayırma ilkesinden önceliklidir.

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
