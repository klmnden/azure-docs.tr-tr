---
title: Azure IOT Edge ile Linux kapsayıcıları Windows yükleme | Microsoft Docs
description: Azure IOT kenar yükleme yönergeleri Windows'da Linux kapsayıcılarla
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: cd517d7e652b38c7ecf28a17657936698416413a
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036260"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Linux kapsayıcılarını ile kullanmak için Windows Azure IOT kenar çalışma zamanı yükleme

Azure IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. Üç bileşeni vardır. **IOT kenar güvenlik arka plan programı** sağlar ve güvenlik standartları sınır cihazı üzerinde korur. Arka plan programı her önyüklemede başlatır ve cihazın IOT kenar Aracısı'nı başlatarak bootstraps. **IOT kenar Aracısı** dağıtım ve kenar IOT hub'ı dahil sınır cihazı modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makalede, Windows x64 (AMD/Intel) Azure IOT kenar çalışma zamanı yükleme adımlarını listeler sistem. Windows Destek şu anda önizlemede değil.

>[!NOTE]
Linux kapsayıcıları üzerinde Windows sistemini kullanarak Azure IOT kenar için önerilen veya desteklenen üretim yapılandırma değil. Ancak, bunu geliştirme ve sınama amacıyla kullanılabilir.

## <a name="supported-windows-versions"></a>Desteklenen Windows sürümlerine
Azure IOT Kenar Geliştirme ve test aşağıdaki Windows sürümleri üzerinde Linux kapsayıcıları kullanırken kullanılabilir:
  * Windows 10 veya daha yeni masaüstü işletim sistemleri.
  * Windows Server 2016 ya da yeni sunucu işletim sistemleri.

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme 

Azure IOT kenar kullanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı (örneğin, Docker). 

Kullanabileceğiniz [Windows için Docker] [ lnk-docker-for-windows] geliştirme ve test etme için. Windows için Docker olduğundan emin olun [Linux kapsayıcılar kullanmak üzere yapılandırılmış][lnk-docker-config]

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT kenar güvenlik arka plan programı yükleme

>[!NOTE]
>Lisans Koşulları'nı (lisans dizininde) paketleri bulunan Azure IOT kenar yazılım paketleri tabidir. Lütfen paketini kullanarak önce lisans koşullarını okuyun. Yükleme ve kullanım paketi bu koşulları kabul meydana gelir. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

### <a name="download-the-edge-daemon-package-and-install"></a>Edge arka plan programı paketini indirin ve yükleyin

Bir yönetici PowerShell penceresinde aşağıdaki komutları çalıştırın:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$env:Path += ";C:\ProgramData\iotedge"
SETX /M PATH "$env:Path"
```

Vcruntime kullanarak yükleyin:

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Oluşturma ve başlatma *iotedge* hizmeti:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Hizmet tarafından kullanılan bağlantı noktaları için güvenlik duvarı özel durumları ekleyin:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Oluşturma bir **iotedge.reg** dosya aşağıdaki içeriğe ve alma için Windows kayıt defteri çift veya kullanarak `reg import iotedge.reg` komutu:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IOT kenar güvenlik arka plan programı yapılandırın

Arka plan programı, yapılandırma dosyasından kullanılarak yapılandırılabilir `C:\ProgramData\iotedge\config.yaml` sınır cihazı yapılandırılabilir <!--[automatically via Device Provisioning Service][lnk-dps] or--> el ile kullanarak bir [cihaz bağlantı dizesi][lnk-dcs].

El ile yapılandırma için aygıt bağlantı dizesinde girin **sağlama:** bölümünü **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

Edge kullanarak cihaz adı alınmaya `hostname` komutu PowerShell'de ve değeri olarak ayarlamak **ana bilgisayar adı:** yapılandırma yaml içinde. Örneğin:

```yaml
  ###############################################################################
  # Edge device hostname
  ###############################################################################
  #
  # Configures the environment variable 'IOTEDGE_GATEWAYHOSTNAME' injected into
  # modules.
  #
  ###############################################################################

  hostname: "edgedevice-1"
```

Ardından, biz IP adresi verin ve için bağlantı noktası gerekir **workload_uri** ve **management_uri** içinde **bağlanın:** yapılandırma bölümü.

IP adresi için girin `ipconfig` PowerShell penceresinde ve IP adresini seçin **vEthernet (DockerNAT)**' arabirim (IP adresi, sisteminizdeki farklı olabilir) aşağıdaki örnekte gösterildiği gibi:

![DockerNat][img-docker-nat]

```yaml
connect:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

Aynı adresleri girin **dinleme:** yapılandırma bölümü. Örneğin:

```yaml
listen:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

PowerShell penceresinde bir ortam değişkeni oluşturma **IOTEDGE_HOST** ile **management_uri** adresi, örneğin:

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://10.0.75.1:15580")
```

Son olarak, olun **ağ:** altında ayarı **moby_runtime:** uncommented ve kümesine **azure IOT kenar**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "azure-iot-edge"
```

Yapılandırma dosyasını kaydedin ve hizmeti yeniden başlatın:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IOT kenar hizmetinin durumunu kontrol edebilirsiniz: 

```powershell
Get-Service iotedge
```

Hizmet günlüklerini kullanarak son 5 dakika inceleyin:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Ve modüller ile çalışan listesi:

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Düzgün checkout yükleme kenar çalışma zamanı sorunları yaşıyorsanız [sorun giderme] [ lnk-trouble] sayfası.


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows

