---
title: Azure CLI'dan IOT Central'ı yönetme | Microsoft Docs
description: IOT Central, Azure CLI üzerinden yönetin.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/07/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 9e5d842cece316bc9c53e1e8583f40a0f222b91d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66151952"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLI'dan IOT Central'ı yönetme

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

Oluşturma ve IOT Central'dan IOT Central uygulamaları yönetmek yerine [Uygulama Yöneticisi](https://aka.ms/iotcentral) kullanabileceğiniz sayfasında [Azure CLI](/cli/azure/) uygulamalarınızı yönetmek için.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yı yerel makinenizde çalıştırmak isterseniz, bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yı yerel olarak çalıştırdığınızda kullanmak **az login** komutu, bu makaledeki komutları çalışmadan önce Azure'da oturum açın.

## <a name="create-an-application"></a>Uygulama oluşturma

Kullanım [az iotcentral uygulaması oluşturma](/cli/azure/iotcentral/app#az-iotcentral-app-create) Azure aboneliğinizde bir IOT Central uygulaması oluşturmak için komutu. Örneğin:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku S1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

Bu komutlar, ilk Doğu ABD bölgesinde uygulama için bir kaynak grubu oluşturun. Kullanılan parametreler aşağıdaki tabloda açıklanmıştır **az iotcentral uygulaması oluşturma** komutu:

| Parametre         | Açıklama |
| ----------------- | ----------- |
| resource-group    | Uygulamayı içeren kaynak grubu. Bu kaynak grubunun aboneliğinizde zaten mevcut olmalıdır. |
| konum          | Varsayılan olarak, bu komut, kaynak grubu konumu kullanır. Şu anda IOT Central uygulamada oluşturabilirsiniz **Doğu ABD**, **Batı ABD**, **Kuzey Avrupa**, veya **Batı Avrupa** bölgeleri. |
| name              | Azure portalında uygulama adı. |
| Alt etki alanı         | Uygulama URL'sini alt etki alanı. Örnekte, uygulama URL'si olan https://mysubdomain.azureiotcentral.com. |
| sku               | Şu anda yalnızca bir bölüm değerdir **S1** (standart katman). Bkz: [Azure IOT Central fiyatlandırma](https://azure.microsoft.com/pricing/details/iot-central/). |
| şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın: |
| görünen ad      | Uygulamanın kullanıcı Arabiriminde görüntülenen adı. |

**Uygulama Şablonları**

| Şablon adı            | Açıklama |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |
| iotc-demo@1.0.0          | Bir Soğutmalı Otomat için önceden oluşturulmuş cihaz şablonunu içeren bir uygulama oluşturur. Azure IoT Central'ı incelemeye başlamak için bu şablonu kullanın. |
| iotc-devkit-sample@1.0.0 | MXChip veya Raspberry Pi cihazını bağlamak amacıyla sizin için hazırlanmış cihaz şablonlarıyla bir uygulama oluşturur. Cihaz geliştiricisiyseniz tüm bu cihazların denemeler bu şablonu kullanın. |

## <a name="view-your-applications"></a>Uygulamalarınızı görüntüleyin

Kullanım [az iotcentral uygulama listesini](/cli/azure/iotcentral/app#az-iotcentral-app-list) IOT Central uygulamalarınızın listelenmesi ve meta verileri görmek için komutu.

## <a name="modify-an-application"></a>Bir uygulamayı değiştirme

Kullanım [az iotcentral uygulama güncelleştirmesi](/cli/azure/iotcentral/app#az-iotcentral-app-update) IOT Central uygulamasına meta verilerini güncelleştirmek için komutu. Örneğin, uygulamanızın görünen adını değiştirmek için şunu yazın:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Kullanım [az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) IOT Central uygulamasına silmek için komutu. Örneğin:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI üzerinden Azure IOT Central uygulamaları yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
