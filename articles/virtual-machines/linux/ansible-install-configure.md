---
title: "Yükleme ve Ansible Azure sanal makineler ile kullanmak için yapılandırma | Microsoft Docs"
description: "Yükleme ve Ubuntu, CentOS ve SLES Azure kaynaklarını yönetmek için Ansible yapılandırma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: a27d4422e0d7b116d2aea6f743b9efc27570cdb9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Yükleme ve Azure sanal makineleri yönetmek için Ansible yapılandırma
Bu makalede Ansible ve gerekli Azure Python SDK'sını modüllerini bazı yaygın Linux distro'lar için yükleme ayrıntılarını verir. Yüklü paketler belirli platformunuz uyacak şekilde ayarlayarak diğer distro'lar üzerinde Ansible yükleyebilirsiniz. Azure kaynaklarını güvenli bir şekilde oluşturmak için de oluşturmak ve kullanmak Ansible için kimlik bilgilerini tanımlamak nasıl öğrenin. 

Daha fazla yükleme seçenekleri ve ek platformlar için adımları için bkz: [Ansible Yükleme Kılavuzu](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Ansible yükleyin
İlk olarak, bir kaynak grubu ile oluşturmak [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAnsible* içinde *eastus* konumu:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Şimdi, bir VM oluşturmak ve tercih ettiğiniz aşağıdaki distro'lar biri için Ansible yükleyin:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [CentOS 7.3](#centos-73)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM kullanarak SSH `publicIpAddress` VM çıktısından not oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM, Azure Python SDK'sını modülleri ve Ansible gerekli paketleri gibi yükleyin:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
pip install ansible[azure]
```

Şimdi oturum taşıma [oluşturma Azure kimlik bilgilerini](#create-azure-credentials).


### <a name="centos-73"></a>CentOS 7.3
[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM kullanarak SSH `publicIpAddress` VM çıktısından not oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM, Azure Python SDK'sını modülleri ve Ansible gerekli paketleri gibi yükleyin:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Şimdi oturum taşıma [oluşturma Azure kimlik bilgilerini](#create-azure-credentials).


### <a name="sles-12-sp2"></a>SLES 12 SP2
[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM kullanarak SSH `publicIpAddress` VM çıktısından not oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM, Azure Python SDK'sını modülleri ve Ansible gerekli paketleri gibi yükleyin:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Şimdi oturum taşıma [oluşturma Azure kimlik bilgilerini](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Azure kimlik bilgileri oluşturun
Ansible bir kullanıcı adı ve parola veya bir hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve Ansible gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlar. Yalnızca bir kullanıcı adı ve parola sağlayarak üzerinden güvenliğini artırmak için bu örnek temel bir hizmet sorumlusu oluşturur.

Ana bilgisayar ile bir hizmet sorumlusu oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) ve Ansible gereken kimlik bilgilerini çıktı:

```azurecli
az ad sp create-for-rbac --query [client_id: appId, secret: password, tenant: tenant]
```

Çıkış örneği önceki komutlarındaki aşağıdaki gibidir:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure için kimlik doğrulaması için Azure abonelik Kimliğinizi elde etmeniz [az hesabı Göster](/cli/azure/account#az_account_show):

```azurecli
az account show --query "{ subscription_id: id }"
```

Sonraki adımda bu iki komut çıktısı kullanın.


## <a name="create-ansible-credentials-file"></a>Ansible kimlik bilgileri dosyası oluşturma
Ansible için kimlik bilgilerini sağlamak için ortam değişkenleri tanımlayın veya bir yerel kimlik bilgileri dosyası oluşturun. Ansible kimlik tanımlama hakkında daha fazla bilgi için bkz: [kimlik bilgileri sağlayan Azure modüllerle](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

Bir geliştirme ortamı oluşturma bir *kimlik bilgileri* Ansible için VM konağınız üzerinde aşağıdaki gibi dosya:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

*Kimlik bilgileri* dosyasının kendisini abonelik kimliği bir hizmet sorumlusu oluşturma çıkış ile birleştirir. Önceki sürümden çıktı [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) komutu aynıdır için gerektiği şekilde *client_id*, *gizli*, ve *Kiracı*. Aşağıdaki örnek *kimlik bilgileri* dosyasını önceki çıkış eşleşen bu değerleri gösterir. Aşağıdaki gibi kendi değerlerinizi girin:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Ansible ortam değişkenlerini kullanma
Ansible Kule ya da Jenkins gibi araçları kullanacaksanız, ortam değişkenleri şu şekilde tanımlayabilirsiniz. Bu değişkenler bir hizmet sorumlusu oluşturuluyor gelen çıkış abonelik kimliği birleştirin. Önceki sürümden çıktı [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#create-for-rbac) komuttur aynı sırada için gerektiği şekilde *AZURE_CLIENT_ID*, *AZURE_SECRET*, ve *AZURE_TENANT*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Sonraki adımlar
Artık Ansible ve yüklü gerekli Azure Python SDK'sını modüllerini ve Ansible için tanımlanan kimlik bilgilerini kullanmak için sahipsiniz. Bilgi edinmek için nasıl [Ansible ile bir VM oluşturma](ansible-create-vm.md). Ayrıca öğrenebilirsiniz nasıl [tam bir Azure VM oluşturun ve Ansible kaynaklarla destekleyici](ansible-create-complete-vm.md).
