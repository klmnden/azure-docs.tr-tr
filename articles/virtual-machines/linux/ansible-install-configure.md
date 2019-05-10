---
title: Hızlı Başlangıç - azure'da Linux sanal makinelerinde yükleme Ansible | Microsoft Docs
description: Bu hızlı başlangıçta, yükleme ve Ubuntu, CentOS ve SLES üzerindeki Azure kaynaklarını yönetmek için ansible'ı yapılandırma hakkında bilgi edinin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 60cefe24feab9de4262e81eb1bc313aeadc7eb05
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409249"
---
# <a name="quickstart-install-ansible-on-linux-virtual-machines-in-azure"></a>Hızlı Başlangıç: Azure'da Linux sanal makinelerinde Ansible'ı yükleme

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, bazı yaygın Linux dağıtımları için ansible'ı yapılandırma gösterilmektedir. Ansible'ı üzerinde diğer dağıtım paketlerini yüklemek için belirli platform için yüklü paketleri ayarlayın. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-sp.md](../../../includes/open-source-devops-prereqs-create-service-principal.md)]
- **Linux veya Linux sanal makinesine erişim** - Linux makineniz yoksa [Linux sanal makinesi](/azure/virtual-network/quick-create-cli) oluşturun.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Bir Azure Linux sanal makinesine Ansible yükleme

Linux makinenizde oturum açın ve Ansible yükleme adımları için aşağıdaki dağıtımlardan birini seçin:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Bu bölümde, CentOS, ansible'ı kullanmak için yapılandırın.

1. Bir terminal penceresi açın.

1. Azure Python SDK'sı modüller için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
    sudo yum install -y python-pip python-wheel
    ```

1. Ansible'ı gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Azure kimlik bilgileri oluşturma](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Bu bölümde, Ubuntu ansible'ı kullanmak için yapılandırın.

1. Bir terminal penceresi açın.

1. Azure Python SDK'sı modüller için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip
    ```

1. Ansible'ı gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Azure kimlik bilgileri oluşturma](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Bu bölümde, SLES ansible'ı kullanmak için yapılandırın.

1. Bir terminal penceresi açın.

1. Azure Python SDK'sı modüller için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
        python-devel libopenssl-devel libtool python-pip python-setuptools
    ```

1. Ansible'ı gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. Çakışan Python cryptography paketi kaldırmak için aşağıdaki komutu girin:

    ```bash
    sudo pip uninstall -y cryptography
    ```

1. [Azure kimlik bilgileri oluşturma](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma

Ansible kimlik bilgilerini yapılandırmak için aşağıdaki bilgiler gereklidir:

* Azure abonelik Kimliğinizi 
* Hizmet sorumlusu değerleri

Hizmet sorumlusu değerleri, Ansible Tower veya Jenkins kullanıyorsanız, ortam değişkenleri olarak bildirin.

Ansible'ı kimlik bilgileri aşağıdaki tekniklerden birini kullanarak yapılandırın:

- [Ansible kimlik bilgileri dosyası oluşturma](#file-credentials)
- [Ansible ortam değişkenlerini kullanma](#env-credentials)

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible kimlik bilgileri dosyası oluşturma

Bu bölümde, Ansible için kimlik bilgilerini sağlamak için bir yerel kimlik bilgileri dosyası oluşturun. 

Ansible kimlik tanımlama hakkında daha fazla bilgi için bkz. [kimlik bilgilerini sağlayarak Azure modüllerine](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

1. Adlı bir dosya için bir geliştirme ortamı oluşturma `credentials` konak sanal makinede:

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. Dosyasına aşağıdaki satırları ekleyin. Yer tutucuları hizmet asıl değerlerle değiştirin.

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. Dosyayı kaydedin ve kapatın.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible ortam değişkenlerini kullanma

Bu bölümde, Ansible kimlik bilgilerinizi yapılandırmak için hizmet sorumlusu değerleri dışarı aktarın.

1. Bir terminal penceresi açın.

1. Hizmet sorumlusu değerleri dışarı aktarın:

    ```bash
    export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
    export AZURE_CLIENT_ID=<security-principal-appid>
    export AZURE_SECRET=<security-principal-password>
    export AZURE_TENANT=<security-principal-tenant>
    ```

## <a name="verify-the-configuration"></a>yapılandırıldığını doğrulayın

Başarılı yapılandırmasını doğrulamak için bir Azure kaynak grubu oluşturmak için ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı Başlangıç: Ansible'ı kullanarak Azure'da bir Linux sanal makinesi yapılandırma](./ansible-create-vm.md)