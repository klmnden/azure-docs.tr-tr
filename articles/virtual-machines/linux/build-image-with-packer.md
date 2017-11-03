---
title: "Packer ile Linux Azure VM görüntülerini oluşturmak | Microsoft Docs"
description: "Linux sanal makineleri görüntülerini oluşturmak için Packer kullanmayı öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 1752d2e0a497bf94309a744562cf4462866d6f99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a>Linux sanal makine görüntülerini oluşturmak için Packer kullanma
Her sanal makine (VM) Azure Linux dağıtım ve işletim sistemi sürümü tanımlayan bir görüntüden oluşturulur. Görüntüleri, önceden yüklenmiş uygulamalar ve yapılandırmalar içerebilir. Gereksinimlerinize göre tasarlanmıştır, kendi özel görüntülerinizi oluşturmak veya en yaygın dağıtımları ve uygulama ortamları için Azure Marketi birçok ilk ve üçüncü taraf görüntüleri sağlar. Bu makalede açık kaynak aracının nasıl kullanılacağını ayrıntıları [Packer](https://www.packer.io/) tanımlamak ve Azure özel görüntülerinizi oluşturmak için.


## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun
Kaynak VM oluştururken oluşturma işlemi sırasında geçici Azure kaynaklarını Packer oluşturur. Bir görüntü olarak kullanmak için bu kaynak VM yakalamak için bir kaynak grubu tanımlamanız gerekir. Çıktısı Packer oluşturma işlemi, bu kaynak grubunda depolanır.

[az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Azure kimlik bilgileri oluşturun
Packer bir hizmet sorumlusu kullanarak Azure ile kimliğini doğrular. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve Packer gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlar.

Bir hizmet sorumlusu ile oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) ve Packer gereken kimlik bilgilerini çıktı:

```azurecli
az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
```

Çıkış örneği önceki komutlarındaki aşağıdaki gibidir:

```azurecli
{
    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure için kimlik doğrulaması için Azure abonelik Kimliğinizi elde etmeniz [az hesabı Göster](/cli/azure/account#show):

```azurecli
az account show --query "{ subscription_id: id }"
```

Sonraki adımda bu iki komut çıktısı kullanın.


## <a name="define-packer-template"></a>Packer şablon oluştur
Görüntüleri oluşturmak için bir şablon bir JSON dosyası oluşturun. Şablonda oluşturucular ve gerçek derleme işlemini yürütmek provisioners tanımlayın. Packer sahip bir [Azure sağlayıcısı](https://www.packer.io/docs/builders/azure.html) sağlayan Azure kaynaklarını tanımlamak önceki oluşturulan hizmet asıl kimlik bilgilerini adım gibi.

Adlı bir dosya oluşturun *ubuntu.json* ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | İlk satırı çıktısı `az ad sp` Oluştur komutu - *AppID* |
| *client_secret*                     | İkinci satır çıktısı `az ad sp` Oluştur komutu - *parola* |
| *tenant_id*                         | Üçüncü satır çıktısı `az ad sp` Oluştur komutu - *Kiracı* |
| *ABONELİK_KİMLİĞİ*                   | Çıktı `az account show` komutu |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsü için adı |


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

Bu şablon bir Ubuntu 16.04 LTS görüntü oluşturur, NGINX yükler ve ardından VM deprovisions.

> [!NOTE]
> Kullanıcı kimlik bilgilerini sağlamak için bu şablonu genişletirseniz, Ayarla okumak için Azure Aracısı deprovisions sağlayıcısı komutu `-deprovision` yerine `deprovision+user`.
> `+user` Bayrağını tüm kullanıcı hesaplarının kaynak VM kaldırır.


## <a name="build-packer-image"></a>Packer yansıması oluştur
Yerel makinenizde yüklü Packer zaten yoksa [Packer yükleme yönergelerini izleyin](https://www.packer.io/docs/install/index.html).

Görüntü, Packer belirterek şablon dosyası gibi oluşturun:

```bash
./packer build ubuntu.json
```

Çıkış örneği önceki komutlarındaki aşağıdaki gibidir:

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


## <a name="create-vm-from-azure-image"></a>Azure görüntüden VM oluşturma
Artık bir VM ile görüntüden oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#create). Oluşturduğunuz görüntüsünü belirtin `--image` parametresi. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* gelen *myPackerImage* ve zaten mevcut değilse SSH anahtarları oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. VM oluşturulduktan sonra not edin `publicIpAddress` Azure CLI tarafından görüntülenir. Bu adresi bir web tarayıcısı aracılığıyla NGINX sitesine erişmek için kullanılır.

Bağlantı noktası 80 Internet'ten açın, VM ulaşmak web trafiğe izin verecek şekilde [az vm Aç-port](/cli/azure/vm#open-port):

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Test VM ve NGINX
Bir web tarayıcısı açın ve girin artık `http://publicIpAddress` adres çubuğundaki. Kendi ortak sağlamak VM IP adresinden oluşturma işlemi. Varsayılan NGINX sayfası aşağıdaki örnekte olduğu gibi görüntülenir:

![Varsayılan NGINX sitesi](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, Packer NGINX zaten yüklü bir VM görüntüsü oluşturmak için kullanılır. Varolan dağıtım iş akışları, yanı sıra bu VM görüntüsü gibi Ansible, Chef veya Puppet görüntüden oluşturulan VM'ler için uygulamanızı dağıtmak için kullanabilirsiniz.

Diğer Linux distro'lar için ek örnek Packer şablonları için bkz: [bu GitHub deposuna](https://github.com/hashicorp/packer/tree/master/examples/azure).