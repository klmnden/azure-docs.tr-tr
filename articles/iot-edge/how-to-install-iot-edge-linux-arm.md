---
title: Linux üzerinde Azure IOT Edge yükleme | Microsoft Docs
description: ARM32 üzerinde Linux üzerinde Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: kgremban
ms.openlocfilehash: 7720e0471c6d8f2ba20f28753773829a28f93c7a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060907"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta yükleme

Azure IOT Edge çalışma zamanı, tüm IOT Edge cihazlarında dağıtılır. Üç bileşeni vardır. **IOT Edge güvenlik arka plan programı** sağlar ve sınır cihazı güvenlik standartlarını korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge Aracısı'nı başlatarak bootstraps. **IOT Edge Aracısı** dağıtım ve IOT Edge hub'ı dahil olmak üzere sınır cihazı, modülleri izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Bu makalede, Azure IOT Edge çalışma zamanı bir Linux ARM32v7/armhf Edge cihazda (örneğin, Raspberry Pi) yüklemek için adımları listelenir.

>[!NOTE]
>Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IOT Edge dayanır bir [OCI uyumlu] [ lnk-oci] kapsayıcı çalışma zamanı. Üretim senaryoları için kullanmanız önerilir [Moby tabanlı] [ lnk-moby] aşağıda sağlanan altyapısı. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntülerini Moby tabanlı çalışma zamanı ile uyumludur.

Aşağıdaki komutları Moby tabanlı altyapısı ve komut satırı arabirimi (CLI) yükleyin. CLI, geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

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

## <a name="install-the-iot-edge-security-daemon"></a>IOT Edge güvenlik Daemon'ı yükleme

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

Dosyayı kaydedin ve kapatın. 

   `CTRL + X`, `Y`, `Enter`

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
>[!NOTE]
>Kaynak üzerinde kısıtlı RaspberryPi gibi cihazları, yüksek oranda önerilir *OptimizeForPerformance* ortam değişkeni ayarlandığında *false* yönergeleri uyarınca [ sorun giderme kılavuzu.][lnk-trouble]


## <a name="next-steps"></a>Sonraki adımlar

Düzgün bir şekilde, kullanıma alma yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız [sorun giderme] [ lnk-trouble] sayfası.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-trouble]: https://docs.microsoft.com/azure/iot-edge/troubleshoot#stability-issues-on-resource-constrained-devices
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
