---
title: Azure CLI kullanarak IOT hub'a karşıya dosya yüklemeyi yapılandırma | Microsoft Docs
description: Platformlar arası Azure CLI kullanarak Azure IOT Hub için yapılandırma dosyası yükler.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: robinsh
ms.openlocfilehash: fe6ce23b9e87235521739b7808712a9d541dabf9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59048971"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Dosya karşıya yükleyen Azure CLI kullanarak IOT hub'ı yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

İçin [bir CİHAZDAN karşıya dosya yükleme](iot-hub-devguide-file-upload.md), önce bir Azure depolama hesabı ile IOT hub'ınıza ilişkilendirmelisiniz. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* Azure IOT hub'ı. IOT hub'ı yoksa, kullanabileceğiniz [ `az iot hub create` komut](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) oluşturmak için veya [portalı kullanarak IOT hub oluşturma](iot-hub-create-through-portal.md).

* Azure Depolama hesabı. Bir Azure depolama hesabınız yoksa, kullanabileceğiniz [Azure CLI - depolama hesaplarını yönetme](../storage/common/storage-azure-cli.md#manage-storage-accounts) oluşturun veya portalını kullanarak [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md).

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde çalıştırın [oturum açma komutunu](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest):

    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

2. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Aşağıdaki [Azure hesaplarını listelemek için komut](https://docs.microsoft.com/cli/azure/account) kullanabilmeniz için kullanılabilir:

    ```azurecli
    az account list
    ```

    IOT hub'ınızı oluşturmak için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabınızın ayrıntıları alınamıyor

Aşağıdaki adımları kullanarak, depolama hesabı oluşturduğunuz varsayılır **Resource Manager** dağıtım modeli ve **Klasik** dağıtım modeli.

Cihazlardan dosya yüklemeleri yapılandırmak için Azure depolama hesabınız için bağlantı dizesi gerekir. Depolama hesabı, IOT hub'ınız ile aynı abonelikte olmalıdır. Depolama hesabındaki bir blob kapsayıcısının adı da gerekir. Depolama hesap anahtarlarınızı almak için aşağıdaki komutu kullanın:

```azurecli
az storage account show-connection-string --name {your storage account name} \
  --resource-group {your storage account resource group}
```

Not **connectionString** değeri. Aşağıdaki adımlarda ihtiyacınız.

Mevcut bir blob kapsayıcısı için dosya yüklemeleriniz kullanabilir veya yeni bir tane oluşturun:

* Depolama hesabınızdaki mevcut blob kapsayıcıları listelemek için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Depolama hesabınızdaki blob kapsayıcısı oluşturmak için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container create --name {container name} \
      --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Karşıya dosya yükleme

Özelliği etkinleştirmek için IOT hub'ınızı şimdi yapılandırabilirsiniz [IOT hub'ına dosyaları karşıya yükleme](iot-hub-devguide-file-upload.md) kullanarak depolama hesabınızın ayrıntıları.

Aşağıdaki değerleri yapılandırmasını gerektirir:

* **Depolama kapsayıcısı**: İle IOT hub'ınızı ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısı. Gerekli depolama hesabı bilgileri önceki bölümde aldığınız. IOT hub'ı SAS URI'leriyle bu blob kapsayıcısında, karşıya dosya yükleme sırasında kullanılacak cihazlar için yazma izinlerine sahip otomatik olarak oluşturur.

* **Karşıya yüklenen dosyalar için bildirimlerin**: Etkinleştirmek veya dosya karşıya yükleme bildirimleri devre dışı.

* **SAS TTL**: Bu ayar, zaman yaşam IOT Hub tarafından cihaza verilen SAS URI'ın desteklenir. Bir saat için varsayılan olarak ayarlayın.

* **Dosya bildirim ayarları varsayılan TTL**: Geçerlilik süresi doluncaya kadar önce zaman yaşam dosyasının bildirim karşıya yükleyin. Bir gün için varsayılan olarak ayarlayın.

* **Dosya bildirim en yüksek teslimat sayısı**: IOT hub'ı bir dosyayı teslim girişiminde sayısı bildirim karşıya yükleyin. Varsayılan olarak 10'a ayarlayın.

IOT hub'ınızda bir dosya karşıya yükleme ayarları yapılandırmak için aşağıdaki Azure CLI komutları kullanın:

<!--Robinsh this is out of date, add cloud powershell -->

Bir bash kabuğunda kullanın:

```azurecli
az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} \
  --set properties.enableFileUploadNotifications=true

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Aşağıdaki komutu kullanarak IOT hub'ınızda bir dosya karşıya yükleme yapılandırması gözden geçirebilirsiniz:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz. [dosyaları bir CİHAZDAN karşıya yükle](iot-hub-devguide-file-upload.md).

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IOT çözümünüzü baştan güvenli hale getirme](../iot-fundamentals/iot-security-ground-up.md)
