---
title: "Uzaktan izleme çözümü - Azure aygıt benzetim | Microsoft Docs"
description: "Bu öğretici ile Uzaktan izleme önceden yapılandırılmış çözümü aygıt benzeticisi kullanmayı gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3343dbe0093ad8fbeebe5893d44abdbe356e1789
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="test-your-solution-with-simulated-devices"></a>Çözümünüzü sanal cihazlar ile test

Bu öğretici, önceden yapılandırılmış Uzaktan izleme çözümü, aygıt benzeticisi mikro özelleştirmek nasıl gösterir. Aygıt benzeticisi özelliklerini göstermek için Bu öğretici Contoso IOT uygulamada iki senaryo kullanır.

İlk senaryoda, Contoso yeni bir akıllı ampul aygıt test istiyor. Testleri gerçekleştirmek için aşağıdaki özelliklere sahip yeni bir sanal cihaz oluşturun:

*Özellikleri*

| Ad                     | Değerler                      |
| ------------------------ | --------------------------- |
| Renk                    | Beyaz, kırmızı, mavi            |
| Parlaklığını               | 0 ile 100 arası                    |
| Tahmini kalan kullanım ömrü | Geri sayım 10.000 saat |

*Telemetri*

Aşağıdaki tabloda, bir veri akışı olarak buluta ampul raporları veri gösterilmektedir:

| Ad   | Değerler      |
| ------ | ----------- |
| Durum | "on", "off" |
| Sıcaklık | Derece F |
| Çevrimiçi | TRUE, false |

> [!NOTE]
> **Çevrimiçi** telemetri değeri tüm sanal türleri için zorunludur.

*Yöntemleri*

Aşağıdaki tabloda yeni cihaz destekler eylemleri gösterir:

| Ad        |
| ----------- |
| Geçiş   |
| Kapat  |

*İlk durumu*

Aşağıdaki tabloda cihaz ilk durumunu gösterir:

| Ad                     | Değerler |
| ------------------------ | -------|
| İlk rengi            | Beyaz  |
| İlk parlaklığını       | 75     |
| İlk kalan kullanım ömrü   | 10,000 |
| İlk telemetri durumu | "açık"   |
| İlk telemetri sıcaklık | 200   |

İkinci senaryoda, yeni bir telemetri türü contoso varolan eklemek **Soğutucu** aygıt.

Bu öğretici ile Uzaktan izleme önceden yapılandırılmış çözümü aygıt benzeticisi kullanmayı gösterir:

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Yeni bir aygıt türü oluşturma
> * Özel aygıt davranışı benzetimi
> * Yeni bir cihaz türü için Pano ekleyin
> * Var olan bir cihaz türünden özel telemetri Gönder

Aşağıdaki videoda benzetimli ve gerçek cihazları Uzaktan izleme çözümüne bağlama bir kılavuz gösterir:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-38-Customizing-Azure-IoT-Suite-solution-and-connect-a-real-device/Player]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izleyin, gerekir:

* Azure aboneliğiniz Uzaktan izleme çözümünde dağıtılan bir örneği. Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

* Visual Studio 2017. Visual Studio yüklü 2017 yoksa, ücretsiz indirebilirsiniz [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) sürümü.

* [Cloud Explorer için Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) Visual Studio uzantısı.

* Bir hesap üzerinde [Docker hub'a](https://hub.docker.com/). Ücretsiz başlamak kaydolabilirsiniz.

* [Git](https://git-scm.com/downloads) Masaüstü makinenize yüklü.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Uzaktan izleme çözümünüz için yeni bir sanal cihaz eklemek için geliştirme ortamınızı hazırlamak için aşağıdaki görevleri tamamlayın:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>Azure'da çözüm sanal makine SSH erişimi yapılandırma

Uzaktan izleme çözüm oluşturduğunuzda [www.azureiotsuite.com](https://www.azureiotsuite.com), çözüm adı seçtiniz. Çözüm adı çözüm kullanır çeşitli dağıtılan kaynakları içeren Azure kaynak grubu adı haline gelir. Adlı bir kaynak grubu aşağıdaki komutları kullanın **Contoso-01**, değiştirmeniz gerekir **Contoso-01** , kaynak grubu adı.

Aşağıdaki komutları kullanın `az` komutunu [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Geliştirme makinenizde Azure CLI 2.0 yükleyebilmek veya kullanabilmek [bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview) içinde [Azure portal](http://portal.azure.com). Azure CLI 2.0 bulut Kabuğu'nda önceden yüklenir.

1. Uzaktan izleme kaynaklarınızı içeren kaynak grubunun adını doğrulamak için aşağıdaki komutu çalıştırın:

    ```sh
    az group list | grep "name"
    ```

    Bu komut, aboneliğinizdeki tüm kaynak grupları listeler. Uzaktan izleme çözümünüz aynı ada sahip bir kaynak grubu içermelidir.

1. Sonraki komutlar için varsayılan grubu kaynağınız yapmak için kaynak grubu adı yerine kullanarak şu komutu çalıştırın **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Kaynak grubunuzun kaynaklarında listelemek için aşağıdaki komutu çalıştırın:

    ```sh
    az resource list -o table
    ```

    Sanal makineniz ve ağ güvenlik grubu adlarını not edin. Sonraki adımlarda bu değerleri kullanın.

1. Sanal makineniz SSH erişimini etkinleştirmek için önceki adımdan ağ güvenlik grubunun adı kullanarak şu komutu çalıştırın:

    ```sh
    az network nsg rule create --name SSH --nsg-name your-network-security-group --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Ağınız için gelen kuralları listesini görüntülemek için aşağıdaki komutu çalıştırın:

    ```sh
    az network nsg rule list --nsg-name Contoso-01-nsg -o table
    ```

1. Bildiğiniz bir parola için sanal makine parolasını değiştirmek için aşağıdaki komutu çalıştırın. Daha önce not ettiğiniz sanal makinenin adını ve tercih ettiğiniz bir parola kullanın:

    ```sh
    az vm user update --name your-vm-name --username azureuser --password your-password
    ```
1. Sanal makinenin IP adresini bulmak için aşağıdaki komutu kullanın ve genel IP adresini not edin:

    ```sh
    az vm list-ip-addresses --name your-vm-name
    ```

1. Artık, sanal makinenize bağlanmak için SSH kullanabilirsiniz. `ssh` Komuttur bulut Kabuğu'nda önceden yüklenmiş. Önceki adımdan genel IP adresi kullanın ve parola istendiğinde, sanal makine için yapılandırılmış:

    ```sh
    ssh azureuser@public-ip-address
    ```

    Şimdi Kabuğu Docker kapsayıcıları Uzaktan izleme çözümünde çalışan sanal makinede erişebilirsiniz. Çalışan kapsayıcılar görüntülemek için aşağıdaki komutu kullanın:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>Hizmet bağlantı dizeleri bulma

Öğreticide, çözümün Cosmos DB ve IOT hub'ı Hizmetleri'ne bağlanır Visual Studio çözümü ile çalışır. Aşağıdaki adımlar bağlantı ihtiyacınız dize değerlerini bulmak için bir yol gösterir:

1. Cosmos DB bağlantı dizesi bulmak için sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Bağlantı dizesini not edin. Öğreticide daha sonra bu değeri kullanın.

1. IOT Hub bağlantı dizesine bulmak için sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Bağlantı dizesini not edin. Öğreticide daha sonra bu değeri kullanın.

> [!NOTE]
> Bu bağlantı dizeleri kullanarak veya Azure portalında bulabilirsiniz `az` komutu.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Sanal makinedeki aygıt benzetimi hizmetini durdurun

Cihaz benzetimi hizmet değiştirdiğinizde, yaptığınız değişiklikler yerel olarak test etmek için çalıştırabilirsiniz. Cihaz benzetimi hizmeti yerel olarak çalıştırmadan önce aşağıdaki gibi sanal makinede çalışan örnek durdurmanız gerekir:

1. Bulunacak **KAPSAYICI kimliği** , **aygıt benzetimi** hizmet, sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    docker ps
    ```

    Kapsayıcı Kimliğini Not **aygıt benzetimi** hizmet.

1. Durdurmak için **aygıt benzetimi** kapsayıcı, aşağıdaki komutu çalıştırın:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>GitHub depolarının kopyalama

Bu öğreticide, çalıştığınız **aygıt benzetimi** ve **depolama bağdaştırıcısı** Visual Studio projeleri. Kaynak kodu depoları github'dan kopyalayabilirsiniz. Bu adım, Visual Studio'nun yüklü olduğu yerel geliştirme makinenizde gerçekleştirin:

1. Bir komut istemi açın ve kopyasını kaydetmek istediğiniz klasöre gidin **aygıt benzetimi** ve **depolama bağdaştırıcısı** GitHub depolarının.

1. .NET sürümü kopyalamak için **aygıt benzetimi** deposu, aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    Yeni cihaz türleri benzetimli ve Uzaktan izleme çözümü cihaz benzetimi hizmetinde yerleşik sanal cihaz türleri için değişiklik yapmanızı sağlar. Özel cihaz türlerini Uzaktan izleme çözümünün davranışını fiziksel aygıtlarınızı bağlanmadan önce test etmek için kullanabilirsiniz.

1. .NET sürümü kopyalamak için **depolama bağdaştırıcısı** deposu, aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Azure/storage-adapter.git
    ```

    Cihaz benzetimi hizmeti depolama bağdaştırıcısı hizmeti Azure Cosmos DB hizmete bağlanmak için kullanır. Uzaktan izleme çözümü sanal cihaz yapılandırma verilerini Cosmos DB veritabanında depolar.

### <a name="run-the-storage-adapter-service-locally"></a>Depolama bağdaştırıcısı hizmeti yerel olarak çalıştırma

Cihaz benzetimi hizmeti depolama bağdaştırıcısı hizmeti çözümün Cosmos DB veritabanına bağlanmak için kullanır. Cihaz benzetimi hizmeti yerel olarak çalıştırırsanız, aynı zamanda depolama bağdaştırıcısı hizmeti yerel olarak çalıştırmalısınız. Aşağıdaki adımlar depolama bağdaştırıcısı hizmeti Visual Studio'dan çalıştırma gösterir:

1. Visual Studio'da açın **bilgisayarları depolama adapter.sln** yerel kopyasını çözüm dosyasında **depolama bağdaştırıcısı** deposu.

1. Çözüm Gezgini'nde sağ **WebService** seçin, proje **özellikleri**ve ardından **hata ayıklama**.

1. İçinde **ortam değişkenleri** bölümünde, değerini Düzenle **bilgisayarları\_STORAGEADAPTER\_DOCUMENTDB\_SQLCOMMAND** Cosmos DB bağlantı değişkeni daha önce not ettiğiniz dizesi. Daha sonra değişikliklerinizi kaydedin.

1. Çözüm Gezgini'nde sağ **WebService** seçin, proje **hata ayıklama**ve ardından **başlangıç yeni örnek**.

1. Hizmeti yerel olarak çalışmaya başlar ve açılır `http://localhost:9022/v1/status` varsayılan tarayıcınızda. Doğrulayın **durum** değer "Tamam: Canlı ve iyi."

1. Öğretici tamamlayıncaya kadar yerel olarak çalışan depolama bağdaştırıcısı hizmeti bırakın.

Artık her şeyin yerinde sahipsiniz ve Uzaktan izleme çözümünüz için yeni bir sanal cihaz türü eklemeye başlamak hazırsınız.

## <a name="create-a-simulated-device-type"></a>Bir sanal cihaz türü oluşturma

Cihaz benzetimi hizmetinde yeni bir aygıt türü oluşturmak için kolay kopyalamak ve mevcut bir türle değiştirmek için yoludur. Aşağıdaki adımlar nasıl yerleşik kopyalanacağını gösterir **Soğutucu** yeni bir aygıta **ampul** aygıt:

1. Visual Studio'da açın **aygıt simulation.sln** yerel kopyasını çözüm dosyasında **aygıt benzetimi** deposu.

1. Çözüm Gezgini'nde sağ **SimulationAgent** seçin, proje **özellikleri**ve ardından **hata ayıklama**.

1. İçinde **ortam değişkenleri** bölümünde, değerini Düzenle **bilgisayarları\_IOTHUB\_SQLCOMMAND** değişken IOT Hub bağlantı dizesine olması, daha önce not ettiğiniz. Daha sonra değişikliklerinizi kaydedin.

1. Çözüm Gezgini'nde sağ **aygıt benzetimi** çözüm ve **başlangıç projelerini Ayarla**. Seçin **tek başlangıç projesi** seçip **SimulationAgent**. Daha sonra, **Tamam**'a tıklayın.

1. Her cihaz türü bir JSON modeli dosyası ve ilişkili komut dosyalarında sahip **veri/Services/devicemodels** klasör. Çözüm Gezgini'nde kopyalama **Soğutucu** oluşturacak şekilde dosyaları **ampul** dosyaları aşağıdaki tabloda gösterildiği gibi:

    | Kaynak                      | Hedef                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | komut dosyalarını/Soğutucu-01-state.js | komut dosyalarını/ampul-01-state.js |
    | yeniden başlatma/betikleri-method.js    | SwitchOn/betikleri-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Yeni cihaz türünün özelliklerini tanımlayın

**Ampul 01.json** dosya türünün özelliklerini tanımlar, telemetri gibi oluşturur ve yöntemleri destekler. Aşağıdaki adımlar güncelleştirme **ampul 01.json** tanımlamak için dosya **ampul** aygıt:

1. İçinde **ampul 01.json** dosya, cihaz meta verilerini aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. İçinde **ampul 01.json** dosya, aşağıdaki kod parçacığında gösterildiği gibi benzetimi tanımını güncelleştirin:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. İçinde **ampul 01.json** dosya, aygıt türü özellikleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. İçinde **ampul 01.json** dosya, aygıt türü telemetri tanımları aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "temperature": "double",
            "temperature_unit": "text",
            "status": "text"
          }
        }
      }
    ],
    ```

1. İçinde **ampul 01.json** dosya, aygıt türü yöntemleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Kaydet **ampul 01.json** dosya.

### <a name="simulate-custom-device-behavior"></a>Özel aygıt davranışı benzetimi

**Betikleri/ampul-01-state.js** dosya benzetimi davranışını tanımlar **ampul** türü. Aşağıdaki adımlar güncelleştirme **betikleri/ampul-01-state.js** davranışını tanımlamak için dosya **ampul** aygıt:

1. Durum tanımı'nda Düzenle **betikleri/ampul-01-state.js** dosya aşağıdaki kod parçacığında gösterildiği gibi:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Ekleme bir **çevir** yükseltmesinden sonra **farklılık** aşağıdaki tanımını işleviyle:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Düzen **ana** işlevi aşağıdaki kod parçacığında gösterildiği gibi davranış uygulamak için:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Kaydet **betikleri/ampul-01-state.js** dosya.

**SwitchOn/betikleri-method.js** dosya uygulayan **anahtar üzerinde** yönteminde bir **ampul** aygıt. Aşağıdaki adımlar güncelleştirme **SwitchOn/betikleri-method.js** dosyası:

1. Durum tanımı'nda Düzenle **SwitchOn/betikleri-method.js** dosya aşağıdaki kod parçacığında gösterildiği gibi:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Ampul üzerinde geçiş yapmak için düzenleme **ana** gibi işlev:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Kaydet **SwitchOn/betikleri-method.js** dosya.

1. Bir kopya **SwitchOn/betikleri-method.js** adlı dosyaya **SwitchOff/betikleri-method.js**.

1. Ampul geçiş yapmak için düzenleme **ana** işlevi **SwitchOff/betikleri-method.js** gibi dosya:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Kaydet **SwitchOff/betikleri-method.js** dosya.

1. Çözüm Gezgini'nde, dört yeni dosyaların her birini sırayla seçin. İçinde **özellikleri** pencere her bir dosyanın doğrulayın **çıktı dizinine Kopyala** ayarlanır **yeniyse Kopyala**.

### <a name="configure-the-device-simulation-service"></a>Cihaz benzetimi hizmeti yapılandırma

Test sırasında çözümü arasında bağlantı sanal cihaz sayısını sınırlandırmak için hizmetini tek Soğutucu ve tek ampul cihaz çalışacak şekilde yapılandırın. Yapılandırma verilerini çözümün kaynak grubu Cosmos DB örneğinde depolanır. Yapılandırma verilerini düzenlemek için kullanın **Cloud Explorer** Visual Studio görünümünde:

1. Açmak için **Cloud Explorer** görünümünde Visual Studio'da, seçin **Görünüm** ve ardından **Cloud Explorer**.

1. Benzetim yapılandırma belgesini bulmak için **kaynaklar için arama** girin **simualtions.1**.

1. Çift **simulations.1** belgeyi düzenlemek için açın.

1. İçin değer **veri**, bulun **DeviceModels** aşağıdaki kod parçacığını gibi görünüyor dizi:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Tek bir Soğutucu ve tek ampul sanal bir cihaz tanımlamak için yerini **DeviceModels** dizi aşağıdaki kod ile:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Değişikliği kaydetmek **simulations.1** belge.

> [!NOTE]
> Ayrıca Cosmos DB Veri Gezgini Azure portalında düzenlemek için kullanabileceğiniz **simulations.1** belge.

### <a name="test-the-lightbulb-device-type-locally"></a>Ampul aygıt türü yerel olarak test etme

Şimdi yeni sanal ampul türünüzü aygıt benzetimi projeyi yerel olarak çalıştırarak test etmek hazırsınız.

1. Çözüm Gezgini'nde sağ **SimulationAgent**, seçin **hata ayıklama** ve ardından **başlangıç yeni örnek**.

1. İki sanal cihazlar IOT Hub'ınıza bağlanan denetlemek için tarayıcınızda Azure Portalı'nı açın.

1. Uzaktan izleme çözümünüz içeren kaynak grubunda IOT hub'ına gidin.

1. İçinde **izleme** bölümünde, seçin **ölçümleri**. Ardından doğrulayın sayısını **bağlı cihazları** iki olan:

    ![Bağlı aygıt sayısı](media/iot-suite-remote-monitoring-test/connecteddevices.png)

1. Tarayıcınızda gidin **Pano** Uzaktan izleme çözümünüz için. Üzerinde telemetri panelinde **Pano**seçin **sıcaklık**. İki sanal cihazlarınızın sıcaklık grafik görüntüler:

    ![Sıcaklık telemetri](media/iot-suite-remote-monitoring-test/telemetry.png)

Şimdi yerel olarak çalışan ampul aygıt benzetimi sahipsiniz. Sonraki adım, Uzaktan izleme mikro Azure'da çalışan sanal makineye güncelleştirilmiş simulator kodunuzu dağıtmaktır.

Devam etmeden önce aygıt benzetimi ve depolama bağdaştırıcısı Visual Studio projelerinde hata ayıklama durdurabilirsiniz.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>Güncelleştirilmiş simulator buluta Dağıt

Uzaktan izleme çözümünde mikro docker kapsayıcılarında çalıştırın. Kapsayıcılar çözümün sanal makine azure'da barındırılır. Bu bölümde şunları yapacaksınız:

* Yeni bir cihaz benzetimi docker görüntüsü oluşturun.
* Görüntü docker hub'a deponuza karşıya yükleyin.
* Görüntü çözümünüzün sanal makine alın.

Aşağıdaki adımları adlı bir depo sahip olduğunuzu varsaymaktadır **ampul** Docker hub'a hesabınızda.

1. Visual Studio içinde **aygıt benzetimi** projesi, dosyayı açma **solution\scripts\docker\build.cmd**.

1. Ayarlar satırı düzenleyin **DOCKER_IMAGE** Docker hub'a deposu adınızı ortam değişkenine:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Yaptığınız değişikliği kaydedin.

1. Visual Studio içinde **aygıt benzetimi** projesi, dosyayı açma **solution\scripts\docker\publish.cmd**.

1. Ayarlar satırı düzenleyin **DOCKER_IMAGE** Docker hub'a deposu adınızı ortam değişkenine:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Yaptığınız değişikliği kaydedin.

1. Komut istemini yönetici olarak açın. Klasöre gidin **scripts\docker** , kopyası içinde **aygıt benzetimi** GitHub depo.

1. Docker görüntü oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    build.cmd
    ```

1. Docker hub'a hesabınızda oturum açmak için aşağıdaki komutu çalıştırın:

    ```cmd
    docker login
    ```

1. Docker hub'a hesabınıza yeni görüntünüzü karşıya yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    publish.cmd
    ```

1. Karşıya yükleme doğrulamak için gidin [https://hub.docker.com/](https://hub.docker.com/). Bulun, **ampul** depo ve **ayrıntıları**. Ardından **etiketleri**:

    ![Docker hub](media/iot-suite-remote-monitoring-test/dockerhub.png)

    Eklenen komut dosyaları **sınama** görüntüye etiketi.

1. Azure'daki çözümünüzün sanal makineye bağlanmak için SSH kullanın. Ardından gidin **uygulama** klasörü ve düzenleme **docker compose.yaml** dosyası:

    ```sh
    cd /app
    sudo nano docker-compose.yaml
    ```

1. Docker görüntünüzü kullanmak aygıt benzetimi hizmeti girişini düzenleyin:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Yaptığınız değişiklikleri kaydedin.

1. Yeni ayarlarla tüm hizmetleri yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```sh
    sudo ./start.sh
    ```

1. Günlük dosyası, yeni cihaz benzetimi kapsayıcısından denetlemek için kapsayıcı Kimliğini bulmak için aşağıdaki komutu çalıştırın:

    ```sh
    docker ps
    ```

    Ardından kapsayıcı kimliği kullanarak şu komutu çalıştırın:

    ```sh
    docker logs {container ID}
    ```

Uzaktan izleme çözümünüz için cihaz benzetimi hizmet güncelleştirilmiş bir sürümünü dağıtma adımları tamamladınız.

Tarayıcınızda gidin **Pano** Uzaktan izleme çözümünüz için. Üzerinde telemetri panelinde **Pano**seçin **sıcaklık**. İki sanal cihazlarınızın sıcaklık grafik görüntüler:

![Sıcaklık telemetri](media/iot-suite-remote-monitoring-test/telemetry.png)

Üzerinde **aygıtları** sayfasında, yeni türünüzü örnekleri sağlayabilirsiniz:

![Kullanılabilir benzetimleri listesini görüntüleyin](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Sanal cihaz telemetrisinden görüntüleyebilirsiniz:

![Görünüm ampul telemetri](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Çağırabilirsiniz **SwitchOn** ve **SwitchOff** aygıtınızda yöntemleri:

![Ampul yöntemlerini çağırın](media/iot-suite-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Yeni bir telemetri türü ekleme

Bu bölümde, yeni bir telemetri türünü desteklemek için varolan bir sanal cihaz türünü değiştirme açıklar.

### <a name="locate-the-chiller-device-type-files"></a>Soğutucu aygıt türü dosyalarını bulun

Aşağıdaki adımlar yerleşik tanımlama dosyaları bulmak nasıl gösterir **Soğutucu** aygıt:

1. Zaten yapmadıysanız, kopyalamak için aşağıdaki komutu kullanın **aygıt benzetimi** GitHub deposunu yerel makinenize:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Her cihaz türü bir JSON modeli dosyası ve ilişkili komut dosyalarında sahip `data/devicemodels` klasör. Benzetim tanımlayan dosyaların **Soğutucu** aygıt türü şunlardır:

    * **data/devicemodels/chiller-01.json**
    * **data/devicemodels/scripts/chiller-01-state.js**

### <a name="specify-the-new-telemetry-type"></a>Yeni telemetri türünü belirtin

Aşağıdaki adımlar yeni bir ekleme gösterir **iç sıcaklık** için yazın **Soğutucu** aygıt türü:

1. Açık **Soğutucu 01.json** dosya.

1. Güncelleştirme **SchemaVersion** gibi değer:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. İçinde **ilk durum** bölümünde, aşağıdaki iki tanımları ekleyin:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. İçinde **Telemetri** dizisi, aşağıdaki tanımını ekleyin:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Kaydet **Soğutucu 01.json** dosya.

1. Açık **betikleri/Soğutucu-01-state.js** dosya.

1. Aşağıdaki alanları ekleyin **durumu** değişkeni:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Aşağıdaki satırı ekleyin **ana** işlevi:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Kaydet **betikleri/Soğutucu-01-state.js** dosya.

### <a name="test-the-chiller-device-type"></a>Soğutucu aygıt türü test

Güncelleştirilmiş test etmek için **Soğutucu** aygıt türü, yerel bir kopyasını ilk çalıştırma **aygıt benzetimi** , cihaz türünüz test etmek için hizmet beklendiği gibi davranır. Test ve güncelleştirilmiş cihaz türünüz yerel olarak hata ayıklaması, kapsayıcı derlemenizi ve yeniden dağıtmanızı **aygıt benzetimi** Azure hizmet.

Çalıştırdığınızda **aygıt benzetimi** Uzaktan izleme çözümünüz için telemetri gönderir hizmeti yerel olarak. Üzerinde **aygıtları** sayfasında, güncelleştirilmiş türünüz örnekleri sağlayabilirsiniz.

Test ve değişikliklerinizi yerel olarak hata ayıklama için önceki bölüme bakın [ampul aygıt türü yerel olarak Test](#test-the-lightbulb-device-type-locally).

Güncelleştirilmiş cihaz benzetimi hizmetinizi çözümün sanal makine azure'da dağıtmak için önceki bölüm bakın [güncelleştirilmiş simulator buluta dağıtmak](#deploy-the-updated-simulator-to-the-cloud).

Üzerinde **aygıtları** sayfasında, güncelleştirilmiş türünüz örnekleri sağlayabilirsiniz:

![Güncelleştirilmiş Soğutucu ekleme](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Yeni görüntüleyebilirsiniz **iç sıcaklık** sanal cihaz telemetrisinden.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Yeni bir aygıt türü oluşturma
> * Özel aygıt davranışı benzetimi
> * Yeni bir cihaz türü için Pano ekleyin
> * Var olan bir cihaz türünden özel telemetri Gönder

Şimdi cihaz benzetimi Hizmeti özelleştirmeyi öğrendiniz. Önerilen sonraki adıma edinmektir nasıl [bir fiziksel aygıt, Uzaktan izleme çözümüne bağlama](iot-suite-connecting-devices-node.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->