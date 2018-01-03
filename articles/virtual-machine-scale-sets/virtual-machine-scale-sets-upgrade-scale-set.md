---
title: "Bir Azure sanal makine ölçek kümesini yükseltme | Microsoft Docs"
description: "Bir Azure sanal makine ölçek kümesini yükseltme"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: gunegatybo
ms.openlocfilehash: fbdc9d40173a40f35eee60cadfdd258293509d53
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini yükseltme
Bu makalede, nasıl bir Azure sanal makine ölçek herhangi kesinti olmadan ayarlamak için bir işletim sistemi güncelleştirmeyi geri alabilirsiniz açıklanmaktadır. Bu bağlamda bir işletim sistemi güncelleştirme sürümü veya SKU işletim sisteminin değiştirme veya özel bir görüntü URI'si değiştirme içerir. Sanal makineleri birer birer birer veya gruplar (örneğin, bir seferde bir hata etki alanı) tüm aynı anda yerine güncelleştirme kapalı kalma süresi anlamına gelir olmadan güncelleştiriliyor. Bunu yaparak, değil yükseltilen tüm sanal makineleri çalışmaya devam.

Karışıklığı önlemek için şimdi gerçekleştirmek isteyebileceğiniz işletim sistemi güncelleştirme dört tür ayırt:

* Sürüm veya bir platform görüntüsü SKU'su değiştirme. Örneğin, Ubuntu değiştirme 14.04.2-LTS sürümden 14.04.201506100 14.04.201507060, veya Ubuntu 15.10/son SKU 16.04.0-LTS/latest için değiştirme. Bu senaryo, bu makalede ele alınmıştır.
* Özel görüntü yeni bir sürümüne işaret URİ'SİNİN değiştirilmesine yerleşik (**özellikleri** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **görüntü** > **URI**). Bu senaryo, bu makalede ele alınmıştır.
* Azure yönetilen diskleri kullanılarak oluşturulmuş bir ölçek kümesinin resim başvurusu değiştirme.
* Sanal makine içinden OS düzeltme eki uygulama (Bu örnekleri arasında bir güvenlik düzeltme eki yükleme ve Windows Update çalıştıran). Bu senaryo desteklenir, ancak bu makalede ele alınan değil.

Bir parçası olarak dağıtılan sanal makine ölçek kümeleri bir [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) küme ele alınmamıştır burada. Service Fabric düzeltme eki uygulama hakkında daha fazla bilgi için bkz: [düzeltme eki Windows işletim sisteminde, Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-patch-orchestration-application)

Bir platform görüntüsü, işletim sistemi sürümü/SKU veya özel bir görüntü URI'si değiştirmek için temel sıralı şu şekilde görünür:

1. Sanal makine ölçek kümesi modelinde alın.
2. Sürüm, SKU, resim başvurusu veya modelinde URI değeri değiştirin.
3. Modeli güncelleştirin.
4. Yapmak bir *manualUpgrade* ölçek kümesindeki sanal makinelerde çağırın. Bu adım yalnızca ilgili değilse *upgradePolicy* ayarlanır **el ile** , Ölçek kümesi. Bu ayarlanırsa **otomatik**, böylece kapalı kalma süresi neden olan tüm sanal makineleri aynı anda yükseltilir.

Aklınızda bu bilgi ile PowerShell ve REST API kullanarak bir ölçek sürümünü nasıl güncelleştirebilir görelim. Bu örnekler bir platform görüntüsü durumunun kapak, ancak bu makale, bu işlem için özel bir görüntü uyarlamak yeterli bilgi sağlar.

## <a name="powershell"></a>PowerShell
Bu örnek bir Windows sanal makine ölçek kümesi (en yeni sürüme 4.0.20160229 oluşturuluyor. güncelleştirir Model güncelleştirdikten sonra bunu aynı anda bir güncelleştirme bir sanal makine örneğini yapar.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Platform görüntü sürümü değiştiğinde yerine özel bir görüntü için URI güncelleştiriyorsanız "yeni sürümü ayarlama" satırı kaynak görüntü URI güncelleştiren bir komutu değiştirin. Azure yönetilen diskleri kullanmadan ölçek kümesini oluşturulduysa, örneğin, güncelleştirme şöyle olabilir:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Özel görüntü tabanlı ölçek kümesi Azure yönetilen diskleri kullanılarak oluşturulduysa, sonra resim başvurusu güncelleştirilmesi. Örneğin:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a>REST API
Burada, birkaç bir işletim sistemi sürümü güncelleştirmeyi alma için Azure REST API'sini kullanın Python örnekler verilmiştir. Her ikisini de basit kullanmanız [azurerm](https://pypi.python.org/pypi/azurerm) kitaplığı Azure REST API sarmalayıcı işlevlerinin bir GET ölçekte yapmak için model, güncelleştirilmiş bir modelle PUT arkasından ayarlayın. Bunlar ayrıca güncelleştirme etki alanı tarafından sanal makineleri tanımak amacıyla sanal makine örnekleri görünümleri bakın.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) çalışan bir sanal makine ölçek bir işletim sistemi yükseltme alma için kullanılan bir Python komut dosyası, aynı anda tek bir güncelleştirme etki alanı kümesi.

![Sanal makine ya da bir güncelleştirme etki alanı seçme Vmssupgrade komut dosyası](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Bu komut dosyasını güncelleştirmek veya bir güncelleştirme etki alanı belirtmek için belirli sanal makinelerin seçmenize olanak sağlar. Platform görüntü sürümü değiştirme veya özel bir görüntü URI'yı destekler.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) sanal gösterir sanal makine ölçek kümeleri heatmap Durum makinesi için bir genel amaçlı bir güncelleştirme etki alanı bir satır temsil ettiği düzenleyicisidir. Yanı sıra, modeli ölçek kümesi için bir yeni sürümü, SKU veya özel görüntü URI'si ile güncelleştirin ve ardından yükseltmek için hata etki alanlarını seçin. Bunu yaptığınızda, bu güncelleştirme etki alanındaki tüm sanal makineleri yeni modeli yükseltilir. Alternatif olarak, tercih ettiğiniz toplu boyutuna göre çalışırken yükseltme yapabilirsiniz.  

Aşağıdaki ekran görüntüsünde ölçeği Ubuntu 14.04-2LTS için sürüm 14.04.201507060 Ayarla modelinin gösterir. Bu ekran alındıktan sonra bu aracı için pek çok seçenek eklenmiştir.

![Ubuntu 14.04-2LTS için ayarlanmış bir ölçek Vmsseditor modeli](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Tıklattıktan sonra **yükseltme** ve ardından **ayrıntıları alma**, sanal makinelerinizde UD 0 Başlat güncelleştirmek.

![Vmsseditor gösteren güncelleştirmesi devam ediyor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

