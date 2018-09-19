---
title: Yerel olarak - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/17/2018
ms.topic: conceptual
ms.openlocfilehash: 966af342937a36adc5932a7a4c92ee127723b4a0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295742"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak dağıtma

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Bu yaklaşım, mikro hizmetler için yerel bir Docker kapsayıcısı dağıtır ve bulutta IOT Hub, Cosmos DB ve Azure Time Series Insights Hizmetleri kullanır.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Uzaktan izleme kaynak kodu GitHub deposu indirin, yapılandırma ve mikro hizmetleri içeren Docker görüntülerini çalıştırmak için ihtiyaç duyduğunuz Docker yapılandırma dosyalarını içerir. Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde uygun bir klasöre gidin ve ardından aşağıdaki komutlardan birini çalıştırın, komut satırı ortamı kullanın:

Mikro hizmetler Java uygulamalarını yüklemek için çalıştırın:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git
```

.net mikro hizmet uygulamaları yüklemek için çalıştırın:

```cmd\sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
```

> [!NOTE]
> Bu komutlar, tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro hizmetler Docker'da çalıştırma için kaynak kodu gerekmez ancak daha sonra çözüm Hızlandırıcısını değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar bulutta çalışan Azure Hizmetleri bağlıdır. Bu Azure hizmetlerini dağıtabileceğiniz [Azure Portalı aracılığıyla el ile](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), ya sağlanan komut dosyasını kullanın. Aşağıdaki betik örnekleri, bir Windows makinede .NET deposu kullanmakta olduğunuz varsayılır. Başka bir ortamda çalışıyorsanız, yol, dosya uzantılarını ve yol ayırıcıları uygun şekilde ayarlayın. Sağlanan betikleri kullanmak için:

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturma

Gerekli Azure kaynakları henüz oluşturmadıysanız, şu adımları izleyin:

1. Komut satırı ortamınızda gidin **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch** kopyaladığınız deponun klasöründe.

2. Çalıştırma **start.cmd** betiği ve yönergeleri izleyin. Betik için aşağıdaki bilgileri ister:
    * Bir çözüm adı.
    * Kullanılacak Azure aboneliği.
    * Kullanmak için Azure veri merkezi konumu.

    Betik, çözümünüzün adına ile Azure'da kaynak grubu oluşturur. Bu kaynak grubu, çözüm Hızlandırıcısını Azure kaynaklarını içerir.

3. Komut satırı ortamınızda gidin **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch\os\win** kopyaladığınız deponun klasöründe.

4. Çalıştırma **kümesi env uri.cmd** betiği.

5. En son sürümlerine sahip olduğunuzdan emin olmak için git alt güncelleştirin: `cd <repo-name>` ve ardından aşağıdaki komutu çalıştırın `git submodule foreach git pull origin master`

> [!NOTE]
> Azure-iot-pcs-remote-monitoring-dotnet depo kopyaladıysanız, betikler klasörüne Hizmetleri (klasör) alt modül altında mevcuttur. Yüklemeye çalıştığında bu komut dosyası sudo izin veya yönetici ayrıcalıkları gerektirebilir [bilgisayarları-cli](https://github.com/Azure/pcs-cli).

### <a name="use-existing-azure-resources"></a>Mevcut Azure kaynakları

Gerekli Azure kaynakları oluşturmuş ve bunları güncelleştirmek yalnızca ihtiyacınız varsa, yalnızca tamamlamak **bir** biri:

* Ortam değişkenlerini makinenizde genel olarak ayarlayın.
* **VS Code:** düzenleyerek başlatma yapılandırmada ortam değişkenlerini ayarlamak **launch.json** dosya.
* **Visual Studio:** ekleyerek Web hizmeti projesi mikro hizmetler için ortam değişkenlerini ayarlamak **özellikleri > hata ayıklama > ortam değişkenlerini**.

Son olarak, en son sürümlerine sahip olduğunuzdan emin olmak için git alt güncelleştirin: `cd <repo-name>` ve ardından aşağıdaki komutu çalıştırın `git submodule foreach git pull origin master`.

Önerilmemesine rağmen ortam değişkenleri WebService klasörü altında her bir mikro hizmetler için mevcut appsettings.ini dosyasında da ayarlanabilir.

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
