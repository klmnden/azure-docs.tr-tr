---
title: "Karşıya dosya yükleme yapılandırmak için Azure PowerShell kullanın | Microsoft Docs"
description: "Azure PowerShell cmdlet'lerini dosya etkinleştirmek için IOT hub'ınızı yapılandırma için nasıl kullanılacağını bağlı aygıtlardan yükler. Hedef Azure depolama hesabı yapılandırma hakkında bilgi içerir."
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
ms.openlocfilehash: a72bda794b2da3e044c46249559610d06b1f1843
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>IOT Hub'ın PowerShell kullanarak dosya yüklemeleri yapılandırın

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Kullanılacak [dosya karşıya yükleme işlevselliği IOT hub'da][lnk-upload], IOT hub'ınıza ilk Azure storage hesabı ilişkilendirmelisiniz. Mevcut bir depolama hesabını kullanma veya yeni bir tane oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure PowerShell cmdlet'leri][lnk-powershell-install].
* Azure IOT hub'ı. IOT hub'ı yoksa, kullanabileceğiniz [yeni AzureRmIoTHub cmdlet] [ lnk-powershell-iothub] oluşturun veya portalını kullanarak [IOT hub oluşturma][lnk-portal-hub].
* Bir Azure depolama hesabı. Bir Azure depolama hesabınız yoksa, kullanabileceğiniz [Azure Storage PowerShell cmdlet'lerini] [ lnk-powershell-storage] oluşturun veya portalını kullanarak [depolama hesabı oluşturma] [lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açma ve Azure hesabınızı ayarlama

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. PowerShell komut isteminde çalıştırmak **Login-AzureRmAccount** cmdlet:

    ```powershell
    Login-AzureRmAccount
    ```

1. Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgileriyle ilişkili tüm Azure abonelikleri için size erişim verir. Azure aboneliklerini kullanabilmeniz için kullanılabilir listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzureRMSubscription
    ```

    IOT hub'ınızı yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komut çıktısı abonelik adı veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabı ayrıntıları alma

Aşağıdaki adımlarda, depolama hesabı kullanılarak oluşturulan varsayılmaktadır **Resource Manager** dağıtım modeli ve **Klasik** dağıtım modeli.

Aygıtlarınızı dosya yüklemelerini yapılandırmak için bağlantı dizesi için bir Azure depolama hesabı gerekiyor. Depolama hesabı, IOT hub'ınızı ile aynı abonelikte olması gerekir. Depolama hesabındaki blob kapsayıcısının adı da gerekir. Depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Not **key1** depolama hesabı anahtar değeri. Bunu aşağıdaki adımlarda gerekir.

Var olan bir blob kapsayıcı, dosya yüklemeleri için kullanabilir veya yeni bir tane oluşturun:

* Depolama hesabınızdaki mevcut blob kapsayıcıları listelemek için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* Depolama hesabınızda blob kapsayıcısı oluşturmak için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>IOT hub'ınızı yapılandırma

Şimdi etkinleştirmek için IOT hub'ınızı yapılandırma [dosya karşıya yükleme işlevselliği] [ lnk-upload] depolama hesabı bilgilerinizi kullanarak.

Aşağıdaki değerleri yapılandırmasını gerektirir:

**Depolama kapsayıcısı**: IOT hub ile ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki blob kapsayıcısı. Önceki bölümde gerekli depolama hesabı bilgilerini aldı. IOT hub'ı SAS URI'ler dosyaları karşıya yükleme sırasında kullanmak cihazlar için bu blob kapsayıcısına yazma izinlerine sahip otomatik olarak oluşturur.

**Karşıya yüklenen dosyalar için bildirimlerin**: etkinleştirmek veya dosya karşıya yükleme bildirimlerini devre dışı.

**SAS TTL**: saat yaşam IOT Hub tarafından cihaza döndürülen SAS URI, bu ayar değildir. Bir saat için varsayılan olarak ayarlayın.

**Dosya bildirim ayarları varsayılan TTL**: süresi doldu önce bildirim zaman yaşam dosyasının karşıya. Bir gün için varsayılan olarak ayarlayın.

**Dosya bildirim maksimum teslimat sayısı**: IOT hub'ı bir dosya teslim etmek için kaç deneme sayısı bildirim karşıya yükleyin. Varsayılan olarak 10 olarak ayarlandı.

Dosya yapılandırmak için aşağıdaki PowerShell cmdlet'ini IOT hub'ınızı ayarlarını karşıya yükle:

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz: [karşıya bir aygıtı dosyalarından][lnk-upload].

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu IOT cihazları yönetme][lnk-bulk]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Bir cihaz IOT Edge benzetimini yapma][lnk-iotedge]
* [IOT çözümünüzden zemin oluşturan güvenli][lnk-securing]

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md