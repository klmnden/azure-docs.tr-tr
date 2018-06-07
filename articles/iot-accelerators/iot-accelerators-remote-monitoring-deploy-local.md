---
title: Azure yerel olarak - uzaktan izleme çözümü dağıtma | Microsoft Docs
description: Bu öğretici, test ve geliştirme için yerel Makinenizi Uzaktan izleme Çözüm Hızlandırıcısı dağıtma gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/07/2018
ms.topic: conceptual
ms.openlocfilehash: 3f723d716a652e64527310a499d6b06a6cf6bc6f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627240"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Uzaktan izleme Çözüm Hızlandırıcısı yerel olarak dağıtma

Bu makalede Uzaktan izleme Çözüm Hızlandırıcısı test ve geliştirme için yerel makinenizi dağıtma gösterilmektedir. Bu yaklaşım, yerel bir Docker kapsayıcısı mikro dağıtır ve IOT Hub, Cosmos DB ve Azure storage Hizmetleri bulutta kullanır. Çözüm Hızlandırıcıları (bilgisayarlar) Azure bulut Hizmetleri dağıtmak için CLI kullanın.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme Çözüm Hızlandırıcısı tarafından kullanılan Azure Hizmetleri dağıtmak için etkin bir Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

Yerel dağıtım tamamlamak için yerel geliştirme makinenizde aşağıdaki araçları gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose'u](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -bu yazılımın Bilgisayarlarını CLI için bir önkoşuldur.
* BİLGİSAYARLARI CLI
* Kaynak kodun yerel deposu

> [!NOTE]
> Bu araçlar, Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

### <a name="install-the-pcs-cli"></a>Bilgisayarları CLI'yı yükleme

Npm aracılığıyla bilgisayarları CLI yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

CLI hakkında daha fazla bilgi için bkz: [CLI kullanma](https://github.com/Azure/pcs-cli/blob/master/README.md).

### <a name="download-the-source-code"></a>Kaynak kodu indirme

 Kaynak kodu Uzaktan izleme deposu karşıdan yüklemek, yapılandırmak ve mikro içeren Docker görüntüler çalıştırmak için gereken Docker yapılandırma dosyaları içerir. Kopyalama ve depoyu yerel bir sürümünü oluşturmak için sık kullanılan komut satırı veya terminal aracılığıyla yerel makinenizde uygun bir klasöre gidin ve aşağıdaki komutlardan birini çalıştırın:

Mikro, Java uygulamalarını yüklemek için çalıştırın:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Mikro, .net uygulamalarını yüklemek için çalıştırın:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Uzak Monioring önceden yapılandırılmış çözüm deposuna & submodules [ [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) | [.Net](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) ]

> [!NOTE]
> Bu komutlar tüm mikro için kaynak kodunu indirebilir. Mikro Docker içinde çalıştırmak için kaynak kodu gerekli değildir ancak kaynak kodu daha sonra önceden yapılandırılmış çözümü değiştirin ve değişikliklerinizi yerel olarak test etmek planlıyorsanız yararlıdır.

## <a name="deploy-the-azure-services"></a>Azure Hizmetleri dağıtma

Bu makalede mikro yerel olarak çalıştırmak nasıl gösterir, ancak bulutta çalışan üç Azure Hizmetleri bağlıdır. Bu Azure Hizmetleri dağıtabilirsiniz [Azure Portalı aracılığıyla el ile](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), veya bilgisayarları CLI kullanın. Bu makalede nasıl kullanılacağı gösterilmektedir `pcs` aracı.

### <a name="sign-in-to-the-cli"></a>CLI için oturum açın

Çözüm Hızlandırıcısı dağıtmadan önce CLI kullanarak Azure aboneliğinizde oturum gerekir:

```cmd/sh
pcs login
```

İzleyin ekran oturum açma işlemini tamamlamak için yönergeleri. CLI veya oturum açma başarısız olabilir iç herhangi bir yere tıklayın yok emin olun. Oturum açma tamamladıysanız, CLI başarılı oturum açma iletisinde görürsünüz. 

### <a name="run-a-local-deployment"></a>Yerel bir dağıtımı çalıştırın

Yerel dağıtım başlatmak için aşağıdaki komutu kullanın. Bu gerekli azure kaynakları oluşturun ve environemnt değişkenlerini konsola çıkışı yazdırın. 

```cmd/pcs
pcs -s local
```

Komut dosyası için aşağıdaki bilgileri ister:

* Çözüm adı.
* Kullanılacak Azure aboneliği.
* Kullanmak için Azure veri merkezi konum.

> [!NOTE]
> Komut dosyası IOT hub'ı, Azure aboneliğinizde bir kaynak grubunda örneği, Cosmos DB örneği ve Azure storage hesabı oluşturur. Kaynak grubunun adını, ne zaman çalıştırıldığı seçtiğiniz çözüm adıdır `pcs` yukarıdaki aracı. 

> [!IMPORTANT]
> Komut dosyasını çalıştırmak için birkaç dakika sürer. Tamamlandığında, bir ileti görür `Copy the following environment variables to /scripts/local/.env file:`. Kopya ortamı iletiden değişken tanımları, bunları bir sonraki adımda kullanacağınız.

## <a name="run-the-microservices-in-docker"></a>Docker içinde mikro çalıştırın

Mikro Docker içinde çalıştırmak için önce Düzenle **betikleri\\yerel\\.env** yukarıdaki önceki adımların birinde kopyaladığınız deponun yerel kopyasını dosyasında. Dosyanın tüm içeriğini ne zaman çalıştırıldığı bir not yaptığınız ortam değişkeni tanımlarla değiştirin `pcs` son adımda komutu. Bu ortam değişkenleri tarafından oluşturulan Azure hizmetlerine bağlanmak için Docker kapsayıcısı içinde mikro etkinleştirmek `pcs` aracı.

Çözüm Hızlandırıcısı çalıştırmak için gidin **scripts\local** klasöründe komut satırı ortamı ve aşağıdaki komutu çalıştırın:

```cmd\sh
docker-compose up
```

İlk defa bu komutu çalıştırdığınızda Docker mikro hizmet görüntüleri kapsayıcıları yerel olarak oluşturmak için Docker hub'dan indirir. Sonraki üzerinde Docker kapsayıcıları hemen çalıştırılır.

Ayrı bir kabuk kapsayıcısından günlükleri görüntülemek için kullanabilirsiniz. Kapsayıcı Kimliğini kullanarak ilk Bul `docker ps -a` komutu. Ardından `docker logs {container-id} --tail 1000` belirtilen kapsayıcı için son 1000 günlük girişlerini görüntülemek için.

Uzaktan izleme çözümü panoya erişmek için gidin [ http://localhost:8080 ](http://localhost:8080) tarayıcınızda.

## <a name="clean-up"></a>Temizleme

Sınamanızı tamamladığınızda gereksiz ücretlerden kaçınmak için Azure aboneliğinizden bulut hizmetlerini kaldırın. Gidilecek hizmetlerini kaldırmak için en kolay yolu olan [Azure portal](https://ms.portal.azure.com) ve aracılığıyla oluşturduğunuz kaynak grubunu silmek `pcs` aracı.

Kullanım `docker-compose down --rmi all` Docker görüntüleri kaldırmak ve yerel makinenizde alan boşaltmak için komutu. Kaynak kodu github'dan klonlanmış oluşturulan uzaktan izleme deposu yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir yerel geliştirme ortamını ayarlama
> * Çözüm Hızlandırıcısı yapılandırın
> * Çözüm Hızlandırıcısı dağıtma
> * Çözüm Hızlandırıcısı oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](iot-accelerators-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->