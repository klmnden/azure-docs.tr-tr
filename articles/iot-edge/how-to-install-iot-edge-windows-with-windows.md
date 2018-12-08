---
title: Azure IOT Edge üzerinde Windows ile Windows kapsayıcıları yükleyin. | Microsoft Docs
description: Windows cihazlarda Windows kapsayıcıları için yapılandırılmış Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 9d7f9c68220f02a19f4e3f8f553cc06e9ace5424
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103821"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Windows kapsayıcıları ile kullanmak için Windows Azure IOT Edge çalışma zamanını yükleyin

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. 

IOT Edge çalışma zamanı nasıl çalıştığını ve hangi bileşenler dahildir hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede, Windows x64 (AMD/Intel) Windows kapsayıcıları ile Azure IOT Edge çalışma zamanı yükleme adımlarını listeler sistem. 

Windows Destek şu anda Önizleme aşamasındadır.

## <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri
Azure IOT Edge Windows kapsayıcıları ile birlikte kullanılabilir:
  * Windows 10/IOT Enterprise/IOT Core ile Nisan 2018 Güncelleştirmesi (derleme 17134).
  * Windows Server 1803

Hangi işletim sistemleri şu anda desteklenen daha fazla bilgi için [Azure IOT Edge desteği](support.md#operating-systems).

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme 

>[!NOTE]
>Windows IOT Core üzerinde kapsayıcı altyapısı yükleme için adımları izleyin [IOT Core cihazı makale sağlama](how-to-install-iot-core.md) ve sonra aşağıdaki yönergeleri ile devam edin.

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) kapsayıcı çalışma zamanı (örneğin, Docker). Kullanabileceğiniz [için Docker Windows](https://www.docker.com/docker-windows) geliştirme ve test için. 

Docker için Windows yapılandırma [Windows kapsayıcılarını kullanmaya](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

### <a name="install-and-manually-provision"></a>Yükleme ve el ile sağlama

1. Bağlantısındaki [yeni bir Azure IOT Edge cihazı kaydedin](how-to-register-device-portal.md) Cihazınızı kaydedemedik ve cihaz bağlantı dizesini almak için. 

2. IoT Edge cihazınızda PowerShell'i yönetici olarak çalıştırın. 

3. IOT Edge çalışma zamanı yükleyip yeniden açın. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Windows
   ```

4. İstendiğinde bir **DeviceConnectionString**, IOT Hub'ından alınan bağlantı dizesini belirtin. Bağlantı dizesini tırnak içermez. 

### <a name="install-and-automatically-provision"></a>Yükleme ve otomatik olarak sağlama

1. Bağlantısındaki [oluşturma ve sağlama Windows sanal bir TPM Edge cihazında](how-to-auto-provision-simulated-device-windows.md) cihaz sağlama hizmetini ayarlama ve almak için kendi **kapsam kimliği**almak ve bir TPM cihazını benzetme kendi  **Kayıt Kimliği**, sonra bireysel kayıt oluşturma. Cihazınızı IOT hub'ına kaydedildiğinde, yükleme işlemine devam edin.  

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

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

Ağınız proxy sunucusu varsa, adımları [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge Cihazınızı yapılandırma](how-to-configure-proxy-support.md) yüklemek ve IOT Edge çalışma zamanı'nı başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

Düzgün bir şekilde yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.
