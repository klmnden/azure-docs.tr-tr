---
title: "SensorTag cihaz & Azure IOT ağ geçidi - sorunlarını giderme | Microsoft Docs"
description: "Sayfa Intel NUC ağ geçidi için sorun giderme"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT sorunları, Internet şeyler sorunları"
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7e80951de55ade6b5140608dcff8ebb064f942ca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting"></a>Sorun giderme

## <a name="hardware-issues"></a>Donanım sorunları

### <a name="ti-sensortag-cannot-be-connected"></a>TI SensorTag bağlı

SensorTag bağlantı sorunlarını gidermek için kullanmak [SensorTag uygulama](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Intel NUC ile ilgili bir sorun olması

Önyükleme sorunlarını gidermek için bkz [Intel NUC üzerinde Hayır önyükleme sorunlarını giderme](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

İşletim sistemi sorunlarını gidermek için bkz [Intel NUC üzerinde işletim sistemi sorunlarını giderme](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

Diğer sorunları gidermek için bkz [Blink kodları ve Intel NUC kodları bip sesi](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Node.js paket sorunları

### <a name="no-response-during-gulp-tasks"></a>Gulp görevler sırasında yanıt yok

Ekleyebileceğiniz gulp görevleri çalıştırma sorunlarla karşılaşmanız halinde, `--verbose` hata ayıklama seçeneği. Geçerli gulp görevleri kullanarak sonlandırmak deneyin `Ctrl + C`, hata ayıklama iletileri aşağıdaki görmek için konsol penceresinde komutunu çalıştırın. Ayrıntılı hata iletileri, konsol çıktısı görebilirsiniz.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Aygıt Bulma sorunları

İlgili genel sorunları gidermede yardım için `discover-sensortag` komutu, onay [wiki sayfa](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm sorunları

Aşağıdaki komutu çalıştırarak npm paket güncelleştirmeyi deneyin:

```bash
npm install -g npm
```

Sorun devam ederse, bu makalenin sonunda yorumlarınızı bırakın veya bir GitHub sorunu oluşturmak bizim [örnek depo](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Uzaktan hata ayıklama
> Bu öğreticide kullanılan node.js komut hata ayıklama için aşağıdaki yönergeleri yöneliktir.
### <a name="run-the-sample-application-in-debug-mode"></a>Örnek uygulamayı hata ayıklama modunda çalıştırın

Örnek uygulama, aşağıdaki komutu çalıştırarak hata ayıklama modunda çalıştırın:

```bash
gulp run --debug
```

Hata ayıklama altyapısı hazır olduğunda, görmelisiniz `Debugger listening on port 5858` konsol çıkışı.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Visual Studio Code uzak cihaza bağlanmak için yapılandırın

1. Açık **hata ayıklama** sol tarafındaki paneli.
2. Yeşil tıklatın **hata ayıklamayı Başlat** (F5) düğmesi. Visual Studio Code açılır bir `launch.json` dosyası.
3. Güncelleştirme `launch.json` dosya aşağıdaki içeriğe sahip. Değiştir `[device hostname or IP address]` gerçek aygıt IP adresi veya ana bilgisayar adı.

   ``` json
   {
     "version": "0.2.0",
     "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Uzaktan hata ayıklama yapılandırması](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Uzak uygulamasına ekleme

Yeşil tıklatın **hata ayıklamayı Başlat** hata ayıklamayı başlatmak için (F5) düğmesini.

Okuma [JavaScript VS code'da](https://code.visualstudio.com/docs/languages/javascript#_debugging) hata ayıklayıcı hakkında daha fazla bilgi için.

![Hata ayıklama bırak örnek](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Azure CLI sorunları

Azure komut satırı arabirimi (Azure CLI) önizleme yapıdır. Çözümleri aramak için kullanabileceğiniz [Önizleme Yükleme Kılavuzu](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Tüm hatalar aracı ile karşılaşırsanız, dosya bir [sorunu](https://github.com/Azure/azure-cli/issues) içinde **sorunları** GitHub depo bölümü.

Sık karşılaşılan sorunları giderme konusunda yardım için kontrol [Benioku](https://github.com/Azure/azure-cli/blob/master/README.rst).

Lütfen "gereksinimini karşılayan bir sürüm bulunamadı" karşılıyorsa, PIP en son sürümüne yükseltmek için aşağıdaki komutu çalıştırın.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Python yükleme sorunları

### <a name="legacy-installation-issues-macos"></a>Eski yükleme sorunları (macOS)

PIP yüklüyorsanız, eski paketleri ile yüklendiğinde izin hatası atılır **su** izinleri. Python brew (macOS) kullanarak önceki bir yüklemesini tümüyle kaldırılmamış. Bu durum oluşur. Önceki yüklemeye ait bazı PIP paketler izni hataya neden olan kök tarafından oluşturuldu. Kök tarafından yüklenen bu paketleri kaldırmak için çözümüdür. Bu görevi tamamlamak için aşağıdaki adımları kullanın:

1. Şuraya gidin: `/usr/local/lib/python2.7/site-packages`
2. Kök tarafından oluşturulan listesi paketler:`ls -l | grep root`
3. Paketler, adım 2 ' Kaldır:`sudo rm -rf {package name}`
4. Python yeniden yükleyin.

## <a name="azure-iot-hub-issues"></a>Azure IOT hub'ı sorunları

Azure CLI ile Azure IOT hub'ınızı başarıyla kaynak sağlandı ve IOT hub'ınıza bağlanan cihazları yönetmek için bir aracı ihtiyacınız varsa aşağıdaki araçları deneyin.

### <a name="device-explorer"></a>Cihaz Gezgini

[Cihaz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows yerel makinenizde çalıştırır ve Azure IOT hub'ınıza bağlanır. Aşağıdaki ile iletişim kurar [IOT Hub uç noktaları](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Cihaz Kimlik Yönetimi sağlamak ve cihazları yönetmek için IOT hub'ınıza kayıtlı.
- Cihazınızın IOT hub'ına gönderilen iletileri izleyebilmek cihaz bulut alırsınız.
- IOT hub'ından aygıtlarınıza iletileri gönderebilmesi bulut cihaz gönderin.

IOT hub bağlantı dizenizi tüm özellikleri kullanmak için bu aracı içinde yapılandırın.

### <a name="iothub-explorer"></a>ıothub Gezgini

[ıothub explorer](https://github.com/Azure/iothub-explorer) aygıt istemcileri yönetmek için bir örnek çok platformlu CLI aracıdır. Cihaz kimlik kayıt defterinde yönetmek, cihaz bulut iletilerini izlemek ve bulut-cihaz komutlarını göndermek için aracını kullanabilirsiniz.

Iothub explorer Aracı (ön) en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:

```bash
npm install -g iothub-explorer@latest
```

Tüm ıothub explorer komutları ve bunların parametreleri hakkında ek Yardım almak için aşağıdaki komutu çalıştırın:

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a>Azure portalı

Tam CLI deneyimi oluşturma ve Azure kaynaklarınızı yönetmenize yardımcı olur. Kullanmak isteyebilirsiniz [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) sağlama Yardım, yönetmek ve Azure kaynaklarınızı hata ayıklamak için.

## <a name="azure-storage-issues"></a>Azure depolama sorunları

[Microsoft Azure Storage Gezgini (Önizleme)](http://storageexplorer.com/) Windows, macOS ve Linux Azure Storage verilerle çalışmak için kullanabileceğiniz bir Microsoft tek başına uygulamadır. Bu aracı kullanarak tablonuza bağlanabilir ve içindeki verilerin bakın. Azure Storage sorunlarını gidermek için bu aracı kullanabilirsiniz.
