---
title: Cihaz benzetimi Uzaktan izleme çözümünde - Azure | Microsoft Docs
description: Bu öğretici ile Uzaktan izleme çözüm Hızlandırıcısını cihaz simülatörü kullanma işlemini gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/15/2018
ms.topic: conceptual
ms.openlocfilehash: 9c0c1ba9dd343baa453f10ad82c0cc8b8e69da7b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596163"
---
# <a name="create-a-new-simulated-device"></a>Yeni bir simülasyon cihazı oluşturun

Bu öğretici, cihaz simülatörü mikro Uzaktan izleme çözüm Hızlandırıcısını özelleştirme işlemini göstermektedir. Cihaz simülatörü özelliklerini göstermek için Bu öğreticide iki senaryo Contoso IOT uygulama kullanılır.

Aşağıdaki video, cihaz simülatörü mikro hizmet özelleştirme seçenekleri'ne genel bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

Bu senaryoda, yeni bir akıllı ampul cihazı test etmek Contoso istiyor. Testleri gerçekleştirmek için aşağıdaki özelliklere sahip yeni bir simülasyon cihazı oluşturun:

*Özellikleri*

| Ad                     | Değerler                      |
| ------------------------ | --------------------------- |
| Renk                    | Beyaz, kırmızı, mavi            |
| Parlaklık               | 0'dan 100'e                    |
| Tahmini kalan kullanım ömrü | Geri sayım 10.000 saat |

*Telemetri*

Aşağıdaki tabloda, bir veri akışı olarak buluta ampul rapor verilerini gösterir:

| Ad   | Değerler      |
| ------ | ----------- |
| Durum | "açık", "kapalı" |
| Sıcaklık | Derece F |
| Çevrimiçi | TRUE, false |

> [!NOTE]
> **Çevrimiçi** telemetri değeri tüm sanal türleri için zorunludur.

*Yöntemleri*

Aşağıdaki tablo, yeni cihazın desteklediği eylemleri gösterir:

| Ad        |
| ----------- |
| Geçiş   |
| Kapat  |

*Başlangıç durumu*

Aşağıdaki tabloda, cihaz ilk durumunu gösterir:

| Ad                     | Değerler |
| ------------------------ | -------|
| İlk renk            | Beyaz  |
| İlk parlaklık       | 75     |
| İlk kalan kullanım ömrü   | 10,000 |
| İlk telemetri durumu | "on"   |
| İlk telemetri sıcaklık | 200   |

İkinci senaryoda Contoso için yeni bir telemetri türünü, mevcut olan eklediğiniz **Soğutucu** cihaz.

Bu öğretici ile Uzaktan izleme çözüm Hızlandırıcısını cihaz simülatörü kullanma işlemini gösterir:

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Yeni bir cihaz türü oluşturma
> * Özel cihaz davranışını benzetmekte
> * Yeni bir cihaz türü panoya ekleme
> * Varolan bir aygıtı türden özel telemetri gönderme

Aşağıdaki video, sanal ve gerçek cihazlar Uzaktan izleme çözümüne bağlama bir kılavuz gösterir:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-38-Customizing-Azure-IoT-Suite-solution-and-connect-a-real-device/Player]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi uygulamak için ihtiyacınız vardır:

* Uzaktan izleme çözümü, Azure aboneliğinizde dağıtılmış bir örneği. Uzaktan izleme çözümünü dağıttığınız henüz henüz tamamlamanız gereken [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](../iot-accelerators/quickstart-remote-monitoring-deploy.md) öğretici.

* Visual Studio 2017. Visual Studio 2017 yoksa, ücretsiz indirebileceğiniz [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) sürümü.

* [Visual Studio 2017 için cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) Visual Studio uzantısı.

* Bir hesapta [Docker Hub](https://hub.docker.com/). Ücretsiz kullanmaya başlamak kaydolabilirsiniz.

* [Git](https://git-scm.com/downloads) Masaüstü makinenizde yüklü.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Uzaktan izleme çözümünüze yeni bir simülasyon cihazı eklemek için geliştirme ortamınızı hazırlamak üzere aşağıdaki görevleri tamamlayın:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>Azure'da çözüm sanal makineye SSH erişimini yapılandırma

Uzaktan izleme çözümünüzü oluşturduğunuzda [www.azureiotsolutions.com](https://www.azureiotsolutions.com), seçtiğiniz bir çözüm adı. Çözüm adı çözümü kullanan çeşitli dağıtılan kaynakları içeren Azure kaynak grubunun adı olur. Adlı bir kaynak grubu aşağıdaki komutları kullanın **Contoso-01**, yerini mi **Contoso-01** yerine kaynak grubunuzun adını.

Aşağıdaki komutları kullanın `az` komutunu [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest). Azure CLI 2.0 geliştirme makinenize yükleyin veya kullanın [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) içinde [Azure portalında](http://portal.azure.com). Azure CLI 2.0, Cloud Shell'de önceden yüklüdür.

1. Uzaktan izleme kaynaklarınızı içeren kaynak grubunu adını doğrulamak için aşağıdaki komutu çalıştırın:

    ```sh
    az group list | grep "name"
    ```

    Bu komut, aboneliğinizdeki tüm kaynak gruplarını listeler. Uzaktan izleme çözümünüzü aynı ada sahip bir kaynak grubu içermelidir.

1. Kaynağınız için sonraki komutlarda varsayılan grubu yapmak yerine kaynak grubunuzun adı kullanarak şu komutu çalıştırın. **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Kaynak grubunuzda kaynakları listelemek için aşağıdaki komutu çalıştırın:

    ```sh
    az resource list -o table
    ```

    Sanal makinenizi ve ağ güvenlik grubunuzu adlarını not edin. Sonraki adımlarda bu değerleri kullanırsınız.

1. Sanal makinenizi SSH erişimini etkinleştirmek için önceki adımdan gelen ağ güvenlik grubu adını kullanarak şu komutu çalıştırın:

    ```sh
    az network nsg rule create --name SSH --nsg-name YOUR-NETWORK-SECURITY-GROUP --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Ağınızın gelen kural listesini görüntülemek için aşağıdaki komutu çalıştırın:

    ```sh
    az network nsg rule list --nsg-name YOUR-NETWORK-SECURITY-GROUP -o table
    ```

    Yalnızca, test ve geliştirme sırasında SSH erişimini etkinleştirmeniz gerekir. SSH, etkinleştirirseniz [yeniden olabildiğince çabuk devre dışı](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-azure-virtual-machines)

1. Sanal makine parolası bildiğiniz bir parolayla değiştirmek için aşağıdaki komutu çalıştırın. Sanal makine, daha önce not ettiğiniz adı ve parola, tercih ettiğiniz kullanın:

    ```sh
    az vm user update --name YOUR-VM-NAME --username azureuser --password YOUR-PASSWORD
    ```
1. Sanal makinenizin IP adresini bulmak için aşağıdaki komutu kullanın ve genel IP adresini not edin:

    ```sh
    az vm list-ip-addresses --name YOUR-VM-NAME
    ```

1. Artık, sanal makinenize bağlanmak için SSH kullanabilirsiniz. `ssh` Komutu, Cloud Shell'de önceden yüklenmiş. Önceki adımdan gelen genel IP adresini kullanın ve parola istendiğinde, sanal makine için yapılandırılan:

    ```sh
    ssh azureuser@public-ip-address
    ```

    Artık Kabuk erişimi Uzaktan izleme çözümünde Docker kapsayıcılarını çalıştıran sanal makine var. Çalışan kapsayıcıları görmek için aşağıdaki komutu kullanın:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>Hizmet bağlantısı dizeleri bulun

Öğreticide, çözümün Cosmos DB ve IOT hub'ı hizmetlerine bağlanan Visual Studio çözümü ile çalışırsınız. Aşağıdaki adımlar bağlantı ihtiyacınız dize değerleri bulmanın bir yolu gösterir:

1. Cosmos DB bağlantı dizesi bulmak için sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Bağlantı dizesini not edin. Bu değeri öğreticinin sonraki bölümlerinde kullanacaksınız.

1. IOT Hub bağlantı dizesine bulmak için sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Bağlantı dizesini not edin. Bu değeri öğreticinin sonraki bölümlerinde kullanacaksınız.

> [!NOTE]
> Bu bağlantı dizeleri kullanarak veya Azure portalında bulabilirsiniz `az` komutu.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Cihaz benzetimi hizmetinde sanal makineyi Durdur

Cihaz benzetimi hizmeti değiştirdiğinizde, değişikliklerinizi yerel olarak test çalıştırabilirsiniz. Cihaz benzetimi hizmeti yerel olarak çalıştırmadan önce aşağıdaki gibi sanal makinede çalışan örneği durdurmanız gerekir:

1. Bulunacak **KAPSAYICI kimliği** , **cihaz benzetimi dotnet** hizmeti, sanal makineye bağlı SSH oturumunda aşağıdaki komutu çalıştırın:

    ```sh
    docker ps
    ```

    Kapsayıcı kimliği Not **cihaz benzetimi dotnet** hizmeti.

1. Durdurmak için **cihaz benzetimi dotnet** kapsayıcı, aşağıdaki komutu çalıştırın:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>GitHub depoları kopyalayın

Bu öğreticide, çalıştığınız **cihaz benzetimi** ve **depolama bağdaştırıcısı** Visual Studio projeleri. Github'dan kaynak kodu depoları kopyalayabilirsiniz. Visual Studio'nun yüklü olduğu yerel geliştirme makinenizde bu adımı uygulayın:

1. Bir komut istemi açın ve kopyasını kaydetmek istediğiniz klasöre gidin **cihaz benzetimi** ve **depolama bağdaştırıcısı** GitHub depoları.

1. .NET sürümü kopyalamak için **cihaz benzetimi** depo, aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    Cihaz benzetimi hizmet Uzaktan izleme çözümünde yerleşik sanal cihaz türleri için değişiklik yapmanızı sağlar ve yeni cihaz türleri benzetimi. Özel cihaz türlerini Uzaktan izleme çözümünün davranışını fiziksel cihazlarınızı bağlayın önce test etmek için kullanabilirsiniz.

1. .NET sürümü kopyalamak için **depolama bağdaştırıcısı** depo, aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Azure/pcs-storage-adapter-dotnet.git
    ```

    Cihaz benzetimi hizmeti depolama bağdaştırıcısı hizmeti Azure Cosmos DB hizmetine bağlanmak için kullanır. Uzaktan izleme çözümü, sanal cihaz yapılandırma verileri bir Cosmos DB veritabanında depolar.

### <a name="run-the-storage-adapter-service-locally"></a>Depolama bağdaştırıcısı hizmeti yerel olarak çalıştırma

Cihaz benzetimi hizmeti, çözümün Cosmos DB veritabanına bağlanmak için depolama bağdaştırıcısı hizmeti kullanır. Cihaz benzetimi hizmeti yerel olarak çalıştırırsanız, depolama bağdaştırıcısı hizmeti yerel olarak da çalıştırmanız gerekir. Aşağıdaki adımlar Visual Studio'dan depolama bağdaştırıcısı hizmeti çalıştırma işlemini gösterir:

1. Visual Studio'da açın **bilgisayarları depolama adapter.sln** çözüm dosyasında, yerel kopyasını **depolama bağdaştırıcısı** depo.

1. Çözüm Gezgini'nde sağ **WebService** projesinin **özellikleri**ve ardından **hata ayıklama**.

1. İçinde **ortam değişkenlerini** bölümünde, değerini düzenlemek **bilgisayarları\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** Cosmos DB bağlantı olmasını değişkeni daha önce not ettiğiniz dize. Değişikliklerinizi kaydedin.

1. Çözüm Gezgini'nde sağ **WebService** projesinin **hata ayıklama**ve ardından **yeni örnek Başlat**.

1. Hizmet yerel olarak çalışmaya başlar ve açılır `http://localhost:9022/v1/status` varsayılan tarayıcınızda. Doğrulayın **durumu** değeridir "Tamam: etkin ve iyi."

1. Öğreticisi tamamlanana kadar yerel olarak çalışan depolama bağdaştırıcısı hizmeti bırakın.

Artık her şeyi yerinde sahipsiniz ve Uzaktan izleme çözümünüze yeni bir sanal cihaz türü eklemeye başlamak hazırsınız.

## <a name="create-a-simulated-device-type"></a>Bir sanal cihaz türü oluşturma

Cihaz benzetimi hizmetinde yeni bir cihaz türü oluşturmak için en kolay yolu, var olan bir türü kopyalayıp sağlamaktır. Yerleşik kopyalamak nasıl bir aşağıdaki adımlarda **Soğutucu** yeni bir cihaz **ampul** cihaz:

1. Çözüm Gezgini'nde sağ **WebService** projesinin **özellikleri**ve ardından **hata ayıklama**.

1. İçinde **ortam değişkenlerini** bölümünde, değerini düzenlemek **bilgisayarları\_IOTHUB\_CONNSTRING** değişken IOT Hub bağlantı dizesine olmasını, daha önce not ettiğiniz. Değişikliklerinizi kaydedin.

1. Çözüm Gezgini'nde sağ **cihaz benzetimi** çözüm ve **başlangıç projelerini Ayarla**. Seçin **tek başlangıç projesi** seçip **WebService**. Daha sonra, **Tamam**'a tıklayın.

1. Her cihaz türünün bir JSON model dosyası ve ilişkili betikler içinde olan **veri/Services/devicemodels** klasör. Çözüm Gezgini'nde kopyalama **Soğutucu** oluşturacak şekilde dosyaları **ampul** aşağıdaki tabloda gösterildiği gibi dosyalar:

    | Kaynak                      | Hedef                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | Ampul 01.json             |
    | betikleri/Soğutucu-01-state.js | betikleri/ampul-01-state.js |
    | yeniden başlatma/betikleri-method.js    | betikleri/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Yeni cihaz türü özelliklerini tanımlar

**Ampul 01.json** dosyası türü özelliklerini tanımlar, bu gibi telemetri oluşturur ve yöntemleri destekler. Aşağıdaki adımlar güncelleştirme **ampul 01.json** tanımlamak için dosya **ampul** cihaz:

1. İçinde **ampul 01.json** dosyasında, aşağıdaki kod parçacığında gösterildiği gibi cihaz meta verilerini güncelleştir:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. İçinde **ampul 01.json** dosya, benzetim tanımını aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Interval": "00:00:20",
      "Scripts": [
        {
          "Type": "javascript",
          "Path": "lightbulb-01-state.js"
        }
      ]
    },
    ```

1. İçinde **ampul 01.json** dosya, cihaz türü özellikleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. İçinde **ampul 01.json** dosyasında, aşağıdaki kod parçacığında gösterildiği gibi cihaz türü telemetri tanımları güncelleştir:

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

1. İçinde **ampul 01.json** dosya, cihaz türü yöntemleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

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

### <a name="simulate-custom-device-behavior"></a>Özel cihaz davranışını benzetmekte

**Betikleri/ampul-01-state.js** dosya benzetim davranışını tanımlar **ampul** türü. Aşağıdaki adımlar güncelleştirme **betikleri/ampul-01-state.js** davranışını tanımlamak için dosya **ampul** cihaz:

1. Durum tanımında Düzenle **betikleri/ampul-01-state.js** aşağıdaki kod parçacığında gösterildiği gibi dosya:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Ekleme bir **çevir** yükseltmesinden sonra **farklılık** aşağıdaki tanımıyla işlevi:

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

1. Düzen **ana** işlevi davranışı aşağıdaki kod parçacığında gösterildiği gibi uygulamak için:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global device properties and the global state before
        // generating the new telemetry, so that the telemetry can apply changes
        // using the previous function state.
        restoreSimulation(previousState, previousProperties);

        state.temperature = vary(200, 5, 150, 250);

        // Make this flip every so often
        state.status = flip(state.status);

        updateState(state);

        return state;
    }
    ```

1. Kaydet **betikleri/ampul-01-state.js** dosya.

**SwitchOn/betikleri-method.js** dosya uygular **anahtar üzerinde** yönteminde bir **ampul** cihaz. Aşağıdaki adımlar güncelleştirme **SwitchOn/betikleri-method.js** dosyası:

1. Durum tanımında Düzenle **SwitchOn/betikleri-method.js** aşağıdaki kod parçacığında gösterildiği gibi dosya:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Ampul üzerinde geçiş yapmak için Düzenle **ana** gibi işlev:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Kaydet **SwitchOn/betikleri-method.js** dosya.

1. Bir kopya **SwitchOn/betikleri-method.js** adlı dosya **SwitchOff/betikleri-method.js**.

1. Ampul geçiş yapmak için Düzenle **ana** işlevi **SwitchOff/betikleri-method.js** aşağıdaki gibi:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Kaydet **SwitchOff/betikleri-method.js** dosya.

1. Çözüm Gezgini içinde her biri dört yeni dosyalarınızı sırayla seçin. İçinde **özellikleri** her dosya için pencere doğrulayın **çıkış dizinine Kopyala** ayarlanır **yeniyse Kopyala**.

### <a name="configure-the-device-simulation-service"></a>Cihaz benzetimi hizmeti yapılandırma

Test sırasında çözümüne bağlama sanal cihaz sayısını sınırlandırmak için hizmetini tek Soğutucu ve tek bir ampul cihaz çalışacak şekilde yapılandırın. Yapılandırma verilerini Cosmos DB örneğine çözümün kaynak grubu içinde depolanır. Yapılandırma verilerini düzenlemek için **Cloud Explorer** Visual Studio'da görüntüle:

1. Açmak için **Cloud Explorer** görüntülemek Visual Studio'da öğesini **görünümü** ardından **Cloud Explorer**.

1. Benzetim yapılandırma belgesi bulmak için **kaynak Ara** girin **simualtions.1**.

1. Çift **simulations.1** belge düzenlemek üzere açın.

1. İçin değer **veri**, bulun **DeviceModels** aşağıdaki kod parçacığı gibi görünen bir dizi:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Tek bir Soğutucu ve tek bir ampul sanal bir cihazı tanımlamak için değiştirin **DeviceModels** dizi aşağıdaki kod ile:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Değişikliği kaydetmek **simulations.1** belge.

> [!NOTE]
> Ayrıca Cosmos DB Veri Gezgini Azure portalında düzenlemek için kullanabileceğiniz **simulations.1** belge.

### <a name="test-the-lightbulb-device-type-locally"></a>Ampul cihaz türü yerel olarak test etme

Yeni sanal ampul türünüz cihaz benzetimi projeyi yerel olarak çalıştırarak test etmek artık hazırsınız.

1. Çözüm Gezgini'nde sağ **WebService**, seçin **hata ayıklama** seçip **yeni örnek Başlat**.

1. İki sanal cihazlar IOT Hub'ınıza bağlı olduğundan emin olun için Azure portal, tarayıcınızda açın.

1. Uzaktan izleme çözümünüzü içeren kaynak grubunu IOT hub'ına gidin.

1. İçinde **izleme** bölümünde, seçin **ölçümleri**. Ardından doğrulayın sayısını **bağlı cihazları** ikidir:

    ![Bağlı cihazların sayısı](./media/iot-accelerators-remote-monitoring-test/connecteddevices.png)

1. Tarayıcınızda gidin **Pano** Uzaktan izleme çözümünüz için. Üzerinde telemetri panelinde **Pano**seçin **sıcaklık**. Sanal cihazlarınızın sıcaklık grafiği görüntüler:

    ![Sıcaklık telemetrisi](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

Şimdi yerel olarak çalışan ampul cihaz benzetimi sahipsiniz. Sonraki adım, Uzaktan izleme mikro Hizmetleri Azure'da çalışan sanal makineye güncelleştirilmiş simülatör kodunuzu dağıtmaktır.

Devam etmeden önce hem cihaz benzetimi hem de depolama bağdaştırıcısı projeleri Visual Studio'da hata ayıklama durdurabilirsiniz.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>Güncelleştirilmiş simülatör buluta dağıtın

Uzaktan izleme çözümünde mikro Hizmetleri docker kapsayıcılarında çalıştırın. Kapsayıcıları, Azure'da sanal makine çözümün içinde barındırılır. Bu bölümde şunları yapacaksınız:

* Yeni cihaz benzetimi docker görüntüsü oluşturun.
* Görüntüyü docker hub deponuza yükleyin.
* Görüntü çözümünüzün sanal makine aktarın.

Aşağıdaki adımları olarak adlandırılan bir depoya sahip olduğunuzu varsaymaktadır **ampul** Docker Hub hesabı.

1. Visual Studio içinde **cihaz benzetimi** dosyasını açın, proje **solution\scripts\docker\build.cmd**.

1. Ayarlar satırı düzenleyin **DOCKER_IMAGE** ortam değişkeni, Docker Hub depo adı:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Yaptığınız değişikliği kaydedin.

1. Visual Studio içinde **cihaz benzetimi** dosyasını açın, proje **solution\scripts\docker\publish.cmd**.

1. Ayarlar satırı düzenleyin **DOCKER_IMAGE** ortam değişkeni, Docker Hub depo adı:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Yaptığınız değişikliği kaydedin.

1. Yönetici olarak bir komut istemi açın. Ardından klasörüne gidin **scripts\docker** , kopyasını içinde **cihaz benzetimi** GitHub deposu.

1. Docker görüntüsünü oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    build.cmd
    ```

1. Docker Hub hesabınıza oturum açmak için aşağıdaki komutu çalıştırın:

    ```cmd
    docker login
    ```

1. Docker Hub hesabınıza yeni görüntünüzü karşıya yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    publish.cmd
    ```

1. Karşıya yükleme doğrulamak için gidin [ https://hub.docker.com/ ](https://hub.docker.com/). Bulun, **ampul** depo seçin **ayrıntıları**. Ardından **etiketleri**:

    ![Docker hub'ı](./media/iot-accelerators-remote-monitoring-test/dockerhub.png)

    Eklenen komut dosyaları **test** görüntüye etiketi.

1. Çözümünüzün azure'daki sanal makineye bağlanmak için SSH kullanın. Ardından gidin **uygulama** klasörü ve düzenleme **docker-compose.yml** dosyası:

    ```sh
    cd /app
    sudo nano docker-compose.yml
    ```

1. Docker görüntünüzü kullanmak cihaz benzetimi hizmeti için girişi düzenleyin:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Yaptığınız değişiklikleri kaydedin.

1. Yeni ayarlarla tüm hizmetleri yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```sh
    sudo ./start.sh
    ```

1. Yeni cihaz benzetimi kapsayıcınızı günlük dosyasından denetlemek için kapsayıcı Kimliğini bulmak için aşağıdaki komutu çalıştırın:

    ```sh
    docker ps
    ```

    Ardından kapsayıcı kimliği kullanarak şu komutu çalıştırın:

    ```sh
    docker logs {container ID}
    ```

Uzaktan izleme çözümünüze cihaz benzetimi hizmeti güncelleştirilmiş bir sürümünü dağıtma adımları tamamladınız.

Tarayıcınızda gidin **Pano** Uzaktan izleme çözümünüz için. Üzerinde telemetri panelinde **Pano**seçin **sıcaklık**. İki sanal cihazlarınızın sıcaklık grafiği görüntüler:

![Sıcaklık telemetrisi](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

Üzerinde **cihazları** sayfasında, yeni örneklerinin sağlayabilirsiniz:

![Kullanılabilir benzetimleri listesini görüntüleyin](./media/iot-accelerators-remote-monitoring-test/devicesmodellist.png)

Sanal CİHAZDAN telemetri görüntüleyebilirsiniz:

![Ampul telemetrisini görüntüleme](./media/iot-accelerators-remote-monitoring-test/devicestelemetry.png)

Çağırabilirsiniz **SwitchOn** ve **SwitchOff** Cihazınızda yöntemler:

![Ampul yöntemlerini çağırın](./media/iot-accelerators-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Yeni bir telemetri türü Ekle

Bu bölümde, yeni bir telemetri türünü desteklemek için mevcut bir sanal cihaz türünü değiştirmek açıklar.

### <a name="locate-the-chiller-device-type-files"></a>Soğutucu aygıt türü dosyalarını bulun

Yerleşik tanımlama dosyaları bulmak nasıl bir aşağıdaki adımlarda **Soğutucu** cihaz:

1. Zaten yapmadıysanız, kopyalamak için aşağıdaki komutu kullanın **cihaz benzetimi dotnet** GitHub deposunu yerel makinenize:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Her cihaz türünün bir JSON model dosyası ve ilişkili betikler içinde olan `data/devicemodels` klasör. Benzetim tanımlayan dosyaların **Soğutucu** cihaz türü:

    * **data/devicemodels/chiller-01.json**
    * **Data/devicemodels/scripts/chiller-01-State.js**

### <a name="specify-the-new-telemetry-type"></a>Yeni bir telemetri türünü belirtin

Aşağıdaki adımlarda yeni bir gösterilmektedir **iç ortam sıcaklığı** için yazın **Soğutucu** cihaz türü:

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

### <a name="test-the-chiller-device-type"></a>Test Soğutucu cihaz türü

Güncelleştirilmiş test etmek için **Soğutucu** cihaz türü, yerel bir kopyasını ilk çalıştırma **cihaz benzetimi dotnet** cihaz türünüz test etmek için hizmet beklendiği gibi davranır. Test ve güncelleştirilmiş cihaz türünüz yerel olarak hata ayıklaması yaparken kapsayıcıyı yeniden derlemeye ve yeniden dağıtma **cihaz benzetimi dotnet** azure'a hizmet.

Çalıştırdığınızda **cihaz benzetimi dotnet** Uzaktan izleme çözümünüzü telemetri gönderdiği hizmeti yerel olarak. Üzerinde **cihazları** sayfasında, güncelleştirilmiş örneklerinin sağlayabilirsiniz.

Değişikliklerinizi yerel olarak hata ayıklama ve test etmek için önceki bölüme bakın [ampul cihaz türü yerel olarak Test](#test-the-lightbulb-device-type-locally).

Güncelleştirilmiş cihaz benzetimi hizmetinizi çözümün Azure'da sanal makine dağıtmak için önceki bölüme bakın [güncelleştirilmiş simülatör buluta dağıtın](#deploy-the-updated-simulator-to-the-cloud).

Üzerinde **cihazları** sayfasında, güncelleştirilmiş örneklerinin sağlayabilirsiniz:

![Güncelleştirilmiş Soğutucu Ekle](./media/iot-accelerators-remote-monitoring-test/devicesupdatedchiller.png)

Yeni görüntüleyebilirsiniz **iç ortam sıcaklığı** sanal CİHAZDAN telemetri.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Yeni bir cihaz türü oluşturma
> * Özel cihaz davranışını benzetmekte
> * Yeni bir cihaz türü panoya ekleme
> * Varolan bir aygıtı türden özel telemetri gönderme

Artık cihaz benzetimi hizmetin özelleştirmek nasıl öğrendiniz. Önerilen sonraki adıma öğrenmektir nasıl [fiziksel bir cihaz, Uzaktan izleme çözümüne bağlama](iot-accelerators-connecting-devices-node.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
