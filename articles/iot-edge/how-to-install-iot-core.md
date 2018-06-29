---
title: Azure IOT kenar IOT Core üzerinde yükleme | Microsoft Docs
description: Bir Windows IOT Core cihazda Azure IOT kenar çalışma zamanı yükleme
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ae5644a62b794dc8d6ace52f21a452fa70027d39
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029569"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Windows IOT Core üzerinde IOT kenar çalışma zamanı yükleme - Önizleme

Azure IOT kenarı ve [Windows IOT Core](https://docs.microsoft.com/windows/iot-core/) birlikte bile küçük cihazlarda computing kenar etkinleştirmek için çalışır. Azure IOT kenar çalışma zamanı bile IOT sektördeki çok yaygın küçük tek Panosu bilgisayar (SBC) aygıtları çalıştırabilirsiniz. 

Bu makalede Windows IOT Core çalıştıran bir geliştirme panosunda çalışma zamanı sağlama aracılığıyla anlatılmaktadır. 

**Şu anda Windows IOT Core yalnızca Intel x64 tabanlı işlemciler üzerinde Azure IOT kenar destekler.**

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

1. Panonuzu ile yapılandırma **yapı 17134 (RS4)** IOT Core görüntü. 
1. Cihazda sonra kapatma [PowerShell ile uzaktan oturum açma][lnk-powershell].
1. PowerShell konsolunda kapsayıcı yükleyin: 

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
   >Bu kapsayıcı çalışma zamanı Moby proje Yapı sunucusundan olduğu ve yalnızca değerlendirme amacıyla tasarlanmıştır. Bu değil test, Destekli veya Docker tarafından desteklenir.

## <a name="finish-installing"></a>Yüklemeyi tamamlamak

IOT kenar güvenlik arka plan programı yükleme ve yönergeleri kullanarak yapılandırma [bu makalede][lnk-install-windows-on-windows]

## <a name="next-steps"></a>Sonraki adımlar

IOT kenar çalışma zamanı çalıştıran bir cihaza sahip olduğunuza göre bilgi nasıl [dağıtma ve izleme IOT kenar modülleri ölçekte][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md#download-the-edge-daemon-package-and-install
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers