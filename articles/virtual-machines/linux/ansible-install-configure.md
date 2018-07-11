---
title: Yükleme ve Azure sanal makineler ile kullanmak için Ansible'ı yapılandırma | Microsoft Docs
description: Ubuntu, CentOS ve SLES üzerindeki Azure kaynaklarını yönetmek için ansible'ı yapılandırma ve yükleme hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: e7d57ead2caff87db07380582b9085b831844f1e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930078"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Yükleme ve Azure sanal makineleri yönetmek için ansible'ı yapılandırma

Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Azure, aynı diğer herhangi bir kaynağa olduğu gibi sanal makinelerinizde (VM'ler) yönetmek için Ansible'ı kullanabilirsiniz. Bu makalede, Ansible ve gerekli Azure Python SDK'sı modülleri bazı yaygın Linux dağıtımları için yükleme işlemi açıklanmaktadır. Yüklü paketleri, belirli bir platform uyacak şekilde ayarlayarak, Ansible hakkında diğer dağıtım paketlerini yükleyebilirsiniz. Azure kaynaklarını güvenli bir şekilde oluşturmak için ayrıca oluşturmak ve kullanmak için ansible'ı için kimlik bilgilerini tanımlamak nasıl öğrenin.

Daha fazla yükleme seçenekleri ve ek platformlarına yönelik adımlar için bkz. [Ansible Yükleme Kılavuzu](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.30 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="install-ansible"></a>Ansible'ı yükleme

Ansible'ı Azure ile kullanmak için en kolay yollarından biri, Azure Cloud Shell ile yönetmek ve Azure kaynaklarını geliştirmek için bir tarayıcı tabanlı kabuk deneyimi olan. Ansible'ı yükleyin ve Git konusunda yönergeleri atlayabilirsiniz Ansible Cloud Shell'de önceden yüklenmiş [oluşturma Azure kimlik bilgileri](#create-azure-credentials). Ek araçlar ayrıca Cloud Shell'de kullanılabilir bir listesi için bkz. [özellikler ve araçlar için Azure Cloud Shell'deki Bash hizmetinde](../../cloud-shell/features.md#tools).

Aşağıdaki yönergeler Ansible'ı yükleyin ve çeşitli işlemleri için bir Linux VM oluşturma işlemini göstermektedir. Bir Linux VM oluşturmanız gerekmez, bir Azure kaynak grubu oluşturmak için bu ilk adımı atlayın. Bir sanal makine oluşturmanız gerekiyorsa, önce bir kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Şimdi gerekirse bir VM oluşturma adımları için aşağıdaki dağıtım paketlerini seçin ve ardından Ansible'ı yükleyin:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Gerekirse, ile VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnekte adlı bir VM oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH kullanarak VM'ye `publicIpAddress` VM çıktısından not ettiğiniz oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM'NİZDE gibi Azure Python SDK'sı modüller ve Ansible için gerekli paketleri yükleyin:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Artık geçin [oluşturma Azure kimlik bilgileri](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Gerekirse, ile VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnekte adlı bir VM oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH kullanarak VM'ye `publicIpAddress` VM çıktısından not ettiğiniz oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM'NİZDE gibi Azure Python SDK'sı modüller ve Ansible için gerekli paketleri yükleyin:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Artık geçin [oluşturma Azure kimlik bilgileri](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Gerekirse, ile VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnekte adlı bir VM oluşturur *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH kullanarak VM'ye `publicIpAddress` VM çıktısından not ettiğiniz oluşturma işlemi:

```bash
ssh azureuser@<publicIpAddress>
```

VM'NİZDE gibi Azure Python SDK'sı modüller ve Ansible için gerekli paketleri yükleyin:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Artık geçin [oluşturma Azure kimlik bilgileri](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Azure kimlik bilgileri oluşturma

Ansible, bir kullanıcı adı ve parola veya hizmet sorumlusu kullanarak Azure ile iletişim kurar. Bir Azure hizmet sorumlusu, uygulamaları, hizmetleri ve Ansible gibi Otomasyon araçları ile kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlayın. Yalnızca bir kullanıcı adı ve parola sağlama üzerinden güvenliğini artırmak için bu örneği temel bir hizmet sorumlusu oluşturur.

Ana bilgisayarınızda veya Azure Cloud Shell'i kullanarak bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Ansible'ı gerekli kimlik bilgilerini çıkış ekranına şunlardır:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Yukarıdaki komutlarda bir örnek çıktı aşağıdaki gibidir:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure'da kimlik doğrulamak için Azure abonelik kimliği kullanarak elde etmeniz [az hesabı show](/cli/azure/account#az-account-show):

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

Sonraki adımda bu iki komut çıktısı kullanırsınız.

## <a name="create-ansible-credentials-file"></a>Ansible'ı kimlik bilgileri dosyası oluşturma

Ansible'ı için kimlik bilgilerini sağlamak için ortam değişkenleri tanımlayın veya bir yerel kimlik bilgileri dosyası oluşturun. Ansible kimlik tanımlama hakkında daha fazla bilgi için bkz. [kimlik bilgilerini sağlayarak Azure modüllerine](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Bir geliştirme ortamı için oluşturma bir *kimlik bilgilerini* konağınızdaki VM Ansible için dosya. Ansible'ı bir önceki adımda yüklediğiniz VM'de bir kimlik bilgileri dosyası oluşturun:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

*Kimlik bilgilerini* dosyasının kendisini abonelik kimliği bir hizmet sorumlusu oluşturma çıkış ile birleştirir. Çıktı önceki [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) komutu, aynı için gerektiği şekilde *client_id*, *gizli*, ve *Kiracı*. Aşağıdaki örnek *kimlik bilgilerini* dosyasını önceki çıkış eşleşen bu değerleri gösterir. Kendi değerlerinizi aşağıdaki gibi girin:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```

Dosyayı kaydedin ve kapatın.

## <a name="use-ansible-environment-variables"></a>Ansible ortam değişkenlerini kullanma

Ansible Tower veya Jenkins gibi araçlarla kullanacaksanız, ortam değişkenleri tanımlamak gerekir. Bu adım yalnızca Ansible istemcinin olacak ve önceki adımda Azure kimlik bilgileri dosyası oluşturduysanız atlanabilir. Ortam değişkenlerini Azure kimlik bilgileri dosyası olarak aynı bilgileri tanımlayın:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Sonraki adımlar

Ansible'ı ve gerekli Azure Python SDK'sı modülleri yüklü ve Ansible için tanımlanan kimlik bilgilerini kullanmak için artık sahipsiniz. Bilgi edinmek için nasıl [Ansible ile VM oluşturma](ansible-create-vm.md). Ayrıca edinebilirsiniz nasıl [tam bir Azure VM oluşturma ve destekleyen kaynaklar Ansible ile](ansible-create-complete-vm.md).
