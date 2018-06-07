---
title: Azure CLI (az.py) kullanarak IOT Hub oluşturma | Microsoft Docs
description: Platformlar arası Azure CLI 2.0 (az.py) kullanan bir Azure IOT hub oluşturma
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 9f97775a5a49077a340efb0e3de14b7064db5fe4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632773"
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanan IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Giriş

Azure CLI 2.0 (az.py) oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu makalede Azure CLI 2.0 (az.py) IOT hub'ı oluşturmak için nasıl kullanılacağı gösterilmektedir.

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – Klasik ve kaynak yönetimi dağıtım modelleri için CLI.
* Azure CLI 2.0 (az.py) - Bu makalede anlatıldığı gibi kaynak yönetimi dağıtım modeli için yeni nesil CLI.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açma ve Azure hesabınızı ayarlama

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde [oturum açma komutunu][lnk-login-command] çalıştırın:
    
    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

2. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Kullanabileceğiniz [Azure hesaplarını listelemek için aşağıdaki komutu][lnk-az-account-command] kullanın:
    
    ```azurecli
    az account list 
    ```

    IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>IOT Hub oluşturma

Bir kaynak grubu oluşturun ve IOT hub'ı eklemek için Azure CLI kullanın.

1. IOT hub'ı oluşturduğunuzda, bir kaynak grubunda oluşturmanız gerekir. Mevcut bir kaynak grubunu kullanın veya [kaynak grubu oluşturmak için aşağıdaki komutu][lnk-az-resource-command] çalıştırabilirsiniz:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Önceki örnekte kaynak grubu Batı ABD konumunda oluşturulur. `az account list-locations -o table` komutunu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz.
    >
    >

2. Aşağıdaki komutu çalıştırarak [IOT hub'ı oluşturmak için komutu] [ lnk-az-iot-command] kaynak grubunuzdaki IOT hub'ınız için genel benzersiz bir ad kullanarak:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> Önceki komutu fiyatlandırma katmanı için faturalandırılır S1 bir IOT hub oluşturur. Daha fazla bilgi için bkz: [Azure IOT Hub fiyatlandırma][lnk-iot-pricing].
>

## <a name="remove-an-iot-hub"></a>IOT hub'ı kaldırma

Azure CLI için kullanabileceğiniz [tek başına bir kaynak silme][lnk-az-resource-command]bir IOT hub'ı veya silme gibi bir kaynak grubu ve tüm IOT hub'ları da dahil olmak üzere tüm kaynaklarını,.

Bir IoT hub’ını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Bir kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar
IOT Hub için geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT hub'ı yönetmek için Azure portalını kullanma][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
