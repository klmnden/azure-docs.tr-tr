---
title: "Azure IOT Hub cihaz sağlama hizmeti için portalda Ayarla | Microsoft Docs"
description: "Azure Portalı'nda Otomatik cihaz IOT hub'ı sağlama"
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
ms.openlocfilehash: 088d127521ce89d3a82e30ad8797fe5746ae7e03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Cihaz IOT Hub cihaz sağlama hizmeti ile sağlama bulut kaynaklarını yapılandırma

Bu öğretici, bulut için otomatik cihaz IOT Hub cihaz sağlama hizmetini kullanarak sağlama ayarlama gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir IOT Hub cihaz sağlama hizmeti oluşturma ve kimliği kapsam almak için Azure portalını kullanın
> * IoT hub oluşturma
> * IOT hub cihaz sağlama hizmeti için bağlantı
> * Cihaz sağlama hizmette ayırma ilkesi ayarlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Cihaz sağlama hizmet örneği oluşturma ve kimliği kapsam Al

Yeni bir cihaz sağlama hizmet örneği oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalında sol üst köşesinde tıklatın **yeni**.
2. Arama kutusuna **cihaz sağlamayı**. 
3. Tıklatın **IOT Hub cihaz hizmet sağlama**.
4. Doldurmak **IOT Hub cihaz hizmeti sağlama** form aşağıdaki bilgilerle:
    
   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Ad** | Benzersiz bir ad | -- | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |   

   ![Portalda, dağıtım noktaları ile ilgili temel bilgileri girin](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. **Oluştur**'a tıklayın.
6. *Kimliği kapsam* kayıt kimlikleri tanımlamak için kullanılır ve kayıt kimliği benzersiz olduğunu garanti sağlar. Bu değeri elde etmek için tıklatın **genel bakış** açmak için **Essentials** aygıt hizmeti sağlama için sayfa. Kopya **kimliği kapsam** daha sonra kullanmak için geçici bir konuma değeri.
7. Ayrıca Not **Hizmeti uç noktası** değer veya daha sonra kullanmak için geçici bir konuma kopyalayın. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Aygıt hizmeti sağlama bir IOT hub'ına bağlama

Sonraki adım böylece IOT Hub cihaz hizmeti sağlama, hub'ına cihazlarını kaydedebilir aygıt hizmeti sağlama ve IOT hub bağlamaktır. Hizmet, yalnızca sağlama cihazı hizmetine bağlı IOT hub'ları cihazlara sağlayabilirsiniz. Aşağıdaki adımları izleyin.

1. İçinde **tüm kaynakları** sayfasında, daha önce oluşturduğunuz cihaz sağlama hizmet örneği'ı tıklatın.
2. Cihaz sağlama hizmeti sayfasında tıklatın **bağlantılı IOT hub'ları**.
3. **Ekle**'ye tıklayın.
4. İçinde **IOT hub'ına Bağlantı Ekle** sayfasında, bağlı IOT hub'ı geçerli abonelik ya da farklı bir abonelik bulunup bulunmadığını belirtmek için radyo düğmelerini kullanın. Ardından, IOT hub'ından adını seçin **IOT hub'ı** kutusu.
5. **Kaydet** düğmesine tıklayın.

   ![DPS portalında bağlamak için hub adı Bağla](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Cihaz sağlama hizmette ayırma ilkesi ayarlama

Ayırma ilkesi aygıtları bir IOT hub'ına nasıl atanacağını belirleyen bir IOT Hub cihaz hizmeti sağlama ayardır. Üç desteklenen ayırma ilkeleri vardır: 

1. **En düşük gecikme süresine**: cihazları cihaz için en düşük gecikme süresine sahip hub'ına bağlı bir IOT hub'ına sağlanan.
2. **Dağıtım'eşit ağırlıklı** (varsayılan): bağlantılı IOT hub'ları için sağlaması yapılan aygıtlar eşit şekilde etkileyebilir. Bu varsayılan ayardır. Yalnızca bir IOT hub'ına aygıtları sağlıyorsanız, bu ayarı tutabilirsiniz. 
3. **Kayıt listesi aracılığıyla statik Yapılandırması**: İstenen IOT hub'ı kayıt listesinde belirtimi cihaz sağlama hizmet düzeyi ayırma ilkesine göre öncelik alır.

Aygıt hizmeti sağlama sayfasını tıklatın ayırma ilkesini ayarlamak için **ayırma ilkesini yönetmek**. Emin olun ayırma ilkesi ayarlanmış **eşit, dağıtım ağırlıklı** (varsayılan). Herhangi bir değişiklik yaparsanız, tıklatın **kaydetmek** işiniz bittiğinde.

![Ayırma ilkesi yönetme](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer öğreticileri sonra Bu öğreticide oluşturun. Öğreticiler veya sonraki hızlı başlangıçlar ile çalışmaya devam etmek planlıyorsanız, temiz Bu öğreticide oluşturduğunuz kaynakları yukarı değil. Devam etmek düşünmüyorsanız Bu öğreticide Azure portal tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden **tüm kaynakları** ve ardından, IOT Hub cihaz sağlama hizmet örneği seçin. Üstündeki **tüm kaynakları** sayfasında, **silmek**.  
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. Üstündeki **tüm kaynakları** sayfasında, **silmek**.
 
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir IOT Hub cihaz sağlama hizmeti oluşturma ve kimliği kapsam almak için Azure portalını kullanın
> * IoT hub oluşturma
> * IOT hub cihaz sağlama hizmeti için bağlantı
> * Cihaz sağlama hizmette ayırma ilkesi ayarlama

Sağlama için Cihazınızı ayarlama konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sağlama için cihaz ayarlama](tutorial-set-up-device.md)
