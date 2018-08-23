---
title: Linux üzerinde Azure IOT Edge yükleme | Microsoft Docs
description: Linux üzerinde Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: kgremban
ms.openlocfilehash: 5cd12d4fab97f295cad1e0ea06112fc53e376b12
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42057429"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Azure IOT Edge çalışma zamanı (x64) Linux'ta yükleme

Azure IOT Edge çalışma zamanı, tüm IOT Edge cihazlarında dağıtılır. Üç bileşeni vardır. **IOT Edge güvenlik arka plan programı** sağlar ve sınır cihazı güvenlik standartlarını korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge Aracısı'nı başlatarak bootstraps. **IOT Edge Aracısı** dağıtım ve IOT Edge hub'ı dahil olmak üzere sınır cihazı, modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makalede x64 (Intel/AMD), Linux üzerinde Azure IOT Edge çalışma zamanı yükleme adımlarını listeler Edge cihazı.

>[!NOTE]
>Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtar ve yazılım deposu akışı kaydedin

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme 

Azure IOT Edge dayanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı. Üretim senaryoları için kullanmanız önerilir [Moby tabanlı] [ lnk-moby] aşağıda sağlanan altyapısı. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntülerini Moby çalışma zamanı ile uyumludur.

Apt-get güncelleştirin.

```bash
sudo apt-get update
```

Moby Altyapısı'nı yükleyin. 

```bash
sudo apt-get install moby-engine
```

Moby komut satırı arabirimi (CLI) yükleyin. CLI, geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yükleme

Aşağıdaki komutları da standart sürümü yüklemeniz **iothsmlib** zaten mevcut değilse.

```bash
sudo apt-get update
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yapılandırma

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml`. Dosya yazma korumalı varsayılan olarak, düzenlemek için yükseltilmiş izinlere ihtiyaç duyabilirsiniz.

```bash
sudo nano /etc/iotedge/config.yaml
```

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

Yapılandırmada sağlama bilgilerini girdikten sonra Daemon programını yeniden başlatın:

```cmd/sh
sudo systemctl restart iotedge
```

>[!TIP]
>Çalıştırmak için yükseltilmiş ayrıcalıklarınız olması gerekir `iotedge` komutları. Makinenizde dışında ve geri IOT Edge çalışma zamanını yükledikten sonra ilk kez oturum açmalarını sağlama sonra izinlerinizi otomatik olarak güncelleştirilir. O zamana kadar kullanın **sudo** komutları önünde. 

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Kullandıysanız **otomatik yapılandırma** adımları çalışma zamanı ile sizin adınıza IOT hub'ınıza Cihazınızı kaydetmek için bazı ek adımları tamamlaması gerekir. Sonraki adımlar için bkz: [oluşturma ve sağlama Linux sanal makinesinde sanal bir TPM Edge cihazı](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

IOT Edge arka plan programı kullanarak durumu denetleyebilirsiniz:

```cmd/sh
systemctl status iotedge
```

Kullanarak arka plan programının günlüklerini inceleyin:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Ve modüller ile çalışan listesi:

```cmd/sh
sudo iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Düzgün bir şekilde yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme] [ lnk-trouble] sayfası.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
