---
title: Azure IOT Edge üzerinde Windows ile Windows kapsayıcıları yükleme | Microsoft Docs
description: Azure IOT Edge yükleme yönergeleri Windows ile Windows kapsayıcıları
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: kgremban
ms.openlocfilehash: 39e0de6b378ed61ab375c6468b58c8c4a87b5fb9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39575973"
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

Docker için Windows yapılandırma [Windows kapsayıcılarını kullanmaya][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

### <a name="install-and-manually-provision"></a>Yükleme ve el ile sağlama

1. Bağlantısındaki [yeni bir Azure IOT Edge cihazı kaydedin] [ lnk-dcs] Cihazınızı kaydedemedik ve cihaz bağlantı dizesini almak için. 

2. IoT Edge cihazınızda PowerShell'i yönetici olarak çalıştırın. 

3. IOT Edge çalışma zamanı yükleyip yeniden açın. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Windows
   ```

4. İstendiğinde bir **DeviceConnectionString**, IOT Hub'ından alınan bağlantı dizesini belirtin. Bağlantı dizesini tırnak içermez. 

### <a name="install-and-automatically-provision"></a>Yükleme ve otomatik olarak sağlama

1. Bağlantısındaki [oluşturma ve sağlama Windows üzerinde sanal bir TPM Edge cihazı] [ lnk-dps] cihaz sağlama hizmetini ayarlama ve almak için kendi **kapsam kimliği**, TPM benzetimi cihaz ve almak, **kayıt kimliği**, sonra bireysel kayıt oluşturma. Cihazınızı IOT hub'ına kaydedildiğinde, yükleme işlemine devam edin.  

   >[!TIP]
   >TPM simülatörünü yüklenmesi sırasında açık ve test çalıştıran pencereyi tutun. 

2. IoT Edge cihazınızda PowerShell'i yönetici olarak çalıştırın. 

3. IOT Edge çalışma zamanı yükleyip yeniden açın. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Windows
   ```

4. İstendiğinde, sağlayın **ScopeId** ve **RegistrationId** sağlama hizmeti ve cihaz için. 

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

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
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Ve modüller ile çalışan listesi:

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak][lnk-modules].

Düzgün bir şekilde yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme] [ lnk-trouble] sayfası.


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
[lnk-modules]: how-to-deploy-modules-portal.md
