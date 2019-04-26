---
title: Oluşturma ve Azure'a OpenBSD VM görüntüsünü karşıya yükleme | Microsoft Docs
description: Oluşturma ve bir sanal sabit içeren bir Azure sanal makinesini Azure CLI aracılığıyla oluşturmak için OpenBSD işletim sistemi diski (VHD) yükleme hakkında bilgi edinin
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
ms.openlocfilehash: 2e580a94e568f201587c06efa827006386cd6bd9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60327689"
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a>Oluşturma ve Azure'a OpenBSD disk görüntü yükleme
Bu makalede oluşturma ve sanal sabit OpenBSD işletim sistemi içeren bir disk (VHD) yükleme gösterilmektedir. Karşıya yüklediğiniz sonra bunu kendi görüntünüzü Azure CLI ile azure'daki bir sanal makine (VM) oluşturmak için kullanabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşağıdaki öğelerin bulunduğunu varsayar:

* **Bir Azure aboneliğine** -bir hesabınız yoksa, yalnızca birkaç dakika içinde bir tane oluşturabilirsiniz. Bir MSDN aboneliğine sahip değilse [Visual Studio aboneleri için aylık Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Aksi takdirde, bilgi nasıl [ücretsiz bir deneme hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure CLI** -en son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-azure-cli) yüklü ve Azure hesabınızda oturum [az login](/cli/azure/reference-index).
* **Bir .vhd dosyasına OpenBSD işletim sistemi** - işletim sistemi desteklenen bir OpenBSD ([6.2 sürümü AMD64](https://ftp.openbsd.org/pub/OpenBSD/6.2/amd64/)) bir sanal sabit diske yüklenmesi gerekir. Birden çok araç, .vhd dosyaları oluşturmak için mevcut. Örneğin, .vhd dosyasını oluşturup işletim sistemini yüklemek için Hyper-V gibi bir sanallaştırma çözümü kullanabilirsiniz. Yükleme ve Hyper-V kullanma hakkında yönergeler için bkz. [Hyper-V yükleyin ve sanal makine oluşturma](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Azure için OpenBSD görüntüsü hazırlama
OpenBSD işletim Hyper-V eklenen sistemi 6.1, yüklediğiniz VM'de desteği, aşağıdaki yordamları tamamlayın:

1. DHCP yükleme sırasında etkin değilse, hizmeti şu şekilde etkinleştirin:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Bir seri Konsolu aşağıdaki gibi ayarlayın:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Paket yüklemesi aşağıdaki gibi yapılandırın:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Varsayılan olarak, `root` kullanıcı Azure içindeki sanal makinelerde devre dışıdır. Kullanıcılar çalışma zamanı komutları yükseltilmiş ayrıcalıklarla kullanarak `doas` OpenBSD VM'de komutu. Doas varsayılan olarak etkindir. Daha fazla bilgi için [doas.conf](https://man.openbsd.org/doas.conf.5). 

5. Yükleme ve Azure Aracısı önkoşulları aşağıdaki gibi yapılandırın:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. Azure Aracısı'nın en son sürümü her zaman bulunabilir [GitHub](https://github.com/Azure/WALinuxAgent/releases). Aracıyı aşağıdaki gibi yükleyin:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Azure Aracısı'nı yükledikten sonra şu şekilde çalıştığını doğrulamak için iyi bir fikirdir:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Sistemin temizlemesini ve çıkış için uygun hale getiren sağlamasını kaldırın. Aşağıdaki komut, son sağlanan kullanıcı hesabı ve ilişkili veriler de siler:

    ```sh
    waagent -deprovision+user -force
    ```

Şimdi, VM'yi kapatmanız yeterlidir.


## <a name="prepare-the-vhd"></a>VHD hazırlama
VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**. Disk Hyper-V Yöneticisi veya Powershell kullanarak sabit VHD biçimine dönüştürebilirsiniz [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet'i. Örnek olarak verilebilir aşağıdaki.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Depolama kaynaklarını oluşturma ve karşıya yükleme
Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

VHD'nizi karşıya yüklemek için bir depolama hesabı oluşturmanız [az depolama hesabı oluşturma](/cli/azure/storage/account). Depolama hesabı adları benzersiz olmalıdır; bu nedenle kendi adınızı sağlayın. Aşağıdaki örnekte adlı bir depolama hesabı oluşturur *mystorageaccount*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

Depolama hesabına erişimi denetlemek için depolama anahtarı ile elde [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys) gibi:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

Karşıya yüklediğiniz VHD'ler mantıksal olarak ayırmak için ile depolama hesabında bir kapsayıcı oluşturma [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Son olarak, VHD'niz karşıya [az storage blob upload](/cli/azure/storage/blob) gibi:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>Bir VHD'den VM oluşturma
Bir VM ile oluşturduğunuz bir [örnek komut dosyası](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) veya doğrudan [az vm oluşturma](/cli/azure/vm). Karşıya yüklediğiniz OpenBSD VHD belirtmek için kullanın `--image` parametresini aşağıdaki şekilde:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

OpenBSD vm'nizin IP adresini alın [az vm-IP-adreslerini](/cli/azure/vm) gibi:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Artık normal olarak OpenBSD sanal makinenize yönelik SSH yapabilirsiniz:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Sonraki adımlar
OpenBSD6.1 Hyper-V desteği hakkında daha fazla bilgi edinmek istiyorsanız, okuma [OpenBSD 6.1](https://www.openbsd.org/61.html) ve [hyperv.4](https://man.openbsd.org/hyperv.4).

Yönetilen diskten bir VM oluşturmak istiyorsanız, okuma [az disk](/cli/azure/disk). 
