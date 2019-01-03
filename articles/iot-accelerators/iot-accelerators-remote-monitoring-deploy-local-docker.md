---
title: Uzaktan izleme çözümünü yerel olarak - Docker - dağıtma Azure | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için Docker'ı kullanarak yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: cf3c30d33e618ae3fd9d4ad942c77d211a414e82
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601160"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---docker"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Docker dağıtma

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Yerel Docker kapsayıcıları için mikro Hizmetleri dağıtmayı öğrenin. Yerel mikro hizmetlerin dağıtımı aşağıdaki bulut hizmetlerini kullanır: Bulutta IOT Hub, Cosmos DB, Azure akış analizi ve Azure Time Series Insights Hizmetleri.

Uzaktan izleme çözüm Hızlandırıcısını IDE içinde yerel makinenizde çalıştırmak istiyorsanız, bkz. [Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Visual Studio dağıtma](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine Kurulumu

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices-in-docker"></a>Mikro hizmetler Docker'da çalıştırma

Yeni bir komut istemi emin olmak için ayarlanan ortam değişkenlerine erişimi açın **start.cmd** betiği. Windows üzerinde aşağıdaki komutu çalıştırarak ortam değişkenlerini ayarlamak doğrulayabilirsiniz:

```cmd
set PCS
```

Komutu ayarlanmış olan tüm ortam değişkenlerini gösterir **start.cmd** betiği.

Docker yerel makinenizde çalıştığından emin olun.

Azure bulut hizmetlerine erişmek yerel Docker kapsayıcılarda çalıştırılan mikro hizmetler gerekir. Bir kapsayıcı içinde bir Internet adresi ping işlemi yapmak için aşağıdaki komutu kullanarak Docker ortamınızın internet bağlantısını test edebilirsiniz:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Çözüm Hızlandırıcısını çalıştırma için gidin **Hizmetleri\\betikleri\\yerel** klasöründe komut satırı ortamı ve şu komutu çalıştırın:

```cmd/sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker kapsayıcıları yerel olarak oluşturmak için Docker hub'ından mikro hizmet görüntüleri yükler. Aşağıdaki çalışır, Docker kapsayıcıları hemen çalıştırılır.

> [!TIP]
> Microsoft, yeni Docker görüntülerini yeni işlevlerle sık yayımlar. En son olanları çekme önce yerel Docker kapsayıcıları ve ilgili görüntüleri aşağıdaki temizleme komutları kümesini kullanabilirsiniz:

```cmd/sh
docker list
docker rm <list_of_containers>
docker rmi <list_of_images>
```

Kapsayıcı günlüklerini görüntülemek için ayrı bir kabuk kullanabilirsiniz. Kimliği kullanarak kapsayıcıdaki ilk bulma `docker ps` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 girişlerini görüntülemek için.

### <a name="start-the-stream-analytics-job"></a>Stream Analytics işini başlatın

Stream Analytics işi başlatmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Gidin **kaynak grubu** çözümünüz için oluşturulur. Kaynak grubunun adı çalıştırdığınızda çözümünüz için seçtiğiniz addır **start.cmd** betiği.
1. Tıklayarak **Stream Analytics işi** kaynakları listesinde.
1. Stream Analytics işinde **genel bakış** sayfasında **Başlat** düğmesi. Ardından **Başlat** işini şimdi başlatmak için.

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan izleme çözümü panosuna erişmek için gidin [ http://localhost:8080 ](http://localhost:8080) tarayıcınızda. Artık Web UI ve yerel mikro hizmetler kullanabilirsiniz.

## <a name="clean-up"></a>Temizleme

Gereksiz önlemek için bulut Hizmetleri ücretleri sınamanızı tamamladığınızda, Azure aboneliğinizden kaldırın. Hizmetlerini kaldırmak için gidin [Azure portalında](https://ms.portal.azure.com) ve delete kaynak grubunda **start.cmd** oluşturulan komut dosyası.

Kullanım `docker-compose down --rmi all` Docker görüntülerini kaldırmak ve yerel makinenizde alan boşaltmak için komutu. Ayrıca, kaynak kodunu github'dan kopyaladığınız oluşturulan uzaktan izleme depo yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan izleme çözüm dağıttığınıza göre sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md).
