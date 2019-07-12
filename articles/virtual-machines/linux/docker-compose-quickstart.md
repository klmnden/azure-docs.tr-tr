---
title: Docker Compose'u kullanma azure'da bir Linux VM'de | Microsoft Docs
description: Yükleme ve Azure CLI ile Linux sanal makinelerinde, Docker ve Compose kullanın
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2019
ms.author: cynthn
ms.openlocfilehash: 29b9b2b7868a39b8e14a559b27f60f9e46bad565
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667997"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Tanımlamak ve Azure'da çok kapsayıcılı bir uygulama çalıştırmak için Docker ve Compose kullanmaya başlama
İle [Compose](https://github.com/docker/compose), birden fazla Docker kapsayıcılarını oluşan bir uygulamanın tanımlamak için basit bir metin dosyası kullanın. Ardından, uygulamanızda tanımlanmış ortamınıza dağıtmak için her şeyi yapan tek bir komut, hızla çalıştırın. Bu makalede örnek olarak, arka uç bir Ubuntu sanal MariaDB SQL veritabanı ile WordPress blogu hızlı bir şekilde ayarlama işlemini gösterir. Oluştur, daha karmaşık uygulamalar ayarlamak için de kullanabilirsiniz.

Bu makalede son 14/2/2019 kullanarak test [Azure Cloud Shell](https://shell.azure.com/bash) ve [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 2.0.58 sürümü.

## <a name="create-docker-host-with-azure-cli"></a>Azure CLI ile Docker konağı oluşturma
Son yükleme [Azure CLI](/cli/azure/install-az-cli2) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index).

İlk olarak, Docker ortamınız için bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myDockerGroup --location eastus
```

Adlı bir dosya oluşturun *cloud-init.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. 

```yaml
#include https://get.docker.com
```

Şimdi [az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. `--custom-data` parametresini kullanarak cloud-init yapılandırma dosyanızı geçirin. Dosyayı mevcut çalışma dizininizin dışına kaydettiyseniz *cloud-init.txt* yapılandırmasının tam yolunu belirtin. Aşağıdaki örnekte adlı bir VM oluşturur *myDockerVM* ve açılır web trafiği için 80 numaralı bağlantı noktası.

```azurecli-interactive
az vm create \
    --resource-group myDockerGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
az vm open-port --port 80 \
    --resource-group myDockerGroup \
    --name myDockerVM
```

VM’nin oluşturulması, paketlerin yüklenmesi ve uygulamanın başlatılması birkaç dakika sürebilir. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. VM oluşturulduktan sonra, Azure CLI tarafından görüntülenen `publicIpAddress` değerini not edin. 

                 

## <a name="install-compose"></a>Yükleme oluştur


Yeni Docker ana VM için SSH. Kendi IP adresini belirtin.

```bash
ssh azureuser@10.10.111.11
```

Yükleme VM oluşturun.

```bash
sudo apt install docker-compose
```


## <a name="create-a-docker-composeyml-configuration-file"></a>Docker-compose.yml yapılandırma dosyası oluşturma
Oluşturma bir `docker-compose.yml` VM'de çalıştırmak için Docker kapsayıcılarını tanımlamak için yapılandırma dosyası. Dosya, her kapsayıcı, gerekli ortam değişkenlerini ve bağımlılıkları, bağlantı noktaları ve bağlantıları arasında kapsayıcılarını çalıştırmaya yönelik resmi belirtir. Yml dosyası sözdizimi hakkında daha fazla bilgi için bkz. [dosya başvurusu Compose](https://docs.docker.com/compose/compose-file/).

Oluşturma bir *docker-compose.yml* dosya. Bazı veri dosyasına eklemek için sık kullandığınız metin düzenleyiciyi kullanın. Aşağıdaki örnek, dosya için bir istemle oluşturur `sensible-editor` için kullanmak istediğiniz bir düzenleyici seçin.

```bash
sensible-editor docker-compose.yml
```

Aşağıdaki örnek, Docker Compose dosyanıza yapıştırın. Bu yapılandırma görüntüleri kullanan [DockerHub kayıt defteri](https://registry.hub.docker.com/_/wordpress/) WordPress (açık kaynak Web günlüğü ve içerik yönetim sistemi) ve bağlı bir arka uç MariaDB SQL veritabanı'nı yüklemek için. Kendi girin *MYSQL_ROOT_PASSWORD*.

```yml
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
sudo docker-compose up -d
```

Bu komut belirtilen Docker kapsayıcıları başlatır *docker-compose.yml*. Bir veya bu adımı tamamlamak için iki dakika sürer. Aşağıdakine benzer bir çıktı görürsünüz:

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```


Kapsayıcıları çalışır durumda olduğunu doğrulamak için `sudo docker-compose ps`. Benzer bir şey görmeniz gerekir:

```
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Şimdi WordPress bağlantı noktası 80 üzerinde bir VM üzerinde doğrudan bağlanabilirsiniz. Bir web tarayıcısı açın ve sanal makinenizin IP adresi adı girin. WordPress görmelisiniz başlangıç ekranında, yüklemenin tamamlanması ve uygulama kullanmaya başlayın.

![WordPress başlangıç ekranı](./media/docker-compose-quickstart/wordpressstart.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kullanıma [komut satırı başvurusu Compose](https://docs.docker.com/compose/reference/) ve [Kullanıcı Kılavuzu](https://docs.docker.com/compose/) oluşturmaya ve çok kapsayıcılı uygulamalar dağıtmaya ilişkin daha fazla örnek için.
* Bir Azure Resource Manager şablonu kullanın ya da, kendi ya da bir katkıda bulunan [topluluk](https://azure.microsoft.com/documentation/templates/), Docker ve Compose ile ayarlanmış bir uygulama ile bir Azure VM dağıtmak için. Örneğin, [WordPress blogu ile Docker dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) hızla bir MySQL arka ucuna bir Ubuntu sanal ile WordPress dağıtmak için şablonu kullanan Docker ve Compose.
* Docker Compose, Docker Swarm kümesi ile tümleştirme deneyin. Bkz: [kullanarak Compose Swarm ile](https://docs.docker.com/compose/swarm/) senaryolar için.

