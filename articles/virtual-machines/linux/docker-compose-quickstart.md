---
title: Docker Compose'u kullanma azure'da bir Linux VM'de | Microsoft Docs
description: Azure CLI ile Linux sanal makinelerinde, Docker ve Compose kullanma
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: zarhoads
ms.openlocfilehash: 5cf9047a2115e2d486a433542928afbe295b5962
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54844628"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Tanımlamak ve Azure'da çok kapsayıcılı bir uygulama çalıştırmak için Docker ve Compose kullanmaya başlama
İle [Compose](http://github.com/docker/compose), birden fazla Docker kapsayıcılarını oluşan bir uygulamanın tanımlamak için basit bir metin dosyası kullanın. Ardından, uygulamanızda tanımlanmış ortamınıza dağıtmak için her şeyi yapan tek bir komut, hızla çalıştırın. Bu makalede örnek olarak, arka uç bir Ubuntu sanal MariaDB SQL veritabanı ile WordPress blogu hızlı bir şekilde ayarlama işlemini gösterir. Oluştur, daha karmaşık uygulamalar ayarlamak için de kullanabilirsiniz.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Bir Linux VM Docker konağı olarak ayarlama
Bir Linux VM oluşturma ve bir Docker konağı olarak ayarlamak için Azure Market'teki çeşitli Azure yordamları ve geçerli görüntüleri veya Resource Manager şablonlarını kullanabilirsiniz. Örneğin, [ortamınıza dağıtmak için Docker VM uzantısını kullanarak](dockerextension.md) kullanarak Azure Docker VM uzantısı ile hızlı bir şekilde bir Ubuntu sanal makinesi oluşturmak için bir [Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Docker VM uzantısını kullandığınızda, VM otomatik olarak bir Docker konağı olarak ayarlanır ve Compose zaten yüklü.


### <a name="create-docker-host-with-azure-cli"></a>Azure CLI ile Docker konağı oluşturma
Son yükleme [Azure CLI](/cli/azure/install-az-cli2) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index).

İlk olarak, Docker ortamınız için bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, ile VM dağıtma [az grubu dağıtım oluşturma](/cli/azure/group/deployment#az_group_deployment_create) Azure Docker VM uzantısını içeren [github'da bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). İstendiğinde, kendi benzersiz değerleri sağlayın *newStorageAccountName*, *adminUsername*, *adminPassword*, ve *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürer.


## <a name="verify-that-compose-is-installed"></a>Compose yüklü olduğunu doğrulayın
DNS adı, sanal makinenizin, ayrıntılarını görüntülemek için kullanın [az vm show](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --show-details \
    --query [fqdns] \
    --output tsv
```

SSH kullanarak yeni bir Docker konağı için. Kendi kullanıcı adı ve DNS adı önceki adımlarda sağlar:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Oluşturma VM'de yüklü olduğunu denetlemek için aşağıdaki komutu çalıştırın:

```bash
docker-compose --version
```

Benzer bir çıktı görmeniz *1.6.2 docker-compose, 4 d 72027 yapı*.

> [!TIP]
> Bir Docker konağı oluşturma ve yüklemek başka bir yöntem kullandıysanız kendiniz oluşturun, bkz: [belgeleri Compose](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Docker-compose.yml yapılandırma dosyası oluşturma
Ardından, oluşturduğunuz bir `docker-compose.yml` VM'de çalıştırmak için Docker kapsayıcılarını tanımlamak için yalnızca bir metin yapılandırma dosyası, dosya,. Her kapsayıcı üzerinde çalıştırmak için görüntü dosyasını belirtir (veya bir derlemeden bir Dockerfile olabilir), gerekli ortam değişkenlerini ve bağımlılıkları, bağlantı noktaları ve kapsayıcılar arasındaki bağlantıları. Yml dosyası sözdizimi hakkında daha fazla bilgi için bkz. [dosya başvurusu Compose](https://docs.docker.com/compose/compose-file/).

Oluşturma bir *docker-compose.yml* dosya. Bazı veri dosyasına eklemek için sık kullandığınız metin düzenleyiciyi kullanın. Aşağıdaki örnek, dosya için bir istemle oluşturur `sensible-editor` kullanmak istediğiniz bir düzenleyici seçmek için:

```bash
sensible-editor docker-compose.yml
```

Aşağıdaki örnek, Docker Compose dosyanıza yapıştırın. Bu yapılandırma görüntüleri kullanan [DockerHub kayıt defteri](https://registry.hub.docker.com/_/wordpress/) WordPress (açık kaynak Web günlüğü ve içerik yönetim sistemi) ve bağlı bir arka uç MariaDB SQL veritabanı'nı yüklemek için. Kendi girin *MYSQL_ROOT_PASSWORD* gibi:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>Kapsayıcıları oluşturma ile Başlat
Aynı dizinde, *docker-compose.yml* dosya, aşağıdaki komutu çalıştırın (ortamınıza bağlı olarak çalıştırmanız gerekebilir `docker-compose` kullanarak `sudo`):

```bash
docker-compose up -d
```

Bu komut belirtilen Docker kapsayıcıları başlatır *docker-compose.yml*. Bir veya bu adımı tamamlamak için iki dakika sürer. Aşağıdaki örneğe benzer bir çıktı görürsünüz:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Kullandığınızdan emin olun **-d** seçeneğine üzerinde başlatma kapsayıcıları arka planda sürekli olarak çalıştırın.


Kapsayıcıları çalışır durumda olduğunu doğrulamak için `docker-compose ps`. Benzer bir şey görmeniz gerekir:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Şimdi WordPress bağlantı noktası 80 üzerinde bir VM üzerinde doğrudan bağlanabilirsiniz. Bir web tarayıcısı açın ve sanal makinenizin DNS adını girin (örneğin `http://mypublicdns.eastus.cloudapp.azure.com`). WordPress görmelisiniz başlangıç ekranında, yüklemenin tamamlanması ve uygulama kullanmaya başlayın.

![WordPress başlangıç ekranı][wordpress_start]

## <a name="next-steps"></a>Sonraki adımlar
* Git [Docker VM uzantısı Kullanıcı Kılavuzu](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Docker ve Compose, Docker sanal yapılandırmak daha fazla seçenek için. Örneğin, bir seçenek (JSON'a dönüştürülür) oluşturma yml dosyayı doğrudan Docker VM uzantısını yapılandırmasında eklemektir.
* Kullanıma [komut satırı başvurusu Compose](http://docs.docker.com/compose/reference/) ve [Kullanıcı Kılavuzu](http://docs.docker.com/compose/) oluşturmaya ve çok kapsayıcılı uygulamalar dağıtmaya ilişkin daha fazla örnek için.
* Bir Azure Resource Manager şablonu kullanın ya da, kendi ya da bir katkıda bulunan [topluluk](https://azure.microsoft.com/documentation/templates/), Docker ve Compose ile ayarlanmış bir uygulama ile bir Azure VM dağıtmak için. Örneğin, [WordPress blogu ile Docker dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) hızla bir MySQL arka ucuna bir Ubuntu sanal ile WordPress dağıtmak için şablonu kullanan Docker ve Compose.
* Docker Compose, Docker Swarm kümesi ile tümleştirme deneyin. Bkz: [kullanarak Compose Swarm ile](https://docs.docker.com/compose/swarm/) senaryolar için.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
