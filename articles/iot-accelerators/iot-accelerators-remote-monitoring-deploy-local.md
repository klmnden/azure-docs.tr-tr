---
title: Yerel olarak - uzaktan izleme çözümünü Azure'da dağıtma | Microsoft Docs
description: Bu öğreticide, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/07/2018
ms.topic: conceptual
ms.openlocfilehash: 21bc8c27a44c940279b0c5bdcdbe04e579dc4bfa
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188795"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak dağıtma

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Bu yaklaşım, mikro hizmetler için yerel bir Docker kapsayıcısı dağıtır ve IOT Hub, Cosmos DB ve Azure depolama hizmetleri, bulutta kullanır. Çözüm Hızlandırıcıları (bilgisayarlar) CLI, Azure bulut Hizmetleri dağıtmak için kullanın.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -bu yazılımı bilgisayarları CLI önkoşuldur.
* BİLGİSAYARLARI CLI
* Kaynak kodun yerel depo

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="install-the-pcs-cli"></a>Bilgisayarları CLI'yı yükleme

Npm aracılığıyla bilgisayarları CLI'yı yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

CLI hakkında daha fazla bilgi için bkz: [clı'yi nasıl](https://github.com/Azure/pcs-cli/blob/master/README.md).

### <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

 Uzaktan izleme kaynak kodu deposu indirin, yapılandırma ve mikro hizmetleri içeren Docker görüntülerini çalıştırmak için ihtiyaç duyduğunuz Docker yapılandırma dosyalarını içerir. Kopyalamak ve depoya yerel bir sürümünü oluşturmak için yerel makinenizde sık kullandığınız komut satırı veya terminal yoluyla uygun bir klasöre gidin ve aşağıdaki komutlardan birini çalıştırın:

Mikro hizmetler Java uygulamalarını yüklemek için çalıştırın:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

.net mikro hizmet uygulamaları yüklemek için çalıştırın:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Uzak Monioring önceden yapılandırılmış çözüm depo & alt modüller [ [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) | [.Net](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) ]

> [!NOTE]
> Bu komutlar, tüm mikro hizmetler için kaynak kodunu indirebilir. Kaynak kodu, mikro Hizmetleri Docker çalıştırmak için kaynak kodu gerekli değildir ancak daha sonra önceden yapılandırılmış çözümü değiştirin ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlı olur.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro Hizmetleri yerel olarak çalıştırmak nasıl gösterir, ancak bunlar üç Azure Hizmetleri bulutta çalışan bağlıdır. Bu Azure hizmetlerini dağıtabileceğiniz [Azure Portalı aracılığıyla el ile](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), veya bilgisayarları CLI'yı kullanın. Bu makalede nasıl kullanılacağını gösterir `pcs` aracı.

### <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Çözüm Hızlandırıcısını dağıtmadan önce şu şekilde CLI kullanarak Azure aboneliğinizde oturum açmanız gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri. CLI veya oturum açma başarısız olabilir iç içinde her yerden tıklamayın emin olun. Oturum açma tamamladıysanız, CLI bir oturum açma başarılı iletisi görürsünüz. 

### <a name="run-a-local-deployment"></a>Yerel bir dağıtımı çalıştırın

Yerel dağıtımı başlatmak için aşağıdaki komutu kullanın. Gerekli azure kaynakları oluşturmak ve ortam değişkenlerini konsola yazdırmak. 

```cmd/pcs
pcs -s local
```

Betik için aşağıdaki bilgileri ister:

* Bir çözüm adı.
* Kullanılacak Azure aboneliği.
* Kullanmak için Azure veri merkezi konumu.

> [!NOTE]
> Betik bir IOT Hub, Azure aboneliğinizde bir kaynak grubundaki örneği, Cosmos DB örneğine ve bir Azure depolama hesabı oluşturur. Kaynak grubu adını çalıştırdığınızda seçtiğiniz çözüm adıdır `pcs` yukarıdaki aracı. 

> [!IMPORTANT]
> Betiği çalıştırmak için birkaç dakika sürer. Tamamlandığında, bir ileti görürsünüz `Copy the following environment variables to /scripts/local/.env file:`. Kopya ortamı değişken tanımları aşağıdaki ileti, onları daha sonraki bir adımda kullanacağınız.

## <a name="run-the-microservices-in-docker"></a>Mikro hizmetler Docker'da çalıştırma

Mikro Hizmetleri Docker çalıştırmak için önce Düzenle **betikleri\\yerel\\.env** yukarıdaki önceki adımların birinde kopyaladığınız deponun yerel kopyası dosyasında. Ortam değişken tanımları çalıştırdığınızda Not yaptığınız dosyanın tüm içeriğini değiştirin `pcs` son adımda komutu. Mikro Hizmetleri Docker kapsayıcısı oluşturan Azure hizmetlerine bağlanmak için bu ortam değişkenlerini etkinleştirmek `pcs` aracı.

Çözüm Hızlandırıcısını çalıştırma için gidin **scripts\local** klasöründe komut satırı ortamı ve şu komutu çalıştırın:

```cmd\sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker kapsayıcıları yerel olarak oluşturmak için Docker hub'ından mikro hizmet görüntüleri yükler. Sonraki çalıştırmaları üzerinde Docker kapsayıcıları hemen çalıştırılır.

Kapsayıcı günlüklerini görüntülemek için ayrı bir kabuk kullanabilirsiniz. Kimliği kullanarak kapsayıcıdaki ilk bulma `docker ps -a` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 günlük girişlerini görüntülemek için.

Uzaktan izleme çözümü panosuna erişmek için gidin [ http://localhost:8080 ](http://localhost:8080) tarayıcınızda.

## <a name="clean-up"></a>Temizleme

Test işlemini bitirdikten sonra gereksiz ücretlerden kaçınmak için bulut hizmetlerini Azure aboneliğinizden kaldırın. Gidilecek hizmetlerini kaldırmak için en kolay yolu olan [Azure portalında](https://ms.portal.azure.com) ve aracılığıyla oluşturduğunuz kaynak grubunu silin `pcs` aracı.

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