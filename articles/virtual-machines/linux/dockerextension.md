---
title: Azure Docker VM uzantısını kullanma | Microsoft Docs
description: Azure Resource Manager şablonları ve Azure CLI kullanarak bir Docker ortamında hızlı ve güvenli bir şekilde dağıtmak için Docker VM uzantısını kullanmayı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: zarhoads
ms.openlocfilehash: 75959225d6fcc5487466ed26a21ba2d26c55cde9
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465942"
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Docker VM uzantısını kullanarak Azure'da Docker ortamı oluşturma

Docker bir popüler kapsayıcı yönetimi ve hızlı bir şekilde Linux üzerindeki kapsayıcılar ile çalışın olanak tanıyan görüntüleme platformudur. Azure'da Docker'ı ihtiyaçlarınıza göre dağıtabilirsiniz çeşitli yolları vardır. Bu makalede, Azure CLI ile Docker VM uzantısı ve Azure Resource Manager şablonlarını kullanmaya odaklanmıştır. 

> [!WARNING]
> Linux için Azure Docker VM uzantısı kullanım dışıdır ve Kasım 2018'den kullanımdan kaldırılacaktır.
> Cloud-init veya özel betik uzantısı gibi Alternatiflere seçim Docker sürümünü yüklemek için daha iyi bir yolu olacak şekilde uzantısı yalnızca Docker'ı yükler. Cloud-init kullanma hakkında daha fazla bilgi için bkz. [cloud-init ile Linux VM özelleştirme](tutorial-automate-vm-deployment.md).

## <a name="azure-docker-vm-extension-overview"></a>Azure Docker VM uzantısı genel bakış
Azure Docker VM uzantısını yükler ve Docker daemon, Docker istemcisi ve Docker Compose, Linux sanal makinesi (VM) yapılandırır. Azure Docker VM uzantısı kullanarak, daha fazla denetim ve yalnızca Docker Machine kullanarak veya kendiniz Docker konağı oluşturma özellikleri vardır. Gibi bu ek özellikleri [Docker Compose](https://docs.docker.com/compose/overview/), Azure Docker VM uzantısını daha sağlam bir geliştirici ya da üretim ortamları için uygun yapın.

Docker makinesi ve Azure Container Services kullanma dahil olmak üzere farklı dağıtım yöntemi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Uygulama bir kolayca prototip için kullanarak tek bir Docker konağı oluşturma [Docker Machine](docker-machine.md).
* Ek planlama ve yönetim araçları sağlayan üretime hazır ve ölçeklenebilir ortamlar oluşturmak için dağıtabileceğiniz bir [Kubernetes](../../container-service/kubernetes/index.yml) veya [Docker Swarm](../../container-service/dcos-swarm/index.yml) Azure Container Service kümesinde.


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Azure Docker VM uzantısı ile bir şablonu dağıtma
Yüklemek ve Docker konağı yapılandırmak için Azure Docker VM uzantısını kullanan bir Ubuntu VM oluşturmak için var olan bir Hızlı Başlangıç şablonu kullanalım. Şablon buradan görüntüleyebilirsiniz: [basit Docker ile bir Ubuntu VM dağıtımını](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). En son ihtiyacınız [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index#az_login).

Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, ile VM dağıtma [az grubu dağıtım oluşturma](/cli/azure/group/deployment#az_group_deployment_create) Azure Docker VM uzantısını içeren [github'da bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). İstendiğinde, kendi benzersiz değerleri sağlayın *newStorageAccountName*, *adminUsername*, *adminPassword*, ve *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürer.


## <a name="deploy-your-first-nginx-container"></a>İlk, NGINX kapsayıcısı dağıtma
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

Docker ana bilgisayara oturum açtıktan sonra şimdi NGINX kapsayıcısını çalıştırın:

```bash
sudo docker run -d -p 80:80 nginx
```

NGINX görüntüsü indirilir ve kapsayıcı çalışmaya çıktı aşağıdaki örneğe benzer olacaktır:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Kapsayıcılar, Docker ana bilgisayarında şu şekilde çalışan durumunu kontrol edin:

```bash
sudo docker ps
```

Çıktı aşağıdaki örneğe benzer, NGINX kapsayıcısını gösteren çalıştığını ve TCP bağlantı noktaları 80 ve 443 ve iletilen:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Kapsayıcınızı iş başında görmek için bir web tarayıcısını açın ve Docker ana DNS adını girin:

![Çalışan ngnix kapsayıcı](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM uzantısı şablon başvurusu
Önceki örnekte, var olan bir Hızlı Başlangıç şablonu kullanır. Ayrıca, Azure Docker VM uzantısı kendi Resource Manager şablonları ile dağıtabilirsiniz. Bunu yapmak için aşağıdaki tanımlama, Resource Manager şablonlarınızı ekleyin `vmName` vm'nizin uygun şekilde:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

İzlenecek yol: okuyarak Resource Manager şablonlarını kullanma hakkında daha ayrıntılı bulabilirsiniz [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
İsteyebilirsiniz [Docker Daemon programını TCP bağlantı noktası yapılandırma](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), anlamak [Docker güvenliği](https://docs.docker.com/engine/security/security/), ya da kullanarak kapsayıcıları dağıtın [Docker Compose](https://docs.docker.com/compose/overview/). Azure Docker VM uzantısını kendisi hakkında daha fazla bilgi için bkz. [GitHub projesini](https://github.com/Azure/azure-docker-extension/).

Azure'da ek Docker dağıtım seçenekleri hakkında daha fazla bilgi edinin:

* [Azure sürücüsü ile Docker makinesi kullanma](docker-machine.md)  
* [Tanımlamak ve bir Azure sanal makinesinde çok kapsayıcılı bir uygulama çalıştırmak için Docker ve Compose kullanmaya başlama](docker-compose-quickstart.md).
* [Azure Container Service kümesi dağıtma](../../container-service/dcos-swarm/container-service-deployment.md)

