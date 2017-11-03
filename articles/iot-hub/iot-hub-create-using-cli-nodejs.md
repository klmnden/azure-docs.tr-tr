---
title: "Azure CLI (azure.js) kullanarak IOT hub oluşturma | Microsoft Docs"
description: "Platformlar arası Azure CLI (azure.js) kullanarak Azure IOT hub oluşturma"
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: boltean
ms.openlocfilehash: 5e37c6c5e8625ce446ab203f19f9a8b2f1cd5a46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Azure CLI kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Giriş

Azure CLI (azure.js) oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu makalede Azure CLI (azure.js) IOT hub'ı oluşturmak için nasıl kullanılacağı gösterilmektedir.

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

* Azure CLI (azure.js) – bu makalesinde açıklandığı gibi Klasik ve kaynak yönetimi dağıtım modelleri için CLI.
* [Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - kaynak yönetimi dağıtım modeli için yeni nesil CLI.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure CLI 0.10.4] [ lnk-CLI-install] veya sonraki bir sürümü. Yüklü Azure CLI zaten varsa, geçerli sürümü komut isteminde aşağıdaki komutu kullanarak doğrulayabilirsiniz:

```azurecli
azure --version
```

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Azure CLI, Azure Kaynak Yöneticisi modunda olması gerekir:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Azure hesabınızı ve aboneliğinizi ayarlama

1. Komut isteminde aşağıdaki komutu yazarak oturum açma:

   ```azurecli
    azure login
   ```

   Önerilen web tarayıcısı ve kod kimliğini doğrulamak için kullanın.
1. Birden çok Azure aboneliğiniz varsa, Azure'a bağlanılıyor kimlik bilgilerinizle ilişkili tüm Azure abonelikleri için size erişim verir. Azure aboneliklerini görüntüleyin ve hangisinin varsayılan değer komutunu kullanarak belirleyin:

   ```azurecli
    azure account list
   ```

   Altında komutları kullanım rest çalıştırmak istediğiniz abonelik bağlamına ayarlamak için:

   ```azurecli
    azure account set <subscription name>
   ```

1. Bir kaynak grubu yoksa, bir adlı oluşturabilirsiniz **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> Makaleyi [Azure kaynakları ve kaynak gruplarını yönetmek için Azure CLI kullanma] [ lnk-CLI-arm] Azure kaynaklarınızı yönetmek için Azure CLI kullanma hakkında daha fazla bilgi sağlar.

## <a name="create-an-iot-hub"></a>IOT Hub oluşturma

Gerekli Parametreler:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **kaynak grubu**. Kaynak grubu adı. Büyük küçük harfe duyarlı alfasayısal, alt çizgi ve tire, 1-64 uzunluğu biçimindedir.
* **name**. Oluşturulacak IOT hub adı. Büyük küçük harfe duyarlı alfasayısal, alt çizgi ve tire, 3-50 uzunluğu biçimindedir.
* **Konum**. IOT hub'ı sağlamak için konum (azure bölge/veri merkezi).
* **SKU adı**. Sku birini adı: [F1, S1, S2, S3]. En son tam listesi için IOT Hub için fiyatlandırma sayfasına bakın.
* **birimleri**. Sağlanan birim sayısı. Aralık: F1 [1-1]: S1, S2 [1-200]: [1-10] S3. IOT Hub birimleri toplam ileti sayınıza ve bağlamak istediğiniz cihaz sayısını temel alır.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Oluşturma için kullanılabilen tüm parametreleri görmek için komut isteminde Yardım komutunu kullanabilirsiniz:

```azurecli
azure iothub create -h
```

Örnek: bir IOT hub'ı oluşturmak için çağrılan **exampleIoTHubName** kaynak grubunda **exampleResourceGroup**, aşağıdaki komutu çalıştırın:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Bu Azure CLI komut S1 standart IOT için faturalandırılır hub'ı oluşturur. IOT hub'ı silebilmek için **exampleIoTHubName** komutu kullanarak:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub için geliştirme hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

* [IOT SDK'ları][lnk-sdks]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT hub'ı yönetmek için Azure portalını kullanma][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
