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
ms.openlocfilehash: 996111fbe23000182dab774ba3bbad0cc6435824
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412716"
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

[!INCLUDE [iot-accelerators-local-setup-java](../../includes/iot-accelerators-local-setup-java.md)]

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
