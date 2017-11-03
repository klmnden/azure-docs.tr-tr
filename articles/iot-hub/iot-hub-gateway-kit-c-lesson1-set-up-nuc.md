---
title: "SensorTag cihaz & Azure IOT ağ geçidi - Ders 1: Intel NUC ayarlama | Microsoft Docs"
description: "Algılayıcı ve algılayıcı bilgileri toplamak ve IOT Hub'ına göndermek için Azure IOT Hub arasında bir IOT ağ geçidi olarak çalışmak için Intel NUC ayarlayın."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT ağ geçidi, Intel nuc nuc bilgisayar, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Intel NUC IOT ağ geçidi olarak ayarlayın
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Ne yapacağını

- Intel NUC IOT ağ geçidi olarak ayarlayın.
- Azure IOT kenar paketi üzerinde Intel NUC yükleyin.
- "Hello_world" örnek bir uygulama ağ geçidi işlevlerini doğrulamak için Intel NUC üzerinde çalıştırın.

  > Herhangi bir sorun varsa, çözümleri için Ara [sorun giderme sayfası](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

Bu alıştırmanın ilerisinde şunları öğreneceksiniz:

- Intel NUC çevre birimleri ile bağlanma.
- Yükleme ve Intel akıllı Paket Yöneticisi'ni kullanarak NUC gerekli paketleri güncelleştirme nasıl.
- Ağ geçidi işlevlerini doğrulamak için "hello_world" örnek uygulamayı çalıştırmak nasıl.

## <a name="what-you-need"></a>Ne gerekiyor

- Bir Intel NUC Seti DE3815TYKE Intel IOT ağ geçidi yazılım paketi ile (Rüzgar Akarsu Linux * 7.0.0.13) önceden yüklenmiş. [Grove IOT ticari ağ geçidi Seti satın almak için burayı tıklatın](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Ethernet kablosu.
- Klavye.
- Bir HDMI veya VGA kablosu.
- Bir HDMI veya VGA bağlantı noktasına sahip bir izleme.
- İsteğe bağlı: [Texas Instruments algılayıcı etiketi (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Ağ geçidi Seti](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Intel NUC çevre birimleri ile bağlanma

Aşağıdaki resimde, çeşitli çevre birimleri ile bağlı Intel NUC örneğidir:

1. Bir klavye bağlı.
2. Bir izleyici VGA kablosu veya HDMI kablosuyla bağlı.
3. Ethernet kablosu ile kablolu bir ağa bağlı.
4. Güç kaynağı güç kablosu ile bağlı.

![Intel NUC çevre birimlerine bağlı](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Ana bilgisayarından yoluyla güvenli Kabuk (SSH) Intel NUC sisteme bağlanma

Klavye ve Intel NUC cihazınızın IP adresini almak için bir izleme gerekir. IP adresi zaten biliyorsanız, bu bölümdeki 3. adım atlayabilirsiniz.

1. Güç düğmesine basarak üzerinde Intel NUC kapatma ve oturum açın.

   Varsayılan kullanıcı adı ve parola `root`.

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Çalıştırarak Intel NUC IP adresini alın `ifconfig` Intel NUC aygıtta komutu.

   Komut çıktısı bir örneği burada verilmiştir.

   ![Intel NUC IP gösteren ifconfig çıktı](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   Bu örnekte, değer, izleyen `inet addr:` Intel NUC bir ana bilgisayardan bağlandığınızda gereken IP adresidir.

3. Intel NUC bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın.

    - [PuTTY](http://www.putty.org/) Windows için.
    - Ubuntu veya macOS üzerindeki yerleşik SSH istemcisi.

   Daha etkili ve verimli bir ana bilgisayardan bir Intel NUC çalışması için. Intel NUC'ın IP adresi, kullanıcı adı ve SSH istemcisi bağlanmak için parola gerekir. MacOS üzerinde SSH istemcisi kullanan örnek aşağıda verilmiştir.
   ![MacOS üzerinde çalışan SSH istemcisi](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Azure IOT kenar paketini yükle

Azure IOT kenar paket IOT kenarı ve bağımlılıklarını önceden derlenmiş ikili dosyalarını içerir. Bu ikili dosyaları Azure IOT sınır, Azure IOT SDK'sı ve ilgili araçlar vardır. Ayrıca "hello_world" pakette örnek uygulama ağ geçidi işlevlerini doğrulamak için kullanılır. IOT sınır ağ geçidi çekirdek parçasıdır. 

Paketi yüklemek için aşağıdaki adımları izleyin.

1. Bir terminal penceresi aşağıdaki komutları çalıştırarak IOT bulut deposunu ekleyin:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > 'Y', 'Bu kanal edilsin mi?' istediğinde girin
   
   Alırsanız, bir `import read failed(-1)` hatası, bu sorunu çözmek için aşağıdaki komutları kullanın:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   `rpm` Komut rpm anahtarı alır. `smart channel` Komut akıllı Paket Yöneticisi rpm kanal ekler. Çalıştırmadan önce `smart update` görürsünüz, komut çıktısı ister aşağıda.

   ![RPM ve akıllı kanal komutları çıkış](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Akıllı güncelleştirme komutu yürütün:

   ```bash
   smart update
   ```

3. Aşağıdaki komutu çalıştırarak Azure IOT ağ geçidi paketini yükle:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`Paket adıdır. `smart install` Komutu, paketi yüklemek için kullanılır.

    > Bu hatayı görürseniz aşağıdaki komutu çalıştırın: 'ortak anahtar yok'

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Bu hatayı görürseniz Intel NUC yeniden: 'hiçbir paket kul linux geliştirme sağlar'

   Paket yüklendikten sonra Intel NUC bir ağ geçidi olarak çalışmaya hazırdır.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Azure IOT kenar "hello_world" örnek uygulamayı çalıştırın

Aşağıdaki örnek uygulama ağ geçidi'nden oluşturur bir `hello_world.json` dosyası ve günlük dosyasında (log.txt) 5 saniyede bir hello world ileti için Azure IOT kenar mimarisinin temel bileşenlerini kullanır.

Aşağıdaki komutları yürüterek Hello World örneği çalıştırabilirsiniz:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Birkaç dakika çalıştırın ve sonra bunu durdurmak için Enter tuşuna basın Hello World uygulamasını sağlar.
![Uygulama çıktısı](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Enter tuşuna basın sonra görüntülenen 'geçersiz bağımsız değişken handle(NULL)' hataları yoksayabilirsiniz.

Şimdi hello_world klasörünüzde log.txt dosyasını açarak ağ geçidi başarıyla çalıştırıldığını doğrulayın ![log.txt dizin görünümü](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Aşağıdaki komutu kullanarak log.txt açın:

```bash
vim log.txt
```

Ardından, her 5 saniyede bir ağ geçidi Hello World modülü tarafından yazılmış günlük iletilerini biçimlendirilmiş JSON çıktısını olacaktır log.txt içeriğini görürsünüz.
![log.txt dizin görünümü](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Herhangi bir sorun varsa, çözümleri için Ara [sorun giderme sayfası](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Özet

Tebrikler! Intel NUC ayarlama bir ağ geçidi olarak tamamladınız. Artık ana bilgisayarınızı ayarlama, Azure IOT Hub oluşturma ve Azure IOT hub'ı mantıksal Cihazınızı kaydetmek için sonraki Ders geçmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar
[Bir aygıt Azure IOT Hub'ına bağlanmak için bir IOT ağ geçidi kullanın](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

