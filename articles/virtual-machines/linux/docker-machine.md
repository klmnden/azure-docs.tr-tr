---
title: Azure'da Linux ana bilgisayar oluşturmak için Docker makinesi kullanma | Microsoft Docs
description: Azure'da Docker konakları oluşturmak için Docker Machine kullanmayı açıklar.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 1e946f82cf7dfcec0a6ff451012e6f5f0ac6e955
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671545"
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Azure'da konakları oluşturmak için Docker Machine kullanma
Bu makalede nasıl kullanılacağı ayrıntılı [Docker Machine](https://docs.docker.com/machine/) Azure'da konakları oluşturmak için. `docker-machine` Komut Azure'da bir Linux sanal makinesini (VM) oluşturur, ardından Docker'ı yükler. Daha sonra aynı yerel Araçlar ve iş akışlarını kullanarak azure'da Docker ana yönetebilirsiniz. Windows 10'da docker-machine kullanma için Linux bash kullanmanız gerekir.

## <a name="create-vms-with-docker-machine"></a>Docker makinesi ile VM oluşturma
İlk olarak, Azure abonelik Kimliğinizi almak [az hesabı show](/cli/azure/account) gibi:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Azure'da Docker ana VM'ler oluşturun `docker-machine create` belirterek *azure* sürücüsü. Daha fazla bilgi için [Docker Azure sürücüsü belgeleri](https://docs.docker.com/machine/drivers/azure/)

Aşağıdaki örnekte adlı bir VM oluşturur *myVM*, "Standart D2 v2" planına dayanarak, adlı bir kullanıcı hesabı oluşturur *azureuser*ve bağlantı noktası açar *80* VM konağında. Azure hesabınızda oturum açın ve kaynakları oluşturup yönetmek için Docker Machine izinleri vermek için tüm yönergeleri izleyin.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_DS2_v2" \
    myvm
```

Çıktı aşağıdaki örneğe benzer:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvm) Completed machine pre-create checks.
Creating machine...
(myvm) Querying existing resource group.  name="docker-machine"
(myvm) Creating resource group.  name="docker-machine" location="westus"
(myvm) Configuring availability set.  name="docker-machine"
(myvm) Configuring network security group.  name="myvm-firewall" location="westus"
(myvm) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvm) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvm) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvm) Creating public IP address.  name="myvm-ip" static=false
(myvm) Creating network interface.  name="myvm-nic"
(myvm) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvm) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvm
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvm
```

## <a name="configure-your-docker-shell"></a>Docker kabuğunuz yapılandırın
Azure'da Docker ana bağlanmak için uygun bağlantı ayarlarını tanımlayın. Çıktının sonunda belirtildiği gibi Docker konağı için bağlantı bilgilerini aşağıdaki gibi görüntüleyin: 

```bash
docker-machine env myvm
```

Çıktı aşağıdaki örneğe benzer:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvm)
```

Bağlantı ayarlarını tanımlamak için ya da önerilen yapılandırma komutunu çalıştırabilirsiniz (`eval $(docker-machine env myvm)`), veya ortam değişkenlerini el ile ayarlayabilirsiniz. 

## <a name="run-a-container"></a>Bir kapsayıcı çalıştırın
Bir kapsayıcı uygulamada temel bir NGINX Web sunucusunu çalıştırma sağlar görmek için. İle bir kapsayıcı oluşturmak `docker run` ve gibi web trafiği için 80 numaralı bağlantı noktasını kullanıma sunar:

```bash
docker run -d -p 80:80 --restart=always nginx
```

Çıktı aşağıdaki örneğe benzer:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Çalışan kapsayıcılar ile görünümü `docker ps`. Aşağıdaki örnek çıktıda gösterilen 80 numaralı bağlantı noktası ile çalışan bir NGINX kapsayıcısını gösterir:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>Test kapsayıcısı
Docker konağı genel IP adresini aşağıdaki gibi alın:


```bash
docker-machine ip myvm
```

Kapsayıcı iş başında görmek için bir web tarayıcısı açın ve önceki komut çıkışında da not ettiğiniz genel IP adresini girin:

![Çalışan ngnix kapsayıcı](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Sonraki adımlar
Docker Compose kullanma hakkında daha fazla örnek için bkz: [Docker ve Compose azure'da kullanmaya başlama](docker-compose-quickstart.md).
