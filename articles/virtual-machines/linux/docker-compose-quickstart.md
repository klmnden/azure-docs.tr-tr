---
title: "Kullanım Docker Compose azure'da bir Linux VM üzerinde | Microsoft Docs"
description: "Azure CLI ile Linux sanal makinelerde Docker ve Oluştur kullanma"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 474a2d66cc46fcac35b145633e802d72881b10d8
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Docker ve oluşturma tanımlamak ve Azure'da çok kapsayıcı uygulamayı çalıştırmak için kullanmaya başlama
İle [oluşturma](http://github.com/docker/compose), birden çok Docker kapsayıcıları için oluşan bir uygulamayı tanımlamak için basit bir metin dosyası kullanın. Ardından, uygulamanızda tanımlanan ortamınıza dağıtmak için her şeyi yapar tek bir komut Yukarı Döndür. Örnek olarak, bu makalede, bir WordPress blog arka ucu MariaDB SQL veritabanındaki bir Ubuntu VM ile hızlı bir şekilde ayarlama gösterilmektedir. Oluştur, daha karmaşık uygulamalar ayarlamak için de kullanabilirsiniz.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Bir Linux VM Docker ana bilgisayar olarak ayarlama
Bir Linux VM oluşturun ve Docker ana bilgisayar olarak ayarlamak için Azure Marketi'nde çeşitli Azure yordamları ve kullanılabilir görüntüler veya Resource Manager şablonları kullanabilirsiniz. Örneğin, [ortamınıza dağıtmak için Docker VM uzantısı kullanılarak](dockerextension.md) kullanarak bir Ubuntu VM Azure Docker VM uzantısı ile hızlı bir şekilde oluşturmak için bir [hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Docker VM uzantısı kullandığınızda, VM otomatik olarak Docker ana bilgisayar olarak ayarlanır ve oluşturma zaten yüklü.


### <a name="create-docker-host-with-azure-cli-20"></a>Azure CLI 2.0 ile Docker konak oluştur
En son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/#login).

İlk olarak, Docker ortamınız için bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, bir VM'yi dağıtmak [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#create) Azure Docker VM uzantısını içeren [github'daki bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). İstendiğinde, kendi benzersiz değerler sağlayın *newStorageAccountName*, *adminUsername*, *Admınpassword*, ve *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Tamamlamak için dağıtım için birkaç dakika sürer.


## <a name="verify-that-compose-is-installed"></a>Oluşturma yüklü olduğunu doğrulayın
DNS adı dahil olmak üzere, VM ayrıntılarını görüntülemek için kullanın [az vm Göster](/cli/azure/vm#show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --show-details \
    --query [fqdns] \
    --output tsv
```

SSH, yeni Docker konağına. Kullanıcı adınızı ve yukarıdaki adımları DNS adını girin:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

VM Oluştur yüklü olduğunu denetlemek için aşağıdaki komutu çalıştırın:

```bash
docker-compose --version
```

Benzer bir çıktı görmeniz *1.6.2 docker compose'u, 4 d 72027 yapı*.

> [!TIP]
> Docker ana oluşturmak ve yüklemenize gerek için başka bir yöntem kullandıysanız kendiniz oluşturmak için bkz: [belgeleri oluşturma](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Docker-compose.yml yapılandırma dosyası oluşturma
Ardından, oluşturduğunuz bir `docker-compose.yml` VM çalıştırmak için Docker kapsayıcıları tanımlamak için yalnızca bir metin yapılandırma dosyası, dosya. Görüntünün her kapsayıcı üzerinde çalıştırmak için dosyayı belirtir (veya bir Dockerfile derleme olabilir), gerekli ortam değişkenleri ve bağımlılıkları, bağlantı noktalarını ve kapsayıcılar arasındaki bağlantıları. Yml dosya sözdizimi hakkında daha fazla bilgi için bkz: [dosya başvurusu oluşturan](https://docs.docker.com/compose/compose-file/).

Oluşturma bir *docker-compose.yml* dosya. Bazı verileri eklemek için sık kullandığınız metin düzenleyiciyi kullanın. Aşağıdaki örnek dosya için bir istem oluşturur `sensible-editor` kullanmak istediğiniz bir düzenleyiciyi seçmek için:

```bash
sensible-editor docker-compose.yml
```

Aşağıdaki örnek, Docker Compose dosyasına yapıştırın. Bu yapılandırma görüntülerden kullanıyor [DockerHub kayıt defteri](https://registry.hub.docker.com/_/wordpress/) WordPress (açık kaynak blog ve içerik yönetim sistemi) ve bağlantılı arka uç MariaDB SQL veritabanını yüklemek için. Kendi girin *MYSQL_ROOT_PASSWORD* gibi:

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

Bu komut belirtilen Docker kapsayıcıları başlatır *docker-compose.yml*. Bir veya iki bu adımı tamamlamak için dakika sürer. Aşağıdaki örneğe benzer bir çıktı görürsünüz:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Kullandığınızdan emin olun **-d** kapsayıcıları arka planda sürekli çalışmasını böylece üzerinde başlatma seçeneği.


Kapsayıcılar yukarı olduğunu doğrulamak için aşağıdakileri yazın `docker-compose ps`. Benzer bir şey görmeniz gerekir:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Şimdi doğrudan bağlantı noktası 80 üzerinde VM üzerinde WordPress bağlanabilirsiniz. Bir web tarayıcısı açın ve VM DNS adını girin (gibi `http://mypublicdns.eastus.cloudapp.azure.com`). WordPress görmelisiniz başlangıç ekranında, burada yüklemeyi tamamlamak ve uygulama ile çalışmaya başlama.

![WordPress başlangıç ekranı][wordpress_start]

## <a name="next-steps"></a>Sonraki adımlar
* Git [Docker VM uzantısı Kullanıcı Kılavuzu'na](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Docker ve oluşturma, Docker VM yapılandırmak daha fazla seçenek için. Örneğin, bir (dönüştürülen JSON olarak) oluşturma yml dosyayı yerleştirmek için doğrudan Docker VM uzantısı yapılandırmasında seçenektir.
* Kullanıma [komut satırı başvurusu oluşturan](http://docs.docker.com/compose/reference/) ve [Kullanıcı Kılavuzu](http://docs.docker.com/compose/) oluşturma ve birden çok kapsayıcı uygulamaları dağıtmaya diğer örnekler için.
* Bir Azure Resource Manager şablonu ya da kullanmak, kendi ya da bir katkısı [topluluk](https://azure.microsoft.com/documentation/templates/), Docker ve oluşturma ile ayarlanmış bir uygulama ile bir Azure VM dağıtmak için. Örneğin, [bir WordPress blog docker'la dağıtmak](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) hızlı bir şekilde bir Ubuntu VM bir MySQL arka uç ile WordPress dağıtmak için şablon kullanır Docker ve oluştur.
* Docker Compose Docker Swarm kümesi ile tümleştirme deneyin. Bkz: [kullanarak oluşturan Swarm ile](https://docs.docker.com/compose/swarm/) senaryoları için.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
