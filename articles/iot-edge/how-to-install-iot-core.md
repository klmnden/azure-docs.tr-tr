---
title: "Azure IOT kenar IOT Core üzerinde yükleme | Microsoft Docs"
description: "Bir Windows IOT Core cihazda Azure IOT kenar çalışma zamanı yükleme"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 11/17/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d3ff260b4ac238ce7aaa2a63538dede7bd21a19c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Windows IOT Core üzerinde IOT kenar çalışma zamanı yükleme - Önizleme

Azure IOT kenar çalışma zamanı bile IOT sektördeki çok yaygın küçük tek Panosu bilgisayar (SBC) aygıtları çalıştırabilirsiniz. Bu makalede çalışma zamanı sağlama aracılığıyla anlatılmaktadır bir [MinnowBoard Turbot] [ lnk-minnow] Windows IOT Core çalıştıran geliştirme Panosu.

## <a name="install-the-runtime"></a>Çalışma zamanı yükleme

1. Yükleme [Windows 10 IOT Core Panosu] [ lnk-core] bir konak sisteminde.
1. Adımları [Cihazınızı ayarlamak] [ lnk-board] MinnowBoard Turbot/MAX yapı 16299 görüntüsüyle panonuzu yapılandırmak için. 
1. Cihazda sonra kapatma [PowerShell ile uzaktan oturum açma][lnk-powershell].
1. PowerShell konsolunda kapsayıcı yükleyin: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-17.06.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Bu kapsayıcı çalışma zamanı Moby proje Yapı sunucusundan olduğu ve yalnızca değerlendirme amacıyla tasarlanmıştır. Bu değil test, Destekli veya Docker tarafından desteklenir.

1. IOT kenar çalışma zamanı yükleme ve yapılandırmanızı doğrulayın:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Bu komut dosyası aşağıdakileri sağlar: 
   * Python 3.6
   * IOT kenar denetim komut dosyası (iotedgectl.exe)

Uzak PowerShell penceresinde yeşil iotedgectl.exe aracında bilgilendirme çıktısını görebilirsiniz. Bu, hataları mutlaka göstermez. 

## <a name="next-steps"></a>Sonraki adımlar

IOT kenar çalışma zamanı çalıştıran bir cihaza sahip olduğunuza göre bilgi nasıl [dağıtma ve izleme IOT kenar modülleri ölçekte][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
