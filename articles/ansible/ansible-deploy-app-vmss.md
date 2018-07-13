---
title: Ansible'ı kullanarak azure'da sanal makine ölçek kümeleri uygulamalarını dağıtma
description: Bir sanal makine ölçek kümesi yapılandırmak ve uygulamayı Azure'da sanal makine ölçek dağıtmak için Ansible'ı kullanmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: b9c8058606e13c0db4908530e98cddb69d2caf50
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008856"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Ansible'ı kullanarak azure'da sanal makine ölçek kümeleri uygulamalarını dağıtma
Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Uygulamalarınızı Azure'da dağıtmak için Ansible'ı kullanabilirsiniz. Bu makalede bir Azure sanal makine ölçek kümesine (VMSS) bir Java uygulamasının nasıl dağıtılacağı gösterilmektedir.  

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - oluşturma, bir Azure aboneliği yoksa, bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.
- **Ansible'ı yapılandırma** - [oluşturma Azure kimlik bilgileri ve ansible'ı yapılandırma](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible'ı ve Azure Python SDK'sı modüller** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Sanal makine ölçek kümesi** - sanal makine ölçek yoksa, küme, şunları yapabilirsiniz [sanal makine ölçek kümesi ile Ansible oluşturma](ansible-create-configure-vmss.md). 
- **Git** - [git](https://git-scm.com) Bu öğreticide kullanılan bir Java örneği indirmek için kullanılır.
- **Java SE Development Kit (JDK)** -JDK Java örnek proje oluşturmak için kullanılır.
- **Apache Maven derleme araçlarını** - [Apache Maven derleme araçlarını](https://maven.apache.org/download.cgi) örnek Java projesi oluşturmak için kullanılır.

> [!Note]
> Ansible 2.6 aşağıdaki çalıştırılması için gerekli Bu öğreticide örnek playbook'ları. 

## <a name="get-host-information"></a>Konak bilgilerini alma

Bu bölümde, bir Azure sanal makine grubu için konak bilgilerini almak için ansible'ı kullanmayı gösterir. Bir örnek Ansible playbook aşağıda verilmiştir. Genel IP adreslerini kodunu alır ve yük dengeleyici içinde belirtilen kaynak grubu ve adlı bir konak grubu oluşturur **saclesethosts** stoktaki. 

Aşağıdaki örnek playbook olarak Kaydet `get-hosts-tasks.yml`: 

  ```yaml
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

## <a name="prepare-an-application-for-deployment"></a>Bir uygulama dağıtımı için hazırlama  

Bu bölümde, bir Java örnek projesini github'dan kopyalama ve projeyi oluşturmak için git kullanın. Aşağıdaki playbook olarak Kaydet `app.yml`:

  ```yaml
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

Örnek Ansible playbook ile aşağıdaki komutu çalıştırın:

  ```bash
  ansible-playbook app.yml
  ```

Ansible playbook komutunun çıktısını görüntüler kullanıma gördüğünüz, kopyalanan örnek uygulamasını Github'dan oluşturulmuş aşağıdakine benzer:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Git Clone sample app] ***************************************************************************
  changed: [localhost]

  TASK [Build sample app] ***************************************************
  changed: [localhost]

  PLAY RECAP ***************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-vmss"></a>VMSS uygulamayı dağıtma

Aşağıdaki bölümde bir Ansible playbook JRE (Java Çalışma zamanı ortamı) adlı bir konak grubunda yükler **saclesethosts**ve Java uygulama adlı bir konak grubuna dağıtır **saclesethosts**: 

(Değişiklik `admin_password` kendi parola.)

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      scaleset_name: myVMSS
      loadbalancer_name: myVMSSlb
      admin_username: azureuser
      admin_password: "your_password"
    tasks:   
    - include: get-hosts-tasks.yml

  - name: Install JRE on VMSS
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

Önceki örnek Ansible playbook olarak kaydedebilirsiniz `vmss-setup-deploy.yml`, veya [tüm örnek playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss). 

Kullanılacak ssh bağlantı türü ile Parolaları, sshpass program yüklemeniz gerekir. 
  - Ubunto 16.04 için komutu çalıştırmak `apt-get install sshpass`.
  - CentOS 7.4 için komutu çalıştırmak `yum install sshpass`.

Gibi bir hata görebilirsiniz **SSH parolası yerine bir anahtar kullanarak mümkün değildir çünkü ana bilgisayar anahtarı denetimi etkinse ve sshpass bu desteklemez.  Bu konağın parmak izi bu konak yönetmek için known_hosts dosyanıza ekleyin.** Bu hatayı görürseniz, ana bilgisayar anahtarı ya da aşağıdaki satırı ekleyerek denetimi devre dışı bırakabilirsiniz `/etc/ansible/ansible.cfg` dosya veya `~/.ansible.cfg` dosyası:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Playbook'u ile aşağıdaki komutu çalıştırın:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

Örnek Java uygulamasından, sanal makine ölçek kümesinin konak grubuna yüklenmiş olan ansible playbook komut çalışmasını çıktıyı gösterir:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups] **********************************************
  ok: [localhost]

  TASK [Get loadbalancer info] ****************************************************************************
  ok: [localhost]

  TASK [Add all hosts] *****************************************************************************
  changed: [localhost] ...

  PLAY [Install JRE on VMSS] *****************************************************************************

  TASK [Gathering Facts] *****************************************************************************
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM] *****************************************************************************
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application] ********************************************************************
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP ************************************************************************************************
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

Congratulation! Uygulamanız artık Azure'da çalışıyor. Artık, sanal makine ölçek kümesi için yük dengeleyicinin URL'sine gezinebilirsiniz:

![Azure'da bir sanal makine ölçek kümesinde çalıştırılan Java uygulaması ayarlayın.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [VMSS için örnek playbook Ansible](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)