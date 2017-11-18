---
title: "IOT hub'ına Azure CLI (az.py) kullanarak karşıya dosya yükleme yapılandırma | Microsoft Docs"
description: "Platformlar arası Azure CLI 2.0 (az.py) kullanarak Azure IOT Hub'ına fileuploads yapılandırılır."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 6b100e65aba604fd8becb02c3a205b3348872bc4
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>IOT Hub'ın Azure CLI kullanarak dosya yüklemeleri yapılandırın

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Kullanılacak [dosya karşıya yükleme işlevselliği IOT hub'da][lnk-upload], bir Azure Storage hesabı IOT hub'ınıza ilk ilişkilendirmeniz gerekir. Mevcut bir depolama hesabını kullanma veya yeni bir tane oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure CLI 2.0][lnk-CLI-install].
* Azure IOT hub'ı. IOT hub'ı yoksa, kullanabileceğiniz `az iot hub create` [komutu] [ lnk-cli-create-iothub] oluşturun veya portal [IOT hub'ı Oluştur] kullanmayı [lnk-portal-hub].
* Bir Azure depolama hesabı. Bir Azure depolama hesabınız yoksa, kullanabileceğiniz [Azure CLI 2.0 - depolama hesaplarını yönetme] [ lnk-manage-storage] oluşturun veya portalını kullanarak [depolama hesabı oluşturma] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açma ve Azure hesabınızı ayarlama

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut istemine [oturum açma komut][lnk-login-command]:

    ```azurecli
    az login
    ```

    Kod kullanarak kimlik doğrulaması yapmak için yönergeleri izleyin ve bir web tarayıcısı aracılığıyla Azure hesabınızda oturum açın.

1. Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgilerinizle ilişkilendirilen tüm Azure hesaplar için size erişim verir. Aşağıdaki [Azure hesapları listelemek için komut] [ lnk-az-account-command] kullanmanız için kullanılabilir:

    ```azurecli
    az account list
    ```

    IOT hub'ınızı oluşturması için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komut çıktısı abonelik adı veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabı ayrıntıları alma

Aşağıdaki adımlarda, depolama hesabı kullanılarak oluşturulan varsayılmaktadır **Resource Manager** dağıtım modeli ve **Klasik** dağıtım modeli.

Aygıtlarınızı dosya yüklemelerini yapılandırmak için bağlantı dizesi için bir Azure depolama hesabı gerekiyor. Depolama hesabı, IOT hub'ınızı ile aynı abonelikte olması gerekir. Depolama hesabındaki blob kapsayıcısının adı da gerekir. Depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Not **connectionString** değeri. Bunu aşağıdaki adımlarda gerekir.

Var olan bir blob kapsayıcı, dosya yüklemeleri için kullanabilir veya yeni bir tane oluşturun:

* Depolama hesabınızdaki mevcut blob kapsayıcıları listelemek için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Depolama hesabınızda blob kapsayıcısı oluşturmak için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Karşıya dosya yükleme

Şimdi etkinleştirmek için IOT hub'ınızı yapılandırma [dosya karşıya yükleme işlevselliği] [ lnk-upload] depolama hesabı bilgilerinizi kullanarak.

Aşağıdaki değerleri yapılandırmasını gerektirir:

**Depolama kapsayıcısı**: IOT hub ile ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki blob kapsayıcısı. Önceki bölümde gerekli depolama hesabı bilgilerini aldı. IOT hub'ı SAS URI'ler dosyaları karşıya yükleme sırasında kullanmak cihazlar için bu blob kapsayıcısına yazma izinlerine sahip otomatik olarak oluşturur.

**Karşıya yüklenen dosyalar için bildirimlerin**: etkinleştirmek veya dosya karşıya yükleme bildirimlerini devre dışı.

**SAS TTL**: saat yaşam IOT Hub tarafından cihaza döndürülen SAS URI, bu ayar değildir. Bir saat için varsayılan olarak ayarlayın.

**Dosya bildirim ayarları varsayılan TTL**: süresi doldu önce bildirim zaman yaşam dosyasının karşıya. Bir gün için varsayılan olarak ayarlayın.

**Dosya bildirim maksimum teslimat sayısı**: IOT hub'ı bir dosya teslim etmek için kaç deneme sayısı bildirim karşıya yükleyin. Varsayılan olarak 10 olarak ayarlandı.

IOT hub'ına dosya karşıya yükleme ayarlarını yapılandırmak için aşağıdaki Azure CLI komutları kullanın:

Bir bash Kabuğu'nu kullanın:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Windows komut istemi kullanın:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Aşağıdaki komutu kullanarak, IOT hub'ına dosya karşıya yükleme yapılandırmasını gözden geçirebilirsiniz:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz: [karşıya bir aygıtı dosyalarından][lnk-upload].

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu IOT cihazları yönetme][lnk-bulk]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [AI ile Azure IOT kenar sınır cihazları için dağıtma][lnk-iotedge]
* [IOT çözümünüzden zemin oluşturan güvenli][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md


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
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#az_iot_hub_create