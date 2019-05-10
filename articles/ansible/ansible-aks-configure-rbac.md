---
title: Öğretici - Azure Kubernetes Service (AKS) Ansible kullanarak rol tabanlı erişim denetimi (RBAC) rollerini yapılandırma | Microsoft Docs
description: Azure Kubernetes Service(AKS) kümede RBAC yapılandırmak için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, aks, kubernetes, azure active directory, rbac
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: dbef7c2cb8de5a1b4bbb3073f694b8f77c9f441b
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231290"
---
# <a name="tutorial-configure-role-based-access-control-rbac-roles-in-azure-kubernetes-service-aks-using-ansible"></a>Öğretici: Azure Kubernetes Service (AKS) Ansible kullanarak rol tabanlı erişim denetimi (RBAC) rollerini yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS, kullanılacak yapılandırılabilir [Azure Active Directory (AD)](/azure/active-directory/) kullanıcı kimlik doğrulaması. Yapılandırıldıktan sonra AKS kümesine imzalamak için Azure AD kimlik doğrulama belirteci kullanın. RBAC, bir kullanıcının kimliğini veya directory grubu üyeliği dayanabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Azure AD etkin AKS kümesi oluşturma
> * Kümedeki bir RBAC rolü Yapılandır

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **RedHat OpenShift kitaplığını yükle** - `pip install openshift`

## <a name="configure-azure-ad-for-aks-authentication"></a>AKS kimlik doğrulaması için Azure AD'yi yapılandırma

AKS kimlik doğrulaması için Azure AD'yi yapılandırma, iki Azure AD uygulamaları yapılandırılır. Bu işlem, bir Azure Kiracı Yöneticisi tarafından tamamlanması gerekir. Daha fazla bilgi için [Azure Active Directory Tümleştirme ile AKS](/azure/aks/aad-integration#create-server-application). 

Azure Kiracı Yöneticisi'nden aşağıdaki değerleri alın:

- Sunucu uygulama gizli anahtarı
- Sunucu uygulama kimliği
- İstemci uygulaması kimliği 
- Kiracı Kimliği

Bu değerleri örnek playbook çalıştırmak için gereklidir.  

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Bu bölümde, bir AKS ile oluşturduğunuz [Azure AD uygulaması](#configure-azure-ad-for-aks-authentication).

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Playbook'u yükler `ssh_key` gelen `~/.ssh/id_rsa.pub`. Değiştirirseniz, tek satırlı biçimde (tırnak işaretleri olmadan) "ssh-rsa" ile başlayan - kullanın.
- `client_id` Ve `client_secret` gelen yüklenen değerler `~/.azure/credentials`, varsayılan kimlik bilgileri dosyası olduğu. Bu değerleri hizmetinize asıl ayarlayabilir veya bu değerleri ortam değişkenlerinden yükleyin:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

Aşağıdaki playbook'u `aks-create.yml` olarak kaydedin:

```yml
- name: Create resource group
  azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with RBAC enabled
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      enable_rbac: yes
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      aad_profile:
          client_app_id: "{{ client_app_id }}"
          server_app_id: "{{ server_app_id }}"
          server_app_secret: "{{ server_app_secret }}"
          tenant_id: "{{ app_tenant_id }}"
  register: aks

- name: Save cluster user config
  copy:
      content: "{{ aks.kube_config }}"
      dest: "aks-{{ name }}-kubeconfig-user"

- name: Get admin config of AKS
  azure_rm_aks_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      show_kubeconfig: admin
  register: aks

- name: Save the kubeconfig
  copy:
      content: "{{ aks.aks[0].kube_config }}"
      dest: "aks-{{ name }}-kubeconfig"
```

## <a name="get-the-azure-ad-object-id"></a>Azure AD nesnesi Kimliğini alın

Bir RBAC bağlamayı oluşturmak için ilk Azure AD nesnesi kimliğini almanız gerekir 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Sayfanın üstündeki arama alanına girin `Azure Active Directory`. 

1. Tıklatın `Enter`.

1. İçinde **Yönet** menüsünde **kullanıcılar**.

1. Ad alanında, hesabınız için arama yapın.

1. İçinde **adı** sütun, hesabınızın bağlantısını seçin.

1. İçinde **kimlik** bölümünde, kopya **nesne kimliği**.

    ![Azure AD nesnesi kimliği kopyalayın.](./media/ansible-aks-configure-rbac/ansible-aad-object-id.png)

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bu bölümde, bir rol bağlama veya küme rolü bağlaması aks'deki oluşturun. 

Aşağıdaki playbook'u `kube-role.yml` olarak kaydedin:

```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: <your-aad-account>
```

Değiştirin `&lt;your-aad-account>` yer tutucusunu Azure AD kiracınız ile [nesne kimliği](#get-the-azure-ad-object-id).

-Bu, yeni rolünüz için AKS dağıtır - aşağıdaki playbook Kaydet `aks-kube-deploy.yml`:

```yml
- name: Apply role to AKS
  k8s:
      src: kube-role.yml
      kubeconfig: "aks-{{ name }}-kubeconfig"
```

## <a name="run-the-sample-playbook"></a>Örnek playbook çalıştırın

Bu bölümde, bu makaledeki oluşturma görevleri çağıran tam örnek playbook'u listelenir. 

Aşağıdaki playbook'u `aks-rbac.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exist
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create AKS
       vars:
           client_app_id: <client id>
           server_app_id: <server id>
           server_app_secret: <server secret>
           app_tenant_id: <tenant id>
       include_tasks: aks-create.yml

     - name: Enable RBAC
       include_tasks: aks-kube-deploy.yml
```

İçinde `vars` bölümünde, Azure AD bilgilerinizi aşağıdaki yer tutucularını değiştirin:

- `<client id>`
- `<server id>`
- `<server secret>`
- `<tenant id>`

Kullanarak tam playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook aks-rbac.yml
```

## <a name="verify-the-results"></a>Sonuçları doğrulayın

Bu bölümde, bu makaledeki oluşturma düğümleri kubectl listeyi kullanın.

Bir terminal isteminde aşağıdaki komutu girin:

```bash
kubectl --kubeconfig aks-aksansibletest-kubeconfig-user get nodes
```

Komut bir kimlik doğrulaması sayfasına yönlendirir. Azure hesabınızla oturum açın.

Kimlik doğrulandıktan sonra kubectl düğümleri aşağıdaki sonuçları için benzer bir biçimde listeler:

```txt
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-33413200-0   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-1   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-2   Ready    agent   49m   v1.12.6
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      name: aksansibletest
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
      - name: Remove kubeconfig
        file:
            state: absent
            path: "aks-{{ name }}-kubeconfig"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure üzerinde Ansible](/azure/ansible/)