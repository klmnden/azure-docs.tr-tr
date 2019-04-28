---
title: Öğretici - ansible'ı kullanarak Azure DevTest Labs Laboratuvar yapılandırma | Microsoft Docs
description: Ansible'ı kullanarak Azure DevTest Labs Laboratuvar yapılandırma hakkında bilgi edinin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, devtest labs
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 04/22/2019
ms.openlocfilehash: 2b1a3f614baae7dd5dc03c2e1e5c6324ac36a22e
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767162"
---
# <a name="tutorial-configure-labs-in-azure-devtest-labs-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure DevTest Labs Laboratuvar yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[Azure DevTest Labs](/azure/lab-services/devtest-lab-overview) geliştiricilerin uygulamalarını için VM ortamları oluşturulmasını otomatikleştirin olanak tanır. Bu ortamlar için uygulama geliştirme, test ve eğitim yapılandırılabilir. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Laboratuvar oluşturma
> * Laboratuvar ilkeleri ayarlayın
> * Labs zamanlamaları ayarlayın
> * Laboratuvar sanal ağ oluşturma
> * Laboratuvar için yapıt kaynağı tanımlayın
> * Bir laboratuvar içindeki VM oluşturma
> * Laboratuvar yapıt kaynakları ve yapıları listesi
> * Yapıt kaynakları Azure Resource Manager hakkında bilgi alın
> * Laboratuvar ortamı oluşturma
> * Laboratuvar görüntüsü oluşturma
> * Laboratuvar Sil

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Örnek playbook kod parçacığında, bir Azure kaynak grubu oluşturur. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```yml
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
```

## <a name="create-the-lab"></a>Laboratuvar oluşturma

Örnek Laboratuvar sonraki görev oluşturur.

```yml
- name: Create the lab
  azure_rm_devtestlab:
    resource_group: "{{ resource_group }}"
    name: "{{ lab_name }}"
    location: "{{ location }}"
    storage_type: standard
    premium_data_disks: no
  register: output_lab
```

## <a name="set-the-lab-policies"></a>Laboratuvar ilkeleri ayarlayın

Laboratuvar ilke ayarlarını ayarlayabilirsiniz. Aşağıdaki değerleri ayarlanabilir:

- `user_owned_lab_vm_count` bir kullanıcı için sahip olabileceği VM'lerin sayısı
- `user_owned_lab_premium_vm_count` bir kullanıcı için sahip olabileceği premium VM sayısı
- `lab_vm_count` Laboratuvar Vm'leri maksimum sayısı
- `lab_premium_vm_count` Laboratuvar sayısı premium Vm'leri mi
- `lab_vm_size` izin verilen Laboratuvar Vm'leri size(s) olduğu
- `gallery_image` izin verilen galeri yansımaları olduğu
- `user_owned_lab_vm_count_in_subnet` kullanıcının VM'lerin bir alt ağda en büyük sayı
- `lab_target_cost` Laboratuvar hedef maliyeti

```yml
- name: Set the lab policies
  azure_rm_devtestlabpolicy:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    policy_set_name: myDtlPolicySet
    name: myDtlPolicy
    fact_name: user_owned_lab_vm_count
    threshold: 5
```

## <a name="set-the-lab-schedules"></a>Labs zamanlamaları ayarlayın

Bu bölümdeki örnek görev, Laboratuvar zamanlamasını yapılandırır. 

Aşağıdaki kod parçacığında `lab_vms_startup` değeri, VM'nin başlangıç süresini belirtmek için kullanılır. Benzer şekilde, ayarı `lab_vms_shutdown` Laboratuvar VM kapatma süresi değeri oluşturur.

```yml
- name: Set the lab schedule
  azure_rm_devtestlabschedule:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    name: lab_vms_shutdown
    time: "1030"
    time_zone_id: "UTC+12"
  register: output
```

## <a name="create-the-lab-virtual-network"></a>Laboratuvar sanal ağ oluşturma

Aşağıdaki bu görev varsayılan Laboratuvar sanal ağ oluşturur.

```yml
- name: Create the lab virtual network
  azure_rm_devtestlabvirtualnetwork:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    name: "{{ vn_name }}"
    location: "{{ location }}"
    description: My lab virtual network
  register: output
```

## <a name="define-an-artifact-source-for-the-lab"></a>Laboratuvar için yapıt kaynağı tanımlayın

Bir yapıt kaynağı yapıt tanımı ve Azure Resource Manager şablonlarını içeren düzgün yapılandırılmış bir GitHub deposudur. Her laboratuar önceden tanımlanmış genel yapıtlarla birlikte gelir. İzleme görevleri bir yapıt kaynağı için bir laboratuvar oluşturulacağını gösterir.

```yml
- name: Define the lab artifacts source
  azure_rm_devtestlabartifactsource:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    name: "{{ artifacts_name }}"
    uri: https://github.com/Azure/azure_preview_modules.git
    source_type: github
    folder_path: /tasks
    security_token: "{{ github_token }}"
```

## <a name="create-a-vm-within-the-lab"></a>Bir laboratuvar içindeki VM oluşturma

Bir laboratuvar içindeki bir VM oluşturun.

```yml
- name: Create a VM within the lab
  azure_rm_devtestlabvirtualmachine:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    name: "{{ vm_name }}"
    notes: Virtual machine notes, just something....
    os_type: linux
    vm_size: Standard_A2_v2
    user_name: dtladmin
    password: ZSasfovobocu$$21!
    lab_subnet:
      virtual_network_name: "{{ vn_name }}"
      name: "{{ vn_name }}Subnet"
    disallow_public_ip_address: no
    image:
      offer: UbuntuServer
      publisher: Canonical
      sku: 16.04-LTS
      os_type: Linux
      version: latest
    artifacts:
      - source_name: "{{ artifacts_name }}"
        source_path: "/Artifacts/linux-install-mongodb"
    allow_claim: no
    expiration_date: "2029-02-22T01:49:12.117974Z"
```

## <a name="list-the-labs-artifact-sources-and-artifacts"></a>Laboratuvar yapıt kaynakları ve yapıları listesi

Tüm varsayılan ve laboratuvarda özel yapıtlar kaynakları listelemek için aşağıdaki görevleri kullanın:

```yml
- name: List the artifact sources
  azure_rm_devtestlabartifactsource_facts:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
  register: output
- debug:
    var: output
```

Aşağıdaki görev tüm yapıları listeler:

```yml
- name: List the artifact facts
  azure_rm_devtestlabartifact_facts:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    artifact_source_name: public repo
  register: output
- debug:
    var: output
```

## <a name="get-azure-resource-manager-information-for-the-artifact-sources"></a>Yapıt kaynakları Azure Resource Manager hakkında bilgi alın

Tüm Azure Resource Manager şablonlarında listelemek için `public environment repository`, önceden tanımlanmış şablonları deposuyla:

```yml
- name: List the Azure Resource Manager template facts
  azure_rm_devtestlabartifactsource_facts:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
  register: output
- debug:
    var: output
```

Ve aşağıdaki görevi, belirli bir Azure Resource Manager şablon ayrıntılarını depodan alır:

```yml
- name: Get Azure Resource Manager template facts
  azure_rm_devtestlabarmtemplate_facts:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    artifact_source_name: "public environment repo"
    name: ServiceFabric-LabCluster
  register: output
- debug:
    var: output
```

## <a name="create-the-lab-environment"></a>Laboratuvar ortamı oluşturma

Aşağıdaki görev genel ortam deposundan şablonlarından birini temel laboratuvar ortamı oluşturur.

```yml
- name: Create the lab environment
      azure_rm_devtestlabenvironment:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        user_name: "@me"
        name: myEnvironment
        location: eastus
        deployment_template: "{{ output_lab.id }}/artifactSources/public environment repo/armTemplates/WebApp"
      register: output
```

## <a name="create-the-lab-image"></a>Laboratuvar görüntüsü oluşturma

Aşağıdaki görev, bir sanal makineden bir görüntü oluşturur. Görüntü, birbirinin aynısı olan Vm'leri oluşturmanıza olanak sağlar.

```yml
- name: Create the lab image
  azure_rm_devtestlabcustomimage:
    resource_group: "{{ resource_group }}"
    lab_name: "{{ lab_name }}"
    name: myImage
    source_vm: "{{ output_vm.virtualmachines[0]['name'] }}"
    linux_os_state: non_deprovisioned
```

## <a name="delete-the-lab"></a>Laboratuvar Sil

Laboratuvar silmek için aşağıdaki görevleri kullanın:

```yml
- name: Delete the lab
  azure_rm_devtestlab:
    resource_group: "{{ resource_group }}"
    name: "{{ lab_name }}"
    state: absent
  register: output
- name: Assert the change was correctly reported
  assert:
    that:
      - output.changed
```

## <a name="get-the-sample-playbook"></a>Örnek playbook Al

Tam örnek playbook almanın iki yolu vardır:
- [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/devtestlab-create.yml) ve kaydetmesi `devtestlab-create.yml`.
- Adlı yeni bir dosya oluşturun `devtestlab-create.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
---
- hosts: localhost
  #roles:
  #  - azure.azure_preview_modules
  vars:
    resource_group: "{{ resource_group_name }}"
    lab_name: myLab
    vn_name: myLabVirtualNetwork
    vm_name: myLabVm
    artifacts_name: myArtifacts
    github_token: "{{ lookup('env','GITHUB_ACCESS_TOKEN') }}"
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create the lab
      azure_rm_devtestlab:
        resource_group: "{{ resource_group }}"
        name: "{{ lab_name }}"
        location: eastus
        storage_type: standard
        premium_data_disks: no
      register: output_lab

    - name: Set the lab policies
      azure_rm_devtestlabpolicy:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        policy_set_name: myDtlPolicySet
        name: myDtlPolicy
        fact_name: user_owned_lab_vm_count
        threshold: 5

    - name: Set the lab schedule
      azure_rm_devtestlabschedule:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        name: lab_vms_shutdown
        time: "1030"
        time_zone_id: "UTC+12"
      register: output

    - name: Create the lab virtual network
      azure_rm_devtestlabvirtualnetwork:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        name: "{{ vn_name }}"
        location: eastus
        description: My lab virtual network
      register: output

    - name: Define the lab artifacts source
      azure_rm_devtestlabartifactsource:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        name: "{{ artifacts_name }}"
        uri: https://github.com/Azure/azure_preview_modules.git
        source_type: github
        folder_path: /tasks
        security_token: "{{ github_token }}"

    - name:
      set_fact:
        artifact_source:
          - source_name: "{{ artifacts_name }}"
            source_path: "/Artifacts/linux-install-mongodb"
      when: "github_token | length > 0"

    - name:
      set_fact:
        artifact_source: null
      when: "github_token | length == 0"

    - name: Create a VM within the lab
      azure_rm_devtestlabvirtualmachine:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        name: "{{ vm_name }}"
        notes: Virtual machine notes, just something....
        os_type: linux
        vm_size: Standard_A2_v2
        user_name: dtladmin
        password: ZSasfovobocu$$21!
        lab_subnet:
          virtual_network_name: "{{ vn_name }}"
          name: "{{ vn_name }}Subnet"
        disallow_public_ip_address: no
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          os_type: Linux
          version: latest
        artifacts:
          - source_name: "{{ artifacts_name }}"
            source_path: "/Artifacts/linux-install-mongodb"
        allow_claim: no
        expiration_date: "2029-02-22T01:49:12.117974Z"

    - name: List the artifact sources
      azure_rm_devtestlabartifactsource_facts:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
      register: output
    - debug:
        var: output

    - name: List the artifact facts
      azure_rm_devtestlabartifact_facts:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        artifact_source_name: public repo
      register: output
    - debug:
        var: output

    - name: List the Azure Resource Manager template facts
      azure_rm_devtestlabarmtemplate_facts:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        artifact_source_name: "public environment repo"
      register: output
    - debug:
        var: output

    - name: Get Azure Resource Manager template facts
      azure_rm_devtestlabarmtemplate_facts:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        artifact_source_name: "public environment repo"
        name: ServiceFabric-LabCluster
      register: output
    - debug:
        var: output

    - name: Create the lab environment
      azure_rm_devtestlabenvironment:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        user_name: "@me"
        name: myEnvironment
        location: eastus
        deployment_template: "{{ output_lab.id }}/artifactSources/public environment repo/armTemplates/WebApp"

    - name: Create the lab image
      azure_rm_devtestlabcustomimage:
        resource_group: "{{ resource_group }}"
        lab_name: "{{ lab_name }}"
        name: myImage
        source_vm: "{{ vm_name }}"
        linux_os_state: non_deprovisioned

    - name: Delete the lab
      azure_rm_devtestlab:
        resource_group: "{{ resource_group }}"
        name: "{{ lab_name }}"
        state: absent
```

## <a name="run-the-playbook"></a>Playbook'u Çalıştır

Bu bölümde, bu makalede gösterilen çeşitli özelliklerini test etmek için playbook çalıştırın.

Playbook'u çalıştırmadan önce aşağıdaki değişiklikleri yapın:
- İçinde `vars` bölümünde, değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.
- Adlı bir ortam değişkeni GitHub belirteci Store `GITHUB_ACCESS_TOKEN`.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook devtestlab-create.yml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
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
> [Azure üzerinde Ansible](/azure/ansible/)