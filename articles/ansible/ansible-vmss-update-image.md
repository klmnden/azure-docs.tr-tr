---
title: Öğretici - Azure sanal makine ölçek özel görüntüsünü güncelleştirme ayarlar Ansible kullanarak | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri ile özel görüntüsünü güncelleştirmek için Ansible'ı kullanmayı öğrenin
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: d3eedc5b83190af46669b9b5df8643f3c80e9bb1
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230855"
---
# <a name="tutorial-update-the-custom-image-of-azure-virtual-machine-scale-sets-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure sanal makine ölçek özel görüntüsünü güncelleştirme ayarlar

[!INCLUDE [ansible-27-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

VM dağıtıldıktan sonra yazılım ile VM yapılandırma uygulama ihtiyaçlarınızı. Bu yapılandırma görevi, her VM için yapmak yerine özel bir görüntü oluşturabilirsiniz. Özel bir görüntü yüklü yazılımları içeren mevcut bir VM'nin anlık görüntüsüdür. Olduğunda, [bir ölçek kümesi yapılandırma](./ansible-create-configure-vmss.md), bu ölçek kümesinin Vm'leri için kullanılacak görüntüyü belirtin. Her sanal makine örneği, özel bir görüntü kullanarak uygulamanız için aynı şekilde yapılandırılır. Bazı durumlarda, Ölçek kümenizin özel görüntü güncelleştirmeniz gerekebilir. Bu görevi bu öğreticinin biridir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * HTTPD ile iki VM yapılandırma
> * Mevcut bir VM'den özel görüntü oluşturma
> * Bir ölçek kümesini görüntüden oluşturun
> * Özel görüntü güncelleştirme

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="configure-two-vms"></a>İki VM yapılandırma

Bu bölümdeki playbook kod hem de yüklü HTTPD iki sanal makine oluşturur. 

`index.html` Sayfası her VM bir sınama dizesini görüntüler:

* İlk VM değeri görüntüler. `Image A`
* İkinci VM değeri görüntüler. `Image B`

Bu dize, her VM ile farklı yazılım yapılandırma taklit etmek üzere tasarlanmıştır.

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/01-create-vms.yml) ve kaydetmesi `create_vms.yml`.
* Adlı yeni bir dosya oluşturun `create_vms.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- name: Create two VMs (A and B) with HTTPS
  hosts: localhost
  connection: local
  vars:
    vm_name: vmforimage
    admin_username: testuser
    admin_password: Pass123$$$abx!
    location: eastus
  tasks:
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      address_prefixes: "10.0.0.0/16"

  - name: Create subnets for VM A and B
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      virtual_network: "{{ vm_name }}"
      name: "{{ vm_name }}"
      address_prefix: "10.0.1.0/24"

  - name: Create Network Security Group that allows HTTP
    azure_rm_securitygroup:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      rules:
        - name: HTTP
          protocol: Tcp
          destination_port_range: 80
          access: Allow
          priority: 1002
          direction: Inbound

  - name: Create public IP addresses for VM A and B
    azure_rm_publicipaddress:
      resource_group: "{{ resource_group }}"
      allocation_method: Static
      name: "{{ vm_name }}_{{ item }}"
    loop:
      - A
      - B
    register: pip_output

  - name: Create virtual network inteface cards for VM A and B
    azure_rm_networkinterface:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}_{{ item }}"
      virtual_network: "{{ vm_name }}"
      subnet: "{{ vm_name }}"
      public_ip_name: "{{ vm_name }}_{{ item }}"
      security_group: "{{ vm_name }}"
    loop:
      - A
      - B

  - name: Create VM A and B
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}{{ item }}"
      admin_username: "{{ admin_username }}"
      admin_password: "{{ admin_password }}"
      vm_size: Standard_B1ms
      network_interfaces: "{{ vm_name }}_{{ item }}"
      image:
        offer: UbuntuServer
        publisher: Canonical
        sku: 16.04-LTS
        version: latest
    loop:
      - A
      - B

  - name: Create VM Extension
    azure_rm_virtualmachineextension:
      resource_group: "{{ resource_group }}"
      name: testVMExtension
      virtual_machine_name: "{{ vm_name }}{{ item }}"
      publisher: Microsoft.Azure.Extensions
      virtual_machine_extension_type: CustomScript
      type_handler_version: 2.0
      auto_upgrade_minor_version: true
      settings: {"commandToExecute": "sudo apt-get -y install apache2"}
    loop:
      - A
      - B

  - name: Create VM Extension
    azure_rm_virtualmachineextension:
      resource_group: "{{ resource_group }}"
      name: testVMExtension
      virtual_machine_name: "{{ vm_name }}{{ item }}"
      publisher: Microsoft.Azure.Extensions
      virtual_machine_extension_type: CustomScript
      type_handler_version: 2.0
      auto_upgrade_minor_version: true
      settings: {"commandToExecute": "printf '<html><body><h1>Image {{ item }}</h1></body></html>' >> index.html; sudo cp index.html /var/www/html/"}
    loop:
      - A
      - B

  - debug:
      msg: "Public IP Address A: {{ pip_output.results[0].state.ip_address }}"

  - debug:
      msg: "Public IP Address B: {{ pip_output.results[1].state.ip_address }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutuyla değiştirerek `myrg` kaynak grubunuzun adı ile:

```bash
ansible-playbook create-vms.yml --extra-vars "resource_group=myrg"
```

Nedeniyle `debug` playbook bölümlerini `ansible-playbook` yazdırma her VM'nin IP adresi komutu. Daha sonra kullanmak için bu IP adresleri kopyalayın.

![Sanal Makine IP adresleri](media/ansible-vmss-update-image/vmss-update-vms-ip-addresses.png)

## <a name="connect-to-the-two-vms"></a>İki Vm'lere bağlanma

Bu bölümde, her VM'ye bağlanın. Dizeleri önceki bölümde belirtildiği gibi `Image A` ve `Image B` farklı yapılandırmalarda iki ayrı Vm'lerle sahip taklit etmek.

IP adresleri önceki bölümden kullanarak her iki Vm'lere bağlanma:

![Bir sanal makineden ekran görüntüsü](media/ansible-vmss-update-image/vmss-update-browser-vma.png)

![B sanal makineden ekran görüntüsü](media/ansible-vmss-update-image/vmss-update-browser-vmb.png)

## <a name="create-images-from-each-vm"></a>Her VM görüntüleri oluşturma

Bu noktada biraz farklı yapılandırmaları iki Vm'lerde bulunan (kendi `index.html` dosyaları).

Bu bölümdeki playbook kod her VM için özel bir görüntü oluşturur:

* `image_vmforimageA` -Görüntüler sanal makine için oluşturulan özel görüntü `Image A` kendi giriş sayfasında.
* `image_vmforimageB` -Görüntüler sanal makine için oluşturulan özel görüntü `Image B` kendi giriş sayfasında.

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/02-capture-images.yml) ve kaydetmesi `capture-images.yml`.
* Adlı yeni bir dosya oluşturun `capture-images.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- name: Capture VM Images
  hosts: localhost
  connection: local
  vars:
    vm_name: vmforimage
  tasks:

  - name: Stop and generalize VMs
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}{{ item }}"
      generalized: yes
    loop:
      - A
      - B

  - name: Create an images from a VMs
    azure_rm_image:
      resource_group: "{{ resource_group }}"
      name: "image_{{ vm_name }}{{ item }}"
      source: "{{ vm_name }}{{ item }}"
    loop:
      - A
      - B
```

Kullanarak playbook çalıştırma `ansible-playbook` komutuyla değiştirerek `myrg` kaynak grubunuzun adı ile:

```bash
ansible-playbook capture-images.yml --extra-vars "resource_group=myrg"
```

## <a name="create-scale-set-using-image-a"></a>Resmi kullanarak ölçek kümesi oluşturma

Bu bölümde, bir playbook aşağıdaki Azure kaynakları yapılandırmak için kullanılır:

* Genel IP adresi
* Yük dengeleyici
* Bu başvurular ölçek kümesi `image_vmforimageA`

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/03-create-vmss.yml) ve kaydetmesi `create-vmss.yml`.
* Adlı yeni bir dosya oluşturun `create-vmss.yml` ve aşağıdaki içeriği dosyaya kopyalayın: "

```yml
---
- hosts: localhost
  vars:
    vmss_name: vmsstest
    location: eastus
    admin_username: vmssadmin
    admin_password: User123!!!abc
    vm_name: vmforimage
    image_name: "image_vmforimageA"

  tasks:

    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ vmss_name }}"
      register: pip_output

    - name: Create a load balancer
      azure_rm_loadbalancer:
        name: "{{ vmss_name }}lb"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        public_ip: "{{ vmss_name }}"
        probe_protocol: Tcp
        probe_port: 80
        probe_interval: 10
        probe_fail_count: 3
        protocol: Tcp
        load_distribution: Default
        frontend_port: 80
        backend_port: 80
        idle_timeout: 4
        natpool_frontend_port_start: 50000
        natpool_frontend_port_end: 50040
        natpool_backend_port: 22
        natpool_protocol: Tcp

    - name: Create a scale set
      azure_rm_virtualmachinescaleset:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        vm_size: Standard_DS1_v2
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        ssh_password_enabled: true
        capacity: 2
        virtual_network_name: "{{ vm_name }}"
        subnet_name: "{{ vm_name }}"
        upgrade_policy: Manual
        tier: Standard
        managed_disk_type: Standard_LRS
        os_disk_caching: ReadWrite
        image:
          name: "{{ image_name }}"
          resource_group: "{{ resource_group }}"
        load_balancer: "{{ vmss_name }}lb"

    - debug:
        msg: "Scale set public IP address: {{ pip_output.state.ip_address }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutuyla değiştirerek `myrg` kaynak grubunuzun adı ile:

```bash
ansible-playbook create-vmss.yml --extra-vars "resource_group=myrg"
```

Nedeniyle `debug` playbook bölümünü `ansible-playbook` yazdırma ölçek kümesi IP adresini komutu. Daha sonra kullanmak için bu IP adresini kopyalayın.

![Genel IP Adresi](media/ansible-vmss-update-image/vmss-update-vmss-public-ip.png)

## <a name="connect-to-the-scale-set"></a>Ölçek kümesine bağlanma

Bu bölümde, Ölçek kümesine bağlanın. 

IP adresi önceki bölümden kullanarak ölçek kümesine bağlanın.

Dizeleri önceki bölümde belirtildiği gibi `Image A` ve `Image B` farklı yapılandırmalarda iki ayrı Vm'lerle sahip taklit etmek.

Adlı özel görüntüyü başvuruları ölçek kümesi `image_vmforimageA`. Özel görüntü `image_vmforimageA` , giriş sayfası görüntüler sanal makineden oluşturulan `Image A`.

Sonuç olarak, bir giriş sayfası görüntüleme gördüğünüz `Image A`:

![İlk VM ölçek kümesi ilişkilidir.](media/ansible-vmss-update-image/vmss-update-browser-initial-vmss.png)

Sonraki bölüme devam ederken tarayıcı penceresini açık bırakın.

## <a name="change-custom-image-in-scale-set-and-upgrade-instances"></a>Ölçek değişikliği özel görüntü ayarlayın ve örneklerini yükseltin

Ölçek kümesinin görüntüsünü - bu bölümde playbook kod değişiklikleri `image_vmforimageA` için `image_vmforimageB`. Ayrıca, Ölçek kümesi tarafından dağıtılan tüm geçerli sanal makineleri güncelleştirilir.

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/04-update-vmss-image.yml) ve kaydetmesi `update-vmss-image.yml`.
* Adlı yeni bir dosya oluşturun `update-vmss-image.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- name: Update scale set image reference
  hosts: localhost
  connection: local
  vars:
    vmss_name: vmsstest
    image_name: image_vmforimageB
    admin_username: vmssadmin
    admin_password: User123!!!abc
  tasks:

  - name: Update scale set - second image
    azure_rm_virtualmachinescaleset:
      resource_group: "{{ resource_group }}"
      name: "{{ vmss_name }}"
      vm_size: Standard_DS1_v2
      admin_username: "{{ admin_username }}"
      admin_password: "{{ admin_password }}"
      ssh_password_enabled: true
      capacity: 3
      virtual_network_name: "{{ vmss_name }}"
      subnet_name: "{{ vmss_name }}"
      upgrade_policy: Manual
      tier: Standard
      managed_disk_type: Standard_LRS
      os_disk_caching: ReadWrite
      image:
        name: "{{ image_name }}"
        resource_group: "{{ resource_group }}"
      load_balancer: "{{ vmss_name }}lb"

  - name: List all of the instances
    azure_rm_virtualmachinescalesetinstance_facts:
      resource_group: "{{ resource_group }}"
      vmss_name: "{{ vmss_name }}"
    register: instances

  - debug:
      var: instances

  - name: manually upgrade all the instances 
    azure_rm_virtualmachinescalesetinstance:
      resource_group: "{{ resource_group }}"
      vmss_name: "{{ vmss_name }}"
      instance_id: "{{ item.instance_id }}"
      latest_model: yes
    with_items: "{{ instances.instances }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutuyla değiştirerek `myrg` kaynak grubunuzun adı ile:

```bash
ansible-playbook update-vmss-image.yml --extra-vars "resource_group=myrg"
```

Tarayıcıya dönün ve sayfayı yenileyin. 

Özel görüntü sanal makinenin temel güncelleştirilmiş olduğunu görürsünüz.

![Ölçek kümesi, ikinci sanal makine ile ilişkili kümesidir.](media/ansible-vmss-update-image/vmss-update-browser-updated-vmss.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: myrg
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible)