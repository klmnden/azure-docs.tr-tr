---
title: Yükleme ve Ansible Azure sanal makineler ile kullanmak için yapılandırma | Microsoft Docs
description: Yükleme ve Ubuntu, CentOS ve SLES Azure kaynaklarını yönetmek için Ansible yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/04/2018
ms.author: iainfou
ms.openlocfilehash: c00ebcb771081f8e35c67bf384f5f6822e16f268
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653000"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Yükleme ve Azure sanal makineleri yönetmek için Ansible yapılandırma

Ansible dağıtma ve yapılandırmanın ortamınızdaki kaynakların otomatikleştirmenizi sağlar. Azure, aynı herhangi bir kaynağa olduğu gibi sanal makineleri (VM'ler) yönetmek için Ansible kullanabilirsiniz. Bu makalede Ansible ve gerekli Azure Python SDK'sını modüllerini bazı yaygın Linux distro'lar için yükleme ayrıntılarını verir. Yüklü paketler belirli platformunuz uyacak şekilde ayarlayarak diğer distro'lar üzerinde Ansible yükleyebilirsiniz. Azure kaynaklarını güvenli bir şekilde oluşturmak için de oluşturmak ve kullanmak Ansible için kimlik bilgilerini tanımlamak nasıl öğrenin.

Daha fazla yükleme seçenekleri ve ek platformlar için adımları için bkz: [Ansible Yükleme Kılavuzu](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.30 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="install-ansible"></a>Ansible yükleyin

Ansible Azure ile kullanmak için en kolay yollarından biri Azure bulut kabuğunda, yönetmek ve Azure kaynaklarını geliştirmek için bir kabuk tarayıcı tabanlı deneyimi biridir. Ansible yüklemek ve Git hakkında yönergeler atlayabilirsiniz Ansible bulut Kabuğu'nda önceden yüklenmiş [oluşturma Azure kimlik bilgilerini](#create-azure-credentials). Ek araçlar bulut Kabuğu'nda da kullanılabilir bir listesi için bkz: [özellikler ve araçlar için Azure bulut Kabuk Bash'te](../../cloud-shell/features.md#tools).

Aşağıdaki yönergeler çeşitli distro'lar için bir Linux VM oluşturun ve ardından Ansible yükleyin gösterilmektedir. Bir Linux VM oluşturma gerekmiyorsa, bir Azure kaynak grubu oluşturmak için bu ilk adımı atlayın. Bir VM oluşturmanız gerekiyorsa, ilk sahip bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Şimdi, gerekirse bir VM oluşturma adımları için aşağıdaki distro'lar birini seçin ve ardından Ansible yükleyin:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Gerekirse, bir VM ile oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
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

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Gerekirse, bir VM ile oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
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

### <a name="sles-12-sp2"></a>SLES 12 SP2

Gerekirse, bir VM ile oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
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

Ana bilgisayarınızda veya Azure bulut Kabuğu'nu kullanarak bir hizmet asıl oluşturma [az ad sp oluşturma-için-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Ansible gereken kimlik bilgilerini ekranda çıkış şunlardır:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Çıkış örneği önceki komutlarındaki aşağıdaki gibidir:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure için kimlik doğrulaması için Azure abonelik kimliği kullanarak elde etmeniz [az hesabı Göster](/cli/azure/account#az-account-show):

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

Sonraki adımda bu iki komut çıktısı kullanın.

## <a name="create-ansible-credentials-file"></a>Ansible kimlik bilgileri dosyası oluşturma

Ansible için kimlik bilgilerini sağlamak için ortam değişkenleri tanımlayın veya bir yerel kimlik bilgileri dosyası oluşturun. Ansible kimlik tanımlama hakkında daha fazla bilgi için bkz: [kimlik bilgileri sağlayan Azure modüllerle](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Bir geliştirme ortamı oluşturma bir *kimlik bilgileri* ana bilgisayarınızda VM Ansible için dosya. Önceki adımda Ansible yüklendiği VM üzerinde bir kimlik bilgileri dosyası oluşturun:

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

Dosyayı kaydedin ve kapatın.

## <a name="use-ansible-environment-variables"></a>Ansible ortam değişkenlerini kullanma

Ansible Kule ya da Jenkins gibi araçları kullanacaksanız, ortam değişkenleri tanımlamanız gerekir. Bu adım, yalnızca Ansible istemci kullanacaksanız ve Azure kimlik bilgileri dosyası önceki adımda oluşturduğunuz atlanabilir. Ortam değişkenleri Azure kimlik bilgileri dosyası aynı bilgileri tanımlayın:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Ansible ve yüklü gerekli Azure Python SDK'sını modüllerini ve Ansible için tanımlanan kimlik bilgilerini kullanmak için sahipsiniz. Bilgi edinmek için nasıl [Ansible ile bir VM oluşturma](ansible-create-vm.md). Ayrıca öğrenebilirsiniz nasıl [tam bir Azure VM oluşturun ve Ansible kaynaklarla destekleyici](ansible-create-complete-vm.md).
