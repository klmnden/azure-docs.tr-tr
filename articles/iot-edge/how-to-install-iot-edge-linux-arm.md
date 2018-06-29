---
title: Azure IOT kenar Linux'ta yüklemek nasıl | Microsoft Docs
description: ARM32 Linux'ta Azure IOT kenar yükleme yönergeleri
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: ad70fcc6b9779cb33772a3fce2fb11b4cec804ee
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062607"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IOT kenar çalışma zamanı (ARM32v7/armhf) Linux'ta yükleme

Azure IOT kenar çalışma zamanı, tüm IOT kenar aygıtlarda dağıtılır. Üç bileşeni vardır. **IOT kenar güvenlik arka plan programı** sağlar ve güvenlik standartları sınır cihazı üzerinde korur. Arka plan programı her önyüklemede başlatır ve cihazın IOT kenar Aracısı'nı başlatarak bootstraps. **IOT kenar Aracısı** dağıtım ve kenar IOT hub'ı dahil sınır cihazı modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makale Azure IOT kenar çalışma zamanı Linux ARM32v7/armhf sınır cihazı (örneğin, Raspberry Pi'yi) yüklemek için adımları listeler.

>[!NOTE]
>Linux yazılım depoları paketlerinde bulunan her paket Lisans Koşulları'nı tabidir (/ usr/paylaşmak/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Yükleme ve kullanım paketi bu koşulları kabul meydana gelir. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IOT kenar kullanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı (örneğin, Docker). Docker CE/EE kenar aygıtınızda yüklü zaten varsa, geliştirme ve Azure IOT Edge ile test etme için kullanmaya devam edebilirsiniz. 

Üretim senaryoları için kullanmanız önerilir [Moby tabanlı] [ lnk-moby] aşağıda sağlanan altyapısı. Bu resmi olarak Azure IOT Edge desteklenen yalnızca kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntüleri Moby çalışma zamanı ile tam uyumludur.

Aşağıdaki komutları moby altyapısı ve komut satırı arabirimi (CLI) yükleyin. CLI geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

```cmd/sh

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>IOT kenar güvenlik arka plan programı yükleme

```cmd/sh
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
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
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Düzgün checkout yükleme kenar çalışma zamanı sorunları yaşıyorsanız [sorun giderme] [ lnk-trouble] sayfası.

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md