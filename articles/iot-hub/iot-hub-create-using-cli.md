---
title: Azure CLI kullanarak IOT Hub oluşturma | Microsoft Docs
description: Azure CLI kullanarak bir Azure IOT hub oluşturma
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: robinsh
ms.openlocfilehash: 78ea9071f220b2a78c6d9260d47145f22284d760
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66166281"
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Azure CLI kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede Azure CLI kullanarak IOT hub oluşturma gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır tamamlamak için bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure CLI Cloud Shell'i kullanmak yerine yerel olarak çalıştırıyorsanız, Azure hesabınızda oturum açmanız.

Komut isteminde çalıştırın [oturum açma komutunu](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli):

   ```azurecli
   az login
   ```

Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

## <a name="create-an-iot-hub"></a>IoT Hub'ı oluşturma

Bir kaynak grubu oluşturun ve IOT hub'ı eklemek için Azure CLI'yı kullanın.

1. Bir IOT hub'ı oluşturduğunuzda, bir kaynak grubu oluşturmanız gerekir. Mevcut bir kaynak grubunu kullanın veya aşağıdaki komutu çalıştırın [bir kaynak grubu oluşturmak için komut](https://docs.microsoft.com/cli/azure/resource):
    
   ```azurecli
   az group create --name {your resource group name} --location westus
   ```

   > [!TIP]
   > Önceki örnekte kaynak grubu Batı ABD konumunda oluşturulur. Bu komutu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz: 
   >
   >``` bash
   >az account list-locations -o table
   >```
   >

2. Aşağıdaki komutu çalıştırın [bir IOT hub'ı oluşturmak için komut](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) kaynak grubunuzda, IOT hub'ınız için genel olarak benzersiz bir ad kullanarak:
    
   ```azurecli
   az iot hub create --name {your iot hub name} \
      --resource-group {your resource group name} --sku S1
   ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Önceki komut fiyatlandırma katmanı için faturalandırılırsınız bir IOT hub S1 içinde oluşturur. Daha fazla bilgi için [Azure IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="remove-an-iot-hub"></a>IOT hub'ı Kaldır

Azure CLI'yı kullanabilirsiniz [ayrı bir kaynağı silmek](https://docs.microsoft.com/cli/azure/resource), IOT hub'ı veya silme gibi bir kaynak grubunu ve tüm IOT hub'ları da dahil olmak üzere tüm kaynaklarını,.

İçin [bir IOT hub'ını Sil](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-delete), aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} -\
  -resource-group {your resource group name}
```

İçin [bir kaynak grubunu silme](https://docs.microsoft.com/cli/azure/group#az-group-delete) ve tüm alt kaynaklar için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [IOT hub'ı yönetmek için Azure portalını kullanarak](iot-hub-create-through-portal.md)
