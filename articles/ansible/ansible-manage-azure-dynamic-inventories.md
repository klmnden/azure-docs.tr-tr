---
title: Azure, dinamik envanterleri yönetmek için Ansible'ı kullanın
description: Azure, dinamik envanterleri yönetmek için Ansible'ı kullanmayı öğrenin
ms.service: azure
keywords: ansible'ı, azure, devops, bash, cloudshell, dinamik stok
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/09/2018
ms.topic: tutorial
ms.openlocfilehash: 0ef754b792654281f2a12b8eee613434896d5476
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60396623"
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Azure, dinamik envanterleri yönetmek için Ansible'ı kullanın
Ansible'ı (Azure gibi bulut kaynakları dahil) çeşitli kaynaklardan Envanter bilgilerini çekme için kullanılabilir içine bir *dinamik stok*. Bu makalede, kullandığınız [Azure Cloud Shell](./ansible-run-playbook-in-cloudshell.md) Ansible Azure dinamik iki sanal makine oluşturma envanterini yapılandırmak için bu sanal makinelerden birini etiketi ve Ngınx etiketli sanal makineye yükleyin.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Azure kimlik** - [oluşturma Azure kimlik bilgileri ve ansible'ı yapılandırma](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

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

## <a name="tag-a-virtual-machine"></a>Bir sanal makine etiketi
Yapabilecekleriniz [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) tarafından kullanıcı tanımlı kategoriler. 

Aşağıdakileri girin [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) sanal makineyi etiketlemek için komut `ansible-inventory-test-vm1` anahtarla `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Dinamik bir envanterini oluşturun
Sanal makinelerinizi sonra tanımlanmış (ve ekli), dinamik bir envanterini oluşturmak için zamanı. Ansible sağlar adlı Python betiğini [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) oluşturan Azure kaynaklarınızın dinamik bir envanterini için Azure Resource Manager API'si isteği yaparak. Aşağıdaki adımlar, kullanarak size yol `azure_rm.py` test Azure sanal makineler, iki bağlamak için betiği:

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

## <a name="enable-the-virtual-machine-tag"></a>Sanal makine etiketi etkinleştir
İstenen etiket ayarladıktan sonra "etiketi etkinleştirmek" gerekir. Bir etiketi yapmanın bir yolu adlı bir ortam değişkenine etiket vererek `AZURE_TAGS` aracılığıyla **dışarı** komutu:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Etiket dışarı sonra deneyebilirsiniz `ansible` komutunu tekrar:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Yalnızca bir sanal makine artık bkz (içine, etiket değeri eşleşen karşılığını dışarı **AZURE_TAGS** ortam değişkeni):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Ngınx etiketli VM'de ayarlama
Etiketleri amacı, hızlı ve kolay bir şekilde sanal makinelerinizin alt grupları ile çalışma olanağını etkinleştirmektir. Örneğin, yalnızca sanal makinelere, bir etiket atanmış Ngınx yüklemek istediğiniz varsayalım `nginx`. Aşağıdaki adımlar, ne kadar kolay gerçekleştirmek için olduğunu göstermektedir:

1. Adlı (sizin playbook içerecek şekilde) bir dosya oluşturun `nginx.yml` gibi:

   ```azurecli-interactive
   vi nginx.yml
   ```

1. Yeni oluşturulan aşağıdaki kodu ekleyin `nginx.yml` dosyası:

    ```yml
    ---
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

1. Playbook'u çalıştırdıktan sonra aşağıdaki çıktıya benzer sonuçlar görürsünüz:

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

1. Tuşuna  **&lt;Ctrl > D** klavye SSH oturumunun bağlantısının kesilip kesilmeyeceğini birleşimi.

1. Önceki gerçekleştirme adımları için `ansible-inventory-test-vm2` sanal makine (Bu, bu noktada yüklü yok anlamına gelir) Ngınx edinebileceğiniz belirten bir bilgi iletisidir verir:

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
> [Azure'da Ansible ile temel bir sanal makine oluşturma](/azure/virtual-machines/linux/ansible-create-vm)
