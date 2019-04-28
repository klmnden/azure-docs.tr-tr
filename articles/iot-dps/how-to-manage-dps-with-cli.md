---
title: IOT Hub cihazı sağlama hizmeti yönetmek için Azure CLI ve IOT uzantısını kullanma | Microsoft Docs
description: IOT Hub cihazı sağlama hizmeti yönetmek için Azure CLI ve IOT uzantısını kullanmayı öğrenin
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 59d2277bd99fac1e8357c1b0d7336ca7451bf8dc
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122884"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmeti yönetmek için Azure CLI ve IOT uzantısını kullanma

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) çapraz platform komut satırı aracı IOT Edge gibi Azure kaynaklarını yönetmek için bir açık kaynak. Azure CLI'yı, Windows, Linux ve Macos'ta kullanılabilir. Azure CLI, Azure IOT hub'ı kaynakları, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenize olanak sağlar.

IOT uzantısı, Azure CLI cihaz yönetimi ve tam IOT Edge özelliği gibi özelliklerle zenginleştirir.

Bu öğreticide, ilk Azure CLI ve IOT uzantısını ayarlama adımlarını tamamlayın. Ardından temel cihaz sağlama hizmeti işlemlerini gerçekleştirmek için CLI komutlarını çalıştırma hakkında bilgi edinin. 

## <a name="installation"></a>Yükleme 

### <a name="step-1---install-python"></a>Adım 1 - Python yükleme

[Python 2.7x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="step-2---install-the-azure-cli"></a>2. adım - Azure CLI yükleme

İzleyin [yükleme yönergelerini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ortamınızda Azure CLI'yı ayarlamak için. En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az –version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir.

### <a name="step-3---install-iot-extension"></a>Adım 3 - IoT uzantısı yükleme

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-cli-iot-ext` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-cli-iot-ext` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-cli-iot-ext` kullanabilirsiniz.


## <a name="basic-device-provisioning-service-operations"></a>Temel cihaz sağlama hizmeti işlemleri
Örneğin, Azure hesabınızda oturum açın, bir Azure kaynak grubu (bir Azure çözümü için ilgili kaynakları tutan kapsayıcı) oluşturma, IOT Hub oluşturma, bir cihaz sağlama hizmeti oluşturun, mevcut cihaz sağlama hizmetlerini listeleme işlemini göstermektedir ve CLI komutları ile bağlı bir IOT hub oluşturun. 

Başlamdan önce daha önce açıklanan yükleme adımlarını tamamlayın. Henüz bir Azure hesabınız yoksa hemen [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabında oturum açın
  
    az login

![oturum açma][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Doğu ABD bölgesinde bir IoTHubBlogDemo kaynak grubu oluşturun

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma][2]


### <a name="3-create-two-device-provisioning-services"></a>3. İki cihaz sağlama hizmeti oluşturma

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Cihaz sağlama hizmeti oluşturma][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Tüm mevcut cihaz sağlama hizmetleri bu kaynak grubu altında listeleyin

    az iot dps list --resource-group IoTHubBlogDemo

![Liste cihaz sağlama hizmetleri][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Yeni oluşturulan kaynak grubu altında bir IoT Hub blogDemoHub oluşturun

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub oluşturun][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Var olan bir IOT Hub cihaz sağlama hizmetine bağlayın

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Hub’ı bağlayın][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihazı kaydedin
> * Cihazı başlatın
> * Cihaz kayıtlı olduğunu doğrulayın

Yük dengeli hublar arasında birden çok cihaz sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IoT hubları arasında cihaz sağlama](./tutorial-provision-multiple-hubs.md)
