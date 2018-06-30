---
title: Azure IOT kenar Linux'ta yüklemek nasıl | Microsoft Docs
description: Linux üzerinde Azure IOT kenar yükleme yönergeleri
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 273bb920b1ef2cac425ba9e636d488c8219ad9d2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126976"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Azure IOT kenar (x64) Linux'ta yükleyin

Azure IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. Üç bileşeni vardır. **IOT kenar güvenlik arka plan programı** sağlar ve güvenlik standartları sınır cihazı üzerinde korur. Arka plan programı her önyüklemede başlatır ve cihazın IOT kenar Aracısı'nı başlatarak bootstraps. **IOT kenar Aracısı** dağıtım ve kenar IOT hub'ı dahil sınır cihazı modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makalede, Linux x64 (Intel/AMD) Azure IOT kenar çalışma zamanı yükleme adımlarını listeler sınır cihazı.

>[!NOTE]
>Linux yazılım depoları paketlerinde bulunan her paket Lisans Koşulları'nı tabidir (/ usr/paylaşmak/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Yükleme ve kullanım paketi bu koşulları kabul meydana gelir. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtar ve yazılım havuzu beslemesi kaydetme

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
```

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme 

Azure IOT kenar kullanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı (örneğin, Docker). Docker CE/EE kenar aygıtınızda yüklü zaten varsa, geliştirme ve Azure IOT Edge ile test etme için kullanmaya devam edebilirsiniz. 

Üretim senaryoları için kullanmanız önerilir [Moby tabanlı] [ lnk-moby] aşağıda sağlanan altyapısı. Bu resmi olarak Azure IOT Edge desteklenen yalnızca kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri Moby çalışma zamanı ile tam uyumludur.

*Aşağıdaki yönergeleri moby altyapısı ve komut satırı arabirimi (CLI) yükleyin. CLI geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.*

```cmd/sh
sudo apt-get update
sudo apt-get install moby-engine
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Azure IOT kenar güvenlik arka plan programı yükleme

Aşağıdaki komutları da standart sürümünü yüklemeniz **iothsmlib** henüz yoksa.

```cmd/sh
sudo apt-get update
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IOT kenar güvenlik arka plan programı yapılandırın

Arka plan programı, yapılandırma dosyasından kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml` sınır cihazı yapılandırılabilir <!--[automatically via Device Provisioning Service][lnk-dps] or--> el ile kullanarak bir [cihaz bağlantı dizesi][lnk-dcs].

El ile yapılandırma için aygıt bağlantı dizesinde girin **sağlama** bölümünü **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

*Dosya yazma korumalı varsayılan olarak, kullanmanız gerekebilir `sudo` düzenlemek için. Örneğin `sudo nano /etc/iotedge/config.yaml`*

Yapılandırmada sağlama bilgilerini girdikten sonra arka plan programı yeniden başlatın:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IOT kenar arka plan programı kullanarak durumunu denetleyebilirsiniz:

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

Düzgün checkout yükleme kenar çalışma zamanı sorunları yaşıyorsanız [sorun giderme] [ lnk-trouble] sayfası.

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
