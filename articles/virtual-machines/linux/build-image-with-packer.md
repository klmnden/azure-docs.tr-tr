---
title: Packer ile Linux Azure VM görüntüleri oluşturma | Microsoft Docs
description: Packer ile Azure Linux sanal makine görüntüleri oluşturmak için kullanmayı öğrenin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2019
ms.author: cynthn
ms.openlocfilehash: c0ec2616d8bdcf3cfd6d649f12e9bfceea33690a
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467741"
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a>Azure'da Linux sanal makine görüntüleri oluşturmak için Packer kullanma
Azure'daki her sanal makine (VM) Linux dağıtımına ve işletim sistemi sürümünü tanımlayan bir görüntüden oluşturulur. Görüntüleri, önceden yüklenmiş uygulamalar ve yapılandırmalar içerebilir. Azure marketi, en yaygın dağıtım ve uygulama ortamları için birinci ve üçüncü taraf çok sayıda görüntü sağlar veya uygulamanızın ihtiyaçlarına yönelik kendi özel görüntülerinizi oluşturabilir. Bu makalede, açık kaynak aracı kullanma ayrıntılı [Packer](https://www.packer.io/) tanımlama ve azure'da özel görüntü oluşturma.

> [!NOTE]
> Azure artık Azure Görüntü Oluşturucu (Önizleme) bir hizmet tanımlama ve kendi özel görüntülerinizi oluşturmak için var. Hatta mevcut Packer Kabuk sağlayıcısı betiklerinizi ile kullanabilmesi için azure Görüntü Oluşturucu Packer üzerinde oluşturulmuştur. Azure Görüntü Oluşturucu ile çalışmaya başlamak için bkz. [Azure Görüntü Oluşturucu ile Linux VM oluşturma](image-builder.md).


## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun
Kaynak VM oluştururken derleme işlemi sırasında geçici Azure kaynaklarını Packer oluşturur. Bir görüntü olarak kullanmak için kaynak VM yakalamak için bir kaynak grubu tanımlamanız gerekir. Packer yapı işleminin çıktısı, bu kaynak grubunda depolanır.

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma
Packer ile Azure hizmet sorumlusu kullanarak kimliğini doğrular. Bir Azure hizmet sorumlusu, uygulamaları, hizmetleri ve Packer gibi Otomasyon araçları ile kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlayın.

Bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac](/cli/azure/ad/sp) ve Packer gereken kimlik bilgilerini çıktı:

```azurecli
az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
```

Yukarıdaki komutlarda bir örnek çıktı aşağıdaki gibidir:

```azurecli
{
    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure'da kimlik doğrulamak için Azure abonelik Kimliğinizi elde etmeniz [az hesabı show](/cli/azure/account):

```azurecli
az account show --query "{ subscription_id: id }"
```

Sonraki adımda bu iki komut çıktısı kullanırsınız.


## <a name="define-packer-template"></a>Packer şablonu tanımlama
Görüntüleri oluşturmak için bir şablon bir JSON dosyası oluşturun. Şablonda, Oluşturucular ve gerçek derleme işlemini gerçekleştirmek provisioners siz tanımlarsınız. Packer sahip bir [Azure sağlayıcısı](https://www.packer.io/docs/builders/azure.html) sağlayan Azure kaynakları tanımlamak adım önceki oluşturulan hizmet sorumlusu kimlik bilgileri gibi.

Adlı bir dosya oluşturun *ubuntu.json* ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | İlk satırı çıktısı `az ad sp` create komutu - *AppID* |
| *client_secret*                     | İkinci satırı çıktısı `az ad sp` create komutu - *parola* |
| *Kiracı*                         | Üçüncü satır çıktısı `az ad sp` create komutu - *Kiracı* |
| *subscription_id*                   | Çıktı `az account show` komutu |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsü için ad |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

Bu şablon bir Ubuntu 16.04 LTS görüntüsüne oluşturur, NGINX'i yükleyen ve ardından VM deprovisions.

> [!NOTE]
> Bu şablona sağlama kullanıcı kimlik bilgilerini genişletirseniz, okuma için Azure aracısıyla deprovisions sağlayıcısı komutu ayarlamak `-deprovision` yerine `deprovision+user`.
> `+user` Bayrağı tüm kullanıcı hesaplarının kaynak sanal makineden kaldırır.


## <a name="build-packer-image"></a>Packer görüntü oluşturma
Yerel makinenizde yüklü Packer yoksa [Packer yükleme yönergelerini izleyin](https://www.packer.io/docs/install/index.html).

Görüntü, Packer belirterek şablon dosyası şu şekilde oluşturun:

```bash
./packer build ubuntu.json
```

Yukarıdaki komutlarda bir örnek çıktı aşağıdaki gibidir:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH to become available...
==> azure-arm: Connected to SSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! The waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able to login as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

VM oluşturmak için provisioners çalıştırıp dağıtımını temizleme Packer birkaç dakika sürer.


## <a name="create-vm-from-azure-image"></a>Azure görüntüsünden VM oluşturma
Artık bir VM ile görüntüsünden oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm). İle oluşturduğunuz görüntüsünü belirtin `--image` parametresi. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* gelen *myPackerImage* ve zaten mevcut değilse SSH anahtarlarını oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Farklı kaynak grubuna ya da, Packer görüntü bölgesinden Vm'leri oluşturmak istiyorsanız, görüntü adı yerine görüntü kimliği belirtin. Görüntü kimliği ile edinebilirsiniz [az resmi Göster](/cli/azure/image#az-image-show).

VM'nin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Not `publicIpAddress` Azure CLI tarafından görüntülenen. Bu adres, bir web tarayıcısı üzerinden NGINX siteye erişmek için kullanılır.

Web trafiğinin VM’nize erişmesine izin vermek için, [az vm open-port](/cli/azure/vm) komutuyla İnternet’te 80 numaralı bağlantı noktasını açın:

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>VM ve NGINX test
Artık bir web tarayıcısı açıp adres çubuğuna `http://publicIpAddress` ifadesini girebilirsiniz. VM oluşturma işleminden kendi herkese açık IP adresinizi sağlayın. NGINX varsayılan sayfası aşağıdaki örnekte olduğu gibi görüntülenir:

![Varsayılan NGINX sitesi](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Sonraki adımlar
Mevcut Packer sağlayıcısı betiklerle kullanabilirsiniz [Azure Görüntü Oluşturucu](image-builder.md).
