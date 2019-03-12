---
title: Bağlanmak ve Microsoft Azure veri kutusu Edge cihazı Windows PowerShell arabirimi üzerinden yönetmek | Microsoft Docs
description: Bağlanmak ve ardından veri kutusu Edge Windows PowerShell arabirimi üzerinden yönetmek açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b3effdbace2be582bfe85d0402088f8aa0d96fe7
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555209"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell-preview"></a>Bir Azure veri kutusu Edge cihazı (Önizleme) Windows PowerShell ile yönetme

Azure veri kutusu Edge çözüm, verileri işlemek ve ağ üzerinden Azure'a göndermek olanak tanır. Bu makalede, bazı veri kutusu Edge cihazınız için yapılandırma ve yönetim görevleri açıklanır. Azure portalı, yerel web kullanıcı Arabirimi veya Windows PowerShell arabiriminden Cihazınızı yönetmek için kullanabilirsiniz.

Bu makalede PowerShell arabirimini kullanarak bunu görevlere odaklanır.

Bu makalede, aşağıdaki yordamları içerir:

- PowerShell arabirimine bağlanma
- Bir destek oturumu başlatın
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- Cihaz sıfırlama
- Cihaz bilgilerini görüntüle
- İşlem günlükleri alın
- İzleme ve sorun giderme işlem modülleri

> [!IMPORTANT]
> Azure veri kutusu Edge şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanma

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="start-a-support-session"></a>Bir destek oturumu başlatın

[!INCLUDE [Connect to support runspace](../../includes/data-box-edge-gateway-connect-support.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüle
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Cihazınızı sıfırlama

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>İşlem günlükleri alın

Bilgi işlem rolü Cihazınızda yapılandırılmışsa, ayrıca PowerShell arabirimi üzerinden işlem günlükleri alabilirsiniz.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Get-AzureDataBoxEdgeComputeRoleLogs` cihazınız için işlem günlüklerini almak için.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```
    Cmdlet'i için kullanılan parametreler açıklaması aşağıda verilmiştir: 
    - `Path`: Bir işlem günlüğü paketi oluşturmak için istediğiniz paylaşımı ağ yolunu belirtin.
    - `Credential`: Ağ paylaşımı için kullanıcı adı ve parola sağlayın.
    - `RoleInstanceName`: Bu dize `IotRole` Bu parametre için.
    - `FullLogCollection`: Bu parametre, günlük paketi tüm işlem günlüklerini içerecek sağlar. Varsayılan olarak, yalnızca bir alt günlüklerin günlük paketi içerir.


## <a name="next-steps"></a>Sonraki adımlar

- Azure portalda [Azure Data Box Edge](data-box-edge-deploy-prep.md)’i dağıtın.
