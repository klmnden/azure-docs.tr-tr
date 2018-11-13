---
title: Azure IOT Edge üzerinde IOT Core yükleyin | Microsoft Docs
description: Bir Windows IOT Core cihazda Azure IOT Edge çalışma zamanı yükleme
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 17afc095fa0500ca98559a9523282d75949faa60
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51564835"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>IOT Edge çalışma zamanı üzerinde Windows IOT Core yükleyin - Önizleme

Azure IOT Edge ve [Windows IOT Core](https://docs.microsoft.com/windows/iot-core/) birlikte bile küçük cihazlarda bilgi işlem edge etkinleştirmek için iş. Azure IOT Edge çalışma zamanı, IOT sektörde çok yaygın olan küçük tek Panosu bilgisayar (SBC) cihazlarda bile çalıştırabilirsiniz. 

Bu makalede, Windows IOT Core çalıştıran bir geliştirme panosunda çalışma zamanı sağlama üzerinden gösterilmektedir. 

**Şu anda Windows IOT Core yalnızca Intel işlemci x64 tabanlı Azure IOT Edge destekler.**

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

1. Panonuzu ile yapılandırma **derleme 17134 (RS4)** IOT Core görüntü. 
1. Cihazı, ardından açın [PowerShell ile uzaktan oturum açma](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).
1. PowerShell konsolunda kapsayıcı çalışma zamanı yükleyin: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Bu kapsayıcı çalışma zamanı Moby Proje yapı sunucunuzda ve yalnızca değerlendirme amacıyla tasarlanmıştır. Bu değil test, onaylı veya Docker tarafından desteklenir.

## <a name="finish-installing"></a>Yüklemeyi tamamlayın

IOT Edge güvenlik arka plan programı yükleme ve yapılandırma yönergeleri kullanarak [bu makalede](how-to-install-iot-edge-windows-with-windows.md)

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge çalışma zamanı'nı çalıştıran bir cihaza sahip olduğunuza göre bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).