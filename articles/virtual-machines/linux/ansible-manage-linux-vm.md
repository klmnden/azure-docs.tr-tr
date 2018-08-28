---
title: Ansible kullanarak Azure’da Linux sanal makine yönetme
description: Ansible kullanarak Azure’da Linux sanal makine yönetmeyi öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 66106346b298fae22cce47081916a6c8eec8fd40
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250688"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Ansible kullanarak Azure’da Linux sanal makine yönetme
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Ansible'ı kullanarak Azure sanal makinelerinizi diğer kaynaklar gibi yönetebilirsiniz. Bu makalede bir Ansible playbook'unu kullanarak Linux sanal makinesini başlatma ve durdurma adımları gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Azure Cloud Shell'i yapılandırın** veya **Linux sanal makinesine Ansible yükleyin ve yapılandırın**

  **Azure Cloud Shell'i yapılandırın**

  1. **Azure Cloud Shell'i yapılandırın** - Azure Cloud Shell'i kullanmaya yeni başladıysanız [Azure Cloud Shell'de Bash için hızlı başlangıç](/azure/cloud-shell/quickstart) makalesindeki Cloud Shell'i başlatma ve yapılandırma adımlarını izleyebilirsiniz. 

  1. **Linux sanal makinesi** - Linux sanal makinesine erişiminiz yoksa [Ansible ile bir sanal makine oluşturabilirsiniz](ansible-create-vm.md).

  **--VEYA--**

  **Linux sanal makinesine Ansible yükleyin ve yapılandırın**

  1. **Ansible'ı yükleyin** - [Desteklenen bir Linux platformuna](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine) Ansible'ı yükleyin.

  1. **Ansible'ı yapılandırın** - [Azure kimlik bilgilerini oluşturun ve Ansible'ı yapılandırın](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Ansible'ı kullanarak Azure sanal makinesini serbest bırakma (durdurma)
Bu bölümde Ansible'ı kullanarak bir Azure sanal makinesini serbest bırakma (durdurma) adımları gösterilmektedir

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. `azure_vm_stop.yml` adlı bir dosya (playbook'unuzu içerecek) oluşturun ve aşağıda gösterilen şekilde VI düzenleyicisinde açın:

  ```azurecli-interactive
  vi azure_vm_stop.yml
  ```

1. **I** anahtarını seçerek ekleme moduna geçin.

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Stop Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
            allocated: no 
    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

1. Örnek Ansible playbook'unu çalıştırın.

  ```bash
  ansible-playbook azure_vm_stop.yml
  ```

1. Çıktı, sanal makinenin başarıyla serbest bırakıldığını (durdurulduğunu) gösteren aşağıdaki örneğe benzer olacaktır:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Ansible'ı kullanarak serbest bırakılmış (durdurulmuş) Azure sanal makinesini başlatma
Bu bölümde Ansible'ı kullanarak serbest bırakılmış (durdurulmuş) bir Azure sanal makinesini başlatma adımları gösterilmektedir

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. `azure_vm_start.yml` adlı bir dosya (playbook'unuzu içerecek) oluşturun ve aşağıda gösterilen şekilde VI düzenleyicisinde açın:

  ```azurecli-interactive
  vi azure_vm_start.yml
  ```

1. **I** anahtarını seçerek ekleme moduna geçin.

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Start Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Start the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

1. Örnek Ansible playbook'unu çalıştırın.

  ```bash
  ansible-playbook azure_vm_start.yml
  ```

1. Çıktı, sanal makinenin başarıyla başlatıldığını gösteren aşağıdaki örneğe benzer olacaktır:

    Çıktı, sanal makinenin başarıyla başlatıldığını gösteren aşağıdaki örneğe benzer olacaktır:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Ansible'ı kullanarak Azure dinamik envanterlerinizi yönetme](../../ansible/ansible-manage-azure-dynamic-inventories.md)