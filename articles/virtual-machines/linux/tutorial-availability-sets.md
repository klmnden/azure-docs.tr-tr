---
title: "Linux VM'ler için Azure kullanılabilirlik kümeleri Öğreticisi | Microsoft Docs"
description: "Linux VM'ler için Azure kullanılabilirlik kümeleri hakkında bilgi edinin."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e7780a29f6633b444608d96012fabe67b9b6d924
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-availability-sets"></a>Kullanılabilirlik kümeleri kullanma


Bu öğreticide, kullanılabilirlik ve kullanılabilirlik kümeleri adlı bir özelliği kullanarak azure'da sanal makine çözümlerinizi güvenilirliğini artırmak öğrenin. Kullanılabilirlik kümeleri, Azure üzerinde dağıttığınız VM'ler arasında birden fazla yalıtılmış donanım küme dağıtıldığından emin olmak. Bu Azure içinde bir donanım veya yazılım hatası oluşursa, yalnızca bir alt kümesini Vm'leriniz etkilenmemesini sağlar yapmak ve çözümünüzün genel kullanılabilir ve çalışır durumda kalır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Bir kullanılabilirlik kümesine bir VM oluşturma
> * Kullanılabilir VM boyutları denetleyin


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Kullanılabilirlik kümesi'ne genel bakış

Bir kullanılabilirlik kümesi Azure'da Azure veri merkezi içinde dağıtıldığında içine yerleştirin VM kaynakları birbirinden yalıtılmış olduğundan emin olmak için kullanabileceğiniz bir mantıksal bir gruplandırma bir özelliktir. Azure birden çok fiziksel sunucuda çalıştırmak bir kullanılabilirlik kümesi içinde yerleştirin VM'ler sağlar, işlem rafları, depolama birimi ve ağ anahtarları. Bir donanım veya Azure yazılım hatası oluşursa, yalnızca bir alt kümesini Vm'leriniz etkilendi ve genel uygulamanızı kurma kalır ve müşterileriniz için kullanılabilir olmaya devam eder. Güvenilir bulut çözümleri oluşturmak istediğinizde kullanılabilirlik önemli bir özellik kümesidir.

Şimdi burada 4 ön uç web sunucusu ve bir veritabanı ana bilgisayar 2 arka uç VM kullanmak bir tipik VM tabanlı çözümünü göz önünde bulundurun. Azure ile Vm'leriniz dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: bir kullanılabilirlik kümesi için "web" katmanı ve bir kullanılabilirlik kümesi için "veritabanı" katmanı. Ardından az VM'ye parametre kullanılabilirlik komutu oluşturun ve Azure otomatik olarak oluşturulan VM'ler sağlar belirleyebilirsiniz yeni bir VM oluştururken kullanılabilir kümesi yalıtılmış birden çok fiziksel donanım kaynaklarına arasında. Bir Web sunucusu veya veritabanı sunucusu sanal makineleri çalıştıran fiziksel donanımı bir sorun varsa, çünkü bunlar üzerinde farklı donanım diğer örneklerini Web sunucusu ve veritabanı VM'ler çalışır durumda bildirin.

Azure içinde güvenilir VM tabanlı çözümler dağıtmak istediğinizde kullanılabilirlik kümelerini kullanın.


## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Kullanılabilirlik kümesi kullanarak oluşturabileceğiniz [az vm kullanılabilirlik kümesi oluşturma](/cli/azure/vm/availability-set#create). Bu örnekte, her iki güncelleştirme ve hata etki alanlarının sayısı ayarlarız *2* kullanılabilirlik adlandırılmış kümesi için *myAvailabilitySet* içinde *myResourceGroupAvailability* kaynak grubu.

Bir kaynak grubu oluşturun.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Kullanılabilirlik kümeleri, hata etki alanlarında kaynakları yalıtmak ve güncelleme etki alanları izin verir. A **hata etki alanı** sunucu, ağ + depolama yalıtılmış bir koleksiyonunu temsil eder kaynakları. Önceki örnekte bizim kullanılabilirlik bizim VM'ler dağıtıldığında en az iki hata etki alanları arasında dağıtılacak kümesi istiyoruz gösterir. Biz de iki arasında dağıtılmış kümesi bizim kullanılabilirlik istiyoruz belirtmek **güncelleştirme etki alanları**.  İki güncelleştirme etki alanı Azure yazılım güncelleştirmelerini gerçekleştirdiğinde VM KAYNAKLARIMIZI, aynı anda güncelleştirilmiş bizim VM altında çalışan tüm yazılım önleme, yalıtılmış olduğundan emin olun.


## <a name="create-vms-inside-an-availability-set"></a>VM'ler içinde bir kullanılabilirlik kümesi oluştur

Sanal makineleri kullanılabilirlik donanım üzerinde doğru şekilde dağıtıldığından emin olmak için kümesini içinde oluşturulmalıdır. Kullanılabilirlik oluşturulduktan sonra kümesi için mevcut bir VM'yi ekleyemezsiniz. 

Kullanarak bir VM oluştururken [az vm oluşturma](/cli/azure/vm#create) kullanılarak ayarlanan kullanılabilirlik belirttiğiniz `--availability-set` kullanılabilirlik kümesinin adını belirtmek için parametre.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Şimdi iki sanal makine, yeni oluşturulan kullanılabilirlik kümesinde sahibiz. Aynı kullanılabilirlik kümesinde olduklarından, Azure sanal makineleri ve tüm kaynaklarını (veri diskleri dahil), yalıtılmış fiziksel donanım üzerinde dağıtılmış sağlar. Bu dağıtım kadar yüksek kullanılabilirlik bizim genel VM çözümünün sağlamaya yardımcı olur.

Kullanılabilirlik için kaynak gruplarını giderek portalda kümesini bakarsanız > myResourceGroupAvailability > myAvailabilitySet, sanal makineleri 2 arıza arasında nasıl dağıtıldığını bakın ve güncelleştirme etki alanı gerekir.

![Kullanılabilirlik portalda kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir VM boyutları denetle 

Daha fazla sanal makineleri daha sonra ayarlamak kullanılabilirlik ekleyebilirsiniz, ancak hangi VM boyutları donanımda kullanılabilir olduğunu bilmeniz gerekir.  Kullanım [az vm kullanılabilirlik kümesi listesi-boyutları](/cli/azure/availability-set#list-sizes) tüm donanım üzerinde kullanılabilir boyutları küme için kullanılabilirlik kümesi listelemek için.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Bir kullanılabilirlik kümesine bir VM oluşturma
> * Kullanılabilir VM boyutları denetleyin

Sanal makine ölçek kümeleri hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM ölçek kümesi oluşturma](tutorial-create-vmss.md)

