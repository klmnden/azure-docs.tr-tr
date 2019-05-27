---
title: Yerel olarak (aracılığıyla Intellij IDE için) - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için Intellij kullanarak yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: 2b55fea69fe1affb6cab5d360f1e8355c3bb720d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "66015443"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Intellij dağıtma

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Mikro hizmetler Intellij çalıştırmayı öğrenin. Yerel mikro hizmetlerin dağıtımı aşağıdaki bulut hizmetlerini kullanır: Bulutta IOT Hub, Cosmos DB, Azure akış analizi ve Azure Time Series Insights Hizmetleri.

Uzaktan izleme çözüm Hızlandırıcısını Docker'da yerel makinenizde çalıştırmak istiyorsanız, bkz. [Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Docker dağıtma](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine Kurulumu

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
* [IntelliJ Plugin Scala](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ Plugin SBT](https://plugins.jetbrains.com/plugin/5007-sbt)
* [Intellij eklentisini SBT Yürütücü](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Intellij IDE, Windows ve Mac için kullanılabilir

## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Uzaktan izleme kaynak kodu depoları, kaynak kodu ve mikro Hizmetleri Docker görüntülerini çalıştırmak için gereken Docker yapılandırma dosyalarını içerir.

Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gitmek için komut satırı ortamı kullanın. Ardından komutların java depoyu kopyalamak için aşağıdaki adımlardan birini çalıştırın:

Java mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Bu komutlar, mikro Hizmetleri yerel olarak çalıştırmak için kullandığınız komut dosyalarının yanı sıra tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro hizmetler Docker'da çalıştırma için kaynak kodu gerekmez ancak daha sonra çözüm Hızlandırıcısını değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Azure hizmetlerini dağıtmak için aşağıdaki betiği kullanın. Aşağıdaki betik örnekleri, bir Windows makinede java deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın.

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturma

Gerekli Azure kaynakları henüz oluşturduysanız, şu adımları izleyin:

1. Komut satırı ortamınızda gidin **\services\scripts\local\launch** kopyaladığınız deponun klasöründe.

1. Yüklemek için aşağıdaki komutları çalıştırın **bilgisayarları** CLI aracını kullanarak ve Azure hesabınızda oturum açın:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Çalıştırma **start.cmd** betiği. Betik için aşağıdaki bilgileri ister:
   * Bir çözüm adı.
   * Kullanılacak Azure aboneliği.
   * Kullanmak için Azure veri merkezi konumu.

     Betik, çözümünüzün adına ile Azure'da kaynak grubu oluşturur. Bu kaynak grubu, çözüm Hızlandırıcısını Azure kaynaklarını içerir. İlgili kaynaklara artık ihtiyacınız sonra bu kaynak grubunu silebilirsiniz.

     Betik ayrıca bir ön ek ortam değişkenlerini kümesi ekler **bilgisayarları** yerel makinenize. Bu ortam değişkenleri, bir Azure Key Vault kaynaktan okumak, Uzaktan izleme ayrıntılarını sağlayın. Uzaktan izleme kendi yapılandırma değerlerinden burada okuyacaksa bu Key Vault kaynaktır.

     > [!TIP]
     > Betik tamamlandığında, bu da adlı bir dosyaya ortam değişkenlerini kaydeder  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** . Gelecekteki çözüm Hızlandırıcı dağıtımları için bunları kullanabilirsiniz. Yerel makinenizde, herhangi bir ortam değişkenini değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.

1. Komut satırı ortamınızdan çıkın.

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynakları

Gerekli Azure kaynakları zaten oluşturduysanız, yerel makinenizde karşılık gelen ortam değişkenleri oluşturun.
Aşağıdaki ortam değişkenlerini ayarlayın:
* **PCS_KEYVAULT_NAME** -Azure Key Vault kaynak adı
* **PCS_AAD_APPID** -AAD uygulama kimliği
* **PCS_AAD_APPSECRET** -AAD uygulama gizli anahtarı

Bu Azure anahtar kasası kaynak yapılandırma değerlerini okur. Bu ortam değişkenlerini de kaydedilebilir  **\<, giriş klasörü\>\\.pcs\\\<çözüm adı\>.env** dağıtım dosyasından. Yerel makinenizde ortam değişkenleri değerleri geçersiz kıldığını unutmayın **Hizmetleri\\betikleri\\yerel\\.env** dosyasını çalıştırdığınızda **docker-compose**.

Mikro hizmet tarafından gereken yapılandırma bazıları örneğinde depolanır **Key Vault** ilk dağıtımı oluşturuldu. Karşılık gelen anahtar kasası değişkenleri gerektiği şekilde değiştirilmesi gerekir.

## <a name="run-the-microservices"></a>Mikro Hizmetleri çalıştırın

Bu bölümde, Uzaktan izleme mikro Hizmetleri çalıştırın. Web kullanıcı Arabirimi yerel olarak çalıştırdığınızda, cihaz benzetimi, Docker kimlik doğrulama ve ASA Yöneticisi hizmeti ve Intellij mikro Hizmetleri.

### <a name="run-the-device-simulation-service"></a>Cihaz benzetimi hizmet çalıştırma

Ayarlanan ortam değişkenlerine erişebilir emin olmak için yeni bir komut istemi penceresi açın **start.cmd** önceki bölümde betiği.

Cihaz benzetimi hizmeti için Docker kapsayıcısı başlatmak için aşağıdaki komutu çalıştırın. Hizmeti cihazları için Uzaktan izleme çözümü benzetimini yapar.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Kimlik doğrulama hizmeti Çalıştır

Yeni bir komut istemi penceresi açın ve kimlik doğrulama hizmeti için Docker kapsayıcısı başlatmak için aşağıdaki komutu çalıştırın. Azure IOT çözümleri erişmeye yetkili kullanıcıları yönetmek için hizmet sağlar.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-asa-manager-service"></a>ASA Yöneticisi hizmeti Çalıştır

Yeni bir komut istemi penceresi açın ve ASA Manager hizmeti için Docker kapsayıcısı başlatmak için aşağıdaki komutu çalıştırın. Hizmet yapılandırması ve başlatılmasını, durdurmasını ve bunların durumlarını izleme de dahil olmak üzere, Azure Stream Analytics (ASA) işleri yönetilmesine izin verir.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Yerel makinede diğer mikro hizmetlerin dağıtımı

Aşağıdaki adımları Uzaktan izleme mikro hizmetler Intellij çalıştırma işlemini gösterir:

#### <a name="import-project"></a>Projeyi İçeri Aktar

1. Launch IntelliJ IDE
1. Seçin **Import Project** ve **azure-iot-pcs-remote-monitoring-java\services\build.sbt**

#### <a name="create-run-configurations"></a>Çalıştırma yapılandırmaları oluşturma

1. Seçin **Çalıştır > yapılandırmaları Düzenle**
1. Seçin **yeni Yapılandırması Ekle > sbt görevi** 
1. Girin **adı** girin **görevleri** çalışacak şekilde 
1. Seçin **çalışma dizini** çalıştırmak istediğiniz hizmetini temel alan
1. Tıklayın **Uygula > Tamam** seçimlerinizi kaydetmek için.
1. Aşağıdaki hizmetler için çalıştırma yapılandırmaları oluşturun:
    * Web hizmeti (services\config)
    * Web hizmeti (telemetri services\device)
    * Web hizmeti (services\iothub Yöneticisi)
    * Web hizmeti (services\storage bağdaştırıcısı)

Örneğin, aşağıdaki görüntüde bir hizmeti yapılandırması ekleniyor:

[![Yapılandırması Ekle](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)


#### <a name="create-compound-configuration"></a>Bileşik yapılandırması oluştur

1. Tüm hizmetleri çalıştırmak için birlikte seçin **yeni Yapılandırması Ekle > Bileşik**
1. Girin **adı** ve **sbt görev ekleyin**
1. Tıklayın **Uygula > Tamam** seçimlerinizi kaydetmek için.

Örnek olarak, tüm sbt görevler için tek bir yapılandırması ekleme, aşağıdaki resimde gösterilmektedir:

[![Ekle-All-Services](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Tıklayın **çalıştırma** oluşturun ve web hizmetleri yerel makinede çalıştırın.

Her web hizmeti, bir komut istemi'ni ve web tarayıcı penceresi açılır. Komut isteminde çalışan hizmetin çıktısını görürsünüz ve tarayıcının durumunu izlemenize olanak tanır. Komut istemleri veya web sayfaları kapatmayın, bu eylem web hizmetini durdurur.


Hizmetlerin durumunu erişmek için aşağıdaki URL'lere gidebilirsiniz:
* IOT Hub Yöneticisi [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Cihaz Telemetrisi  [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* yapılandırma [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* Depolama bağdaştırıcısı [http://localhost:9022/v1/status](http://localhost:9022/v1/status)


### <a name="start-the-stream-analytics-job"></a>Stream Analytics işini başlatın

Stream Analytics işi başlatmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Gidin **kaynak grubu** çözümünüz için oluşturulur. Kaynak grubunun adı çalıştırdığınızda çözümünüz için seçtiğiniz addır **start.cmd** betiği.
1. Tıklayın **Stream Analytics işi** kaynakları listesinde.
1. Stream Analytics işinde **genel bakış** sayfasında **Başlat** düğmesi. Ardından **Başlat** işini şimdi başlatmak için.

### <a name="run-the-web-ui"></a>Web kullanıcı arabirimini çalıştırma

Bu adımda, web kullanıcı Arabirimi başlatın. Ayarlanan ortam değişkenlerine erişebilir emin olmak için yeni bir komut istemi penceresi açın **start.cmd** betiği. Gidin **webui** yerel klasöründe depoyu kopyalayın ve aşağıdaki komutları çalıştırın:

```cmd
npm install
npm start
```

Başlangıç tamamlandıktan sonra tarayıcınızı sayfası görüntüler **http:\//localhost:3000 / Pano**. Bu sayfadaki hataları beklenmektedir. Uygulama hatasız görüntülemek için aşağıdaki adımı tamamlayın.

### <a name="configure-and-run-nginx"></a>Yapılandırma ve NGINX çalıştırma

Yerel makinenizde çalışan mikro hizmetler ve web uygulaması bağlamak için bir ters proxy sunucuyu ayarlayın:

* Kopyalama **nginx.conf** dosya **webui\scripts\localhost** deposunun yerel kopyasındaki klasör **nginx\conf** yükleme dizini.
* Çalıştırma **ngınx**.

Çalıştırma hakkında daha fazla bilgi için **ngınx**, bkz: [nginx Windows için](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan izleme çözümü panosuna erişmek için http gidin:\/tarayıcınızda /localhost:9000.

## <a name="clean-up"></a>Temizleme

Gereksiz önlemek için bulut Hizmetleri ücretleri sınamanızı tamamladığınızda, Azure aboneliğinizden kaldırın. Hizmetlerini kaldırmak için gidin [Azure portalında](https://ms.portal.azure.com) ve delete kaynak grubunda **start.cmd** oluşturulan komut dosyası.

Ayrıca, kaynak kodunu github'dan kopyaladığınız oluşturulan uzaktan izleme depo yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan izleme çözüm dağıttığınıza göre sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md).
