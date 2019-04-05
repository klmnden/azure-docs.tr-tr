---
title: Karşıya dosya yüklemeyi yapılandırma için Azure PowerShell'i kullanma | Microsoft Docs
description: Dosya etkinleştirmek için IOT hub'ınızı yapılandırmak için Azure PowerShell cmdlet'lerini kullanmak nasıl bağlı cihazlardan karşıya yükler. ' % S'hedef Azure depolama hesabı yapılandırma hakkında bilgi içerir.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: robinsh
ms.openlocfilehash: 4f1a5d59c340a02dcdc0291046ef6361c6f41c86
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044921"
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>PowerShell kullanarak dosya yüklemeleri IOT hub'ı yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Kullanılacak [dosya karşıya yükleme işlevselliği IOT Hub'ında](iot-hub-devguide-file-upload.md), Azure depolama hesabınız ile IOT hub'ınızdaki ilk ilişkilendirmeniz gerekir. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

* [Azure PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/install-Az-ps).

* Azure IOT hub'ı. IOT hub'ı yoksa, kullanabileceğiniz [AzIoTHub yeni cmdlet](https://docs.microsoft.com/powershell/module/az.iothub/new-aziothub) oluşturun veya portalını kullanarak [IOT hub oluşturma](iot-hub-create-through-portal.md).

* Bir Azure depolama hesabı. Azure depolama hesabınız yoksa, kullanabileceğiniz [Azure Storage PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/module/az.storage/) oluşturun veya portalını kullanarak [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md)

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. PowerShell isteminde çalıştırın **Connect AzAccount** cmdlet:

    ```powershell
    Connect-AzAccount
    ```

2. Birden çok Azure aboneliğiniz varsa Azure'da oturum açma, kimlik bilgilerinizle ilişkili tüm Azure abonelikleri erişim verir. Azure aboneliklerini kullanmak için size sunulan listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

    IOT hub'ınıza yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabınızın ayrıntıları alınamıyor

Aşağıdaki adımları kullanarak, depolama hesabı oluşturduğunuz varsayılır **Resource Manager** dağıtım modeli ve **Klasik** dağıtım modeli.

Cihazlardan dosya yüklemeleri yapılandırmak için Azure depolama hesabınız için bağlantı dizesi gerekir. Depolama hesabı, IOT hub'ınız ile aynı abonelikte olmalıdır. Depolama hesabındaki bir blob kapsayıcısının adı da gerekir. Depolama hesap anahtarlarınızı almak için aşağıdaki komutu kullanın:

```powershell
Get-AzStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Not **key1** depolama hesabı anahtar değeri. Aşağıdaki adımlarda ihtiyacınız.

Mevcut bir blob kapsayıcısı için dosya yüklemeleriniz kullanabilir veya yeni bir tane oluşturun:

* Depolama hesabınızdaki mevcut blob kapsayıcıları listelemek için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzStorageContainer -Context $ctx
    ```

* Depolama hesabınızdaki blob kapsayıcısı oluşturmak için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>IOT hub'ınızı yapılandırma

Artık IOT hub'ınıza yapılandırabilirsiniz [IOT hub'ına dosyaları karşıya yükleme](iot-hub-devguide-file-upload.md) kullanarak depolama hesabınızın ayrıntıları.

Aşağıdaki değerleri yapılandırmasını gerektirir:

* **Depolama kapsayıcısı**: İle IOT hub'ınızı ilişkilendirmek için geçerli Azure aboneliğinizde bir Azure depolama hesabındaki bir blob kapsayıcısı. Gerekli depolama hesabı bilgileri önceki bölümde aldığınız. IOT hub'ı SAS URI'leriyle bu blob kapsayıcısında, karşıya dosya yükleme sırasında kullanılacak cihazlar için yazma izinlerine sahip otomatik olarak oluşturur.

* **Karşıya yüklenen dosyalar için bildirimlerin**: Etkinleştirmek veya dosya karşıya yükleme bildirimleri devre dışı.

* **SAS TTL**: Bu ayar, zaman yaşam IOT Hub tarafından cihaza verilen SAS URI'ın desteklenir. Bir saat için varsayılan olarak ayarlayın.

* **Dosya bildirim ayarları varsayılan TTL**: Geçerlilik süresi doluncaya kadar önce zaman yaşam dosyasının bildirim karşıya yükleyin. Bir gün için varsayılan olarak ayarlayın.

* **Dosya bildirim en yüksek teslimat sayısı**: IOT hub'ı bir dosyayı teslim girişiminde sayısı bildirim karşıya yükleyin. Varsayılan olarak 10'a ayarlayın.

IOT hub'ınızdaki ayarlar karşıya dosya yapılandırmak için aşağıdaki PowerShell cmdlet'ini kullanın:

```powershell
Set-AzIotHub `
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

IOT hub'ı dosya karşıya yükleme özellikleri hakkında daha fazla bilgi için bkz. [dosyaları bir CİHAZDAN karşıya yükle](iot-hub-devguide-file-upload.md).

Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IOT çözümünüzü baştan güvenli hale getirme](../iot-fundamentals/iot-security-ground-up.md)
