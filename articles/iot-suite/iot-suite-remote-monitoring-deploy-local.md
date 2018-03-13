---
title: "Azure yerel olarak - uzaktan izleme çözümü dağıtma | Microsoft Docs"
description: "Bu öğretici, yerel makinenize test ve geliştirme için önceden yapılandırılmış Uzaktan izleme çözümü dağıtma gösterir."
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 03/07/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 77f40e2f10cbdb9930a22a4248e19bb3356af7af
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-locally"></a>Uzaktan izleme önceden yapılandırılmış çözümü yerel olarak dağıtma

Bu makalede, yerel makinenize test ve geliştirme için önceden yapılandırılmış Uzaktan izleme çözümü dağıtma gösterilmektedir. Bu yaklaşım, yerel bir Docker kapsayıcısı mikro dağıtır ve IOT Hub, Cosmos DB ve Azure storage Hizmetleri bulutta kullanır. Önceden yapılandırılmış çözümler (bilgisayarlar) Azure bulut Hizmetleri dağıtmak için CLI kullanın.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme önceden yapılandırılmış çözüm tarafından kullanılan Azure Hizmetleri dağıtmak için etkin bir Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtım tamamlamak için yerel geliştirme makinenizde aşağıdaki araçları gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose'u](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -bu yazılımın Bilgisayarlarını CLI için bir önkoşuldur.
* PCS CLI

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="install-the-pcs-cli"></a>Bilgisayarları CLI'yı yükleme

CLI yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

CLI hakkında daha fazla bilgi için bkz: [CLI kullanma](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="deploy-the-azure-services"></a>Azure Hizmetleri dağıtma

Bu makalede mikro yerel olarak çalıştırmak nasıl gösterir, ancak bulutta çalışan üç Azure Hizmetleri bağlıdır. Bu Azure Hizmetleri Azure Portalı aracılığıyla el ile dağıtmak veya bilgisayarları CLI kullanın. Bu makalede nasıl kullanılacağı gösterilmektedir `pcs` aracı.

### <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Önceden yapılandırılmış çözümü dağıtmadan önce CLI kullanarak Azure aboneliğinizde oturum gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri.

### <a name="run-a-local-deployment"></a>Yerel bir dağıtımı çalıştırın

Yerel dağıtım başlatmak için aşağıdaki komutu kullanın:

```cmd/pcs
pcs -s local
```

Komut dosyası için aşağıdaki bilgileri ister:

* Çözüm adı.
* Kullanılacak Azure aboneliği.
* Kullanmak için Azure veri merkezi konum.

Komut dosyası IOT hub'ı, Azure aboneliğinizde bir kaynak grubunda örneği, Cosmos DB örneği ve Azure storage hesabı oluşturur. Kaynak grubunun adını, ne zaman çalıştırıldığı seçtiğiniz çözüm adıdır `pcs` aracı.

Komut dosyasını çalıştırmak için birkaç dakika sürer. Tamamlandığında, bir ileti görür `Copy the following environment variables to /scripts/local/.env file:`. İleti aşağıdaki ortam değişken tanımları kopyalayın, bir sonraki adımda kullanın.

## <a name="download-the-source-code"></a>Kaynak kodu indirme

Uzak izleme kaynak kod deponuza karşıdan yüklemek, yapılandırmak ve mikro içeren Docker görüntüler çalıştırmak için gereken Docker yapılandırma dosyaları içerir. Depoyu kopyalama yerel makinenizde uygun bir klasöre gidin ve aşağıdaki komutlardan birini çalıştırın:

Mikro, Java uygulamalarını yüklemek için çalıştırın:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Mikro, .net uygulamalarını yüklemek için çalıştırın:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Bu komutlar tüm mikro için kaynak kodunu indirebilir. Mikro Docker içinde çalıştırmak için kaynak kodu gerekli değildir ancak kaynak kodu daha sonra önceden yapılandırılmış çözümü değiştirin ve değişikliklerinizi yerel olarak test etmek planlıyorsanız yararlıdır.

## <a name="run-the-microservices-in-docker"></a>Docker içinde mikro çalıştırın

Mikro Docker içinde çalıştırmak için önce Düzenle **betikleri\\yerel\\.env** depoyu yerel kopyasını dosyasında. Dosyanın tüm içeriğini ne zaman çalıştırıldığı bir not yaptığınız ortam değişkeni tanımlarla değiştirin `pcs` önceden komutu. Bu ortam değişkenleri tarafından oluşturulan Azure hizmetlerine bağlanmak için Docker kapsayıcısı içinde mikro etkinleştirmek `pcs` aracı.

Önceden yapılandırılmış çözümü çalıştırmak için gidin **scripts\local** klasöründe komut satırı ortamı ve aşağıdaki komutu çalıştırın:

```cmd\sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker mikro hizmet görüntüleri kapsayıcıları yerel olarak oluşturmak için Docker hub'dan indirir. Sonraki üzerinde Docker kapsayıcıları hemen çalıştırılır.

Ayrı bir kabuk kapsayıcısından günlükleri görüntülemek için kullanabilirsiniz. Kapsayıcı Kimliğini kullanarak ilk Bul `docker ps -a` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 günlük girişlerini görüntülemek için.

Uzaktan izleme çözüm panosunu erişmek için gidin [http://localhost: 8080](http://localhost:8080) tarayıcınızda.

## <a name="tidy-up"></a>Tutuyor

Sınamanızı tamamladığınızda gereksiz ücretlerden kaçınmak için Azure aboneliğinizden bulut hizmetlerini kaldırın. Tarafından oluşturulan kaynak grubunu silmek için Azure Portalı'nı kullanmaktır hizmetlerini kaldırmak için en kolay yolu, `pcs` aracı.

Kullanım `docker-compose down --rmi all` Docker görüntüleri kaldırmak ve yerel makinenizde alan boşaltmak için komutu. Kaynak kodu github'dan klonlanmış oluşturulan uzaktan izleme deposu yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Önceden yapılandırılmış çözümünüzü yapılandırma
> * Önceden yapılandırılmış çözümü dağıtma
> * Önceden yapılandırılmış çözümü oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->