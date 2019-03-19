---
title: Ansible kullanarak Azure'da sanal makine ölçek kümesi oluşturma
description: Ansible kullanarak Azure’da sanal makine ölçek kümesi oluşturmayı ve yapılandırmayı öğrenin
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/24/2018
ms.openlocfilehash: 1176987ab318a97a7db6a12e619e7b7db06ad2da
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58097898"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Ansible kullanarak Azure'da sanal makine ölçek kümesi oluşturma
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Azure'da sanal makine ölçek kümenizi (VMSS) yönetmek için, tıpkı diğer Azure kaynaklarını yönettiğiniz gibi Ansible kullanabilirsiniz. Bu makalede Ansible'ın sanal makine ölçek kümesi oluşturmak ve ölçeklendirmek için nasıl kullanıldığı gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.6 gerekir. 

## <a name="create-a-vmss"></a>VMSS oluşturma
Bu bölüm, aşağıdaki kaynakları tanımlayan bir örnek Ansible playbook sunar:
- Tüm kaynaklarınızın dağıtılacağı **kaynak grubu**
- 10.0.0.0/16 adres alanında **sanal ağ**
- Sanal ağ içinde **alt ağ**
- İnternet üzerindeki kaynaklara erişmenizi sağlayan **genel IP adresi**
- Sanal makine ölçek kümenize gelen ve giden trafik akışını denetleyen **ağ güvenlik grubu**
- Yük dengeleyici kurallarını kullanarak trafiği tanımlı bir VM'ler kümesi arasında dağıtan **yük dengeleyici**
- Oluşturulan tüm kaynakları kullanan **sanal makine ölçek kümesi**

*admin_password* değeri için kendi parolanızı girin.

  ```yml
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

Ansible ile sanal makine ölçek kümesi ortamını oluşturmak için yukarıdaki playbook'u `vmss-create.yml` olarak kaydedin veya [örnek Ansible playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml).

Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Playbook'u çalıştırdıktan sonra, aşağıdaki örneğe benzeyen çıktı sanal makine ölçek kümesinin başarıyla oluşturulduğunu gösterir:

  ```Output
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

## <a name="scale-out-a-vmss"></a>VMSS ölçeği genişletme
Oluşturulan sanal makine ölçek kümesinin iki örneği vardır. Azure portal'da sanal makine ölçek kümesine giderseniz şunu görürsünüz: **Standard_DS1_v2 (2 örnek)**. Ayrıca, Cloud Shell için aşağıdaki komutu çalıştırarak [Azure Cloud Shell](https://shell.azure.com/) de kullanabilirsiniz:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Aşağıdaki çıktıya benzer sonuçlar göreceksiniz:

  ```bash
  {
    "capacity": 2,
  }
  ```

Şimdi, iki örnekten üç örneğe ölçeklendirelim. Aşağıdaki Ansible playbook kodu sanal makine ölçeği hakkındaki bilgileri alır ve kapasitesini ikiden üçe değiştirir. 

  ```yml
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

Oluşturduğunuz sanal makine ölçek kümesi ölçeğini genişletmek için yukarıdaki playbook'u `vmss-scale-out.yml` olarak kaydedin veya [örnek Ansible playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml). 

Aşağıdaki komut playbook'u çalıştırır:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

Ansible playbook'un çalıştırılması sonucu oluşan çıktı sanal makine ölçek kümesi ölçeğinin başarıyla genişletildiğini gösterir:

  ```Output
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

Azure portal'da yapılandırdığınız sanal makine ölçek kümesine giderseniz şunu görürsünüz: **Standard_DS1_v2 (3 örnek)**. Değişikliği [Azure Cloud Shell](https://shell.azure.com/) ile aşağıdaki komutu çalıştırarak da doğrulayabilirsiniz:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Komutun Cloud Shell'de çalıştırılmasının sonucu artık üç örneğin mevcut olduğunu gösterir. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Ansible'ı kullanarak sanal makine ölçek kümeleri uygulamalarını dağıtma](https://docs.microsoft.com/azure/ansible/ansible-deploy-app-vmss)
> 
> [Ansible'ı kullanarak bir sanal makine ölçek kümesi otomatik olarak ölçeklendirme](https://docs.microsoft.com/azure/ansible/ansible-auto-scale-vmss)