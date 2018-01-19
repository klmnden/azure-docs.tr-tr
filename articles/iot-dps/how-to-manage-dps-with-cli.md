---
title: "Azure CLI 2.0 ve IOT uzantısı hizmetleri sağlama cihazı yönetmek için nasıl kullanılacağını | Microsoft Docs"
description: "Cihaz sağlama hizmetleri yönetmek için Azure CLI 2.0 ve IOT uzantısı kullanmayı öğrenin"
services: iot-dps
keywords: 
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 674245f1e284e7308474fed0f6c53b350ec1c819
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Cihaz hizmetleri sağlama yönetmek için Azure CLI 2.0 ve IOT uzantısı kullanma

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) çapraz platform komut satırı aracı IOT kenar gibi Azure kaynakları yönetmek için bir açık kaynak değil. Azure CLI 2.0, Windows, Linux ve MacOS kullanılabilir. Azure CLI 2.0 Azure IOT Hub kaynakları, cihaz sağlama hizmet örneği ve kutunun dışında bağlantılı-hub yönetmenizi sağlar.

IOT uzantısı aygıt yönetimi ve tam IOT kenar özelliği gibi özellikler ile Azure CLI 2.0 aşağıdakilere zenginleştirir.

Bu öğreticide, ilk Azure CLI 2.0 ve IOT uzantısı kurulum adımlarını tamamlayın. Daha sonra temel cihaz hazırlama hizmet işlemleri gerçekleştirmek için CLI komutların nasıl çalıştırıldığını öğrenin. 

## <a name="installation"></a>Yükleme 

### <a name="step-1---install-python"></a>1. adım - yükleme Python

[Python 2.7 x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="step-2---install-azure-cli-20"></a>Adım 2 - Azure CLI 2.0 yükleyin

İzleyin [yükleme yönergesindeki](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI 2.0 ortamınızda kurulumu. En azından, Azure CLI 2.0 sürümünüzün 2.0.24 olması gerekir veya üstü. Kullanım `az –version` doğrulamak için. Bu sürüm az uzantısını komutları destekler ve Knack komut çerçevesi sunar. Windows yüklemek için bir basit yoludur indirmek ve yüklemek için [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>3. adım - yükleme IOT uzantısı

[IOT uzantısı Benioku](https://github.com/Azure/azure-iot-cli-extension) uzantıyı yüklemek için çeşitli yollar açıklanmaktadır. En basit yolu çalıştırmaktır `az extension add --name azure-cli-iot-ext`. Yükleme sonrasında, kullandığınız `az extension list` yüklü uzantıları doğrulamak için veya `az extension show --name azure-cli-iot-ext` IOT uzantısı ayrıntılarını görmek için. Uzantıyı kaldırmak için kullanabileceğiniz `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Temel cihaz hazırlama hizmet işlemleri
Örnek, Azure hesabınızda oturum açın, Azure kaynak grubu (Azure çözümünü ilgili kaynaklara tutan bir kapsayıcı) oluşturma, IOT Hub oluşturma, bir cihaz servie sağlama oluşturma, var olan cihaz hizmetleri sağlama listesinde gösterir ve bağlantılı bir IOT hub CLI komutları ile oluşturun. 

Başlamadan önce daha önce açıklanan yükleme adımlarını tamamlayın. Bir Azure hesabınız yoksa henüz yapabilecekleriniz, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/?v=17.39a) bugün. 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabında oturum açma
  
    az login

![oturum açma][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Bir kaynak grubu IoTHubBlogDemo içinde eastus oluşturun

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma][2]


### <a name="3-create-two-device-provisioning-services"></a>3. İki cihaz sağlama hizmetleri oluşturma

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Dağıtım noktaları oluşturma][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Bu kaynak grubu altında hizmetleri sağlama varolan cihaz listesi

    az iot dps list --resource-group IoTHubBlogDemo

![Dağıtım noktaları listesi][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Yeni oluşturulmuş bir kaynak grubu altında bir IOT hub'ı blogDemoHub oluşturma

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IOT Hub oluşturma][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Hizmet sağlama bir cihaza bir var olan IOT Hub bağlantı

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Bağlantı Hub][5]

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
> * Cihaz kaydı
> * Cihaz Başlat
> * Cihaz kayıtlı doğrulayın

Yük dengeli hubs arasında birden çok aygıt sağlama konusunda bilgi almak için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IOT hub'ları aygıtlarda sağlama](./tutorial-provision-multiple-hubs.md)
