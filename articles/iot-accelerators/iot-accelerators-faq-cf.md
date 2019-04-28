---
title: Bağlı Fabrika çözümü SSS - Azure | Microsoft Docs
description: Bağlı Fabrika çözüm Hızlandırıcı için sık sorulan sorular
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: ed429d923cad2c715621990c146d4cf3a23e7bca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447939"
---
# <a name="frequently-asked-questions-for-connected-factory-solution-accelerator"></a>Bağlı Fabrika çözüm Hızlandırıcı için sık sorulan sorular

Ayrıca bkz. genel [SSS](iot-accelerators-faq.md) IOT Çözüm Hızlandırıcıları için.

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerator"></a>Çözüm Hızlandırıcısını için kaynak kodu nerede bulabilirim?

Kaynak kodu, aşağıdaki GitHub deposunda depolanır:

* [Bağlı Fabrika Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>OPC UA nedir?

OPC birleşik mimarisi (2008'de yayımlanan UA), bir platformdan bağımsız, hizmet odaklı birlikte çalışabilirlik standart ' dir. OPC UA çeşitli endüstriyel sistemleri ve cihazların sektör bilgisayarları PLC ve algılayıcılar gibi tarafından kullanılır. OPC UA OPC Klasik belirtimleri işlevselliğinin bir Genişletilebilir Framework'e yerleşik güvenlik ile tümleştirir. OPC Fundation tarafından yönetilen bir standarttır. [OPC Fundation](https://opcfoundation.org/) 440'den fazla üyelere sahip olmayan yetkilendirilip kâr kuruluştur. Hedef kuruluşun aracılığıyla çok satıcılı, çok platformlu, güvenli ve güvenilir birlikte çalışabilirlik kolaylaştırmak için OPC belirtimleri kullanmaktır:

* Altyapı
* Belirtimler
* Teknoloji
* İşlemler

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-solution-accelerator"></a>Neden Microsoft bağlı Fabrika çözüm Hızlandırıcı için OPC UA tercih ettiniz?

Microsoft, açık, özel olmayan, platform bağımsız, sektörde tanınmış ve kendini kanıtlamış standart olduğu için OPC UA seçtiniz. Çok sayıda üretim süreçleri ve ekipmanı arasında birlikte çalışabilirlik sağlayarak Industrie 4.0 (RAMI4.0) başvuru mimarisi çözümleri için bir gereksinimdir. Microsoft, müşterilerinin Industrie 4.0 çözümler oluşturmak için talep görüyor. OPC UA desteği, müşterilerin hedeflerine ulaşması engel düşürmenize yardımcı olur ve bunlara anında iş değeri sağlar.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Genel bir IP adresi VM benzetimi nasıl ekleyebilirim?

IP adresi eklemek için iki seçeneğiniz vardır:

* PowerShell betiğini kullanın `Simulation/Factory/Add-SimulationPublicIp.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory). Dağıtım adınız bir parametre olarak geçiriyoruz. Yerel bir dağıtım için kullanmak `<your username>ConnFactoryLocal`. Betiği sanal makinenin IP adresini yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel bir dağıtımı dışında kaynak grubunu çözümü olarak belirttiğiniz adı veya dağıtım adı var. Yapı komut dosyası kullanarak bir yerel dağıtımı için kaynak grubunun adıdır. `<your username>ConnFactoryLocal`. Şimdi yeni bir ekleme **genel IP adresi** kaynak kaynak grubu.

> [!NOTE]
> Her iki durumda da, yönergeleri izleyerek en son düzeltme eklerinin yüklediğiniz olun [Ubuntu Web sitesi](https://wiki.ubuntu.com/Security/Upgrades). Sanal makinenizin genel IP adresi erişilebilir olduğu sürece yükleme için güncel kalmasını sağlayın.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Benzetim VM için genel IP adresini nasıl kaldırabilirim?

IP adresi kaldırmak için iki seçeneğiniz vardır:

* PowerShell komut dosyası, Simulation/Factory/Remove-SimulationPublicIp.ps1 kullanım [depo](https://github.com/Azure/azure-iot-connected-factory). Dağıtım adınız bir parametre olarak geçiriyoruz. Yerel bir dağıtım için kullanmak `<your username>ConnFactoryLocal`. Betiği sanal makinenin IP adresini yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel bir dağıtımı dışında kaynak grubunu çözümü olarak belirttiğiniz adı veya dağıtım adı var. Yapı komut dosyası kullanarak bir yerel dağıtımı için kaynak grubunun adıdır. `<your username>ConnFactoryLocal`. Şimdi kaldırmak **genel IP adresi** kaynak grubundan kaynak.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Nasıl miyim VM benzetimine nasıl oturum açabilirim?

VM benzetimi oturum açarken yalnızca desteklenen PowerShell betiğini kullanarak çözümünüzü dağıttıysanız `build.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory).

Www.azureiotsolutions.com çözümden dağıttıysanız, sanal Makineye oturum açamazsınız. ' De, parolayı rastgele oluşturulan ve size sıfırlayamazsınız olduğundan oturum açamazsınız.

1. VM'ye bir genel IP adresi ekleyin. Bkz: [benzetimi VM'nin genel IP adresi nasıl eklerim?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Sanal makinenin IP adresini kullanarak sanal makinenize yönelik SSH oturumu oluşturun.
1. Kullanılacak kullanıcı adı: `docker`.
1. Şifreyi dağıtmak için kullanılan sürümüne bağlıdır:
    * 1 Haziran 2017'den önce build.ps1 betiği kullanılarak dağıtılan çözümler için paroladır: `Passw0rd`.
    * 1 Haziran 2017'den sonra build.ps1 betiği kullanılarak dağıtılan çözümler için parolayı bulabilirsiniz `<name of your deployment>.config.user` dosya. Parola depolanır **VmAdminPassword** ayarı. Kullanarak belirtmediğiniz sürece parolayı rastgele dağıtım sırasında oluşturulan `build.ps1` komut parametresi `-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Nasıl durdurmak ve tüm docker işlemleri benzetim VM başlatma?

1. VM benzetimi için oturum açın. Bkz: [nasıl oturum açabilirim benzetimi VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. Kapsayıcıları etkin olan denetlemek için çalıştırın: `docker ps`.
1. Tüm benzetim kapsayıcıları durdurmak için şunu çalıştırın: `./stopsimulation`.
1. Tüm benzetim kapsayıcıları başlatmak için:
    * Bir kabuk değişkeni adı ile dışarı aktarma **IOTHUB_CONNECTIONSTRING**. Değerini kullanmak **IotHubOwnerConnectionString** ayarı `<name of your deployment>.config.user` dosya. Örneğin:

        ```sh
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * `./startsimulation` öğesini çalıştırın.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>VM benzetime nasıl güncelleştirebilirim?

Benzetimi herhangi bir değişiklik yaptıysanız, PowerShell betiğini kullanabilirsiniz `build.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory) kullanarak `updatedimulation` komutu. Bu betik tüm benzetim bileşenleri oluşturur, VM'yi benzetime durdurur, yükler, yükler ve bunları başlatır.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Çözümüm tarafından kullanılan IOT hub bağlantı dizesini dışarı nasıl bulabilirim?

Çözümünüzü ile dağıttıysanız `build.ps1` betiğini [depo](https://github.com/Azure/azure-iot-connected-factory), bağlantı dizesi değeri **IotHubOwnerConnectionString** içinde `<name of your deployment>.config.user` dosya.

Bağlantı dizesini Azure portalını kullanarak da bulabilirsiniz. IOT hub'ı dağıtımınızın kaynak grubundaki kaynak bağlantı dizesi ayarlarını bulun.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Hangi IOT Hub cihazları Connected Factory benzetim kullanıyor mu?

Benzetim kendinden aşağıdaki cihazları kaydeder:

* Proxy.Beijing.corp.contoso
* Proxy.capetown.corp.contoso
* Proxy.Mumbai.corp.contoso
* Proxy.munich0.corp.contoso
* Proxy.Rio.corp.contoso
* proxy.seattle.corp.contoso
* publisher.beijing.corp.contoso
* Publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Kullanarak [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) aracı hangi cihazların çözümünüzü kullanarak IOT hub'la kayıtlı kontrol edebilirsiniz. Cihaz Gezgini kullanmak için bağlantı dizesi için IOT hub, dağıtımınızdaki gerekir. IOT uzantısı için Azure CLI kullanmak için IOT hub'ı adı gerekir.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Benzetim bileşenleri günlük verilerini nasıl alabilirim?

Tüm benzetim bileşenleri bilgileri günlük dosyalarına oturum açın. Bu dosyaları sanal Makineye klasöründe bulunabilir. `home/docker/Logs`. Günlükleri almak için PowerShell betiğini kullanabilirsiniz `Simulation/Factory/Get-SimulationLogs.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory).

Bu betik, sanal Makineye oturum açması gerekiyor. Oturum açmak için kimlik bilgilerini sağlamanız gerekebilir. Bkz: [nasıl oturum açabilirim benzetimi VM?](#how-do-i-sign-in-to-the-simulation-vm) kimlik bilgileri bulunamıyor.

Betik ekler / VM'ye genel bir IP adresi henüz bir sahip değil ve kaldırır kaldırır. Betik, tüm günlük dosyalarını bir arşivde koyar ve Arşiv geliştirme istasyonunuza indirir.

Alternatif olarak sanal makineye SSH aracılığıyla oturum açın ve çalışma zamanında günlük dosyalarını inceleyin.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Benzetim bulutuna veri gönderen, nasıl kontrol edebilirim?

İle [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [Azure IOT CLI uzantısı olayları izlemek](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-monitor-events) komutunu belirli cihazlardan IOT Hub'ına gönderilen veriler inceleyin. Bu araçları kullanmak için dağıtımınızdaki IOT hub bağlantı dizesini bilmeniz gerekir. Bkz: [nasıl öğrenebilirim çözümüm tarafından kullanılan IOT hub bağlantı dizesini?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Bir yayımcı cihazlar tarafından gönderilen verilerin inceleyin:

* publisher.beijing.corp.contoso
* Publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

IOT Hub'ına gönderilen veri görürseniz, benzetim ile ilgili bir sorun yoktur. İlk analysis adım olarak benzetim bileşenleri günlük dosyalarını çözümlemeniz gerekir. Bkz: [benzetimi bileşenlerden günlük verilerini nasıl alabilirim?](#how-can-i-get-log-data-from-the-simulation-components) Ardından, durdurmak, benzetimi başlatmak ve gönderilen veri hala yoksa benzetim tamamen güncelleştirme deneyin. Bkz: [VM benzetime nasıl güncelleştirebilirim?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Etkileşimli bir harita bağlı Fabrika çözümünün nasıl etkinleştirebilirim?

Bağlı Fabrika çözümünüzü etkileşimli bir harita etkinleştirmek için bir Azure haritalar hesabı olmalıdır.

Gelen dağıtırken [www.azureiotsolutions.com](https://www.azureiotsolutions.com), dağıtım işlemi bir Azure haritalar hesabı çözüm Hızlandırıcı hizmetleri içeren kaynak grubunu ekler.

Kullanarak dağıtırken `build.ps1` ortam değişkeni bağlı Fabrika GitHub deposu kümesinde betik `$env:MapApiQueryKey` derleme penceresinde [Azure haritalar hesabınızda anahtarı](../azure-maps/how-to-manage-account-keys.md). Etkileşimli harita otomatik olarak etkinleştirilir.

Ayrıca, çözüm hızlandırıcınız dağıtımdan sonra bir Azure haritalar hesabı anahtarı ekleyebilirsiniz. Azure portalına gidin ve bağlı Fabrika dağıtımınızdaki App Service kaynak erişebilirsiniz. Gidin **uygulama ayarları**, bölüm nerede **uygulama ayarları**. Ayarlama **MapApiQueryKey** için [Azure haritalar hesabınızda anahtarı](../azure-maps/how-to-manage-account-keys.md). Ayarları kaydedin ve ardından gidin **genel bakış** ve App Service'ı yeniden başlatın.

### <a name="how-do-i-create-an-azure-maps-account"></a>Azure haritalar hesabı nasıl oluşturulur?

Bkz, [Azure haritalar hesabı ve anahtarları yönetme](../azure-maps/how-to-manage-account-keys.md).

### <a name="how-to-obtain-your-azure-maps-account-key"></a>Azure haritalar hesabı anahtarınızı edinme

Bkz, [Azure haritalar hesabı ve anahtarları yönetme](../azure-maps/how-to-manage-account-keys.md).

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Etkileşimli harita yerel olarak hata ayıklama sırasında nasıl etkinleştirebilirim?

Yerel olarak hata ayıklarken etkileşimli harita etkinleştirmek için ayarın değerini ayarlamak `MapApiQueryKey` dosyalarında `local.user.config` ve `<yourdeploymentname>.user.config` dağıtımınıza değerini kökünde **QueryKey** , kopyalanan daha önce.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Farklı bir görüntü Panom ana sayfanın nasıl kullanabilirim?

Pano giriş sayfası GÇ gösterilen statik resmi değiştirmek için görüntüyü değiştirin `WebApp\Content\img\world.jpg`. Daha sonra yeniden oluşturun ve WebApp yeniden dağıtın.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Bağlı Fabrika ile olmayan OPC UA cihazları nasıl kullanabilirim?

Telemetri verilerini olmayan OPC UA cihazları bağlı Fabrika için göndermek için:

1. [Yeni istasyona Connected Factory topolojisini yapılandırma](iot-accelerators-connected-factory-configure.md) içinde `ContosoTopologyDescription.json` dosya.

1. Bağlı Fabrika uyumlu JSON biçiminde telemetri verilerini alma:

    ```json
    [
      {
        "ApplicationUri": "<the_value_of_OpcUri_of_your_station",
        "DisplayName": "<name_of_the_datapoint>",
        "NodeId": "value_of_NodeId_of_your_datapoint_in_the_station",
        "Value": {
          "Value": <datapoint_value>,
          "SourceTimestamp": "<timestamp>"
        }
      }
    ]
    ```

1. Biçimi `<timestamp>` olan: `2017-12-08T19:24:51.886753Z`

1. Bağlı Fabrika uygulaması hizmetini yeniden başlatın.

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm Hızlandırıcısını dağıtma](quickstart-connected-factory-deploy.md)
* [Baştan sona IOT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
