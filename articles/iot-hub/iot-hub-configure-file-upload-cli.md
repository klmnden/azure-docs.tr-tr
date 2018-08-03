---
title: IOT hub'ına Azure CLI (az.py) kullanarak karşıya dosya yüklemeyi yapılandırma | Microsoft Docs
description: Platformlar arası Azure CLI 2.0 (az.py) kullanarak Azure IOT hub'a fileuploads yapılandırılır.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0eac620d44967827f7703da9cf409703a123ab07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460201"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Dosya karşıya yükleyen Azure CLI kullanarak IOT hub'ı yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Kullanılacak [dosya karşıya yükleme işlevselliği IOT Hub'ında][lnk-upload], önce bir Azure depolama hesabı ile IOT hub'ınıza ilişkilendirmelisiniz. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure CLI 2.0][lnk-CLI-install].
* Azure IOT hub'ı. IOT hub'ı yoksa, kullanabileceğiniz `az iot hub create` [komut] [ lnk-cli-create-iothub] oluşturun veya portalı [IOT hub oluşturma] [lnk-portal-hub].
* Azure Depolama hesabı. Bir Azure depolama hesabınız yoksa, kullanabileceğiniz [Azure CLI 2.0 - depolama hesaplarını yönetme] [ lnk-manage-storage] oluşturun veya portalını kullanarak [depolama hesabı oluşturma] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde [oturum açma komutunu][lnk-login-command] çalıştırın:

    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

1. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Kullanabileceğiniz [Azure hesaplarını listelemek için aşağıdaki komutu][lnk-az-account-command] kullanın:

    ```azurecli
    az account list
    ```

    IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabınızın ayrıntıları alınamıyor

Aşağıdaki adımları kullanarak, depolama hesabı oluşturduğunuz varsayılır **Resource Manager** dağıtım modeli ve **Klasik** dağıtım modeli.

Cihazlardan dosya yüklemeleri yapılandırmak için Azure depolama hesabınız için bağlantı dizesi gerekir. Depolama hesabı, IOT hub'ınız ile aynı abonelikte olmalıdır. Depolama hesabındaki bir blob kapsayıcısının adı da gerekir. Depolama hesap anahtarlarınızı almak için aşağıdaki komutu kullanın:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Not **connectionString** değeri. Aşağıdaki adımlarda ihtiyacınız.

Mevcut bir blob kapsayıcısı için dosya yüklemeleriniz kullanabilir veya yeni bir tane oluşturun:

* Depolama hesabınızdaki mevcut blob kapsayıcıları listelemek için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Depolama hesabınızdaki blob kapsayıcısı oluşturmak için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Karşıya dosya yükleme

Şimdi etkinleştirmek için IOT hub'ınızı yapılandırma [dosya karşıya yükleme işlevselliği] [ lnk-upload] kullanarak depolama hesabınızın ayrıntıları.

Aşağıdaki değerleri yapılandırmasını gerektirir:

**Depolama kapsayıcısı**: ile IOT hub'ınızı ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısı. Gerekli depolama hesabı bilgileri önceki bölümde aldığınız. IOT hub'ı SAS URI'leriyle bu blob kapsayıcısında, karşıya dosya yükleme sırasında kullanılacak cihazlar için yazma izinlerine sahip otomatik olarak oluşturur.

**Karşıya yüklenen dosyalar için bildirimlerin**: etkinleştirmek veya devre dışı dosya karşıya yükleme bildirimleri.

**SAS TTL**: zaman yaşam IOT Hub tarafından cihaza verilen SAS URI'ın bu ayardır. Bir saat için varsayılan olarak ayarlayın.

**Dosya bildirim ayarları varsayılan TTL**: geçerlilik süresi doluncaya kadar önce bildirim zaman yaşam dosyasının karşıya. Bir gün için varsayılan olarak ayarlayın.

**Dosya bildirim en yüksek teslimat sayısı**: IOT hub'ı bir dosyayı teslim etmek için kaç deneme sayısı bildirim karşıya yükleyin. Varsayılan olarak 10'a ayarlayın.

IOT hub'ınızda bir dosya karşıya yükleme ayarları yapılandırmak için aşağıdaki Azure CLI komutları kullanın:

Bir bash Kabuk kullanımda:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Bir Windows komut istemi kullanın:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Aşağıdaki komutu kullanarak IOT hub'ınızda bir dosya karşıya yükleme yapılandırması gözden geçirebilirsiniz:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz. [dosyaları bir CİHAZDAN karşıya yükle][lnk-upload].

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IOT cihazlarını toplu yönetme][lnk-bulk]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İşlem izleme][lnk-monitor]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]
* [IOT çözümünüzü baştan güvenli hale getirme][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: /azure/iot-fundamentals/iot-security-ground-up


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create