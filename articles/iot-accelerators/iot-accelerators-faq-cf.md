---
title: Fabrika çözüm SSS - Azure bağlı | Microsoft Docs
description: Bağlı Fabrika Çözüm Hızlandırıcısı için sık sorulan sorular
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 4ed0cd413480e717e686f7e52123102e1a838f19
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="frequently-asked-questions-for-connected-factory-solution-accelerator"></a>Bağlı Fabrika Çözüm Hızlandırıcısı için sık sorulan sorular

Ayrıca bkz.: Genel [SSS](iot-accelerators-faq.md) IOT Çözüm Hızlandırıcıları için.

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerator"></a>Çözüm Hızlandırıcısı için kaynak kodunu nereden bulabilirim?

Kaynak kodu aşağıdaki GitHub deposunda depolanır:

* [Bağlı Fabrika Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>OPC UA nedir?

OPC birleşik mimarisi (2008'de serbest UA), bir platformdan bağımsız, hizmet odaklı birlikte çalışabilirlik standart ' dir. OPC UA çeşitli endüstriyel sistemleri ve cihazların endüstri PC'ler, PLC ve algılayıcılar gibi tarafından kullanılır. OPC UA OPC Klasik belirtimleri işlevselliğini bir Genişletilebilir Framework'e yerleşik güvenlik ile tümleşir. OPC Foundation tarafından yönetilen bir standarttır. [OPC Foundation](http://opcfoundation.org/) kar için not kuruluşunuzun birden fazla 440 üyelere sahip. Kuruluş amacı çok satıcı, çok platformlu, güvenli ve güvenilir birlikte çalışabilirliği aracılığıyla kolaylaştırmak için OPC belirtimleri kullanmaktır:

* Altyapı
* Belirtimler
* Teknoloji
* İşlemler

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-solution-accelerator"></a>Neden Microsoft OPC UA seçebilir ve bu da için Fabrika bağlı Çözüm Hızlandırıcısı?

Bir açık, olmayan-özel, platform bağımsız, endüstri tanınan ve kanıtlanmış standart olduğu için Microsoft OPC UA seçtiniz. Geniş bir üretim işlemleri kümesi ile donanım arasında birlikte çalışabilirlik sağlama Industrie 4.0 (RAMI4.0) başvuru mimarisi çözümleri için gerekli değildir. Microsoft, müşterilerinin Industrie 4.0 çözümleri oluşturmak üzere talep görür. OPC UA desteği engel müşterilerin hedeflerine ulaşması alt yardımcı olur ve onları hemen iş değerine sağlar.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>VM benzetimi için nasıl bir ortak IP adresi eklensin mi?

IP adresi eklemek için iki seçeneğiniz vardır:

* PowerShell Betiği kullanmak `Simulation/Factory/Add-SimulationPublicIp.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory). Dağıtım adınızı parametre olarak geçirin. Yerel bir dağıtım için kullanmak `<your username>ConnFactoryLocal`. Komut dosyası VM IP adresini yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel bir dağıtımı dışında kaynak grubu çözümü olarak belirtilen adı veya dağıtım adı var. Yapı komut dosyasını kullanarak yerel bir dağıtımı için kaynak grubunun adıdır `<your username>ConnFactoryLocal`. Şimdi yeni bir ekleyin **genel IP adresi** kaynak grubuna kaynak.

> [!NOTE]
> Yönergeleri izleyerek en son düzeltme eklerini yüklediğiniz her iki durumda da olun [Ubuntu Web sitesi](https://wiki.ubuntu.com/Security/Upgrades). VM'yi bir ortak IP adresi üzerinden erişilebilir olduğu müddetçe yükleme için güncel tutun.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Genel IP adresi VM benzetimi için nasıl kaldırılsın mı?

IP adresini kaldırmak için iki seçeneğiniz vardır:

* PowerShell komut dosyası, Simulation/Factory/Remove-SimulationPublicIp.ps1 kullanım [depo](https://github.com/Azure/azure-iot-connected-factory). Dağıtım adınızı parametre olarak geçirin. Yerel bir dağıtım için kullanmak `<your username>ConnFactoryLocal`. Komut dosyası VM IP adresini yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel bir dağıtımı dışında kaynak grubu çözümü olarak belirtilen adı veya dağıtım adı var. Yapı komut dosyasını kullanarak yerel bir dağıtımı için kaynak grubunun adıdır `<your username>ConnFactoryLocal`. Şimdi kaldırmak **genel IP adresi** kaynak kaynak grubu.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>VM benzetimi nasıl oturum?

VM benzetimi için oturum açma yalnızca desteklenen PowerShell Betiği kullanılarak çözümünüzü dağıttıysanız, `build.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory).

Www.azureiotsuite.com çözümden dağıttıysanız VM oturum açamaz. ' De, çünkü parolayı rastgele oluşturulan ve onu sıfırlayamazsınız oturum.

1. Bir ortak IP adresi VM'ye ekleyin. Bkz: [VM benzetimi için bir ortak IP adresi nasıl ekleyebilirim?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. VM IP adresini kullanarak, VM için bir SSH oturumu oluşturun.
1. Kullanılacak kullanıcı adı: `docker`.
1. Kullanılacak parolayı dağıtmak için kullanılan sürümüne bağlıdır:
    * 1 Haziran 2017 önce build.ps1 komut dosyası kullanılarak dağıtılan çözümleri için bir paroladır: `Passw0rd`.
    * 1 Haziran 2017 sonra build.ps1 komut dosyası kullanılarak dağıtılan çözümleri için parolayı bulabilirsiniz `<name of your deployment>.config.user` dosya. Parola depolanan **VmAdminPassword** ayarı. Kullanarak belirtmediğiniz sürece parolayı rastgele dağıtım sırasında oluşturulan `build.ps1` parametresi komut dosyası `-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Nasıl durdurun ve tüm docker işlemleri benzetimi VM başlatma?

1. VM benzetimi için oturum açın. Bkz: [nasıl oturum benzetimi VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. Hangi kapsayıcıları etkin denetlemek için çalıştırın: `docker ps`.
1. Tüm benzetimi kapsayıcıları durdurmak için çalıştırın: `./stopsimulation`.
1. Tüm benzetimi kapsayıcıları başlatmak için:
    * Ada sahip bir kabuk değişken verme **IOTHUB_CONNECTIONSTRING**. Değerini kullanmak **IotHubOwnerConnectionString** ayarı `<name of your deployment>.config.user` dosya. Örneğin:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * `./startsimulation` öğesini çalıştırın.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>VM'deki benzetimi nasıl güncelleştiririm?

Benzetimi herhangi bir değişiklik yaptıysanız, PowerShell Betiği kullanabilirsiniz `build.ps1` içinde [deposu](https://github.com/Azure/azure-iot-connected-factory) kullanarak `updatedimulation` komutu. Bu komut tüm benzetimi bileşenleri oluşturur, VM'deki benzetimi durdurur, yükler, yükler ve bunları başlatır.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>IOT hub'ı my çözümü tarafından kullanılan bağlantı dizesi kullanıma nasıl bulabilirim?

Çözümünüzle birlikte dağıttıysanız `build.ps1` içindeki komut dosyası [deposu](https://github.com/Azure/azure-iot-connected-factory), bağlantı dizesi değeri **IotHubOwnerConnectionString** içinde `<name of your deployment>.config.user` dosya.

Azure Portalı'nı kullanarak bağlantı dizesini de bulabilirsiniz. Kaynak grubu, dağıtımınızın IOT Hub'kaynağında bağlantı dizesi ayarlarının bulun.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Hangi IOT Hub cihazları bağlı Fabrika benzetimi kullanıyor mu?

Benzetim self aşağıdaki cihazları kaydeder:

* Proxy.Beijing.corp.contoso
* Proxy.capetown.corp.contoso
* Proxy.Mumbai.corp.contoso
* Proxy.munich0.corp.contoso
* Proxy.Rio.corp.contoso
* proxy.seattle.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Kullanarak [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) aracı, hangi aygıtların çözümünüzü kullanarak IOT hub ile kaydedilen kontrol edebilirsiniz. Cihaz Gezgini'ni kullanmak için bağlantı dizesi için IOT hub, dağıtımınızdaki gerekiyor. Azure CLI 2.0 için IOT uzantısını kullanmak için IOT Hub adınızı gerekir.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Günlük verileri benzetimi bileşenlerini nasıl alabilirim?

Tüm bileşenleri benzetimde bilgileri günlük dosyalarına oturum açın. Bu dosyalar VM'yi klasöründe bulunabilir `home/docker/Logs`. Günlükleri almak için PowerShell betiğini kullanabilirsiniz `Simulation/Factory/Get-SimulationLogs.ps1` içinde [depo](https://github.com/Azure/azure-iot-connected-factory).

Bu komut VM oturum açması gerekiyor. Oturum açma için kimlik bilgilerini sağlamanız gerekebilir. Bkz: [nasıl oturum benzetimi VM?](#how-do-i-sign-in-to-the-simulation-vm) kimlik bilgileri bulunamadı.

Komut dosyası ekler veya bir ortak IP adresi VM henüz bir sahip değil ve kaldırır kaldırır. Betik tüm günlük dosyalarını bir arşiv koyar ve Arşiv geliştirme istasyonunuza indirir.

Alternatif olarak VM SSH aracılığıyla oturum açın ve çalışma zamanında günlük dosyalarını inceleyin.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Benzetim buluta veri gönderme, nasıl kontrol edebilirsiniz?

İle [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [iothub-explorer](https://github.com/azure/iothub-explorer) aracı, belirli aygıtlardan IOT Hub'ına gönderilen verileri incelemek. Bu araçları kullanmak için dağıtımınızdaki IOT hub'ına yönelik bağlantı dizesini bilmeniz gerekir. Bkz: [nasıl bulabilirim my çözümü tarafından kullanılan IOT hub bağlantı dizesi kullanıma?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Yayımcı aygıtlardan biri tarafından gönderilen verileri inceleyin:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

IOT Hub'ına gönderilen veri görürseniz, benzetimi ile ilgili bir sorun yoktur. İlk çözümleme adım olarak günlük dosyaları benzetimi bileşenlerinin çözümlemeniz gerekir. Bkz: [benzetimi bileşenlerini günlük verilerini nasıl alabilirim?](#how-can-i-get-log-data-from-the-simulation-components) Ardından, durdurmak ve benzetimi başlatmak ve hala gönderilen veri yoksa benzetimi tamamen güncelleştirmek deneyin. Bkz: [VM'deki benzetimi nasıl güncelleştirebilirim?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Bağlı Fabrika çözümümde nasıl etkileşimli bir harita etkinleştirilsin mi?

Etkileşimli bir harita bağlı Fabrika çözümünüzdeki etkinleştirmek için Kurumsal planı için varolan bir Bing haritaları API'si olması gerekir.

Gelen dağıtırken [www.azureiotsuite.com](http://www.azureiotsuite.com), dağıtım işlemi aboneliğiniz Kurumsal planı için etkinleştirilmiş bir Bing haritaları API'si sahiptir ve etkileşimli bir harita bağlı Fabrika otomatik olarak dağıtan doğrular. Bu durumda değilse, hala etkileşimli bir harita dağıtımınızda şu şekilde etkinleştirebilirsiniz:

Dağıttığınızda kullanarak `build.ps1` betik bağlı Fabrika github depo ve kurumsal planı için Bing haritaları API'si sahip, ortam değişkeni `$env:MapApiQueryKey` sorgu anahtarı planınız için derleme penceresinde. Etkileşimli harita otomatik olarak etkinleştirilir.

Kurumsal planı için Bing haritaları API'si yoksa, bağlı Fabrika çözümden dağıtmak [www.azureiotsuite.com](http://www.azureiotsuite.com) veya kullanarak `build.ps1` komut dosyası. Daha sonra kurumsal planı için Bing haritaları API'si açıklandığı gibi aboneliğinize eklediğiniz [kuruluş hesabı için Bing haritaları API'si nasıl oluşturabilirim?](#how-do-i-create-a-bing-maps-api-for-enterprise-account). Açıklandığı gibi bu hesabın sorgu anahtarını arayın [Kurumsal QueryKey için Bing haritaları API'nizi edinme](#how-to-obtain-your-bing-maps-api-for-enterprise-querykey) ve bu anahtar kaydedin. Azure Portalı'na gidin ve bağlı Fabrika dağıtımınızdaki uygulama hizmeti kaynağa erişim. Gidin **uygulama ayarları**, bir bölümü nerede **uygulama ayarları**. Ayarlama **MapApiQueryKey** aldığınız sorgu anahtarı. Ayarları kaydetmek ve ardından gidin **genel bakış** ve uygulama hizmetini yeniden başlatın.

### <a name="how-do-i-create-a-bing-maps-api-for-enterprise-account"></a>Bing Haritalar API'si kuruluş hesabı için nasıl oluşturulur

Ücretsiz elde edebilirsiniz *iç işlemleri düzey 1 Bing Haritalar kuruluş için* planı. Ancak, yalnızca iki bu planlarını bir Azure aboneliğine ekleyebilirsiniz. Kurumsal hesap için Bing haritaları API'si yoksa, Azure portalında tıklayarak oluşturmak **+ kaynak oluşturma**. İçin arama **Kurumsal için Bing haritaları API'si** ve onu oluşturmak için istemleri izleyin.

![Bing anahtarı](./media/iot-accelerators-faq-cf/bing.png)

### <a name="how-to-obtain-your-bing-maps-api-for-enterprise-querykey"></a>Kurumsal QueryKey için Bing haritaları API'nizi edinme

Kurumsal planı için Bing haritaları API'nizi oluşturduktan sonra kurumsal kaynak için Bing Haritalar bağlı Fabrika çözümünüzün Azure portalındaki kaynak grubuna ekleyin.

1. Azure portalında Kurumsal planı için Bing haritaları API'nizi içeren kaynak grubuna gidin.

1. Tıklatın **tüm ayarları**, ardından **anahtar yönetimi**.

1. İki anahtar vardır: **MasterKey** ve **QueryKey**. Kopya **QueryKey** değeri.

1. Tarafından toplanma anahtar `build.ps1` komut dosyası, ortam değişkeni `$env:MapApiQueryKey` PowerShell ortamınızda **QueryKey** planınızın. Derleme betiğinin sonra otomatik olarak değeri uygulama hizmeti ayarlarına ekler.

1. Yerel çalıştırma veya dağıtım kullanarak bulut `build.ps1` komut dosyası.

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Etkileşimli harita yerel olarak hata ayıklama sırasında nasıl etkinleştirebilirim?

Yerel olarak hata ayıklarken etkileşimli harita etkinleştirmek için ayarın değerini ayarlamak `MapApiQueryKey` dosyalarında `local.user.config` ve `<yourdeploymentname>.user.config` dağıtımınızı değerine kök **QueryKey** , kopyalanan daha önce.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Benim Panom giriş sayfanın nasıl farklı bir resim kullanıyor?

Pano giriş sayfası GÇ gösterilen statik görüntü değiştirmek için görüntünün yerini `WebApp\Content\img\world.jpg`. Daha sonra yeniden oluşturun ve WebApp yeniden dağıtın.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Bağlı Fabrika ile nasıl olmayan OPC UA cihazlar kullanıyor?

Telemetri verileri olmayan OPC UA aygıtları bağlı Fabrika göndermek için:

1. [Yeni istasyona bağlı Fabrika topolojisinde yapılandırma](iot-accelerators-connected-factory-configure.md) içinde `ContosoTopologyDescription.json` dosya.

1. Telemetri verileri bağlı Fabrika uyumlu JSON biçiminde alın:

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

1. Biçimi `<timestamp>` değil: `2017-12-08T19:24:51.886753Z`

1. Bağlı Fabrika uygulama hizmetini yeniden başlatın.

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](../iot-suite/iot-suite-predictive-overview.md)
* [Bağlı Fabrika Çözüm Hızlandırıcısı genel bakış](iot-accelerators-connected-factory-overview.md)
* [IOT güvenlik sıfırdan](../iot-suite/securing-iot-ground-up.md)
