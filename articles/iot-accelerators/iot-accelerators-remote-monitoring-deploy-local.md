---
title: Yerel olarak (aracılığıyla Visual Studio IDE) - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için Visual Studio kullanarak yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 1adf59feca7db4c5903b04c59e1bd23290c1855e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967514"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Visual Studio dağıtma

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Mikro hizmetler, Visual Studio'da çalıştırmayı öğrenin. Yerel mikro hizmetlerin dağıtımı aşağıdaki bulut hizmetlerini kullanır: Bulutta IOT Hub, Cosmos DB, Azure akış analizi ve Azure Time Series Insights Hizmetleri.

Uzaktan izleme çözüm Hızlandırıcısını Docker'da yerel makinenizde çalıştırmak istiyorsanız, bkz. [Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Docker dağıtma](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine Kurulumu

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Windows ve Mac için Visual Studio kullanılabilir

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Mikro Hizmetleri çalıştırın

Bu bölümde, Uzaktan izleme mikro Hizmetleri çalıştırın. Web kullanıcı Arabirimi yerel olarak çalıştırdığınızda Docker cihaz benzetimi hizmetinde ve Visual Studio'da mikro hizmetler.

### <a name="run-the-device-simulation-service"></a>Cihaz benzetimi hizmet çalıştırma

Ayarlanan ortam değişkenlerine erişebilir emin olmak için yeni bir komut istemi penceresi açın **start.cmd** önceki bölümde betiği.

Cihaz benzetimi hizmeti için Docker kapsayıcısı başlatmak için aşağıdaki komutu çalıştırın. Hizmeti cihazları için Uzaktan izleme çözümü benzetimini yapar.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Yerel makinede diğer mikro hizmetlerin dağıtımı

Aşağıdaki adımlar Visual Studio'da Uzaktan izleme mikro hizmetleri çalıştırma işlemini gösterir:

1. Visual Studio'yu başlatın.
1. Açık **uzaktan monitoring.sln** çözümde **Hizmetleri** deposunun yerel kopyasında bir klasör.
1. İçinde **Çözüm Gezgini**, çözüm ve ardından sağ **özellikleri**.
1. Seçin **Ortak Özellikler > başlangıç projesi**.
1. Seçin **birden fazla başlangıç projesi** ayarlayıp **eylem** için **Başlat** aşağıdaki projeler için:
    * Web hizmeti (manager\WebService asa)
    * Web hizmeti (auth\WebService)
    * Web hizmeti (config\WebService)
    * Web hizmeti (telemetry\WebService cihaz)
    * Web hizmeti (iothub-manager\WebService)
    * Web hizmeti (adapter\WebService depolama)
1. Tıklayın **Tamam** seçimlerinizi kaydetmek için.
1. Tıklayın **hata ayıklama > hata ayıklamayı Başlat** oluşturun ve web hizmetleri yerel makinede çalıştırın.

Her web hizmeti, bir komut istemi'ni ve web tarayıcı penceresi açılır. Komut isteminde çalışan hizmetin çıktısını görürsünüz ve tarayıcının durumunu izlemenize olanak tanır. Komut istemleri veya web sayfaları kapatmayın, bu eylem web hizmetini durdurur.

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
