---
title: "Azure bulut Kabuğu'nda Ansible çalıştırın"
description: "Azure bulut Kabuğu'nda çeşitli Ansible görevleri öğrenin"
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: d5a818616d382954d0880bcae58bb13b632ad757
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="run-ansible-in-the-azure-cloud-shell"></a>Azure bulut Kabuğu'nda Ansible çalıştırın

Bu öğreticide, Azure bulut Kabuğu'nda çeşitli Ansible görevleri öğrenin. Bu görevler, bir sanal makineye bağlanma ve oluşturmak ve bir Azure kaynak grubu silmek için Ansible playbooks oluşturma içerir.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - oluşturmak bir Azure aboneliğiniz yoksa bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

- **Azure kimlik** - [oluşturma Azure kimlik bilgilerini ve Ansible yapılandırın](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

- **Azure bulut Kabuk yapılandırma** - Azure bulut Kabuk, makaleyi yeniyseniz [Bash Azure bulut kabuğu için Hızlı Başlangıç](https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart) başlatmak ve bulut Kabuk yapılandırmak nasıl gösterir.

## <a name="use-ansible-to-connect-to-a-vm"></a>Bir VM'ye bağlanmak için Ansible kullanın
Ansible adlı bir Python komut dosyası oluşturduğu [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) oluşturan dinamik stok Azure kaynaklarınızın API istekleri için Azure Resource Manager yaparak. Aşağıdaki adımlar kullanılarak yol `azure_rm.py` bir Azure sanal makineye bağlanmak için komut dosyası:

1. Azure bulut Kabuğu'nu açın.

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

## <a name="run-a-playbook-in-cloud-shell"></a>Bulut Kabuğu'nda bir playbook Çalıştır
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