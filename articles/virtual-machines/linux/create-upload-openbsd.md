---
title: Oluşturun ve Azure'a OpenBSD VM görüntüyü karşıya | Microsoft Docs
description: Oluşturun ve sanal sabit bir Azure sanal makinesini Azure CLI aracılığıyla oluşturmak için OpenBSD işletim sistemi içeren bir disk (VHD) karşıya yükleme hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: thomas1206
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: huishao
ms.openlocfilehash: 23ca965b12afc8931a67d32e3a11a4aa9777a5d5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a>Oluşturun ve Azure'a bir OpenBSD disk görüntüsü yükleyin
Bu makalede nasıl oluşturulacağı ve bir sanal sabit OpenBSD işletim sistemini içeren disk (VHD) yüklemek gösterilmektedir. Karşıya yüklediğiniz sonra bunu kendi görüntünüzü Azure CLI aracılığıyla azure'da sanal makine (VM) oluşturmak için kullanabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşağıdaki öğelerin bulunduğunu varsayar:

* **Bir Azure aboneliği** -bir hesabınız yoksa yalnızca birkaç dakika içinde bir oluşturabilirsiniz. Bir MSDN aboneliğiniz varsa, bkz: [Visual Studio aboneleri için aylık Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Aksi takdirde, bilgi nasıl [ücretsiz bir deneme hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure CLI 2.0** -son olduğundan emin olun [Azure CLI 2.0](/cli/azure/install-azure-cli) yüklü ve Azure hesabınızla oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).
* **Bir .vhd dosyası yüklü OpenBSD işletim sistemi** -bir sanal sabit disk için desteklenen bir OpenBSD işletim sistemine (6.1 sürümü) yüklenmesi gerekir. Birden çok araç, .vhd dosyaları oluşturmak için mevcut. Örneğin, .vhd dosyası oluşturun ve işletim sistemini yüklemek için Hyper-V gibi bir sanallaştırma çözümü kullanabilirsiniz. Yükleme ve Hyper-V kullanma hakkında yönergeler için bkz: [Hyper-V yükleyin ve sanal makine oluşturma](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Azure için OpenBSD görüntüsünü hazırlama
OpenBSD işletim Hyper-V eklenen sistem 6.1, yüklediğiniz VM'de desteği, aşağıdaki yordamları tamamlayın:

1. DHCP yükleme sırasında etkin değilse, hizmeti aşağıdaki gibi etkinleştirin:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Bir seri Konsolu aşağıdaki gibi ayarlayın:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Paket yükleme işlemi aşağıdaki gibi yapılandırın:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Varsayılan olarak, `root` kullanıcı azure'daki sanal makinelerde devre dışıdır. Kullanıcılar çalışma komutları yükseltilmiş ayrıcalıklarla kullanarak `doas` OpenBSD VM'de komutu. Doas varsayılan olarak etkindir. Daha fazla bilgi için bkz: [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Yükleyin ve Azure Aracısı önkoşulları aşağıdaki gibi yapılandırın:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. En son sürümü Azure Aracısı'nın her zaman bulunabilir [Github](https://github.com/Azure/WALinuxAgent/releases). Aracıyı aşağıdaki gibi yükleyin:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Azure aracısını yükledikten sonra şu şekilde çalıştığını doğrulamak için iyi bir fikirdir:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Temizlemeyi ve sağlama işleminin için uygun hale sisteme sağlamayı sonlandırın. Aşağıdaki komut, son sağlanan kullanıcı hesabını ve ilişkili veriler de siler:

    ```sh
    waagent -deprovision+user -force
    ```

Şimdi, VM kapatma.


## <a name="prepare-the-vhd"></a>VHD hazırlama
VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**. Diski Hyper-V Yöneticisi'ni veya Powershell kullanarak sabit VHD biçimine dönüştürebilirsiniz [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet'i. Örnek olarak verilebilir aşağıdaki.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Depolama kaynaklarını oluşturma ve yükleme
Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

VHD yüklemek için bir depolama hesabıyla oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). Depolama hesabı adları benzersiz olması gerekir, böylece kendi adını belirtin. Aşağıdaki örnek adlı bir depolama hesabı oluşturur *mystorageaccount*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

Depolama hesabına erişimi denetlemek için depolama anahtarla elde [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list) gibi:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

Karşıya yüklediğiniz VHD'ler mantıksal olarak ayırmak için depolama hesabıyla içinde bir kapsayıcı oluşturmak [az depolama kapsayıcısı oluşturmak](/cli/azure/storage/container#az_storage_container_create):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Son olarak, VHD ile karşıya [az depolama blob karşıya yükleme](/cli/azure/storage/blob#az_storage_blob_upload) gibi:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>VM, VHD'den oluştur
Bir VM ile oluşturabileceğiniz bir [örnek komut dosyası](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) veya doğrudan [az vm oluşturma](/cli/azure/vm#az_vm_create). Karşıya yüklediğiniz OpenBSD VHD belirtmek için kullanın `--image` şekilde parametre:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

OpenBSD VM ile için IP adresini elde [az vm listesi-ip-addresses](/cli/azure/vm#list-ip-addresses) gibi:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Şimdi, OpenBSD VM normal olarak SSH yapabilirsiniz:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Sonraki adımlar
OpenBSD6.1 üzerinde Hyper-V desteği hakkında daha fazla bilgi edinmek istiyorsanız, okuma [OpenBSD 6.1](https://www.openbsd.org/61.html) ve [hyperv.4](http://man.openbsd.org/hyperv.4).

Yönetilen diskten bir VM oluşturmak istiyorsanız, okuma [az disk](/cli/azure/disk). 
