---
title: Öğretici - ansible'ı kullanarak Azure'da sanal makine ölçek kümeleri yapılandırma | Microsoft Docs
description: Ansible'ı oluşturmak ve Azure'da sanal makine ölçek kümeleri yapılandırmak için kullanmayı öğrenin
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 41ef6103a899970142df1a6beee0ad428419f3df
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230732"
---
# <a name="tutorial-configure-virtual-machine-scale-sets-in-azure-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure'da sanal makine ölçek kümeleri yapılandırma

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir VM için kaynakları yapılandırma
> * Bir ölçek kümesi yapılandırma
> * Ölçek, VM örnekleri artırarak kümesini ölçeklendirme 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="configure-a-scale-set"></a>Bir ölçek kümesi yapılandırma

Bu bölümdeki playbook kod, aşağıdaki kaynakları tanımlar:

* **Kaynak grubu** içine tüm kaynaklarınızı dağıtılacak.
* 10.0.0.0/16 adres alanında **sanal ağ**
* Sanal ağ içinde **alt ağ**
* İnternet üzerindeki kaynaklara erişmenizi sağlayan **genel IP adresi**
* **Ağ güvenlik grubu** giriş ve çıkış, Ölçek kümesinin ağ trafiği akışını denetleyen
* Yük dengeleyici kurallarını kullanarak trafiği tanımlı bir VM'ler kümesi arasında dağıtan **yük dengeleyici**
* Oluşturulan tüm kaynakları kullanan **sanal makine ölçek kümesi**

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml) ve kaydetmesi `vmss-create.yml`.
* Adlı yeni bir dosya oluşturun `vmss-create.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    vmss_lb_name: myScaleSetLb
    location: eastus
    admin_username: azureuser
    admin_password: "{{ admin_password }}"
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create virtual network
      azure_rm_virtualnetwork:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        address_prefixes: "10.0.0.0/16"
    - name: Add subnet
      azure_rm_subnet:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        address_prefix: "10.0.1.0/24"
        virtual_network: "{{ vmss_name }}"
    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ vmss_name }}"
    - name: Create Network Security Group that allows SSH
      azure_rm_securitygroup:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        rules:
          - name: SSH
            protocol: Tcp
            destination_port_range: 22
            access: Allow
            priority: 1001
            direction: Inbound

    - name: Create a load balancer
      azure_rm_loadbalancer:
        name: "{{ vmss_lb_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        public_ip: "{{ vmss_name }}"
        probe_protocol: Tcp
        probe_port: 8080
        probe_interval: 10
        probe_fail_count: 3
        protocol: Tcp
        load_distribution: Default
        frontend_port: 80
        backend_port: 8080
        idle_timeout: 4
        natpool_frontend_port_start: 50000
        natpool_frontend_port_end: 50040
        natpool_backend_port: 22
        natpool_protocol: Tcp

    - name: Create Scale Set
      azure_rm_virtualmachinescaleset:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        vm_size: Standard_DS1_v2
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        ssh_password_enabled: true
        capacity: 2
        virtual_network_name: "{{ vmss_name }}"
        subnet_name: "{{ vmss_name }}"
        upgrade_policy: Manual
        tier: Standard
        managed_disk_type: Standard_LRS
        os_disk_caching: ReadWrite
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest
        load_balancer: "{{ vmss_lb_name }}"
        data_disks:
          - lun: 0
            disk_size_gb: 20
            managed_disk_type: Standard_LRS
            caching: ReadOnly
          - lun: 1
            disk_size_gb: 30
            managed_disk_type: Standard_LRS
            caching: ReadOnly
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* İçinde `vars` bölümünde, değiştirin `{{ admin_password }}` yer tutucusunu kendi parolanızı ile.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook vmss-create.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Create public IP address] 
changed: [localhost]

TASK [Create Network Security Group that allows SSH] 
changed: [localhost]

TASK [Create a load balancer] 
changed: [localhost]

TASK [Create Scale Set] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=8    changed=7    unreachable=0    failed=0

```

## <a name="view-the-number-of-vm-instances"></a>Sanal makine örneği sayısını görüntüleyin

[Ölçek kümesi yapılandırılmış](#configure-a-scale-set) şu anda iki örneği yok. Aşağıdaki adımlar, bu değeri doğrulamak için kullanılır:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Yapılandırdığınız ölçek kümesine gidin.

1. Parantez içine adı ile örnek sayısı ölçek görürsünüz: `Standard_DS1_v2 (2 instances)`

1. İle örnek sayısını da doğrulayabilirsiniz [Azure Cloud Shell](https://shell.azure.com/) aşağıdaki komutu çalıştırarak:

    ```azurecli-interactive
    az vmss show -n myScaleSet -g myResourceGroup --query '{"capacity":sku.capacity}' 
    ```

    Cloud Shell'de Azure CLI komutunu çalıştırma sonuçları üç örneği artık mevcut olduğunu gösterir: 

    ```bash
    {
      "capacity": 3,
    }
    ```

## <a name="scale-out-a-scale-set"></a>Bir ölçek kümesini ölçeklendirme

Bu bölümdeki playbook kod, Ölçek kümesi hakkındaki bilgileri alır ve ikisinden üç kapasitesi değiştirir.

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml) ve kaydetmesi `vmss-scale-out.yml`.
* Adlı yeni bir dosya oluşturun `vmss-scale-out.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
  tasks: 
    - name: Get scaleset info
      azure_rm_virtualmachine_scaleset_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        format: curated
      register: output_scaleset

    - name: Dump scaleset info
      debug:
        var: output_scaleset

    - name: Modify scaleset (change the capacity to 3)
      set_fact:
        body: "{{ output_scaleset.ansible_facts.azure_vmss[0] | combine({'capacity': 3}, recursive=True) }}"

    - name: Update something in that scale set
      azure_rm_virtualmachinescaleset: "{{ body }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook vmss-scale-out.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get scaleset info] 
ok: [localhost]

TASK [Dump scaleset info] 
ok: [localhost] => {
    "output_scaleset": {
        "ansible_facts": {
            "azure_vmss": [
                {
                    ......
                }
            ]
        },
        "changed": false,
        "failed": false
    }
}

TASK [Modify scaleset (set upgradePolicy to Automatic and capacity to 3)] 
ok: [localhost]

TASK [Update something in that scale set] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=5    changed=1    unreachable=0    failed=0
```

## <a name="verify-the-results"></a>Sonuçları doğrulayın

Sonuçlarınızı Azure portalından çalışmanızın doğrulayın:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Yapılandırdığınız ölçek kümesine gidin.

1. Parantez içine adı ile örnek sayısı ölçek görürsünüz: `Standard_DS1_v2 (3 instances)` 

1. Değişikliği [Azure Cloud Shell](https://shell.azure.com/) ile aşağıdaki komutu çalıştırarak da doğrulayabilirsiniz:

    ```azurecli-interactive
    az vmss show -n myScaleSet -g myResourceGroup --query '{"capacity":sku.capacity}' 
    ```

    Cloud Shell'de Azure CLI komutunu çalıştırma sonuçları üç örneği artık mevcut olduğunu gösterir: 

    ```bash
    {
      "capacity": 3,
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: Ansible'ı kullanarak azure'da sanal makine ölçek kümeleri için uygulama dağıtma](./ansible-deploy-app-vmss.md)