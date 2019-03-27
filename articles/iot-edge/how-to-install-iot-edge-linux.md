---
title: Azure IOT Edge Linux'ta yükleme | Microsoft Docs
description: Ubuntu çalıştıran Linux AMD64 cihazlarda Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 1d1e0f100a90c28bd7469991dee559abcd88f9a2
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499480"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Azure IOT Edge çalışma zamanı (x64) Linux'ta yükleme

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede Azure IOT Edge çalışma zamanı, Ubuntu Linux x64 (Intel/AMD) yüklemek için adımları listelenir. IOT Edge cihazı. Başvurmak [Azure IOT Edge desteği](support.md#operating-systems) desteklenen AMD64 işletim sistemlerinin bir listesi için.

> [!NOTE]
> Linux yazılım depoları paketlerinde her pakette yer alan lisans koşullarına tabidir (/ usr/paylaşım/doc/*paket adı*). Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft anahtar ve yazılım deposu akışı kaydedin

Cihazınız için IOT Edge çalışma zamanı yükleme hazırlayın.


Depo Yapılandırması'nı yükleyin. Seçin ya da **16.04** veya **18.04** kod parçacığı, Ubuntu sürümü için uygun şekilde.

> [!IMPORTANT]
> Kod parçacığı Ubuntu sürümünüz için doğru kod kutusundan seçtiğinizden emin olun.

* İçin **Ubuntu 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* İçin **Ubuntu 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
Depo Yapılandırması'nı yükleyin. Seçin ya da **16.04** veya **18.04** kod parçacığı, Ubuntu sürümü için uygun şekilde.

Oluşturulan listenin kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Microsoft GPG ortak anahtarı yükle

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-the-container-runtime"></a>Kapsayıcı çalışma zamanı yükleme

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) kapsayıcı çalışma zamanı. Üretim senaryoları için biz kullanmanız önerilir. [Moby tabanlı](https://mobyproject.org/) aşağıda sağlanan altyapısı. Bu, Azure IOT Edge ile resmi olarak desteklenen tek kapsayıcı altyapısıdır. Docker CE/EE kapsayıcı görüntülerini Moby çalışma zamanı ile uyumludur.

Apt güncelleştirme gerçekleştirin.

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

**IOT Edge güvenlik arka plan programı** sağlar ve cihazın IOT Edge güvenlik standartlarını korur. Arka plan programı, her önyükleme başlar ve cihazın IOT Edge çalışma zamanı geri kalanını başlatarak bootstraps.

Yükleme komutunu standart sürümü de yüklenir **iothsmlib** zaten mevcut değilse.

Apt güncelleştirme gerçekleştirin.

   ```bash
   sudo apt-get update
   ```

Güvenlik daemon'ı yükleyin. Paket yüklü `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Azure IOT Edge güvenlik Daemon'ı yapılandırma

Azure IOT hub'ı var olan bir cihaz kimliği ile fiziksel Cihazınızı bağlamak için IOT Edge çalışma zamanı yapılandırın.

Arka plan programı, yapılandırma dosyası kullanılarak yapılandırılabilir `/etc/iotedge/config.yaml`. Dosya yazma korumalı varsayılan olarak, düzenlemek için yükseltilmiş izinlere ihtiyaç duyabilirsiniz.

Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti sağlamak için birçok cihaz olduğunda kullanışlı olan cihazları otomatik olarak sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin.

### <a name="option-1-manual-provisioning"></a>1. seçenek: El ile sağlama

El ile cihaz sağlama için ile sağlamak gereken bir [cihaz bağlantı dizesini](how-to-register-device-portal.md) IOT hub'ına yeni bir cihazı kaydederek oluşturabilirsiniz.

Yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Sağlama dosyasının bölümünü bulun. Açıklamadan çıkarın **el ile** hazırlama modu ve hazırlama modu dps dışında bırakılır emin olun. Değerini güncelleştirin **device_connection_string** IOT Edge cihazınızdan bağlantı dizesiyle.

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

Sağlama dosyasının bölümünü bulun. Açıklamadan çıkarın **dps** sağlama modu ve el ile bölüm yorum. Güncelleştirin **scope_id** ve **regıstratıon_ıd** değerler, IOT Hub cihazı sağlama hizmeti ve IOT Edge Cihazınızı TPM ile.

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

Kullandıysanız **el ile yapılandırma** adımlar önceki bölümde, IOT Edge çalışma zamanı başarıyla sağlanmış ve cihazınız üzerinde olmalıdır. Kullandıysanız **otomatik yapılandırma** adımları çalışma zamanı ile sizin adınıza IOT hub'ınıza Cihazınızı kaydetmek için bazı ek adımları tamamlaması gerekir. Sonraki adımlar için bkz: [oluşturma ve sağlama Linux sanal makinesinde sanal bir TPM IOT Edge cihazı](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Kısıtlanmış bir kaynak cihazlarda ayarlamanız önerilir *OptimizeForPerformance* ortam değişkenine *false* yönergeleri uyarınca [sorun giderme kılavuzu ](troubleshoot.md).

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

Düzgün bir şekilde yükleme IOT Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
