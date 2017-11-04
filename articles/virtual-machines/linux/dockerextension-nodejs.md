---
title: "Azure Docker VM uzantısı ile Azure CLI 1.0 kullanın | Microsoft Docs"
description: "Docker VM uzantısı hızlı ve güvenli bir şekilde Azure Resource Manager şablonları kullanarak Docker bir ortamda dağıtmak için nasıl kullanılacağını öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: a3cbcf63533f4042dcd695e141655c5814bd7068
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension-with-the-azure-cli-10"></a>Docker VM uzantısı ile Azure CLI 1.0 kullanarak azure'da bir Docker ortam oluşturma
Docker popüler kapsayıcı yönetimi ve Linux (ve de Windows) ile kapsayıcıları hızlı bir şekilde çalışmanıza olanak sağlayan görüntüleme platform ' dir. Azure'da, sizin ihtiyaçlarınıza göre Docker dağıtabilirsiniz çeşitli yolları vardır. Bu makalede, Docker VM uzantısı ve Azure Resource Manager şablonları kullanarak odaklanır. 

Docker makine ve Azure kapsayıcı hizmetlerini kullanma dahil farklı dağıtım yöntemi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Uygulama bir hızlı bir şekilde prototip için kullanarak tek bir Docker ana oluşturabilirsiniz [Docker makine](docker-machine.md).
* Daha büyük ve daha kararlı ortamlar için de destekler Azure Docker VM uzantısı kullanabilirsiniz [Docker Compose](https://docs.docker.com/compose/overview/) tutarlı kapsayıcı dağıtımlarını oluşturmak için. Bu makale Azure Docker VM uzantısı kullanılarak ayrıntıları.
* Ek planlama ve yönetim araçları sağlar üretime hazır, ölçeklenebilir ortamlar oluşturmak için dağıtabileceğiniz bir [Azure kapsayıcı hizmetlerini Docker Swarm kümesinde](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#azure-docker-vm-extension-overview) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](dockerextension.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız 

## <a name="azure-docker-vm-extension-overview"></a>Azure Docker VM uzantısı genel bakış
Azure Docker VM uzantısı yükler ve Docker arka plan programı, Docker istemcisi ve Docker Compose, Linux sanal makine (VM) yapılandırır. Azure Docker VM uzantısı kullanarak, daha fazla denetim ve yalnızca Docker makine kullanarak veya kendiniz Docker ana oluşturarak daha özelliklere sahip. Gibi bu ek özellikler [Docker Compose](https://docs.docker.com/compose/overview/), Azure Docker VM uzantısı daha sağlam geliştirici veya üretim ortamları için uygun olun.

Azure Resource Manager şablonları ortamınızın tamamını yapısını tanımlar. Şablonları oluşturmak ve Docker ana bilgisayar sanal makineleri gibi kaynakları, depolama, rol tabanlı erişim denetimi (RBAC) ve tanılama yapılandırmanıza olanak sağlar. Başka dağıtımlar tutarlı bir şekilde oluşturmak için bu şablonları yeniden kullanabilirsiniz. Azure Resource Manager ve şablonları hakkında daha fazla bilgi için bkz: [Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Bir şablonu Azure Docker VM uzantısı ile dağıtma
Şimdi yüklemek ve Docker ana yapılandırmak için Azure Docker VM uzantısını kullanan bir Ubuntu VM oluşturmak için var olan bir hızlı başlangıç şablonunu kullanın. Şablon burada görüntüleyebilirsiniz: [basit bir Ubuntu VM docker'la dağıtımını](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Gereksinim duyduğunuz [en son Azure CLI](../../cli-install-nodejs.md) yüklü ve Resource Manager modunu aşağıdaki gibi kullanarak oturum:

```azurecli
azure config mode arm
```

URI şablonunu belirleme Azure CLI kullanarak şablonu dağıtın. Aşağıdaki örnek *westus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Kendi kaynak grubu adı ve konumu aşağıdaki şekilde kullanın:

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Depolama hesabınızın adı, bir kullanıcı adı ve parola girin ve bir DNS ad ister yanıtlayın. Çıktı aşağıdaki örneğe benzer:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Oluşturulan ve Azure Docker VM uzantısıyla yapılandırılmış yalnızca birkaç saniye, ancak Docker ana bilgisayarınız sonra istemi yine Azure CLI döndürür. Tamamlamak için dağıtım için birkaç dakika sürer. Docker ana bilgisayar durumu kullanma hakkında ayrıntılı bilgi görüntüleyebileceğiniz `azure vm show` komutu.

Aşağıdaki örnek adlı VM durumunu denetler *myDockerVM* (varsayılan ad şablondan - bu adı değişmez) kaynak grubunda adlı *myResourceGroup*. Önceki adımda oluşturduğunuz kaynak grubunun adını girin:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

Çıktısını `azure vm show` komutu aşağıdaki örneğe benzer:

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.westus.cloudapp.azure.com
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Çıktı üst, gördüğünüz **ProvisioningState** VM. Bu görüntülendiğinde *başarılı*, dağıtım sona erdi ve VM için SSH kullanabilirsiniz.

Çıktı sonuna *FQDN* , Docker ana bilgisayarın tam etki alanı adını görüntüler. Bu FQDN ne kalan adımlar, Docker ana SSH için kullandığınız yöntemdir.

## <a name="deploy-your-first-nginx-container"></a>İlk nginx kapsayıcısı dağıtma
Dağıtım, SSH yeni Docker konağına yerel bilgisayarınızdan tamamlandıktan sonra. Kullanıcı adınızı ve FQDN şu şekilde girin:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

Şimdi Docker ana bilgisayara oturum açtıktan sonra bir nginx kapsayıcısı çalıştırın:

```bash
sudo docker run -d -p 80:80 nginx
```

Çıktı nginx görüntü yüklenir ve bir kapsayıcı başlatıldı olarak aşağıdaki örneğe benzer:

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

Aşağıdaki gibi Docker ana bilgisayarında çalışan kapsayıcılar durumunu kontrol edin:

```bash
sudo docker ps
```

Çıktı aşağıdaki örneğe benzer, nginx kapsayıcısı gösteren çalıştığını ve 80 ve 443 numaralı TCP bağlantı noktaları ve iletilen:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Uygulamada, kapsayıcı görmek için bir web tarayıcısını açın ve Docker ana bilgisayar FQDN adını girin:

![Çalışan ngnix kapsayıcı](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM uzantısı şablon başvurusu
Önceki örnekte, var olan bir Hızlı Başlangıç şablonu kullanır. Ayrıca, Azure Docker VM uzantısı kendi Resource Manager şablonları ile dağıtabilirsiniz. Bunu yapmak için aşağıdaki tanımlama, Resource Manager şablonlarınızı ekleyin *vmName* vm'nizin uygun şekilde:

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
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

İzlenecek okuyarak Resource Manager şablonları kullanma hakkında daha ayrıntılı bulabilirsiniz [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
İçin isteyebilir [Docker daemon TCP bağlantı noktası yapılandırma](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), anlamak [Docker güvenlik](https://docs.docker.com/engine/security/security/), veya kullanarak kapsayıcıları dağıtma [Docker Compose](https://docs.docker.com/compose/overview/). Azure Docker VM uzantısı kendisi hakkında daha fazla bilgi için bkz: [GitHub proje](https://github.com/Azure/azure-docker-extension/).

Azure'da ek Docker dağıtım seçenekleri hakkında daha fazla bilgi okuyun:

* [Docker makine Azure sürücüsüyle kullanın](docker-machine.md)  
* [Docker ve oluşturma tanımlamak ve bir Azure sanal makine üzerinde birden çok kapsayıcı uygulamayı çalıştırmak için kullanmaya başlama](docker-compose-quickstart.md).
* [Azure Container Service kümesi dağıtma](../../container-service/dcos-swarm/container-service-deployment.md)

