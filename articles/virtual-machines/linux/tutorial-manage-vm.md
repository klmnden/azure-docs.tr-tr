---
title: "Oluşturma ve Azure CLI ile Linux sanal makineleri yönetme | Microsoft Docs"
description: "Öğretici - oluşturma ve Linux VM'ler Azure CLI ile yönetme"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: bef7f6ef13f6d31c16d40deb46f168ae52a9e61b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a>Oluşturma ve Linux VM'ler Azure CLI ile yönetme

Azure sanal makineler tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici, bir VM boyutu seçerek, bir VM görüntüsü seçme ve bir VM dağıtma gibi temel Azure sanal makine dağıtım öğeleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oluşturma ve bir VM'ye bağlanın
> * Seçin ve VM görüntüleri kullanmak
> * Görüntüleme ve belirli VM boyutları kullanma
> * VM’yi yeniden boyutlandırma
> * Görüntüleyin ve VM durumunu anlamak


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu bir sanal makine önce oluşturulması gerekir. Bu örnekte, bir kaynak grubu adında *myResourceGroupVM* oluşturulan *eastus* bölge. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

Kaynak grubu oluştururken veya değiştirirken Bu öğretici görülebilir bir VM belirtilir.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine oluşturma [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) komutu. 

Bir sanal makine oluştururken, işletim sistemi görüntüsü, disk boyutlandırma ve yönetici kimlik bilgileri gibi birkaç seçenek bulunur. Bu örnekte, bir sanal makine adı ile oluşturulan *myVM* Ubuntu Server çalıştıran. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürebilir. VM oluşturulduktan sonra Azure CLI VM hakkında bilgi verir. Not edin `publicIpAddress`, bu adres sanal makine erişmek için kullanılan... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a>VM'ye bağlanın

Artık Azure bulut Kabuğu'nda veya yerel bilgisayarınızdan SSH VM'ye bağlanabilir. Örnek IP adresiyle değiştirin `publicIpAddress` önceki adımda not ettiğiniz.

```bash
ssh 52.174.34.95
```

VM oturum açtıktan sonra yükleme ve uygulamalarını yapılandırın. İşiniz bittiğinde, normal olarak SSH oturumu kapatın:

```bash
exit
```

## <a name="understand-vm-images"></a>VM görüntüleri anlama

Azure Market VM'ler oluşturmak için kullanılan çok sayıda görüntü içerir. Önceki adımlarda, bir sanal makine bir Ubuntu görüntü kullanılarak oluşturuldu. Bu adımda, Azure CLI sonra ikinci bir sanal makineyi dağıtmak için kullanılan bir CentOS görüntüsü Market aramak için kullanılır.  

Bir liste en yaygın olarak kullanılan görüntüleri görmek için [az vm görüntü listesi](/cli/azure/vm/image#list) komutu.

```azurecli-interactive 
az vm image list --output table
```

Komut çıktısı Azure üzerinde en popüler VM görüntüleri döndürür.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Ekleyerek tam bir listesi görülebilir `--all` bağımsız değişkeni. Resim listesi de göre filtrelenebilir `--publisher` veya `–-offer`. Bu örnekte, tüm görüntüleri için eşleşen bir teklif ile listesi filtrelenir *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Kısmi çıktı:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

Belirli bir görüntüsünü kullanan bir VM'yi dağıtmak için değeri not edin *Urn* sütun. Görüntü belirtirken, görüntü sürüm numarası ile "son", hangi dağıtım en son sürümünü seçer değiştirilebilir. Bu örnekte, `--image` bağımsız değişkeni bir CentOS 6.5 görüntüsü en son sürümünü belirtmek için kullanılır.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>VM boyutları anlama

Bir sanal makine boyutu, sanal makine için kullanılabilir hale getirilir işlem kaynaklarını CPU, GPU ve bellek gibi miktarını belirler. Sanal makineler, beklenen iş yükü için uygun boyutta olması gerekir. İş yükü artarsa, var olan bir sanal makine yeniden boyutlandırılabilir.

### <a name="vm-sizes"></a>VM boyutları

Aşağıdaki tabloda, kullanım örneklerine boyutları kategorilere ayırır.  

| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Dengeli CPU bellekten. Geliştirme için ideal / test ve küçük ve orta uygulamaları ve verileri çözümler.  |
| [İşlem için iyileştirilmiş](sizes-compute.md)   | FS, F             | Yüksek CPU bellekten. Orta düzey trafik uygulamalar, ağ uygulamaları ve toplu işlemler için iyidir.        |
| [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Yüksek bellek için-çekirdek. İlişkisel veritabanları, Orta ve büyük önbellekler ve bellek içi analizi için mükemmel.                 |
| [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md)      | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](sizes-gpu.md)          | NV, NC            | Yoğun Grafik işleme ve video düzenleme için hedeflenen özel VM'ler.       |
| [Yüksek performans](sizes-hpc.md) | H, A8-11          | Bizim en güçlü CPU VM'ler isteğe bağlı yüksek verimlilik ağ arabirimlerine (RDMA) sahip. 


### <a name="find-available-vm-sizes"></a>Kullanılabilir VM boyutları Bul

Belirli bir bölgede kullanılabilir VM boyutlarının listesini görmek için [az vm listesi-boyutları](/cli/azure/vm#list-sizes) komutu. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Kısmi çıktı:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Belirli boyutuyla VM oluşturma

Önceki VM oluşturma örnekte, bir boyut değil sağlanmadığından, varsayılan boyutu sonuçlanır. Oluşturma zamanı kullanarak bir VM boyutu seçilebilir [az vm oluşturma](/cli/azure/vm#create) ve `--size` bağımsız değişkeni. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma

Bir VM dağıtıldıktan sonra artırmak veya kaynak ayırma azaltmak için yeniden boyutlandırılabilir. Bir VM boyutu geçerli görüntüleyebilirsiniz [az vm Göster](/cli/azure/vm#show):

```azurecli-interactive
az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize
```

Bir VM'yi yeniden boyutlandırılırken önce istenen boyut geçerli Azure kümede kullanılabilir olup olmadığını denetleyin. [Az vm listesi-vm-yeniden boyutlandırma-seçenekleri](/cli/azure/vm#list-vm-resize-options) komut boyutlarının listesi döndürür. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
İstenen boyut varsa, işlemi sırasında yeniden başlatılıncaya kadar ancak VM bir gücü açma durumundan boyutlandırılabilir. Kullanım [az VM'yi yeniden boyutlandırın]( /cli/azure/vm#resize) boyutlandırma gerçekleştirmek için komutu.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

İstenen boyut geçerli kümede değilse, VM yeniden boyutlandırma işlemi oluşabilmesi için öncelikle serbest gerekir. Kullanım [az vm serbest bırakma]( /cli/azure/vm#deallocate) durdurun ve VM serbest bırakma için komutu. Not, VM geri açık olduğundan, geçici diskteki tüm verilerin kaldırılmış olabilir. Genel IP adresini de statik IP adresi kullanılmadığı sürece değiştirir. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Yeniden boyutlandırma, serbest sonra ortaya çıkabilir. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Sonra yeniden boyutlandırmak, VM başlatılabilir.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>VM güç durumları

Bir Azure VM birçok güç durumlarını birine sahip. Bu durum, hiper yönetici açısından VM geçerli durumunu temsil eder. 

### <a name="power-states"></a>Güç durumları

| Güç durumu | Açıklama
|----|----|
| Başlangıç | Sanal makinenin başlatıldığı gösterir. |
| Çalışıyor | Sanal makinenin çalıştığını gösterir. |
| Durduruluyor | Sanal makinenin durdurulması olduğunu gösterir. | 
| Durduruldu | Sanal makine durdurulduğunda gösterir. Sanal makine durdurulmuş durumunda hala işlem ücretleri.  |
| Ayırmayı kaldırma | Sanal makine ayırması olduğunu gösterir. |
| Serbest bırakıldı | Sanal makine hiper yönetici alanından kaldırılacak, ancak denetim düzeyi hala kullanılabilir olduğunu gösterir. Deallocated durumunda sanal makineler bilgi işlem ücretleri değil. |
| - | Sanal makinenin güç durumunun bilinmediğini gösterir. |

### <a name="find-power-state"></a>Güç durumu Bul

Belirli bir VM durumunu almak için kullanın [az vm örnek görünümünü Al](/cli/azure/vm#get-instance-view) komutu. Bir sanal makine ve kaynak grubu için geçerli bir ad belirttiğinizden emin olun. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Çıktı:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Yönetim görevleri

Yaşam döngüsü sırasında sanal makinenin, başlatma, durdurma veya bir sanal makine silme gibi yönetim görevleri çalıştırmak isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için komut dosyaları oluşturmak isteyebilirsiniz. Azure CLI kullanarak, birçok ortak yönetim görevlerinin komut satırından veya komut dosyalarında çalıştırabilirsiniz. 

### <a name="get-ip-address"></a>IP adresi al

Bu komut, bir sanal makine özel ve genel IP adreslerini döndürür.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Sanal makineyi durdurma

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Sanal makineyi Başlat

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Kaynak grubunu silme

Bir kaynak grubunun silinmesi, VM, sanal ağ ve disk gibi içerdiği tüm kaynaklar da siler. `--no-wait` Parametresi döndürür denetim komut istemini işlemin tamamlanmasını beklemeden. `--yes` Parametresi onaylar Bunu yapmak için ek bir istemi olmadan kaynakları silmek istiyor.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel VM oluşturmayı ve yönetmeyi nasıl gibi hakkında öğrenilen:

> [!div class="checklist"]
> * Oluşturma ve bir VM'ye bağlanın
> * Seçin ve VM görüntüleri kullanmak
> * Görüntüleme ve belirli VM boyutları kullanma
> * VM’yi yeniden boyutlandırma
> * Görüntüleyin ve VM durumunu anlamak

VM diskleri hakkında bilgi edinmek için sonraki öğretici ilerleyin.  

> [!div class="nextstepaction"]
> [Oluşturma ve yönetme VM diskleri](./tutorial-manage-disks.md)
