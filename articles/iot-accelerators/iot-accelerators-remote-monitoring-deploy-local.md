---
title: Yerel olarak - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/26/2018
ms.topic: conceptual
ms.openlocfilehash: d967112fc1e3630148a419c980813159e9334eb3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243547"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak dağıtma

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Bu makalede açıklanan yaklaşımı, mikro hizmetler için yerel bir Docker kapsayıcısı dağıtır ve bulutta IOT Hub, Cosmos DB ve Azure Time Series Insights Hizmetleri kullanır. Uzaktan izleme çözüm Hızlandırıcısını IDE içinde yerel makinenizde çalıştırma hakkında bilgi edinmek için bkz: [başlangıç mikro hizmetler yerel ortamda](https://github.com/Azure/remote-monitoring-services-java/blob/master/docs/LOCAL_DEPLOYMENT.md) GitHub üzerinde.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Uzaktan izleme kaynak kodu GitHub deposu indirin, yapılandırma ve mikro hizmetleri içeren Docker görüntülerini çalıştırmak için ihtiyaç duyduğunuz Docker yapılandırma dosyalarını içerir. Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gidin ve ardından aşağıdaki komutlardan birini çalıştırın, komut satırı ortamı kullanın:

Java mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:

```cmd/sh
git clone https://github.com/Azure/remote-monitoring-services-java.git
```

.NET mikro hizmet uygulamaları en son sürümünü indirmek için çalıştırın:

```cmd\sh
git clone https://github.com/Azure/remote-monitoring-services-dotnet.git
```

> [!NOTE]
> Bu komutlar, mikro Hizmetleri yerel olarak çalıştırmak için kullandığınız komut dosyalarının yanı sıra tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro hizmetler Docker'da çalıştırma için kaynak kodu gerekmez ancak daha sonra çözüm Hızlandırıcısını değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Bu Azure hizmetlerini dağıtabileceğiniz [Azure Portalı aracılığıyla el ile](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), ya sağlanan komut dosyasını kullanın. Aşağıdaki betik örnekleri, bir Windows makinede .NET deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın. Sağlanan betikleri için kullanılacak:

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturma

Gerekli Azure kaynakları henüz oluşturduysanız, şu adımları izleyin:

1. Komut satırı ortamınızda gidin **Uzaktan izleme hizmetleri dotnet\scripts\local\launch** kopyaladığınız deponun klasöründe.

2. Çalıştırma **start.cmd** betiği ve yönergeleri izleyin. Betik, Azure hesabınızda oturum açın ve komut dosyasını yeniden ister. Betik daha sonra aşağıdaki bilgileri ister:
    * Bir çözüm adı.
    * Kullanılacak Azure aboneliği.
    * Kullanmak için Azure veri merkezi konumu.

    Betik, çözümünüzün adına ile Azure'da kaynak grubu oluşturur. Bu kaynak grubu, çözüm Hızlandırıcısını Azure kaynaklarını içerir.

3. Betik tamamlandığında ortam değişkenlerinin bir listesini görüntüler. Bu değişkenlere kaydetmek için komuttan Çıkış'ndaki yönergeleri izleyin **Uzaktan izleme hizmetleri dotnet\\betikleri\\yerel\\.env** dosya.

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynakları

Zaten oluşturduysanız, gerekli Azure kaynakları ortam değişkeni tanımlarında Düzenle **Uzaktan izleme hizmetleri dotnet\\betikleri\\yerel\\.env** ile dosya Gerekli değerler. **.Env** dosyası gerekli değerleri nerede bulunacağı hakkında ayrıntılı bilgi içerir.

## <a name="run-the-microservices-in-docker"></a>Mikro hizmetler Docker'da çalıştırma

Yerel Docker kapsayıcılarda çalıştırılan mikro hizmetler, Azure'da çalışan hizmetleri erişmeniz gerekebilir. Küçük bir kapsayıcı başlayıp internet adresine ping atmayı çalıştığında aşağıdaki komutu kullanarak Docker ortamınızın internet bağlantısını test edebilirsiniz:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Çözüm Hızlandırıcısını çalıştırma için gidin **Uzaktan izleme hizmetleri dotnet\\betikleri\\yerel** klasöründe komut satırı ortamı ve şu komutu çalıştırın:

```cmd\sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker kapsayıcıları yerel olarak oluşturmak için Docker hub'ından mikro hizmet görüntüleri yükler. Sonraki çalıştırmaları üzerinde Docker kapsayıcıları hemen çalıştırılır.

Kapsayıcı günlüklerini görüntülemek için ayrı bir kabuk kullanabilirsiniz. Kimliği kullanarak kapsayıcıdaki ilk bulma `docker ps -a` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 günlük girişlerini görüntülemek için.

Uzaktan izleme çözümü panosuna erişmek için gidin [ http://localhost:8080 ](http://localhost:8080) tarayıcınızda.

## <a name="clean-up"></a>Temizleme

Gereksiz önlemek için test bittiğinde ücretleri, bulut Hizmetleri Azure aboneliğinizden kaldırın. Gidilecek hizmetlerini kaldırmak için en kolay yolu olan [Azure portalında](https://ms.portal.azure.com) ve çalıştırdığınızda oluşturduğunuz kaynak grubunu silin **start.cmd** betiği.

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
