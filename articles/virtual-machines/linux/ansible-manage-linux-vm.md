---
title: Ansible kullanarak Azure’da Linux sanal makine yönetme
description: Ansible kullanarak Azure’da Linux sanal makine yönetmeyi öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/22/2018
ms.openlocfilehash: 79b10a30eea9e19f7ec21f9f9b7ebb95b4c34bf2
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42813394"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Ansible kullanarak Azure’da Linux sanal makine yönetme
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Ansible'ı kullanarak Azure sanal makinelerinizi diğer kaynaklar gibi yönetebilirsiniz. Bu makalede bir Ansible playbook'unu kullanarak Linux sanal makinesini başlatma ve durdurma adımları gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

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