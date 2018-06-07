---
title: Cihaz sağlama hizmetlerini yönetmek için Azure CLI 2.0 ve IoT uzantısını kullanma | Microsoft Docs
description: Cihaz sağlama hizmetlerini yönetmek için Azure CLI 2.0 ve IoT uzantısını kullanma hakkında bilgi edinin
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 174f8447b17d1fa580472cbb45d0a72f41c793c3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628326"
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Cihaz sağlama hizmetlerini yönetmek için Azure CLI 2.0 ve IoT uzantısını kullanma

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLI 2.0 Windows, Linux ve Mac OS ile kullanılabilir. Azure CLI 2.0’ı kullanarak Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub’ları hemen yönetmeye başlayabilirsiniz.

IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu öğreticide ilk olarak Azure CLI 2.0 ve IoT uzantısını ayarlama adımlarını tamamlayacaksınız. Daha sonra, temel cihaz sağlama hizmeti işlemlerini gerçekleştirmek için CLI komutlarını çalıştırma hakkında bilgi edineceksiniz. 

## <a name="installation"></a>Yükleme 

### <a name="step-1---install-python"></a>Adım 1 - Python yükleme

[Python 2.7x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="step-2---install-azure-cli-20"></a>Adım 2 - Azure CLI 2.0 yükleme

[Yükleme yönergelerini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyerek Azure CLI 2.0’ı ortamınızda ayarlayın. Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir.

### <a name="step-3---install-iot-extension"></a>Adım 3 - IoT uzantısı yükleme

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-cli-iot-ext` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-cli-iot-ext` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-cli-iot-ext` kullanabilirsiniz.


## <a name="basic-device-provisioning-service-operations"></a>Temel cihaz sağlama hizmeti işlemleri
Örnekte Azure hesabınızda oturum açma, bir Azure Kaynak Grubu (bir Azure çözümü için ilgili kaynakları tutan kapsayıcı) oluşturma, bir IoT Hub oluşturma, bir cihaz sağlama hizmeti oluşturma, mevcut cihaz sağlama hizmetlerini listeleme ve CLI komutları ile bağlı bir IoT hub oluşturma işlemleri gösterilmiştir. 

Başlamdan önce daha önce açıklanan yükleme adımlarını tamamlayın. Henüz bir Azure hesabınız yoksa hemen [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabında oturum açın
  
    az login

![oturum açma][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Doğu ABD bölgesinde bir IoTHubBlogDemo kaynak grubu oluşturun

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma][2]


### <a name="3-create-two-device-provisioning-services"></a>3. İki cihaz sağlama hizmeti oluşturun

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![DPS oluşturun][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Bu kaynak grubu altındaki tüm mevcut cihaz sağlama hizmetlerini listeleyin

    az iot dps list --resource-group IoTHubBlogDemo

![DPS’yi listeleyin][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Yeni oluşturulan kaynak grubu altında bir IoT Hub blogDemoHub oluşturun

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub oluşturun][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Mevcut IoT Hub’ı bir cihaz sağlama hizmetine bağlayın

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
