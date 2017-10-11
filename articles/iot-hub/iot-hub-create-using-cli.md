---
title: "Azure CLI (az.py) kullanarak IOT Hub oluşturma | Microsoft Docs"
description: "Platformlar arası Azure CLI 2.0 (az.py) kullanan bir Azure IOT hub oluşturma"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 161089159999a4a63a39b059e69a08b7a9297445
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
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

1. Komut istemine [oturum açma komut][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Kod kullanarak kimlik doğrulaması yapmak için yönergeleri izleyin ve bir web tarayıcısı aracılığıyla Azure hesabınızda oturum açın.

2. Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgilerinizle ilişkilendirilen tüm Azure hesaplar için size erişim verir. Aşağıdaki [Azure hesapları listelemek için komut] [ lnk-az-account-command] kullanmanız için kullanılabilir:
    
    ```azurecli
    az account list 
    ```

    IOT hub'ınızı oluşturması için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komut çıktısı abonelik adı veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>IOT Hub oluşturma

Bir kaynak grubu oluşturun ve IOT hub'ı eklemek için Azure CLI kullanın.

1. IOT hub'ı oluşturduğunuzda, bir kaynak grubunda oluşturmanız gerekir. Varolan bir kaynak grubunu kullanın veya aşağıdaki komutu çalıştırarak [bir kaynak grubu oluşturmak için komutu][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Önceki örnekte Batı ABD konumunda kaynak grubu oluşturur. Komutunu çalıştırarak kullanılabilir konumların bir listesini görüntüleyebilirsiniz `az account list-locations -o table`.
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
>

## <a name="remove-an-iot-hub"></a>IOT hub'ı kaldırma

Azure CLI için kullanabileceğiniz [tek başına bir kaynak silme][lnk-az-resource-command]bir IOT hub'ı veya silme gibi bir kaynak grubu ve tüm IOT hub'ları da dahil olmak üzere tüm kaynaklarını,.

Bir IOT hub'ını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Bir kaynak grubu ve tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

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
