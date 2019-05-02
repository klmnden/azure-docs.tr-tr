---
title: Öğretici - ansible'ı kullanarak Azure kaynaklarınızın dinamik envanterleri yapılandırma | Microsoft Docs
description: Azure, dinamik envanterleri yönetmek için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, cloudshell, dinamik stok
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: bdd78747505664c0824fffbd41a692818000193f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728779"
---
# <a name="tutorial-configure-dynamic-inventories-of-your-azure-resources-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure kaynaklarınızın dinamik envanterleri yapılandırın

Ansible'ı (Azure gibi bulut kaynakları dahil) çeşitli kaynaklardan Envanter bilgilerini çekme için kullanılabilir içine bir *dinamik stok*. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * İki test sanal makineleri yapılandırın. 
> * Sanal makinelerden birini etiketi
> * Etiketli sanal makinelere Ngınx yükleyin
> * Yapılandırılmış Azure kaynakları içeren dinamik bir envanterini yapılandırma

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-test-vms"></a>Test sanal makineleri oluşturma

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)'i açın.

1. Bu öğretici için sanal makinelerin tutmak için bir Azure kaynak grubu oluşturun.

    > [!IMPORTANT]  
    > Bu adımda oluşturduğunuz Azure kaynak grubu adı tamamen küçük harf olması gerekir. Aksi takdirde dinamik stok nesil başarısız olur.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Aşağıdaki tekniklerden birini kullanarak Azure'da Linux sanal makineleri iki oluşturun:

    - **Ansible playbook** -makale [azure'da Ansible ile temel bir sanal makine oluşturma](/azure/virtual-machines/linux/ansible-create-vm) bir Ansible playbook bir sanal makinenin nasıl oluşturulduğunu gösterir. Birini veya ikisini de sanal makineleri tanımlamak için bir playbook kullanıyorsanız, SSH bağlantısını parola yerine kullanılmasını sağlamak.

    - **Azure CLI** -sorun aşağıdaki komutlarını Cloud Shell'de iki sanal makine oluşturmak için:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-vm"></a>Bir VM’yi etiketleme

Yapabilecekleriniz [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) tarafından kullanıcı tanımlı kategoriler. 

Aşağıdakileri girin [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) sanal makineyi etiketlemek için komut `ansible-inventory-test-vm1` anahtarla `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```
## <a name="generate-a-dynamic-inventory"></a>Dinamik bir envanterini oluşturun

Sanal makinelerinizi sonra tanımlanmış (ve ekli), dinamik bir envanterini oluşturmak için zamanı.

### <a name="using-ansible-version--28"></a>Ansible'ı kullanarak sürüm < 2.8

Ansible sağlar adlı Python betiğini [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) , Azure kaynaklarınızın dinamik bir envanterini oluşturur. Aşağıdaki adımlar, kullanarak size yol `azure_rm.py` test Azure sanal makineler, iki bağlamak için betiği:

1. GNU kullanın `wget` almak için komut `azure_rm.py` betiği:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Kullanım `chmod` erişim izinleri değiştirmek için komut `azure_rm.py` betiği. Aşağıdaki komutu kullanır `+x` (çalışan) için belirtilen dosyanın yürütmeye olanak tanımak için parametre (`azure_rm.py`):

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Kullanım [ansible komut](https://docs.ansible.com/ansible/2.4/ansible.html) , kaynak grubunuza bağlanmak için: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Bağlantı kurulduktan sonra aşağıdaki çıktıya benzer sonuçlar görürsünüz:

    ```Output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

### <a name="ansible-version--28"></a>Ansible sürüm > = 2.8

Ansible 2.8 ile başlayarak, Ansible sağlar bir [Azure dinamik stok eklentisi](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/azure_rm.py). Aşağıdaki adımlarda eklentiyi kullanarak, yol:

1. Stok eklenti, bir yapılandırma dosyası gerektirir. Yapılandırma dosyasının sonunda `azure_rm` ve herhangi bir uzantısına sahip `yml` veya `yaml`. Bu öğretici örnek için aşağıdaki playbook olarak Kaydet `myazure_rm.yml`:

    ```yml
    plugin: azure_rm
    include_vm_resource_groups:
    - ansible-inventory-test-rg
    auth_source: auto
    ```

1. Kaynak grubunda sanal makineleri ping işlemi yapmak için aşağıdaki komutu çalıştırın:

    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

1. Yukarıdaki komutu çalıştırırken şu hatayı alabilirsiniz:

    ```Output
    Failed to connect to the host via ssh: Host key verification failed.
    ```
    
    "Ana bilgisayar anahtarı doğrulama" hatasını alırsanız Ansible yapılandırma dosyasına aşağıdaki satırı ekleyin. Ansible'ı yapılandırma dosyası şu konumdadır `/etc/ansible/ansible.cfg`.

    ```bash
    host_key_checking = False
    ```

1. Playbook'u çalıştırdığınızda, aşağıdaki çıktıya benzer sonuçlar görürsünüz:
  
    ```Output
    ansible-inventory-test-vm1_0324 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible-inventory-test-vm2_8971 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ```

## <a name="enable-the-vm-tag"></a>VM etiketi etkinleştir
Bir etiketi ayarladıktan sonra "Bu etiketi etkinleştirmek" gerekir. Bir etiket yapmanın bir yolu olan bir ortam değişkenine etiket vererek `AZURE_TAGS` aracılığıyla `export` komutu:

```azurecli-interactive
export AZURE_TAGS=nginx
```

- Ansible'ı kullanıyorsanız, < 2.8, aşağıdaki komutu çalıştırın:

    ```bash
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping
    ```

- Ansible'ı kullanıyorsanız, > = 2.8, aşağıdaki komutu çalıştırın:
  
    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

Yalnızca bir sanal makine artık bkz (içine, etiket değeri eşleşen karşılığını dışarı `AZURE_TAGS` ortam değişkeni):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Ngınx etiketli VM'de ayarlama

Etiketleri amacı, hızlı ve kolay bir şekilde sanal makinelerinizin alt grupları ile çalışma olanağını etkinleştirmektir. Örneğin, yalnızca sanal makinelere, bir etiket atanmış Ngınx yüklemek istediğiniz varsayalım `nginx`. Aşağıdaki adımlar, ne kadar kolay gerçekleştirmek için olduğunu göstermektedir:

1. Adlı bir dosya oluşturun `nginx.yml`:

   ```azurecli-interactive
   code nginx.yml
   ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yml
    ---
    - name: Install and start Nginx on an Azure virtual machine
      hosts: all
      become: yes
      tasks:
      - name: install nginx
        apt: pkg=nginx state=installed
        notify:
        - start nginx

      handlers:
        - name: start nginx
          service: name=nginx state=started
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Kullanarak playbook çalıştırma `ansible-playbook` komutu:

   - Ansible < 2.8:

    ```bash
    ansible-playbook -i azure_rm.py nginx.yml
    ```

   - Ansible > = 2.8:

    ```bash
     ansible-playbook  -i ./myazure_rm.yml  nginx.yml
    ```

1. Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] 

    TASK [Gathering Facts] 
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] 
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] 
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP 
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Ngınx yüklemesini test edin

Bu bölümde, Ngınx sanal makinenizde yüklü olduğunu test etmek için bir yöntemi gösterir.

1. Kullanım [az vm-IP-adreslerini](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) IP adresini almak için komut `ansible-inventory-test-vm1` sanal makine. Döndürülen değerin (sanal makinenin IP adresi) sonra SSH komutunu parametre olarak sanal makineye bağlanmak için kullanılır.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Bağlıyken `ansible-inventory-test-vm1` sanal makineyi çalıştırın, [nginx - v](https://nginx.org/en/docs/switches.html) Ngınx yüklenmiş olup olmadığını belirlemek için komutu.

    ```azurecli-interactive
    nginx -v
    ```

1. Bir kez çalıştırdığınız `nginx -v` komut Nginx sürümü görürsünüz (ikinci satırı) Ngınx'in yüklü olduğunu gösterir.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)

    tom@ansible-inventory-test-vm1:~$
    ```

1. Tıklayın `<Ctrl>D` klavye SSH oturumunun bağlantısının kesilip kesilmeyeceğini birleşimi.

1. Önceki yapılması adımları için `ansible-inventory-test-vm2` sanal makine (Bu, bu noktada yüklü yok anlamına gelir) Ngınx edinebileceğiniz belirten bir bilgi iletisidir verir:

    ```Output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı Başlangıç: Ansible'ı kullanarak Azure'da Linux sanal makineleri yapılandırma](/azure/virtual-machines/linux/ansible-create-vm)