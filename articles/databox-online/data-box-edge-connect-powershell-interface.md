---
title: Bağlanmak ve Microsoft Azure veri kutusu Edge cihazı Windows PowerShell arabirimi üzerinden yönetmek | Microsoft Docs
description: Bağlanmak ve ardından veri kutusu Edge Windows PowerShell arabirimi üzerinden yönetmek açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/25/2019
ms.author: alkohli
ms.openlocfilehash: 6af95b7f8bde6e77ba356fec9dde123e26a9a4a8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448641"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell"></a>Windows PowerShell aracılığıyla bir Azure veri kutusu Edge cihazı yönetme

Azure veri kutusu Edge çözüm, verileri işlemek ve ağ üzerinden Azure'a göndermek olanak tanır. Bu makalede, bazı veri kutusu Edge cihazınız için yapılandırma ve yönetim görevleri açıklanır. Azure portalı, yerel web kullanıcı Arabirimi veya Windows PowerShell arabiriminden Cihazınızı yönetmek için kullanabilirsiniz.

Bu makalede PowerShell arabirimini kullanarak bunu görevlere odaklanır.

Bu makalede, aşağıdaki yordamları içerir:

- PowerShell arabirimine bağlanma
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- Cihaz sıfırlama
- Cihaz bilgilerini görüntüle
- İşlem günlükleri alın
- İzleme ve sorun giderme işlem modülleri

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanma

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

IOT Edge Cihazınızı ve buna bağlanmak aşağı akış cihazları arasında güvenli bir bağlantı etkinleştirmek için IOT Edge sertifikalarını da karşıya yükleyebilirsiniz. IOT Edge üç sertifika bulunur ( *.pem* biçimi) yüklemek gereken:

- Kök CA sertifikasını veya CA sahibi
- Cihaz CA sertifikası
- Cihaz anahtar sertifikası

Aşağıdaki örnek, IOT Edge sertifikaları yüklemek için bu cmdlet'in kullanımı gösterilmektedir:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
Bu cmdlet'i çalıştırdığınızda, ağ paylaşımı için parolayı girmeniz istenir.

Sertifikalar hakkında daha fazla bilgi için Git [Azure IOT Edge sertifikaları](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) veya [sertifikaları üzerinde bir ağ geçidi yükleme](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway#install-certificates-on-the-gateway).

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüle
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Cihazınızı sıfırlama

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>İşlem günlükleri alın

Bilgi işlem rolü Cihazınızda yapılandırılmışsa, ayrıca PowerShell arabirimi üzerinden işlem günlükleri alabilirsiniz.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Get-AzureDataBoxEdgeComputeRoleLogs` cihazınız için işlem günlüklerini almak için.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection
    ```

    Cmdlet'i için kullanılan parametreler açıklaması aşağıda verilmiştir:
    - `Path`: Bir işlem günlüğü paketi oluşturmak için istediğiniz paylaşımı ağ yolunu belirtin.
    - `Credential`: Ağ paylaşımı için kullanıcı adını belirtin. Bu cmdlet'i çalıştırdığınızda, paylaşımı parolayı girmeniz gerekir.
    - `FullLogCollection`: Bu parametre, günlük paketi tüm işlem günlüklerini içerecek sağlar. Varsayılan olarak, yalnızca bir alt günlüklerin günlük paketi içerir.

## <a name="monitor-and-troubleshoot-compute-modules"></a>İzleme ve sorun giderme işlem modülleri

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/data-box-edge-monitor-troubleshoot-compute.md)]

## <a name="exit-the-remote-session"></a>Uzak oturumu Çık

Uzak PowerShell oturumu çıkmak için PowerShell penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalda [Azure Data Box Edge](data-box-edge-deploy-prep.md)’i dağıtın.
