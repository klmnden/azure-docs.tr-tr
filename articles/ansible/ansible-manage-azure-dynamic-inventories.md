---
title: Azure, dinamik envanterleri yönetmek için Ansible kullanın
description: Azure, dinamik envanterleri yönetmek için Ansible kullanmayı öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, dinamik stok
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: f29f4ec64b79738cae2ad684610f4817739825a9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32153118"
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Azure, dinamik envanterleri yönetmek için Ansible kullanın
Ansible (bulut kaynakları Azure gibi dahil) çeşitli kaynaklardan Envanter bilgilerine pull için kullanılabilir içine bir *dinamik stok*. Bu makalede, kullandığınız [Azure bulut Kabuk](./ansible-run-playbook-in-cloudshell.md) Ansible Azure dinamik iki sanal makine oluşturma envanterini yapılandırmak için bu sanal makineleri birini etiketi ve Nginx etiketli sanal makineye yükleyin.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - oluşturmak bir Azure aboneliğiniz yoksa bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

- **Azure kimlik** - [oluşturma Azure kimlik bilgilerini ve Ansible yapılandırın](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>Test sanal makineler oluşturma

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Açık [bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Bu öğretici için sanal makineleri tutmak için bir Azure kaynak grubu oluşturun.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Aşağıdaki teknikler birini kullanarak Azure'da iki Linux sanal makine oluşturun:

    - **Ansible playbook** -makale [Ansible ile azure'da temel bir sanal makine oluşturmak](/azure/virtual-machines/linux/ansible-create-vm) nasıl Ansible playbook bir sanal makine oluşturulacağı gösterilmektedir. Birini veya her ikisini sanal makineleri tanımlamak için bir playbook kullanırsanız, SSH bağlantısını parola yerine kullanıldığından emin olun.

    - **Azure CLI** -sorun her birini komutları bulut Kabuğu'nda iki sanal makine oluşturmak için:

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

## <a name="tag-a-virtual-machine"></a>Bir sanal makine etiketi
Yapabilecekleriniz [Azure kaynaklarınızı düzenleme için etiketler kullanın](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) tarafından kullanıcı tanımlı kategoriler. 

Aşağıdaki girin [az kaynak etiketi](/cli/azure/resource?view=azure-cli-latest.md#az_resource_tag) sanal makine etiketlemek için komut `ansible-inventory-test-vm1` anahtarla `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Dinamik stok oluştur
Sanal makinelerinizi olduktan sonra tanımlı (ve etiketli), dinamik stok oluşturmak için zamanı. Ansible adlı bir Python komut dosyası sağlar [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) oluşturan dinamik stok Azure kaynaklarınızın API istekleri için Azure Resource Manager yaparak. Aşağıdaki adımlar kullanılarak yol `azure_rm.py` için iki bağlanmak için komut dosyası Azure sanal makineleri test edin:

1. GNU kullanmak `wget` almak için komutu `azure_rm.py` komut dosyası:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Kullanım `chmod` için erişim izinlerini değiştirmek için komut `azure_rm.py` komut dosyası. Aşağıdaki komut kullanır `+x` (çalışan) için belirtilen dosyasının yürütülmesine izin vermek için parametre (`azure_rm.py`):

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Kullanım [ansible komutu](https://docs.ansible.com/ansible/2.4/ansible.html) , kaynak grubuna bağlanmak için: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Bağlantı kurulduktan sonra aşağıdaki çıkış benzer sonuçlar bakın:

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

## <a name="enable-the-virtual-machine-tag"></a>Sanal makine etiketi etkinleştir
İstenen etiket ayarladıktan sonra "etiketi etkinleştirmek" gerekir. Bir etiketi yapmanın bir yolu adlı bir ortam değişkeni etiketi vererek `AZURE_TAGS` aracılığıyla **verme** komutu:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Etiket dışarı sonra deneyebilirsiniz `ansible` komutunu yeniden:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Yalnızca bir sanal makine artık bakın (içine, etiket değeri ile eşleşen bir dışarı **AZURE_TAGS** ortam değişkeni):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Etiketli VM üzerinde Nginx ayarlama
Etiketler amacı, hızlı ve kolay bir şekilde sanal makinelerinizi alt grupları ile çalışma olanağını etkinleştirmektir. Örneğin, yalnızca sanal makinelerin bir etiket atanan Nginx yüklemek istediğinizi düşünelim `nginx`. Aşağıdaki adımlar, ne kadar kolay gerçekleştirmek için olan gösterilmiştir:

1. Adlı (sizin playbook içerecek şekilde) bir dosya oluşturun `nginx.yml` gibi:

  ```azurecli-interactive
  vi nginx.yml
  ```

1. Yeni oluşturulan aşağıdaki kodu ekleyin `nginx.yml` dosyası:

    ```yml
    - name: Install and start Nginx on an Azure virtual machine
    hosts: azure
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

1. Çalıştırma `nginx.yml` playbook:

  ```azurecli-interactive
  ansible-playbook -i azure_rm.py nginx.yml
  ```

1. Playbook çalıştırdıktan sonra aşağıdaki çıkış benzer sonuçlar görürsünüz:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] **********

    TASK [Gathering Facts] **********
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] **********
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] **********
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP **********
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Test Nginx yükleme
Bu bölümde Nginx sanal makinenizde yüklendiğini test etmek için bir yöntemi gösterir.

1. Kullanım [az vm listesi-ip-addresses](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az_vm_list_ip_addresses) IP adresini almak için komutu `ansible-inventory-test-vm1` sanal makine. Döndürülen değeri (sanal makinenin IP adresi) sonra SSH komutu parametre olarak sanal makineye bağlanmak için kullanılır.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. [Nginx - v](https://nginx.org/en/docs/switches.html) komutu Nginx sürüm yazdırmak için genellikle kullanılır. Ancak, bu da Nginx yüklü olup olmadığını belirlemek için kullanılabilir. Bağlıyken girin `ansible-inventory-test-vm1` sanal makine.

    ```azurecli-interactive
    nginx -v
    ```

1. Çalıştırdığınız sonra `nginx -v` komutu, Nginx sürüm bakın (ikinci satır) belirten Nginx yüklenir.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Tuşuna  **&lt;Ctrl > D** klavye SSH oturumun bağlantısını kesmek için birleşimi.

1. Yukarıdaki gerçekleştirme adımları için `ansible-inventory-test-vm2` sanal makine (hangi bu noktada yüklü yok anlamına gelir) Nginx alabileceğiniz belirten bir bilgilendirme iletisi üretir:

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
> [Azure Ansible ile temel bir sanal makine oluşturun](/azure/virtual-machines/linux/ansible-create-vm)
