---
title: Hızlı Başlangıç - ansible'ı kullanarak azure'da Linux sanal makineleri yönetme | Microsoft Docs
description: Bu hızlı başlangıçta, ansible'ı kullanarak azure'da Linux sanal makinesi yönetmeyi öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: cab179980f6093bf259556fd690e55c99a817c79
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63760637"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Hızlı Başlangıç: Ansible'ı kullanarak azure'da Linux sanal makineleri yönetme

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, bir Linux sanal makinesini durdurmak ve başlatmak bir Ansible playbook kullanın. 

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Sanal makineyi durdurma

Bu bölümde, serbest bırakma (Durdur) bir Azure sanal makine için ansible'ı kullanın.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. Adlı bir dosya oluşturun `azure-vm-stop.yml`, düzenleyicide açın:

    ```azurecli-interactive
    code azure-vm-stop.yml
    ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```

1. Değiştirin `{{ resource_group_name }}` ve `{{ vm_name }}` yer tutucuları değerleriniz ile.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Kullanarak playbook çalıştırma `ansible-playbook` komutu:

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Bir sanal makineyi Başlat

Bu bölümde, bir serbest (durduruldu) Azure sanal makineyi başlatmak için ansible'ı kullanın.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. Adlı bir dosya oluşturun `azure-vm-start.yml`, düzenleyicide açın:

    ```azurecli-interactive
    code azure-vm-start.yml
    ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. Değiştirin `{{ resource_group_name }}` ve `{{ vm_name }}` yer tutucuları değerleriniz ile.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Kullanarak playbook çalıştırma `ansible-playbook` komutu:

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: Ansible'ı kullanarak Azure dinamik envanterleri yönetme](~/articles/ansible/ansible-manage-azure-dynamic-inventories.md)