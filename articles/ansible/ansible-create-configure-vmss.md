---
title: Ansible'ı kullanarak Azure'da sanal makine ölçek kümeleri oluşturma
description: Ansible'ı oluşturmak ve sanal makine ölçek kümesi Azure'da yapılandırmak için kullanmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 5f915f7b1b425a3bd6e5d62eb70bb3f633b7eda8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009010"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Ansible'ı kullanarak Azure'da sanal makine ölçek kümeleri oluşturma
Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Azure'da, aynı diğer herhangi bir Azure kaynak yönetme gibi sanal makine ölçek kümesi (VMSS) yönetmek için Ansible'ı kullanabilirsiniz. Bu makalede oluşturacağınız ve ölçeklendireceğiniz sanal makine ölçek kümesi için ansible'ı kullanmayı gösterir. 

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - oluşturma, bir Azure aboneliği yoksa, bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.
- **Ansible'ı yapılandırma** - [oluşturma Azure kimlik bilgileri ve ansible'ı yapılandırma](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible'ı ve Azure Python SDK'sı modüller** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)

> [!Note]
> Ansible 2.6 aşağıdaki çalıştırılması için gerekli Bu öğreticide örnek playbook'ları. 

## <a name="create-a-vmss"></a>Bir VMSS oluşturma
Bu bölüm, aşağıdaki kaynakları tanımlayan bir örnek Ansible playbook sunar:
- **Kaynak grubu** içine tüm kaynaklarınızı dağıtılacağı
- **Sanal ağ** 10.0.0.0/16 adres alanında
- **Alt ağ** sanal ağ içinde
- **Genel IP adresi** bu wllows Internet üzerinden kaynaklara erişmek için
- **Ağ güvenlik grubu** , sanal makine ölçek kümesi, giriş ve çıkış ağ trafiği akışını denetleyen
- **Yük Dengeleyici** bir yük dengeleyici kurallarını kullanarak tanımlanan VM'ler kümesi arasında trafiği dağıtır
- **Sanal makine ölçek kümesi** oluşturulan tüm kaynakları kullanan

Kendi parolanızı girin *admin_password* değeri.

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
      vmss_lb_name: myVMSSlb
      location: eastus
      admin_username: azureuser
      admin_password: "your_password"
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

      - name: Create VMSS
        azure_rm_virtualmachine_scaleset:
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

Ansible ile sanal makine ölçek kümesi ortamı oluşturmak için yukarıdaki playbook olarak Kaydet `vmss-create.yml`, veya [örnek Ansible playbook indirme](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml).

Ansible playbook çalıştırmak için kullandığınız **ansible playbook** komutuyla şu şekilde:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Playbook'u çalıştırdıktan sonra sanal makine ölçek gösteren aşağıdaki örneğe benzer bir çıktı kümesi başarıyla oluşturuldu:

  ```bash
  PLAY [localhost] ***********************************************************

  TASK [Gathering Facts] *****************************************************
  ok: [localhost]

  TASK [Create a resource group] ****************************************************************************
  changed: [localhost]

  TASK [Create virtual network] ****************************************************************************
  changed: [localhost]

  TASK [Add subnet] **********************************************************
  changed: [localhost]

  TASK [Create public IP address] ****************************************************************************
  changed: [localhost]

  TASK [Create Network Security Group that allows SSH] ****************************************************************************
  changed: [localhost]

  TASK [Create a load balancer] ****************************************************************************
  changed: [localhost]

  TASK [Create VMSS] *********************************************************
  changed: [localhost]

  PLAY RECAP *****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0

  ```

## <a name="scale-out-a-vmss"></a>Bir VMSS ölçeklendirin
Oluşturulan sanal makine ölçek kümesi, iki örneği yok. Azure portalında sanal makine ölçek kümesi gidin, gördüğünüz **Standard_DS1_v2 (2 örnekleri)**. Ayrıca [Azure Cloud Shell](https://shell.azure.com/) Cloud Shell içinde aşağıdaki komutu çalıştırarak:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Çıktının aşağıdakine benzer olması gerekir:

  ```bash
  {
    "capacity": 2,
  }
  ```

Şimdi, şimdi iki örneklerden üç örneğe genişletme. Aşağıdaki Ansible playbook kodu sanal makine ölçek hakkındaki bilgileri alır ve ikisinden üç kapasitesi değiştirir. 

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
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

      - name: Update something in that VMSS
        azure_rm_virtualmachine_scaleset: "{{ body }}"
  ```

Oluşturduğunuz sanal makine ölçek kümesi ölçeklendirmek için önceki playbook olarak Kaydet `vmss-scale-out.yml` veya [örnek Ansible playbook indirme](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml)). 

Aşağıdaki komut, playbook çalışır:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

Ansible playbook çalışmasını çıktı, sanal makine ölçek kümesi başarıyla kullanıma ölçeklenmiş olduğunu gösterir:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get scaleset info] ***************************************************************************
  ok: [localhost]

  TASK [Dump scaleset info] ***************************************************************************
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

  TASK [Modify scaleset (set upgradePolicy to Automatic and capacity to 3)] ***************************************************************************
  ok: [localhost]

  TASK [Update something in that VMSS] ***************************************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=5    changed=1    unreachable=0    failed=0
  ```

Varsa, bkz. Azure portalında yapılandırılmış sanal makine ölçek kümesi gidin **Standard_DS1_v2 (3 örnek)**. Değişiklik ile da doğrulayabilirsiniz [Azure Cloud Shell](https://shell.azure.com/) aşağıdaki komutu çalıştırarak:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Cloud Shell'de komutu çalıştırma sonuçları üç örneği artık mevcut olduğunu gösterir. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [VMSS için örnek playbook Ansible](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)