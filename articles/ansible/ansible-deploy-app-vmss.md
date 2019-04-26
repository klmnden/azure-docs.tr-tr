---
title: Ansible kullanarak Azure’da sanal makine ölçek kümelerine uygulama dağıtma
description: Azure'da bir sanal makine ölçek kümesi yapılandırmak ve sanal makine ölçek kümesine uygulama dağıtmak için Ansible'ı nasıl kullanacağınızı öğrenin
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/11/2018
ms.openlocfilehash: 2214dd9505dff86ac26f01967a360140dee0069f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60396890"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Ansible kullanarak Azure’da sanal makine ölçek kümelerine uygulama dağıtma
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Uygulamalarınızı Azure'a dağıtmak için Ansible kullanabilirsiniz. Bu makalede bir Azure sanal makine ölçek kümesine (VMSS) bir Java uygulamasının nasıl dağıtılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Sanal makine ölçek kümesi** - Sanal makine ölçek kümeniz yoksa, [Ansible kullanarak bir sanal makine ölçek kümesi oluşturabilirsiniz](ansible-create-configure-vmss.md).
- **Git** - Bu öğreticide Java örneği indirmek için [git](https://git-scm.com) kullanılmaktadır.
- **Java SE Development Kit (JDK)** - Örnek Java projesini derlemek için [JDK](https://aka.ms/azure-jdks) kullanılır.
- **Apache Maven derleme araçları** - Örnek Java projesini derlemek için [Apache Maven derleme araçları](https://maven.apache.org/download.cgi) kullanılır.

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.6 gerekir.

## <a name="get-host-information"></a>Ana bilgisayar bilgilerini alma

Bu bölümde, bir Azure sanal makine grubu için ana bilgisayar bilgilerini almak üzere Ansible'ın nasıl kullanılacağı gösterilmektedir. Aşağıda bir Ansible playbook örneği verilmiştir. Kod, genel IP adreslerini ve yük dengeleyiciyi belirtilen kaynak grubuna alır ve envanterde **scalesethosts** adlı bir konak grubu oluşturur.

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

Bu bölümde, git kullanarak GitHub'dan bir Java örnek projesi kopyalayacak ve projeyi derleyeceksiniz. Aşağıdaki playbook'u `app.yml` olarak kaydedin:

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

Ansible-playbook komutunun çıktısı aşağıdakine benzer şekilde görüntülenir, burada GitHub'dan kopyalanan örnek uygulamanın derlendiğini görebilirsiniz:

  ```Output
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

## <a name="deploy-the-application-to-vmss"></a>Uygulamayı VMSS'e dağıtma

Ansible playbook'un aşağıdaki bölümü JRE'yi (Java Runtime Environment) **saclesethosts** adlı bir ana bilgisayar grubuna yükler ve Java uygulamasını **saclesethosts** adlı bir ana bilgisayar grubuna dağıtır:

(`admin_password` öğesini kendi parolanız ile değiştirin.)

  ```yml
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

Önceki örnek Ansible playbook'u `vmss-setup-deploy.yml` olarak kaydedebilir veya [örnek playbook'un tamamını indirebilirsiniz](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss).

Ssh bağlantı türünü parola ile kullanmak için sshpass programını yüklemeniz gerekir.
  - Ubuntu 16.04 için komutu çalıştırmak `apt-get install sshpass`.
  - CentOS 7.4 için `yum install sshpass` komutunu çalıştırın.

Şuna benzer bir hata mesajı görebilirsiniz: **Using an SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this. Add this host's fingerprint to your known_hosts file to manage this host.** (Ana Bilgisayar Anahtarı kontrolü etkin olduğu ve sshpass bunu desteklemediği için anahtar yerine SSH parolası kullanmak mümkün değil. Bu ana bilgisayarı yönetmek için bu ana bilgisayarın parmak izini known_hosts dosyanıza ekleyin.) Bu hatayı görürseniz, aşağıdaki satırı `/etc/ansible/ansible.cfg` veya `~/.ansible.cfg` dosyasına ekleyerek ana bilgisayar anahtarı kontrolünü devre dışı bırakabilirsiniz:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Playbook'u aşağıdaki komut ile çalıştırın:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

Ansible-playbook komutunun çalıştırılması sonucu oluşan çıktı, örnek Java uygulamasının sanal makine ölçek kümesinin ana bilgisayar grubuna yüklendiğini belirtir:

  ```Output
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

Tebrikler! Uygulamanız artık Azure'da çalışıyor. Artık sanal makine ölçek kümenizin yük dengeleyicisinin URL'sine gidebilirsiniz:

![Azure'da bir sanal makine ölçek kümesinde çalıştırılan Java uygulaması.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Ansible'ı kullanarak bir sanal makine ölçek kümesi otomatik olarak ölçeklendirme](https://docs.microsoft.com/azure/ansible/ansible-auto-scale-vmss)
