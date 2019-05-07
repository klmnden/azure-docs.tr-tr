---
title: Azure IOT Edge üzerinde Linux ARM32 yükleyin | Microsoft Docs
description: Azure IOT Edge yükleme yönergeleri Linux'ta ARM32 cihazlarda bir Raspberry PI gibi
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kgremban
ms.openlocfilehash: 6c22680102c57fdfc3d25beb19e5bc9847995b28
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65152722"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta yükleme

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. 

IOT Edge çalışma zamanı nasıl çalıştığını ve hangi bileşenler dahildir hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede, Azure IOT Edge çalışma zamanı Linux ARM32v7/armhf IOT Edge cihazında yüklemek için adımları listelenir. Örneğin, aşağıdaki adımları Raspberry Pi'yi cihazlar için işe yarar. Desteklenen ARM32 işletim sistemlerinin listesi için bkz. [Azure IOT Edge desteklenen sistemler](support.md#operating-systems). 

>[!NOTE]
>Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) kapsayıcı çalışma zamanı. Üretim senaryoları için kullanmanız önerilir [Moby tabanlı](https://mobyproject.org/) aşağıda sağlanan altyapısı. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntülerini Moby tabanlı çalışma zamanı ile uyumludur.

Aşağıdaki komutlar Moby tabanlı altyapısı ve komut satırı arabirimi (CLI) yükleyin. CLI, geliştirme için kullanışlıdır ancak üretim dağıtımları için isteğe bağlı değildir.

```bash
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

**IOT Edge güvenlik arka plan programı** sağlar ve cihazın IOT Edge güvenlik standartlarını korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge çalışma zamanı geri kalanını başlatarak bootstraps. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="connect-your-device-to-an-iot-hub"></a>Cihazınızı IOT hub'a bağlama 

Azure IOT hub'ı var olan bir cihaz kimliği ile fiziksel Cihazınızı bağlamak için IOT Edge çalışma zamanı yapılandırın. 

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml`. Düzenlemek için yükseltilmiş izinlere ihtiyacınız olabilecek şekilde varsayılan olarak, yazma korumalı dosyasıdır.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

### <a name="option-1-manual-provisioning"></a>1. seçenek: El ile sağlama

El ile cihaz sağlama için ile sağlamak gereken bir [cihaz bağlantı dizesini](how-to-register-device-portal.md) IOT hub'ına yeni bir IOT Edge cihazı kaydederek oluşturabilirsiniz.

Yapılandırma dosyasını açın. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Sağlama dosyasının bölümünü bulun ve açıklama durumundan çıkarın **el ile** sağlama modu. Değerini güncelleştirin **device_connection_string** IOT Edge cihazınızdan bağlantı dizesiyle.

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

Dosyayı kaydedin ve kapatın. 

`CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasında sağlama bilgilerini girdikten sonra Daemon programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>2. seçenek: Otomatik sağlama

Otomatik olarak bir cihazı sağlamak için [cihaz sağlama hizmetini ayarlama ve cihaz kayıt Kimliğinizi almak](how-to-auto-provision-simulated-device-linux.md). Otomatik sağlama, yalnızca Güvenilir Platform Modülü (TPM) yongasına sahip cihazlarla çalışır. Örneğin, Raspberry Pi cihazlar TPM ile varsayılan olarak gelmeyen. 

Yapılandırma dosyasını açın. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Sağlama dosyasının bölümünü bulun ve açıklama durumundan çıkarın **dps** sağlama modu. Güncelleştirin **scope_id** ve **regıstratıon_ıd** değerler, IOT Hub cihazı sağlama hizmeti ve IOT Edge Cihazınızı TPM ile. 

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

Yapılandırma dosyasında sağlama bilgilerini girdikten sonra Daemon programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Veya kullandıysanız **otomatik yapılandırma** adımları çalışma zamanı ile sizin adınıza IOT hub'ınıza Cihazınızı kaydetmek için bazı ek adımları tamamlaması gerekir. Sonraki adımlar için bkz: [oluşturma ve sağlama Linux sanal makinesinde sanal bir TPM IOT Edge cihazı](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

IOT Edge arka plan programı kullanarak durumu denetleyebilirsiniz:

```bash
systemctl status iotedge
```

Kullanarak arka plan programının günlüklerini inceleyin:

```bash
journalctl -u iotedge --no-pager --no-full
```

Ve modüller ile çalışan listesi:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Çalışma zamanını yükledikten sonra makinenizin dışında oturum ve izinlerinizi otomatik olarak güncelleştirmek için yeniden oturum açın. O zamana kadar kullanın **sudo** herhangi önünde `iotedge` komutları.

Kısıtlanmış bir kaynak cihazlarda ayarlamanız önerilir *OptimizeForPerformance* ortam değişkenine *false* yönergeleri uyarınca [sorun giderme kılavuzu ](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Bir proxy sunucusu varsa, ağ adımları izlerseniz [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge Cihazınızı yapılandırma](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>IOT Edge kaldırma

IOT Edge yükleme Linux cihazınızdan kaldırmak istiyorsanız, komut satırından aşağıdaki komutları kullanın. 

IoT Edge çalışma zamanını kaldırın. 

```bash
sudo apt-get remove --purge iotedge
```

IOT Edge çalışma zamanı kaldırıldığında, oluşturulan kapsayıcı durduruldu ancak Cihazınızda mevcut. Hangilerinin kalır görmek için tüm kapsayıcıları görüntüleyin. 

```bash
sudo docker ps -a
```

Kapsayıcıları iki çalışma zamanı kapsayıcılarındaki dahil olmak üzere cihazınızın silin. 

```bash
sudo docker rm -f <container name>
```

Son olarak, kapsayıcı çalışma zamanı cihazınızdan kaldırın. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

Düzgün bir şekilde yükleme IOT Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız başvurmak [sorun giderme](troubleshoot.md#stability-issues-on-resource-constrained-devices) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
