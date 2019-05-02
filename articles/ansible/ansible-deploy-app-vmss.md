---
title: Öğretici - ansible'ı kullanarak azure'da sanal makine ölçek kümeleri için uygulamaları dağıtma | Microsoft Docs
description: Azure sanal makine ölçek kümeleri yapılandırmak ve ölçek kümesinde bir uygulamayı dağıtmak için Ansible'ı kullanmayı öğrenin
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 3c45a0bc5bbabeb6f4511140f83b08fd2943127c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694942"
---
# <a name="tutorial-deploy-apps-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Öğretici: Ansible'ı kullanarak azure'da sanal makine ölçek kümeleri için uygulama dağıtma

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Azure Vm'leri bir grup için ana bilgisayar bilgilerini alma
> * Kopyalamak ve örnek uygulaması derleme
> * Bir ölçek kümesinde ' % s'JRE (Java Çalışma zamanı ortamı) yükleyin
> * Bir ölçek kümesine Java uygulaması dağıtma

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- [!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]
- **Git** - Bu öğreticide Java örneği indirmek için [git](https://git-scm.com) kullanılmaktadır.
- **Java SE Development Kit (JDK)** - Örnek Java projesini derlemek için [JDK](https://aka.ms/azure-jdks) kullanılır.
- **Apache Maven** - [Apache Maven](https://maven.apache.org/download.cgi) örnek Java projesi oluşturmak için kullanılır.

## <a name="get-host-information"></a>Ana bilgisayar bilgilerini alma

Bu bölümdeki playbook kodu, bir sanal makine grubu için ana bilgisayar bilgilerini alır. Kod, genel IP adreslerini alır ve yük dengeleyici belirtilen kaynak grubunda ve adlı bir konak grubu oluşturur `scalesethosts` stoktaki.

Aşağıdaki örnek playbook'u `get-hosts-tasks.yml` olarak kaydedin:

  ```yml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_facts.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Uygulamaları dağıtım için hazırlama

Bu bölümdeki playbook kod `git` Java örnek projesini github'dan kopyalama için ve projeyi oluşturur. 

Aşağıdaki playbook'u `app.yml` olarak kaydedin:

  ```yml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks:
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Örnek Ansible playbook'u aşağıdaki komut ile çalıştırın:

  ```bash
  ansible-playbook app.yml
  ```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

  ```Output
  PLAY [localhost] 

  TASK [Gathering Facts] 
  ok: [localhost]

  TASK [Git Clone sample app] 
  changed: [localhost]

  TASK [Build sample app] 
  changed: [localhost]

  PLAY RECAP 
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-a-scale-set"></a>Bir ölçek kümesine uygulama dağıtma

Bu bölümdeki playbook kod için kullanılır:

* Adlı bir konak grubunda JRE yükleyin `saclesethosts`
* Adlı bir konak grubu için Java uygulaması dağıtma `saclesethosts`

Örnek playbook almanın iki yolu vardır:

* [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-setup-deploy.yml) ve kaydetmesi `vmss-setup-deploy.yml`.
* Adlı yeni bir dosya oluşturun `vmss-setup-deploy.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    scaleset_name: myScaleSet
    loadbalancer_name: myScaleSetLb
    admin_username: azureuser
    admin_password: "{{ admin_password }}"
  tasks:
  - include: get-hosts-tasks.yml

- name: Install JRE on a scale set
  hosts: scalesethosts
  become: yes
  vars:
    workspace: ~/src/helloworld
    admin_username: azureuser

  tasks:
  - name: Install JRE
    apt:
      name: default-jre
      update_cache: yes

  - name: Copy app to Azure VM
    copy:
      src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
      dest: "/home/{{ admin_username }}/helloworld.jar"
      force: yes
      mode: 0755

  - name: Start the application
    shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
    async: 5000
    poll: 0
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* İçinde `vars` bölümünde, değiştirin `{{ admin_password }}` yer tutucusunu kendi parolanızı ile.
* Kullanılacak ssh parolaları bağlantı türüyle sshpass program yükle:

    Ubuntu:

    ```bash
    apt-get install sshpass
    ```

    CentOS:

    ```bash
    yum install sshpass
    ```

* Bazı ortamlarda bir SSH parolası yerine bir anahtar kullanma hakkında bir hata görebilirsiniz. Bu hatayı alırsanız, ana bilgisayar anahtarı aşağıdaki satırı ekleyerek denetimi devre dışı bırakabilirsiniz `/etc/ansible/ansible.cfg` veya `~/.ansible.cfg`:

    ```bash
    [defaults]
    host_key_checking = False
    ```

Playbook'u aşağıdaki komut ile çalıştırın:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

Örnek Java uygulamasından ölçek kümesinin konak grubuna yüklendiğini ansible playbook komut çalışmasını çıktıyı gösterir:

  ```Output
  PLAY [localhost]

  TASK [Gathering Facts]
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups]
  ok: [localhost]

  TASK [Get loadbalancer info]
  ok: [localhost]

  TASK [Add all hosts]
  changed: [localhost] ...

  PLAY [Install JRE on scale set]

  TASK [Gathering Facts]
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM]
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application]
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

## <a name="verify-the-results"></a>Sonuçları doğrulayın

İş sonuçlarını ölçek kümeniz için yük dengeleyicinin URL'sine giderek doğrulayın:

![Azure'da bir ölçek kümesinde çalıştırılan Java uygulaması ayarlayın.](media/ansible-vmss-deploy/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Otomatik ölçeklendirme sanal makine ölçek kümeleri Azure'da ansible'ı kullanma](./ansible-auto-scale-vmss.md)