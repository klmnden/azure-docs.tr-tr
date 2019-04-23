---
title: Ansible'ı Azure sanal makinelerine yükleme
description: Ubuntu, CentOS ve SLES üzerinde Azure kaynaklarını yönetmek için Ansible'ı yüklemeyi ve yapılandırmayı öğrenin
ms.service: virtual-machines-linux
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 38a1ffdc815b357f7bb7ebe2c337b55a738fb6b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60187737"
---
# <a name="install-ansible-on-azure-virtual-machines"></a>Ansible'ı Azure sanal makinelerine yükleme

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Ansible'ı kullanarak Azure'daki sanal makinelerinizi (VM) diğer kaynaklar gibi yönetebilirsiniz. Bu makalede en yaygın Linux dağıtımları için Ansible'ı ve gerekli Azure Python SDK'sı modüllerini yükleme adımları anlatılmaktadır. Ansible'ı diğer dağıtımlara yüklemek için yüklenen paketleri platformunuza uyacak şekilde ayarlamanız gerekir. Azure kaynaklarını güvenli bir şekilde oluşturmak için Ansible'da kimlik bilgisi oluşturma ve tanımlamayı da öğreneceksiniz. Cloud Shell'de bulunan ek araçların listesi için bkz. [Azure Cloud Shell'deki Bash özellikleri ve araçlar](../../cloud-shell/features.md#tools).

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Linux veya Linux sanal makinesine erişim** - Linux makineniz yoksa [Linux sanal makinesi](https://docs.microsoft.com/azure/virtual-network/quick-create-cli) oluşturun.

- **Azure hizmet sorumlusu**: Bölümündeki yönergeleri izleyerek **hizmet sorumlusu oluşturma** makale bölümünde [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). **appId**, **displayName**, **password** ve **tenant** değerlerini not edin.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Bir Azure Linux sanal makinesine Ansible yükleme

Linux makinenizde oturum açın ve Ansible yükleme adımları için aşağıdaki dağıtımlardan birini seçin:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Aşağıdaki komutları terminal veya Bash penceresine girerek Azure Python SDK'sı modülleri ve Ansible için gerekli paketleri yükleyin:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

[Azure kimlik bilgilerini oluşturma](#create-azure-credentials) bölümündeki yönergeleri izleyin.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Aşağıdaki komutları terminal veya Bash penceresine girerek Azure Python SDK'sı modülleri ve Ansible için gerekli paketleri yükleyin:


```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

[Azure kimlik bilgilerini oluşturma](#create-azure-credentials) bölümündeki yönergeleri izleyin.

### <a name="sles-12-sp2"></a>SLES 12 SP2

Aşağıdaki komutları terminal veya Bash penceresine girerek Azure Python SDK'sı modülleri ve Ansible için gerekli paketleri yükleyin:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

[Azure kimlik bilgilerini oluşturma](#create-azure-credentials) bölümündeki yönergeleri izleyin.

## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma

Ansible kimlik bilgilerini iki farklı şekilde yapılandırmak için abonelik kimliği ve hizmet sorumlusu oluşturma adımında döndürülen bilgiler kullanılır:

- [Ansible kimlik bilgileri dosyası oluşturma](#file-credentials)
- [Ansible ortam değişkenlerini kullanma](#env-credentials)

Ansible Tower veya Jenkins gibi araçlar kullanacaksanız hizmet sorumlusu değerlerini ortam değişkeni olarak bildirme seçeneğini kullanmanız gerekir.

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible kimlik bilgileri dosyası oluşturma

Bu bölümde kimlik bilgilerini Ansible'a iletmek için kullanılacak yerel kimlik bilgileri dosyasının nasıl oluşturulacağı açıklanmaktadır. Ansible kimlik bilgilerini tanımlama hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini Azure modüllerine iletme](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Geliştirme ortamı kullanıyorsanız Ansible için ana sanal makinenizde aşağıdaki şekilde bir *kimlik bilgileri* dosyası oluşturun:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Aşağıdaki satırlar *kimlik bilgileri* dosyasına ekleyin ve yer tutucuların yerine hizmet sorumlusu oluşturma sırasında verilen bilgileri yazın.

```bash
[default]
subscription_id=<your-subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>
```

Dosyayı kaydedin ve kapatın.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible ortam değişkenlerini kullanma

Bu bölümde Ansible kimlik bilgilerinizi ortam değişkeni olarak dışarı aktarma ve yapılandırma adımları açıklanmaktadır.

Bir terminal veya Bash penceresine aşağıdaki komutları girin:

```bash
export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Yapılandırmanın başarılı olduğunu doğrulamak için Ansible'ı kullanarak bir kaynak grubu oluşturabilirsiniz.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Ansible kullanarak Azure’da Linux sanal makine oluşturma](./ansible-create-vm.md)
