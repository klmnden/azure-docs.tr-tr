---
title: Azure IOT Edge üzerinde Windows ile Windows kapsayıcıları yükleme | Microsoft Docs
description: Azure IOT Edge yükleme yönergeleri Windows ile Windows kapsayıcıları
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 18a1481b72904b0ac9c27e100271dc0fd0666baf
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001771"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Windows kapsayıcıları ile kullanmak için Windows Azure IOT Edge çalışma zamanını yükleyin

Azure IOT Edge çalışma zamanı, tüm IOT Edge cihazlarında dağıtılır. Üç bileşeni vardır. **IOT Edge güvenlik arka plan programı** sağlar ve sınır cihazı güvenlik standartlarını korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge Aracısı'nı başlatarak bootstraps. **IOT Edge Aracısı** dağıtım ve IOT Edge hub'ı dahil olmak üzere sınır cihazı, modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makalede Azure IOT Edge çalışma zamanı, Windows x64 (AMD/Intel) yüklemek için adımları listelenmektedir sistem. 

Windows Destek şu anda Önizleme aşamasındadır.

## <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri
Azure IOT Edge Windows kapsayıcıları ile birlikte kullanılabilir:
  * Windows 10/IOT Enterprise/IOT Core ile Nisan 2018 Güncelleştirmesi (derleme 17134).
  * Windows Server 1803

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme 

>[!NOTE]
>Windows IOT Core üzerinde kapsayıcı altyapısı yükleme için adımları izleyin [IOT Core cihazı makale sağlama] [ lnk-iot-core] ve sonra aşağıdaki yönergeleri ile devam edin.

Azure IOT Edge dayanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı (örneğin, Docker). Kullanabileceğiniz [için Docker Windows] [ lnk-docker-for-windows] geliştirme ve test için. 

**Docker için Windows olduğundan emin olun [Windows kapsayıcıları kullanacak şekilde yapılandırılmış][lnk-docker-config]**

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

### <a name="download-the-edge-daemon-package-and-install"></a>Edge arka plan programının paket indirip yükleyin

Bir yönetici PowerShell penceresinde aşağıdaki komutları yürütün:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
$path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
Set-ItemProperty -Path $sysenv -Name Path -Value $path
```

(Bir IOT core Edge cihazında bu adımı atlayabilirsiniz) vcruntime kullanarak yükleyin:

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Oluşturma ve başlatma **iotedge** hizmeti:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Hizmet tarafından kullanılan bağlantı noktaları için güvenlik duvarı özel durumları ekleyin:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Oluşturma bir **iotedge.reg** dosya aşağıdaki içeriğe ve alma Windows kayıt defterine çift veya kullanarak `reg import iotedge.reg` komutu:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yapılandırma

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `C:\ProgramData\iotedge\config.yaml`.

Sınır cihazı kullanarak el ile yapılandırılabilir bir [cihaz bağlantı dizesini] [ lnk-dcs] veya [otomatik olarak cihaz sağlama hizmeti aracılığıyla] [ lnk-dps].

* El ile yapılandırma için açıklama durumundan çıkarın **el ile** sağlama modu. Değerini güncelleştirin **device_connection_string** IOT Edge cihazınızdan bağlantı dizesiyle.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Otomatik yapılandırma için açıklama durumundan çıkarın **dps** sağlama modu. Güncelleştirin **scope_id** ve **regıstratıon_ıd** IOT hub'ı DPS örneğiniz ile TPM ile IOT Edge cihazınız değerlerle. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Edge kullanarak cihaz adı alınmaya `hostname` değeri olarak ayarlayın ve komutu PowerShell'de **ana bilgisayar adı:** yapılandırma yaml içinde. Örneğin:

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

Ardından, IP adresi verin ve için bağlantı noktası **workload_uri** ve **management_uri** içinde **bağlanın:** ve **dinleme:** bölümleri yapılandırma.

IP adresiniz almak için girin `ipconfig` , PowerShell penceresinde ve IP adresini kopyalayın **vEthernet (nat)** arabirimi (IP adresi sisteminize farklı olabilir) aşağıdaki örnekte gösterildiği gibi:  

![nat][img-nat]

Güncelleştirme **workload_uri** ve **management_uri** içinde **bağlanın:** yapılandırma dosyasının. Değiştirin **\<GATEWAY_ADDRESS\>** kopyaladığınız vEthernet IP adresine sahip.

```yaml
connect:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Aynı adresi girin **dinleme:** bölümü.

```yaml
listen:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

PowerShell penceresinde, bir ortam değişkenini oluşturmak **IOTEDGE_HOST** ile **management_uri** adresi.

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<GATEWAY_ADDRESS>:15580")
```

Ortam değişkenini yeniden başlatma işlemlerinden sonra kalıcı hale getirin.

```powershell
SETX /M IOTEDGE_HOST "http://<GATEWAY_ADDRESS>:15580"
```

Son olarak, olun **ağ:** bölümündeki **moby_runtime:** açıklamalar ve kümesine **nat**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "nat"
```

Yapılandırma dosyasını kaydedin ve hizmeti yeniden başlatın:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Kullandıysanız **otomatik yapılandırma** adımları çalışma zamanı ile sizin adınıza IOT hub'ınıza Cihazınızı kaydetmek için bazı ek adımları tamamlaması gerekir. Sonraki adımlar için bkz: [oluşturma ve sağlama Windows sanal bir TPM Edge cihazında](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).

IOT Edge hizmetinin durumu kontrol edebilirsiniz: 

```powershell
Get-Service iotedge
```

Son 5 kullanarak dakika için hizmet günlüklerini inceleyin:

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

Düzgün bir şekilde, kullanıma alma yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız [sorun giderme] [ lnk-trouble] sayfası.


<!-- Images -->
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md

