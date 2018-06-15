---
title: Azure bulut Kabuğu'nda Bash ile Ansible çalıştırın
description: Azure bulut Kabuğu'nda Bash çeşitli Ansible görevleri gerçekleştirmek öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 02/01/2018
ms.topic: article
ms.openlocfilehash: 9fe65f4cf10119002bcb7a3855d112d850e20f1a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150388"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Azure bulut Kabuğu'nda Bash ile Ansible çalıştırın

Bu öğretici kapsamında, bulut Kabuk Bash'te çeşitli Ansible görevleri gerçekleştirmek nasıl öğrenin. Bu görevler, bir sanal makineye bağlanma ve oluşturmak ve bir Azure kaynak grubu silmek için Ansible playbooks oluşturma içerir.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - oluşturmak bir Azure aboneliğiniz yoksa bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

- **Azure kimlik** - [oluşturma Azure kimlik bilgilerini ve Ansible yapılandırın](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

- **Azure bulut Kabuk yapılandırma** - Azure bulut Kabuk, makaleyi yeniyseniz [Bash Azure bulut kabuğu için Hızlı Başlangıç](https://docs.microsoft.com/azure/cloud-shell/quickstart) başlatmak ve bulut Kabuk yapılandırmak nasıl gösterir. Ayrılmış bir Web sitesi bulut kabuğu için burada başlatın:

[![Bulut Kabuk başlatma](https://shell.azure.com/images/launchcloudshell.png "bulut Kabuğu'nu başlatın")](https://shell.azure.com)

## <a name="use-ansible-to-connect-to-a-vm"></a>Bir VM'ye bağlanmak için Ansible kullanın
Ansible adlı bir Python komut dosyası oluşturduğu [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) oluşturan dinamik stok Azure kaynaklarınızın API istekleri için Azure Resource Manager yaparak. Aşağıdaki adımlar kullanılarak yol `azure_rm.py` bir Azure sanal makineye bağlanmak için komut dosyası:

1. Bulut Kabuğu'nda Bash'i açın. Kabuk türü bulut Kabuk pencerenin sol tarafında gösterilir.

1. Kullanmak için bir sanal makine yoksa, hangi test etmek bir sanal makine oluşturmak için bulut Kabuğu'nu içine aşağıdaki komutları girin:

  ```azurecli-interactive
  az group create --resource-group ansible-test-rg --location eastus
  ```

  ```azurecli-interactive
  az vm create --resource-group ansible-test-rg --name ansible-test-vm --image UbuntuLTS --generate-ssh-keys
  ```

1. GNU kullanmak `wget` almak için komutu `azure_rm.py` komut dosyası:

  ```azurecli-interactive
  wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
  ```

1. Kullanım `chmod` için erişim izinlerini değiştirmek için komut `azure_rm.py` komut dosyası. Aşağıdaki komut kullanır `+x` (çalışan) için belirtilen dosyasının yürütülmesine izin vermek için parametre (`azure_rm.py`):

  ```azurecli-interactive
  chmod +x azure_rm.py
  ```

1. Kullanım [ansible komutu](https://docs.ansible.com/ansible/2.4/ansible.html) , sanal makineye bağlanmak için: 

  ```azurecli-interactive
  ansible -i azure_rm.py ansible-test-vm -m ping
  ```

  Bağlantı kurulduktan sonra çıktıya benzer sonuçlar görmeniz gerekir:

  ```Output
  The authenticity of host 'nn.nnn.nn.nn (nn.nnn.nn.nn)' can't be established.
  ECDSA key fingerprint is SHA256:&lt;some value>.
  Are you sure you want to continue connecting (yes/no)? yes
  test-ansible-vm | SUCCESS => {
      "changed": false,
      "failed": false,
      "ping": "pong"
  }
  ```

1. Bu bölümde bir kaynak grubu ve sanal makine oluşturduysanız

  ```azurecli-interactive
  az group delete -n <resourceGroup>
  ```

## <a name="run-a-playbook-in-cloud-shell"></a>Cloud Shell'de playbook çalıştırma
[Ansible playbook](https://docs.ansible.com/ansible/2.4/ansible-playbook.html) komutu Ansible playbooks, hedeflenen konak üzerinde çalışan görevleri yürütür. Bu bölümde, oluşturma ve - bir kaynak grubu ve kaynak grubunu silmek için ikinci bir oluşturmak için iki playbooks yürütme için bulut Kabuğu'nu kullanarak aracılığıyla açıklanmaktadır. 

1. Adlı bir dosya oluşturun `rg.yml` gibi:

  ```azurecli-interactive
  vi rg.yml
  ```

1. Aşağıdaki içeriği (VI Düzenleyicisi'ni örneği barındırmakta) bulut Kabuk penceresine kopyalayın:

  ```yml
  - name: My first Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: demoresourcegroup
        location: eastus
  ```

1. Dosyayı kaydedin ve girerek VI düzenleyiciden çıkmak `:wq` tuşuna basarak &lt;Enter >.

1. Kullanım `ansible-playbook` çalıştırmak için komut `rg.yml` playbook:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Aşağıdaki çıkış benzer sonuçlar görmeniz gerekir:

  ```Output
  PLAY [My first Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Create a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

1. Dağıtımı doğrulama:

  ```azurecli-interactive
  az group show -n demoresourcegroup
  ```

1. Kaynak grubu oluşturduğunuza göre kaynak grubunu silmek için ikinci bir Ansible playbook oluşturun:

  ```azurecli-interactive
  vi rg2.yml
  ```

1. Aşağıdaki içeriği (VI Düzenleyicisi'ni örneği barındırmakta) bulut Kabuk penceresine kopyalayın:

  ```yml
  - name: My second Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: demoresourcegroup
        state: absent
  ```

1. Dosyayı kaydedin ve girerek VI düzenleyiciden çıkmak `:wq` tuşuna basarak &lt;Enter >.

1. Kullanım `ansible-playbook` çalıştırmak için komut `rg2.yml` playbook:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Aşağıdaki çıkış benzer sonuçlar görmeniz gerekir:

  ```Output
  The output is as following. 
  PLAY [My second Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Delete a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure Ansible ile temel bir sanal makine oluşturun](/azure/virtual-machines/linux/ansible-create-vm)
