---
title: DPS - Azure IOT Edge ile Linux cihazı otomatik olarak hazırlarsınız | Microsoft Docs
description: Azure cihaz sağlama hizmeti, Azure IOT Edge için test etmek için bir Linux VM üzerinde sanal bir TPM kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9a549221a9e1864e1b7565f35139cb4c2a6ca65e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61248122"
---
# <a name="create-and-provision-an-iot-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Bir Linux sanal makinesinde sanal bir TPM ile IOT Edge cihazı oluşturma ve sağlama

Azure IOT Edge cihazları otomatik olarak sağlanabilir kullanarak [cihaz sağlama hizmeti](../iot-dps/index.yml). Autoprovisioning işlemine bilmiyorsanız gözden [autoprovisioning kavramları](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce. 

Bu makalede aşağıdaki adımlarla benzetimli bir IOT Edge cihazı üzerindeki autoprovisioning test gösterilmektedir: 

* Hyper-V ile bir sanal Güvenilir Platform Modülü (TPM) donanımı güvenlik için bir Linux sanal makinesini (VM) oluşturun.
* Bir örnek, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Cihazı için bireysel kayıt oluşturma
* IOT Edge çalışma zamanı yükleme ve cihaz IOT hub'a bağlama

Bu makaledeki adımlarda, test amacıyla yöneliktir.

## <a name="prerequisites"></a>Önkoşullar

* Bir Windows geliştirme makinesi ile [Hyper-V etkin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). Bu makalede, Windows 10 çalıştıran bir Ubuntu Server VM kullanır. 
* Etkin bir IOT Hub. 

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Sanal bir TPM ile bir Linux sanal makinesi oluşturma

Bu bölümde, Hyper-V üzerinde yeni bir Linux sanal makine oluşturun. Böylece IOT Edge ile nasıl otomatik sağlama works test etmek için bunu kullanabilirsiniz Bu sanal makineye sanal bir TPM ile yapılandırılmış. 

### <a name="create-a-virtual-switch"></a>Sanal anahtar oluşturma

Bir sanal anahtar, fiziksel bir ağa bağlanmak sanal makinenizi sağlar.

1. Windows makinenizde Hyper-V Yöneticisi'ni açın. 

2. İçinde **eylemleri** menüsünde **sanal Anahtar Yöneticisi**. 

3. Seçin bir **dış** sanal geçiş yapın ve ardından **sanal anahtar oluşturma**. 

4. Yeni sanal anahtarınızın Örneğin, bir ad verin **EdgeSwitch**. Bağlantı türü ayarlandığından emin olun **dış ağ**, ardından **Tamam**.

5. Bir açılır pencere, ağ bağlantısı kesilebilir sizi uyarır. Seçin **Evet** devam etmek için. 

Yeni sanal anahtar oluştururken hatalar görürseniz, diğer bir anahtarlar ethernet bağdaştırıcısı kullandığından emin olun ve diğer bir anahtarlar aynı adı kullanın. 

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Sanal makineniz için kullanın ve yerel olarak kaydetmek için disk görüntü dosyasını indirin. Örneğin, [Ubuntu server](https://www.ubuntu.com/download/server). 

2. Hyper-V Yöneticisi'nde yeniden seçin **yeni** > **sanal makine** içinde **eylemleri** menüsü.

3. Tamamlamak **yeni sanal makine Sihirbazı** aşağıdaki yapılandırmalarla:

   1. **Nesli belirtmeniz**: Seçin **2. nesil**. 2\. nesil sanal makineler sanallaştırma etkinken bir sanal makine üzerinde IOT Edge çalıştırmak için gerekli olan iç içe geçmiş.
   2. **Ağ Yapılandırma**: Değerini **bağlantı** , önceki bölümde oluşturduğunuz sanal anahtara. 
   3. **Yükleme Seçenekleri**: Seçin **bir önyükleme görüntü dosyasından bir işletim sistemini yüklemek** ve yerel olarak kaydettiğiniz disk görüntü dosyasına göz atın.

4. Seçin **son** sanal makine oluşturma Sihirbazı'nda.

Bu, yeni bir VM oluşturmak için birkaç dakika sürebilir. 

### <a name="enable-virtual-tpm"></a>Sanal TPM etkinleştir

VM oluşturulduktan sonra cihaz autoprovision sağlayan sanal güvenilir platform Modülü (TPM) etkinleştirmek için ayarlarını açın. 

1. Sanal makineyi seçin ve ardından açın, **ayarları**.

2. Gidin **güvenlik**. 

3. Onay kutusunu temizleyin **Güvenli Önyükleme etkinleştirme**.

4. Denetleme **Güvenilir Platform Modülü'nü etkinleştirme**. 

5. **Tamam** düğmesine tıklayın.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>TPM veri toplamak ve sanal makineyi Başlat

Sanal makine, derleme, cihazın almak için kullanabileceğiniz bir C SDK'sı aracı **kayıt kimliği** ve **onay anahtarını**. 

1. Sanal makineyi başlatmak ve ona bağlanabilirsiniz.

2. Makineyi yeniden başlatın ve yükleme işlemini bitirmek için sanal makine içindeki yönergeleri izleyin. 

3. Sanal makinenizde oturum açın ve ardından adımları [bir Linux geliştirme ortamı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) yükleyin ve c için Azure IOT cihaz SDK'sını derleme 

   >[!TIP]
   >Bu makalede sırasında kopyalayın ve Hyper-V Yöneticisi'ni bağlantı uygulama kolay değil, sanal makineden yapıştırın. Sanal makineye Hyper-V IP adresini almak için Yöneticisi aracılığıyla bir kez bağlamak isteyebilirsiniz: `ifconfig`. Ardından, SSH bağlanmak için bir IP adresi kullanabilirsiniz: `ssh <username>@<ipaddress>`.

4. Derleme, cihaz sağlama bilgilerini alır bir C SDK'sı aracı için aşağıdaki komutları çalıştırın. 

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

5. Değerlerini kopyalayın **kayıt kimliği** ve **onay anahtarını**. DPS, cihazınız için bireysel kayıt oluşturmak için bu değerleri kullanırsınız. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmetini ayarlama

Azure'da yeni bir IOT Hub cihazı sağlama hizmeti örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri takip edebilirsiniz [IOT hub'ı DPS ' ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışıyor sonra değerini kopyalayın **kimlik kapsamı** genel bakış sayfasında. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın. 

## <a name="create-a-dps-enrollment"></a>DPS kayıt oluşturma

Sanal makinenizden sağlama bilgilerini almak ve, cihaz sağlama hizmetinde bireysel kayıt oluşturmak için kullanın. 

DPS'de bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz İkizi durumu**. Cihaz ikizinde bölge, ortam, konuma veya cihaz türü gibi çözümünüzdeki gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketleri oluşturmak için kullanılan [otomatik dağıtımlar](how-to-deploy-monitor.md). 


1. İçinde [Azure portalında](https://portal.azure.com)ve IOT Hub cihazı sağlama hizmeti örneğine gidin. 

2. Altında **ayarları**seçin **kayıtları Yönet**. 

3. Seçin **Ekle bireysel kayıt** ardından kayıt yapılandırmak için aşağıdaki adımları tamamlayın:  

   1. İçin **mekanizması**seçin **TPM**. 
   
   2. Sağlamak **onay anahtarını** ve **kayıt kimliği** sanal makinenizden kopyaladığınız.
   
   3. Seçin **True** bu sanal makinenin bir IOT Edge cihazı olduğunu bildirmek için. 
   
   4. Bağlantılı seçin **IOT hub'ı** cihazınıza bağlanmak istiyorsanız. Birden çok hub'a seçebilir ve cihaz seçilen ayırma ilkesine göre bunları birine atanır. 
   
   5. İsterseniz cihazınız için bir kimlik belirtin. Cihaz kimliklerini modülü dağıtımı için tek bir cihaza hedeflemek için kullanabilirsiniz. Bir cihaz kimliği sağlamıyorsa, kayıt kimliği kullanılır.
   
   6. Etiketi değer eklemek **ilk cihaz İkizi durumu** istiyorsanız. Hedef cihaz gruplarına etiketleri modülü dağıtımı için kullanabilirsiniz. Örneğin: 

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   7. **Kaydet**’i seçin. 

Bu cihaz için bir kayıt var, IOT Edge çalışma zamanı yüklemesi sırasında cihaz otomatik olarak sağlayabilirsiniz. 

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarında çalıştırmak ve kod ucuna çalıştırabilmeniz için cihaza ek kapsayıcıları dağıtma olanak sağlar. IOT Edge çalışma zamanı, sanal makinenize yükleyin. 

DPS'niz bilmeniz **kimlik kapsamı** ve cihaz **kayıt kimliği** cihaz türünüzle eşleşen makaleye başlamadan önce. Örnek Ubuntu server yüklü değilse, kullanın **x64** yönergeleri. Otomatik değil el ile sağlama için IOT Edge çalışma zamanı yapılandırdığınızdan emin olun. 

* [Azure IOT Edge çalışma zamanı (x64) Linux'ta yükleme](how-to-install-iot-edge-linux.md)
* [Azure IOT Edge çalışma zamanı (ARM32v7/armhf) Linux'ta yükleme](how-to-install-iot-edge-linux-arm.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>IOT Edge erişmesini TPM'ye

Cihazınızı otomatik olarak sağlamak IOT Edge çalışma zamanı için sırada TPM erişimi gerekir. 

Systemd ayarlarını geçersiz kılma tarafından IOT Edge çalışma zamanına TPM erişim verebilir böylece **iotedge** hizmet kök ayrıcalıklara sahiptir. Hizmet ayrıcalıklarını istemiyorsanız, aşağıdaki adımları el ile TPM erişim sağlamak için kullanabilirsiniz. 

1. Cihazınızın TPM donanım modülü olan dosya yolunu bulun ve yerel bir değişkene kaydedin. 

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. IOT Edge çalışma zamanı için tpm0 sürümlere erişmenizi sağlayacaktır yeni bir kural oluşturun. 

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Kurallar dosyası açın. 

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Aşağıdaki erişim bilgileri kuralları dosyaya kopyalayın. 

   ```input 
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Kaydedin ve dosyayı çıkın. 

6. Yeni Kural değerlendirilemedi udev sistem tetikleyin. 

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Kural başarıyla uygulandığını doğrulayın.

   ```bash
   ls -l /dev/tpm0
   ```

   Başarılı çıkış aşağıdaki gibi görünür:

   ```output
   crw-rw---- 1 root iotedge 10, 224 Jul 20 16:27 /dev/tpm0
   ```

   Doğru izinler uygulanmış görmüyorsanız udev yenilemek için makinenizi yeniden deneyin. 

8. Açık IOT Edge çalışma zamanı dosyasını geçersiz kılar. 

   ```bash
   sudo systemctl edit iotedge.service
   ```

9. TPM ortam değişkenini oluşturmak için aşağıdaki kodu ekleyin.

   ```input
   [Service]
   Environment=IOTEDGE_USE_TPM_DEVICE=ON
   ```

10. Kaydedin ve dosyayı çıkın.

11. Geçersiz kılma işleminin başarılı olduğunu doğrulayın.

    ```bash
    sudo systemctl cat iotedge.service
    ```

    Başarılı çıkış görüntüler **iotedge** varsayılan hizmet değişkenleri ve ardından ortam değişkenini ayarladığınız olduğunu gösterir. **override.conf**. 

12. Ayarları yeniden yükleyin.

    ```bash
    sudo systemctl daemon-reload
    ```

## <a name="restart-the-iot-edge-runtime"></a>IOT Edge çalışma zamanı yeniden başlatın

Bu cihaz üzerinde yaptığınız tüm yapılandırma değişiklikleri alır, böylece IOT Edge çalışma zamanı yeniden başlatın. 

   ```bash
   sudo systemctl restart iotedge
   ```

IOT Edge çalışma zamanı çalışıp çalışmadığını denetleyin. 

   ```bash
   sudo systemctl status iotedge
   ```

Sağlama hataları görüyorsanız, yapılandırma değişikliklerinin henüz geçerlik kazanmadı olabilir. IOT Edge daemon'ı yeniden başlatmayı deneyin. 

   ```bash
   sudo systemctl daemon-reload
   ```
   
Ya da değişiklikler yeni bir başlangıç etkisi olur, görmek için sanal makinenizi yeniden başlatmayı deneyin. 

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatıldı, IOT Hub'ına gidin ve yeni Cihazınızı otomatik olarak sağlanan bakın. Artık Cihazınızı IOT Edge modüllerini çalıştırmak hazırdır. 

IOT Edge Daemon durumunu denetleyin.

```cmd/sh
systemctl status iotedge
```

Arka plan programının günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listeleyin.

```cmd/sh
iotedge list
```

Cihaz sağlama hizmetinde oluşturduğunuz bireysel kayıt kullanıldığını doğrulayabilirsiniz. Azure portalında cihaz sağlama hizmeti Örneğinize gidin. Kayıt ayrıntıları, oluşturduğunuz tek kayıt için açın. Kayıt durumu olduğuna dikkat edin **atanan** ve cihaz kimliği listelenir. 

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihaz sağlama gibi cihaz kimliği ve cihaz ikizi etiketleri aynı anda belirlemenizi sağlar. Bu değerleri ayrı ayrı cihazlar ya da otomatik cihaz Yönetimi'ni kullanarak cihaz grupları hedeflemek için kullanabilirsiniz. Bilgi edinmek için nasıl [dağıtma ve izleme IOT Edge modülleri, ölçeklendirme Azure portalını kullanarak](how-to-deploy-monitor.md) veya [Azure CLI kullanarak](how-to-deploy-monitor-cli.md).
