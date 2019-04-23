---
title: Öğretici - Azure sanal makine ölçek kümesi oluşturma ve yönetme | Microsoft Docs
description: Örnek başlatma ve durdurma veya ölçek kümesi kapasitesini değiştirme gibi bazı genel yönetim görevlerinin yanı sıra, sanal makine ölçek kümesi oluşturmak için Azure CLI’nin nasıl kullanılacağını öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b0d2a72567783ca1c127f76d94ddc9c5e007ea89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60188552"
---
# <a name="tutorial-create-and-manage-a-virtual-machine-scale-set-with-the-azure-cli"></a>Öğretici: Oluşturma ve sanal makine ölçek kümesi Azure CLI ile yönetme
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal makine ölçek kümesi oluşturma ve sanal makine ölçek kümesine bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli sanal makine örneği boyutlarını görüntüleme ve kullanma
> * Ölçek kümesini el ile ölçeklendirme
> * Genel ölçek kümesi yönetim görevlerini gerçekleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.29 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sanal makine ölçek kümesinden önce kaynak grubu oluşturulmalıdır. [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Bu örnekte, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturulur. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bu öğreticide bir ölçek kümesi oluşturduğunuzda veya değiştirdiğinizde kaynak grubu adı belirtilir.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
[az vmss create](/cli/azure/vmss) komutuyla bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek *myScaleSet* adlı bir ölçek kümesini ve yoksa SSH anahtarlarını oluşturur:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika sürer. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur.


## <a name="view-the-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini görüntüleme
Bir ölçek kümesindeki sanal makine örneklerinin listesini görüntülemek için [az vmss list-instances](/cli/azure/vmss) komutunu şu şekilde kullanın:

```azurecli-interactive
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

Aşağıdaki örnek çıktı, ölçek kümesindeki iki sanal makine örneğini göstermektedir:

```bash
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUP  c059be0c-37a2-497a-b111-41272641533c
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUP  ec19e7a7-a4cd-4b24-9670-438f4876c1f9
```


Çıktıdaki ilk sütunda bir *InstanceId* gösterilmektedir. Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için [az vmss get-instance-view](/cli/azure/vmss) komutuna `--instance-id` parametresini ekleyin. Aşağıdaki örnekte, *1* sanal makine örneğiyle ilgili bilgiler görüntülenmektedir:

```azurecli-interactive
az vmss get-instance-view \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --instance-id 1
```


## <a name="list-connection-information"></a>Bağlantı bilgilerini listeleme
Tek tek sanal makine örneklerine trafiği yönlendiren yük dengeleyiciye genel bir IP adresi atanır. Varsayılan olarak, belirtilen bir bağlantı noktasındaki her bir sanal makineye uzaktan bağlantı trafiğini ileten Azure Load Balancer’a Ağ Adresi Çevirisi (NAT) kuralları eklenir. Bir ölçek kümesindeki sanal makine örneklerine bağlanmak için, atanan bir genel IP adresine ve bağlantı noktası numarasına uzaktan bağlantı oluşturursunuz.

Bir ölçek kümesindeki sanal makine örneklerine bağlanacak bağlantı noktalarını ve adresi listelemek için [az vmss list-instance-connection-info](/cli/azure/vmss) komutunu kullanın:

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```

Aşağıdaki örnek çıktıda, yük dengeleyicinin genel IP adresi, örnek adı ve NAT kurallarının trafiği ilettiği bağlantı noktası numarası gösterilmektedir:

```bash
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

Birinci sanal makine örneğinizde SSH oturumu açın. Önceki komutta gösterildiği gibi, `-p` parametresiyle genel IP adresinizi ve bağlantı noktası numaranızı belirtin:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Sanal makine örneğinde oturum açtıktan sonra, gerektiğinde bazı el ile yapılandırma değişiklikleri gerçekleştirebilirsiniz. Şimdilik SSH oturumunu normal şekilde kapatın:

```bash
exit
```


## <a name="understand-vm-instance-images"></a>Sanal makine örneği görüntülerini anlama
Öğreticinin başında bir ölçek kümesi oluşturduğunuzda, sanal makine örnekleri için *UbuntuLTS* öğesinin `--image` öğesi belirtildi. Azure marketi, sanal makine örnekleri oluşturmak için kullanılabilecek çok sayıda görüntü içerir. Yaygın olarak kullanılan görüntülerin bir listesini görmek için, [az vm image list](/cli/azure/vm/image) komutunu kullanın.

```azurecli-interactive
az vm image list --output table
```

Aşağıdaki örnek çıktıda, Azure’daki en yaygın sanal makine görüntüleri gösterilmektedir. Bir ölçek kümesi oluştururken şu genel görüntülerden birini belirtmek için *UrnAlias* kullanılabilir.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
```

Tam listeyi görüntülemek için `--all` bağımsız değişkenini ekleyin. Görüntü listesi `--publisher` veya `–-offer` kullanılarak da filtrelenebilir. Aşağıdaki örnekte liste, *CentOS* ile eşleşen teklife sahip tüm görüntüler için filtrelenmiştir:

```azurecli-interactive
az vm image list --offer CentOS --all --output table
```

Aşağıdaki sıkıştırılmış çıktıda, kullanılabilir CentOS 7.3 görüntülerinden bazıları gösterilmektedir:

```azurecli-interactive 
Offer    Publisher   Sku   Urn                                 Version
-------  ----------  ----  ----------------------------------  -------------
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20161221   7.3.20161221
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170421   7.3.20170421
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170517   7.3.20170517
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170612   7.3.20170612
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170707   7.3.20170707
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170925   7.3.20170925
```

Belirli bir görüntüyü kullanan bir ölçek kümesini dağıtmak için *Urn* sütunundaki değeri kullanın. Görüntüyü belirttiğinizde, görüntü sürümü sayısı *en yeni* ile değiştirilerek dağıtımın en yeni sürümü seçilebilir. Aşağıdaki örnekte, bir CentOS 7.3 görüntüsünün son sürümünü belirtmek için `--image` bağımsız değişkeni kullanılmıştır.

Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika süreceğinden, aşağıdaki ölçek kümesini dağıtmanız gerekmez:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSetCentOS \
  --image OpenLogic:CentOS:7.3:latest \
  --admin-user azureuser \
  --generate-ssh-keys
```


## <a name="understand-vm-instance-sizes"></a>Sanal makine örneği boyutlarını anlama
Sanal makine örneğinin boyutu veya *SKU*, sanal makine örneği tarafından kullanılabilen CPU, GPU ve bellek gibi işlem kaynaklarının miktarını belirler. Ölçek kümesindeki sanal makine örneklerinin beklenen iş yüküne uygun olarak boyutlandırılması gerekir.

### <a name="vm-instance-sizes"></a>Sanal makine örneği boyutları
Aşağıdaki tabloda genel sanal makine boyutları, kullanım durumlarına göre kategorilere ayrılmıştır.

| Type                     | Ortak boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](../virtual-machines/linux/sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Dengeli CPU/bellek. Küçük ve orta ölçekli uygulama ve veri çözümlerini geliştirmek/test etmek için idealdir.  |
| [İşlem için iyileştirilmiş](../virtual-machines/linux/sizes-compute.md)   | Fs, F             | Yüksek CPU/bellek. Orta düzey trafiğe sahip uygulamalar, ağ gereçleri ve toplu işlemler için idealdir.        |
| [Bellek için iyileştirilmiş](../virtual-machines/linux/sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Yüksek bellek/çekirdek. İlişkisel veritabanı, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](../virtual-machines/linux/sizes-storage.md)      | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](../virtual-machines/linux/sizes-gpu.md)          | NV, NC            | Ağır grafik işlemleri ile video düzenleme işlemleri için özel olarak hedeflenen VM’ler.       |
| [Yüksek performans](../virtual-machines/linux/sizes-hpc.md) | H, A8-11          | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren VM’lerimiz, şimdiye kadarki en güçlü CPU ile sunuluyor. 

### <a name="find-available-vm-instance-sizes"></a>Kullanılabilir sanal makine örneği boyutlarını bulma
Belirli bir bölgede kullanılabilen sanal makine örneği boyutlarının listesini görmek için, [az vm list-sizes](/cli/azure/vm) komutunu kullanın.

```azurecli-interactive
az vm list-sizes --location eastus --output table
```

Çıktı, her sanal makine boyutuna atanan kaynakları gösteren, aşağıdaki sıkıştırılmış örneğe benzer:

```azurecli-interactive
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
[...]
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
[...]
                 4          2048  Standard_F1                           1           1047552                   16384
                 8          4096  Standard_F2                           2           1047552                   32768
[...]
                24         57344  Standard_NV6                          6           1047552                   38912
                48        114688  Standard_NV12                        12           1047552                  696320
```

### <a name="create-a-scale-set-with-a-specific-vm-instance-size"></a>Belirli bir sanal makine örneği boyutu ile ölçek kümesi oluşturma
Öğreticinin başında bir ölçek kümesi oluşturduğunuzda, sanal makine örnekleri için varsayılan bir *Standard_D1_v2* sanal makine SKU’su sağlanmıştır. [az vm list-sizes](/cli/azure/vm) içindeki çıktıyı temel alarak farklı bir sanal makine örneği belirtebilirsiniz. Aşağıdaki örnek, *Standard_F1* bir sanal makine örneği boyutu belirtmek için `--vm-sku` parametresiyle bir ölçek kümesi oluşturur. Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika süreceğinden, aşağıdaki ölçek kümesini dağıtmanız gerekmez:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSetF1Sku \
  --image UbuntuLTS \
  --vm-sku Standard_F1 \
  --admin-user azureuser \
  --generate-ssh-keys
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Öğreticinin başında bir ölçek kümesi oluşturduğunuzda varsayılan olarak iki sanal makine örneği dağıtılmıştı. Bir ölçek kümesiyle oluşturulan örnek sayısını değiştirmek için [az vmss create](/cli/azure/vmss) ile `--instance-count` parametresini belirtebilirsiniz. Mevcut ölçek kümenizdeki sanal makine örneği sayısını artırmak veya azaltmak için kapasiteyi el ile değiştirebilirsiniz. Ölçek kümesi, gerektiği sayıda sanal makine örneği oluşturur veya kaldırır ve sonra trafiği dağıtmak için yük dengeleyiciyi yapılandırır.

Ölçek kümesindeki sanal makine örneği sayısını elle artırmak veya azaltmak için [az vmss scale](/cli/azure/vmss) komutunu kullanın. Aşağıdaki örnek, ölçek kümenizdeki sanal makine sayısını *3* olarak ayarlar:

```azurecli-interactive
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 3
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Ölçek kümesinde şu anda yer alan örneklerin sayısını görmek için [az vmss show](/cli/azure/vmss) komutunu kullanın ve *sku.capacity* üzerinde bir sorgu çalıştırın:

```azurecli-interactive
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```


## <a name="common-management-tasks"></a>Genel yönetim görevleri
Artık bir ölçek kümesi oluşturabilir, bağlantı bilgilerini listeleyebilir ve sanal makine örneklerine bağlanabilirsiniz. Sanal makine örnekleriniz için farklı bir OS görüntüsünü nasıl kullanabileceğinizi veya örnek sayısını nasıl el ile ölçeklendirebileceğinizi öğrendiniz. Günlük yönetim işlemleriniz kapsamında, ölçek kümenizdeki sanal makine örneklerini durdurmanız, başlatmanız veya yeniden başlatmanız gerekebilir.

### <a name="stop-and-deallocate-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini durdurma ve serbest bırakma
Bir ölçek kümesindeki bir veya daha fazla sanal makine örneğini durdurmak için [az vmss stop](/cli/azure/vmss) komutunu kullanın. `--instance-ids` parametresi, durdurulacak bir veya daha fazla sanal makine örneğini belirtmenize olanak sağlar. Bir örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makine örnekleri durdurulur. Aşağıdaki örnek, *1* örneğini durdurur:

```azurecli-interactive
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

Durdurulan sanal makine örnekleri ayrılmış şekilde kalır ve işlem ücretleri uygulanmaya devam eder. Bunun yerine, sanal makine örneklerinin serbest bırakılmasını ve yalnızca depolama ücreti uygulanmasını istiyorsanız [az vmss deallocate](/cli/azure/vmss) komutunu kullanın. Aşağıdaki örnek, *1* örneğini durdurur ve serbest bırakır:

```azurecli-interactive
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

### <a name="start-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine örneğini başlatmak için [az vmss start](/cli/azure/vmss) komutunu kullanın. `--instance-ids` parametresi, başlatılacak bir veya daha fazla sanal makine örneğini belirtmenize olanak sağlar. Bir örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makine örnekleri başlatılır. Aşağıdaki örnek, *1* örneğini başlatır:

```azurecli-interactive
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

### <a name="restart-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini yeniden başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine örneğini yeniden başlatmak için [az vmss restart](/cli/azure/vmss) komutunu kullanın. `--instance-ids` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine örneğini belirtmenize olanak sağlar. Bir örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makine örnekleri yeniden başlatılır. Aşağıdaki örnek, *1* örneğini yeniden başlatır:

```azurecli-interactive
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubunu sildiğinizde, o kaynak grubunun içindeki sanal makine örnekleri, sanal ağ ve diskler gibi tüm kaynaklar da silinir. `--no-wait` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür. `--yes` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure CLI ile bazı temel ölçek kümesi oluşturma ve yönetme görevlerinin nasıl gerçekleştirileceğini öğrendiniz:

> [!div class="checklist"]
> * Sanal makine ölçek kümesi oluşturma ve sanal makine ölçek kümesine bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli VM boyutlarını görüntüleme ve kullanma
> * Ölçek kümesini el ile ölçeklendirme
> * Genel ölçek kümesi yönetim görevlerini gerçekleştirme

Ölçek kümesi diskleri hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Veri disklerini ölçek kümeleri ile kullanma](tutorial-use-disks-cli.md)
