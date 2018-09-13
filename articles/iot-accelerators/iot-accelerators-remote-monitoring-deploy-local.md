---
title: Yerel olarak - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/06/2018
ms.topic: conceptual
ms.openlocfilehash: aaaf31d5c1faae8176dd9909f74c70300c3f0b4e
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716449"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak dağıtma

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Bu yaklaşım, mikro hizmetler için yerel bir Docker kapsayıcısı dağıtır ve bulutta IOT Hub, Cosmos DB ve Azure Time Series Insights Hizmetleri kullanır. Çözüm Hızlandırıcıları (bilgisayarlar) CLI, Azure bulut Hizmetleri dağıtmak için kullanın.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -bu yazılımı bilgisayarları CLI önkoşuldur.

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

 Uzaktan izleme kaynak kodu GitHub deposu indirin, yapılandırma ve mikro hizmetleri içeren Docker görüntülerini çalıştırmak için ihtiyaç duyduğunuz Docker yapılandırma dosyalarını içerir. Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gidin ve aşağıdaki komutlardan birini çalıştırın, komut satırı ortamı kullanın:

Mikro hizmetler Java uygulamalarını yüklemek için çalıştırın:

```cmd/sh
git clone --recurse-submodules -j8 https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

.net mikro hizmet uygulamaları yüklemek için çalıştırın:

```cmd\sh
git clone --recurse-submodules -j8 https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

> [!NOTE]
> Bu komutlar, tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro Hizmetleri Docker çalıştırmak için kaynak kodu gerekli değildir ancak daha sonra önceden yapılandırılmış çözümü değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Bu Azure hizmetlerini dağıtabileceğiniz [Azure Portalı aracılığıyla el ile](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), ya sağlanan komut dosyasını kullanın. Aşağıdaki betik örnekleri, bir Windows makinede .NET deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın. Sağlanan komut dosyasını kullanmak için:

1. Komut satırı ortamınızda gidin **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch** kopyaladığınız deponun klasöründe.

1. Çalıştırma **start.cmd** betiği ve yönergeleri izleyin. Betik için aşağıdaki bilgileri ister:
    * Bir çözüm adı.
    * Kullanılacak Azure aboneliği.
    * Kullanmak için Azure veri merkezi konumu.

    Betik, çözümünüzün adına ile Azure'da kaynak grubu oluşturur.

1. Komut satırı ortamınızda gidin **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch\os\win** kopyaladığınız deponun klasöründe.

1. Çalıştırma **kümesi env uri.cmd** betiği.

## <a name="run-the-microservices-in-docker"></a>Mikro hizmetler Docker'da çalıştırma

Çözüm Hızlandırıcısını çalıştırma için gidin **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local** klasöründe komut satırı ortamı ve şu komutu çalıştırın:

```cmd\sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker kapsayıcıları yerel olarak oluşturmak için Docker hub'ından mikro hizmet görüntüleri yükler. Sonraki çalıştırmaları üzerinde Docker kapsayıcıları hemen çalıştırılır.

Kapsayıcı günlüklerini görüntülemek için ayrı bir kabuk kullanabilirsiniz. Kimliği kullanarak kapsayıcıdaki ilk bulma `docker ps -a` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 günlük girişlerini görüntülemek için.

Uzaktan izleme çözümü panosuna erişmek için gidin [ http://localhost:8080 ](http://localhost:8080) tarayıcınızda.

## <a name="clean-up"></a>Temizleme

Test işlemini bitirdikten sonra gereksiz ücretlerden kaçınmak için bulut hizmetlerini Azure aboneliğinizden kaldırın. Gidilecek hizmetlerini kaldırmak için en kolay yolu olan [Azure portalında](https://ms.portal.azure.com) ve çalıştırdığınızda oluşturduğunuz kaynak grubunu silin **start.cmd** betiği.

Kullanım `docker-compose down --rmi all` Docker görüntülerini kaldırmak ve yerel makinenizde alan boşaltmak için komutu. Ayrıca, kaynak kodunu github'dan kopyaladığınız oluşturulan uzaktan izleme depo yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir yerel geliştirme ortamını ayarlama
> * Çözüm hızlandırıcısını yapılandırma
> * Çözüm Hızlandırıcısını dağıtma
> * Çözüm Hızlandırıcısını için oturum açın

Uzaktan izleme çözümü dağıttıktan sonra sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->