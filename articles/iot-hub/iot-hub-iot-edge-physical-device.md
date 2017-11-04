---
title: "Bir fiziksel aygıt ile Azure IOT kenar kullanın | Microsoft Docs"
description: "Texas Instruments SensorTag aygıt bir IOT hub'ı Raspberry Pi 3 cihazda çalışan IOT sınır ağ geçidi aracılığıyla veri göndermek için nasıl kullanılacağını. Ağ geçidi, Azure IOT kenar kullanılarak oluşturulur."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: andbuc
ms.openlocfilehash: b24828ee1a09ba8e5f657954e11936f124270173
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="forward-device-to-cloud-messages-to-iot-hub-using-azure-iot-edge-on-a-raspberry-pi"></a>IOT Hub'ın Azure IOT kenar Raspberry Pi'yi kullanarak cihaz bulut iletilerini ilet

Bu kılavuz [Bluetooth düşük enerji örnek] [ lnk-ble-samplecode] nasıl kullanılacağını gösterir [Azure IOT kenar] [ lnk-sdk] için:

* Cihaz bulut telemetri IOT Hub'ına fiziksel CİHAZDAN iletin.
* Bir fiziksel aygıt için rota komutları IOT hub'dan.

Bu kılavuzda aşağıdaki konular ele alınmaktadır:

* **Mimari**: Bluetooth düşük enerji örnek hakkında önemli mimari bilgiler.
* **Derleme ve çalıştırma**: örneği derlemek ve çalıştırmak için gerekli adımlar.

## <a name="architecture"></a>Mimari

İzlenecek yol oluşturun ve IOT sınır ağ geçidi Raspbian Linux çalıştıran bir Raspberry Pi 3 üzerinde çalışmasını gösterilmektedir. Ağ geçidi IOT kenar kullanılarak oluşturulur. Örnek bir Texas Instruments SensorTag Bluetooth düşük enerji (bırak) aygıtı sıcaklık verileri toplamak için kullanır.

IOT sınır ağ geçidi çalıştırdığınızda bu:

* Bir SensorTag cihazda Bluetooth düşük enerji (bırak) protokolünü kullanarak bağlanır.
* IOT Hub HTTPS protokolünü kullanarak bağlanır.
* Telemetri SensorTag cihazın IOT Hub'ına iletir.
* IOT hub'ı komutları SensorTag aygıta yönlendirir.

Ağ geçidi aşağıdaki IOT kenar modüllerini içerir:

* A *bırak Modülü* aygıttan sıcaklık veri almak ve cihaza komut gönderme için bırak aygıtıyla arabirimleri.
* A *aygıt modülü bırak buluta* bırak yönergeler için içine IOT Hub'ından gönderilen JSON iletileri çevirir *bırak Modülü*.
* A *Günlükçü Modülü* , tüm ağ geçidi iletilerini yerel bir dosyaya kaydeder.
* Bir *kimlik eşleme Modülü* bırak aygıt MAC adresi ile Azure IOT Hub cihaz kimlikleri arasında çevirir.
* Bir *IOT hub'ı Modülü* bir IOT hub'ına telemetri verileri yükler ve IOT hub'ından cihaz komutlarını alır.
* A *bırak yazıcı Modülü* bırak cihaz telemetrisinden yorumlar ve baskı siparişi biçimlendirilmiş sorun giderme ve hata ayıklamayı etkinleştirmek için konsola veri.

### <a name="how-data-flows-through-the-gateway"></a>Veri ağ geçidi üzerinden nasıl akar

Aşağıdaki Blok Diyagramı telemetri karşıya yükleme veri akışı ardışık gösterilmektedir:

![Telemetri karşıya yükleme ağ geçidi ardışık düzen](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

IOT Hub'ına bırak aygıttan seyahat öğeyi telemetri götüren adımlar şunlardır:

1. BIRAK aygıt sıcaklık örnek oluşturur ve ağ geçidi bırak modülünde Bluetooth üzerinden gönderir.
1. BIRAK modülü örnek alır ve aygıtın MAC adresi birlikte Aracısı yayımlar.
1. Kimlik eşleme modülü bu iletiyi alır ve bir IOT Hub cihaz kimliği aygıt MAC adresi çevirmek için bir iç tablosunu kullanır. Bir IOT Hub cihaz kimliği, bir cihaz kimliği ve aygıt anahtarı oluşur.
1. Kimlik eşleme modülü şu bilgileri içeren yeni bir ileti yayımlar:
   - Sıcaklık örnek veriler
   - Aygıt MAC adresi
   - Cihaz kimliği
   - Aygıt anahtarı  
1. IOT hub'ı Modülü (kimlik eşleme modülü tarafından oluşturulan) bu yeni bir ileti alır ve IOT Hub'ına yayımlar.
1. Günlükçü modülü tüm iletileri Aracısı'ndan yerel bir dosyaya kaydeder.

Cihaz komutu veri akışı ardışık aşağıdaki blok diyagramını gösterir:

![Cihaz komut ağ geçidi ardışık düzeni](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. IOT hub'ı modülü IOT hub'ı yeni komut iletileri için düzenli aralıklarla yoklar.
1. IOT hub'ı modülü yeni bir komut iletisi aldığında, belgeyi broker yayımlar.
1. Kimlik eşleme modülü komutu iletinin seçer ve bir aygıt MAC adresi için IOT Hub cihaz kimliği çevirmek için bir iç tablosunu kullanır. Ardından, ileti özelliklerini eşlemesinde hedef aygıt MAC adresi içeren yeni bir ileti yayımlar.
1. BIRAK bulut-cihaz modülü bu iletiyi alır ve doğru bırak yönerge bırak modülü için çevirir. Ardından, yeni bir ileti yayımlar.
1. BIRAK modülü bu iletiyi alır ve g/ç yönerge bırak aygıtla iletişim kurarak çalıştırır.
1. Günlükçü modülü tüm iletileri Aracısı'ndan bir disk dosyasına kaydeder.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

> [!NOTE]
> Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].

Komut satırı Raspberry Pi'yi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](http://www.putty.org/).
- Çoğu Linux dağıtımları ve Mac OS komut satırı SSH yardımcı programı içerir. Daha fazla bilgi için bkz: [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Donanımınızın hazırlama

Bu öğreticide kullandığınız varsayılır bir [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) aygıt bağlı Raspbian çalıştıran bir Raspberry Pi 3.

### <a name="install-raspbian"></a>Raspbian yükleyin

Raspbian Raspberry Pi 3 aygıtınızda yüklemek için aşağıdaki seçeneklerden birini kullanabilirsiniz.

* Raspbian en son sürümünü yüklemek için kullandığınız [NOOBS] [ lnk-noobs] grafik kullanıcı arabirimi.
* El ile [karşıdan] [ lnk-raspbian] ve en son Raspbian işletim sistemi görüntüsünü bir SD kartına yazma.

### <a name="sign-in-and-access-the-terminal"></a>Oturum açma ve terminal erişim

Raspberry Pi'yi terminal ortamda erişmek için iki seçeneğiniz vardır:

* Klavye ve monitör, Raspberry Pi'yi bağlı varsa, bir terminal penceresi erişmek için Raspbian GUI kullanabilirsiniz.

* Masaüstü makinenizden SSH kullanarak, Raspberry Pi'yi komut satırında erişin.

#### <a name="use-a-terminal-window-in-the-gui"></a>GUI içinde bir terminal penceresi kullanın

Kullanıcı adı Raspbian için varsayılan kimlik bilgileri olan **PI** ve parola **raspberry**. GUI görev çubuğunda, başlatabilirsiniz **Terminal** gibi bir izleyici arar simgesini kullanarak yardımcı programı.

#### <a name="sign-in-with-ssh"></a>Oturum SSH oturum

SSH, Raspberry Pi'yi komut satırı erişimi için kullanabilirsiniz. Makaleyi [SSH (Secure Shell)] [ lnk-pi-ssh] , Raspberry Pi'yi SSH yapılandırma ve bağlanması açıklar [Windows] [ lnk-ssh-windows] veya [ Linux ve Mac OS][lnk-ssh-linux].

Kullanıcı adıyla oturum **PI** ve parola **raspberry**.

### <a name="install-bluez-537"></a>BlueZ 5.37 yükleyin

Bluetooth donanım BlueZ yığın aracılığıyla bırak modülleri konuşun. Doğru çalışması için BlueZ modülleri için 5.37 sürümü gerekir. Bu yönergeleri BlueZ doğru sürümünün yüklü olduğundan emin olun.

1. Geçerli bluetooth arka plan programı durdurun:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. BlueZ bağımlılıkları yükler:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf libtool glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. BlueZ kaynak kodu bluez.org yükleyin:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Kaynak kodu sıkıştırmasını açın:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Yeni oluşturulan klasöre dizinleri değiştirin:

    ```sh
    cd bluez-5.37
    ```

1. Oluşturulacak BlueZ kod yapılandırın:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. BlueZ oluşturun:

    ```sh
    make
    ```

1. Bunu yaptıktan sonra BlueZ yükleme oluşturma:

    ```sh
    sudo make install
    ```

1. Yeni bluetooth arka plan programı dosyasına işaret şekilde bluetooth systemd hizmet yapılandırmasını değiştirme `/lib/systemd/system/bluetooth.service`. 'ExecStart' satırın aşağıdaki metinle değiştirin:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Bağlantı SensorTag aygıta Raspberry Pi 3 aygıtınızdan etkinleştir

Örneği çalıştırmadan önce Raspberry Pi 3 SensorTag aygıta bağlanabileceği doğrulamanız gerekir.

1. Olun `rfkill` yardımcı programı yüklenir:

    ```sh
    sudo apt-get install rfkill
    ```

1. Bluetooth Raspberry Pi 3'te engelini kaldırmak ve sürüm numarasını olup olmadığını denetleyin **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. Etkileşimli bluetooth Kabuk girmek için bluetooth hizmetini başlatın ve yürütme **bluetoothctl** komutu:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Aşağıdaki komutu girin **güç açma** güç bluetooth denetleyicisi kurmak için. Komut çıktı aşağıdaki örneğe benzer şekilde döndürür:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. Etkileşimli bluetooth Kabuğu'nda komutu girin **taraması** bluetooth cihazları için taramak için. Komut çıktı aşağıdaki örneğe benzer şekilde döndürür:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. SensorTag aygıt bulunabilir (yeşil LED flash) küçük düğmesine basarak olun. Raspberry Pi 3 SensorTag aygıtı keşfet:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    Bu örnekte, SensorTag aygıt MAC adresi olduğunu görebilirsiniz **A0:E6:F8:B5:F6:00**.

1. Girerek taramayı kapatabilirsiniz **kapalı tarama** komutu:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. MAC adresini girerek kullanarak SensorTag Cihazınızı bağlanmak **bağlanmak \<MAC adresi\>**. Aşağıdaki örnek çıkış daha anlaşılır olması için kısaltılır:

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Kullanarak yeniden aygıtın GATT özelliklerini listeleyebilirsiniz **liste öznitelikleri** komutu.

1. Cihazı kullanarak artık bağlantısını kesebilirsiniz **bağlantısını** komut ve kullanarak bluetooth Kabuğu'ndan çıkın **çıkın** komutu:

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Artık Raspberry Pi 3'te bırak IOT kenar örneği çalıştırmak hazırsınız.

## <a name="run-the-iot-edge-ble-sample"></a>IOT kenar Bırak örneğini çalıştırın

IOT kenar bırak örneği çalıştırmak için üç görevleri tamamlamanız gerekir:

* İki örnek cihazlar IOT Hub'ınıza yapılandırın.
* IOT kenar Raspberry Pi 3 aygıtınızda oluşturun.
* Yapılandırın ve Raspberry Pi 3 aygıtınızda Bırak örneğini çalıştırın.

Yazma zaman IOT kenar yalnızca bırak modülleri Linux üzerinde çalışan ağ geçitleri de destekler.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>İki örnek cihazlar IOT hub'ınızı yapılandırma

* [IOT hub oluşturma] [ lnk-create-hub] Azure aboneliğinizde bu yönlendirmeyi tamamlamak için hub adı gerekiyor. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* Adlı bir aygıt Ekle **SensorTag_01** IOT hub ve kendi kimliği ve cihaz anahtarını Not. Kullanabileceğiniz [aygıt explorer veya iothub-explorer] [ lnk-explorer-tools] araçları önceki adımda oluşturduğunuz IOT hub'ına bu cihazı eklemeniz ve kendi anahtarını almak için. Ağ geçidi yapılandırdığınızda bu aygıtın SensorTag cihaza eşleyin.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Azure IOT kenar, Böğürtlenli Pi 3 derleme

Bağımlılıklar için Azure IOT kenar yükleyin:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev libtool
```

IOT kenarı ve kendi submodules giriş dizininize kopyalamak için aşağıdaki komutları kullanın:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

IOT kenar havuzu tam bir kopyasını Raspberry Pi 3'te olduğunda, SDK'sı içerir klasöründen aşağıdaki komutu kullanarak oluşturabilirsiniz:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Yapılandırma ve Raspberry Pi 3'te bırak örnek çalıştırma

Bootstrap ve örneği çalıştırmak için ağ geçidi katılan her IOT kenar modülü yapılandırın. Bu yapılandırma bir JSON dosyası sağlanır ve tüm beş katılımcı IOT kenar modülleri yapılandırmanız gerekir. Adlı depo örnek bir JSON dosyası olduğu **ağ geçidi\_sample.json** kendi yapılandırma dosyası oluşturmak için başlangıç noktası olarak kullanabilirsiniz. Bu dosya **ble_gateway/samples/src** IOT kenar depoyu yerel kopyasını klasöründe.

Aşağıdaki bölümlerde bu yapılandırma dosyasının bırak örnek için nasıl düzenleneceği açıklanmaktadır. IOT kenar havuzu olduğunu varsayıyoruz **/home/pi/iot-edge /** klasörü Raspberry Pi 3. Havuz başka bir yerde ise, yolları uygun şekilde ayarlayın.

#### <a name="logger-configuration"></a>Günlükçü yapılandırma

Ağ geçidi deposu varsayılarak bulunduğu **/home/pi/iot-edge /** klasörünü Günlükçü Modülü aşağıdaki gibi yapılandırın:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>BIRAK modül yapılandırması

BIRAK cihaz için örnek yapılandırma Texas Instruments SensorTag aygıt varsayar. Çevre bir GATT çalışmalıdır olarak çalışabilir herhangi bir standart bırak aygıtı ancak, verilere ve GATT özellik kimlikleri güncelleştirmeniz gerekebilir. SensorTag cihazınızın MAC adresini ekleyin:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

SensorTag aygıt kullanmıyorsanız GATT karakteristiğini kimlikleri ve veri değerleri güncelleştirme gerekip gerekmediğini belirlemek bırak cihazınız için belgelerini gözden geçirin.

#### <a name="iot-hub-module"></a>IOT hub'ı Modülü

IOT hub'ınızın adını ekleyin. Sonek genellikle değerdir **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Kimlik eşleme modülü yapılandırması

MAC adresi SensorTag Cihazınızı cihaz kimliği ve anahtarı eklemek **SensorTag_01** aygıt IOT Hub'ınıza eklendi:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>BIRAK yazıcı modülü yapılandırması

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>BLEC2D modülü yapılandırması

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Yönlendirme yapılandırması

Aşağıdaki yapılandırma aşağıdaki IOT kenar modülleri arasında yönlendirme sağlar:

* **Günlükçü** modülü alır ve tüm iletileri günlüğe kaydeder.
* **SensorTag** modülü hem de iletileri gönderir **eşleme** ve **bırak yazıcı** modüller.
* **Eşleme** modülü iletileri gönderir **Iothub** IOT Hub'ına gönderilen modülü.
* **Iothub** modül gönderir iletileri başa **eşleme** modülü.
* **Eşleme** modülü iletileri gönderir **BLEC2D** modülü.
* **BLEC2D** modül gönderir iletileri başa **algılayıcı etiketi** modülü.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Örneği çalıştırmak için bir parametre olarak JSON yapılandırma dosyası yolu geçirmek **bırak\_ağ geçidi** ikili. Aşağıdaki komut, kullanmakta olduğunuz varsayar **gateway_sample.json** yapılandırma dosyası. Bu komutu yürütmek **IOT kenar** Raspberry Pi'yi klasörü:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Küçük bir düğme örneği çalıştırmadan önce bulunabilir yapmak için SensorTag aygıtta basmanız gerekebilir.

Örneği çalıştırdığınızda, kullanabileceğiniz [aygıt explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) veya [iothub-explorer](https://github.com/Azure/iothub-explorer) IOT sınır ağ geçidi iletir SensorTag aygıttan iletileri izlemek için aracı. Örneğin, ıothub explorer kullanarak aşağıdaki komutu kullanarak cihaz bulut iletilerini izleyebilirsiniz:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

BIRAK modülü gönderen komutlarından IOT Hub cihaz için de destekler. Kullanabilirsiniz [aygıt explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) veya [iothub-explorer](https://github.com/Azure/iothub-explorer) bırak ağ geçidi modülü açın bırak aygıt iletir JSON iletileri gönderme aracı.

Texas Instruments SensorTag cihaz kullanıyorsanız, IOT Hub'ından komutlar göndererek kırmızı LED, yeşil LED veya sesli uyaran kapatabilirsiniz. İlk IOT Hub'ından komutları göndermeden önce aşağıdaki iki JSON ileti sırada gönderin. Ardından, ışık veya sesli uyaran için komutlardan herhangi birini gönderebilirsiniz.

1. Tüm LED'leri ve sesli uyaran sıfırlama (devre dışı bırakma):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. G/ç 'uzaktan' yapılandırın:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Şimdi ışık veya sesli uyaran SensorTag cihazda etkinleştirmek için aşağıdaki komutlardan herhangi birini gönderebilirsiniz:

* Üzerinde kırmızı LED açın:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Yeşil LED üzerinde açın:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Sesli uyaran üzerinde açın:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Sonraki Adımlar

IOT kenar daha gelişmiş anlamak ve bazı kod örnekleri ile denemek istiyorsanız, aşağıdaki Geliştirici öğreticiler ve kaynakları ziyaret edin:

* [Azure IOT kenar][lnk-sdk]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
