---
title: "Linux ana oluşturmak için Docker makine kullanın | Microsoft Docs"
description: "Docker makine Docker ana bilgisayarları oluşturmak için nasıl kullanılacağını açıklar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: a7c346b259f6635589f80a9c52c748fc0c05eef1
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Docker makine konakları oluşturmak için nasıl kullanılacağını
Bu makalede nasıl kullanılacağını ayrıntıları [Docker makine](https://docs.docker.com/machine/) konakları oluşturma. `docker-machine` Komutu Azure'da bir Linux sanal makine (VM) oluşturur sonra Docker yükler. Ardından, Docker ana bilgisayarları aynı yerel araçlarını ve iş akışları'nı kullanarak azure'da yönetebilirsiniz. Windows 10'da docker makine kullanmak için Linux bash kullanmanız gerekir.

## <a name="create-vms-with-docker-machine"></a>Docker makineyle VM'ler oluşturma
İlk olarak, Azure abonelik Kimliğinizi elde [az hesabı Göster](/cli/azure/account#show) gibi:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Azure ile Docker ana bilgisayar sanal makineleri oluşturmak `docker-machine create` belirterek *azure* sürücü olarak. Daha fazla bilgi için bkz: [Docker Azure sürücü belgeleri](https://docs.docker.com/machine/drivers/azure/)

Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM*, "Standart D2 v2" plana göre adlı bir kullanıcı hesabı oluşturur *azureuser*ve bağlantı noktası açar *80* VM konaktaki. Azure hesabınızda oturum açın ve oluşturmak ve kaynakları yönetmek için Docker makine izinleri vermek için tüm komut istemlerini izleyin.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_D2_v2" \
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

## <a name="configure-your-docker-shell"></a>Docker Kabuk yapılandırın
Azure'da, Docker ana bilgisayara bağlanmak için uygun bir bağlantı ayarlarını tanımlar. Çıktı sonunda belirtildiği gibi Docker ana bilgisayarınız için bağlantı bilgilerini aşağıdaki gibi görüntüleyin: 

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

Bağlantı ayarlarını tanımlamak için ya da önerilen yapılandırma komutunu çalıştırabilirsiniz (`eval $(docker-machine env myvm)`), ya da ortam değişkenleri el ile ayarlayabilirsiniz. 

## <a name="run-a-container"></a>Bir kapsayıcı çalıştırın
Eylem, bir temel NGINX Web çalıştırmak sağlar kapsayıcısında görmek için. İle bir kapsayıcı oluşturmak `docker run` ve bağlantı noktası 80 web trafiği için aşağıdaki gibi açın:

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

Çalışan kapsayıcılar ile Görünüm `docker ps`. Aşağıdaki örnek çıkış 80 kullanıma sunulan bağlantı noktası ile çalışan NGINX kapsayıcısı gösterir:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>Test kapsayıcısı
Docker ana genel IP adresi aşağıdaki gibi alın:


```bash
docker-machine ip myvm
```

Eylem kapsayıcısında görmek için bir web tarayıcısı açın ve yukarıdaki komut çıktısında not ettiğiniz ortak IP adresini girin:

![Çalışan ngnix kapsayıcı](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Sonraki adımlar
Konaklarla oluşturabilirsiniz [Docker VM uzantısı](dockerextension.md). Docker Compose kullanarak ile ilgili örnekler için bkz: [Docker ve azure'da oluşturma kullanmaya başlama](docker-compose-quickstart.md).
