---
title: Yapma sanal makine ölçek Azure yığınında kullanılabilen ayarlar | Microsoft Docs
description: Bir bulut işleci Azure yığın Marketinde sanal makine ölçek nasıl ekleyebileceğinizi öğrenin
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: 12425ab53ca16bb985a0a8658b5058998565b01a
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Sanal makine ölçek kümeleri Azure yığın kullanılabilmesini

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Sanal makine ölçek kümeleri Azure yığın işlem kaynaktır. Bunları dağıtmak ve aynı sanal makineler kümesini yönetmek için kullanabilirsiniz. Tüm sanal makinelerle aynı ölçek kümeleri sanal makinelerin önceden sağlama gerektirmeyen yapılandırılmış. Big compute, büyük veri ve kapsayıcılı iş yükleri hedef büyük ölçekli hizmetler oluşturmak kolaydır.

Bu makalede Azure yığın Marketi'nde ölçek kümeleri kullanılabilir hale getirmek sürecinde size kılavuzluk eder. Bu yordamı tamamladıktan sonra kullanıcılarınızın kendi abonelikler için sanal makine ölçek ayarlar ekleyebilirsiniz.

Sanal makine ölçek kümeleri Azure yığında azure'da sanal makine ölçek kümeleri gibidir. Daha fazla bilgi için aşağıdaki videoları bakın:
* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure yığında otomatik ölçek sanal makine ölçek kümeleri desteklemez. Azure yığın portal, Resource Manager şablonları veya PowerShell kullanılarak ayarlanan bir ölçek başka örnekler ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* **PowerShell ve araçları**

   Yükleme ve Azure yığını için yapılandırılmış PowerShell ve Azure yığın araçları. Bkz: [Azure yığınında PowerShell ile başlamak ve çalıştırmak](azure-stack-powershell-configure-quickstart.md).

   Azure yığın araçları yüklendikten sonra aşağıdaki PowerShell modülünü içeri aktardığınız emin olun (göreli yolu. \ComputeAdmin AzureStack araçları ana klasöründe):
  ````PowerShell
        Import-Module .\AzureStack.ComputeAdmin.psm1
  ````

* **İşletim sistemi görüntüsü**

   Bir işletim sistemi görüntüsü, Azure yığın Market eklemediyseniz, bkz: [Azure yığın Market Windows Server 2016 VM görüntüsü eklemek](azure-stack-add-default-image.md).

   Linux desteği için Ubuntu Server 16.04 karşıdan yükleyip kullanarak eklemek ```Add-AzsPlatformImage``` aşağıdaki parametrelerle: ```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```.


## <a name="add-the-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi Ekle

Ortamınız için aşağıdaki PowerShell komut dosyasını düzenleyin ve ardından çalıştırın, Azure yığın Market ayarlamak bir sanal makine ölçek eklemek için. 

``$User`` Yönetici portalı'nı bağlanmak için kullandığınız hesaptır. Örneğin, serviceadmin@contoso.onmicrosoft.com.

````PowerShell  
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
````

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi görüntülerinde güncelleştir 
Bir sanal makine ölçek kümesini oluşturduktan sonra kullanıcıların yeniden oluşturulması gerek kalmadan ayarlamak ölçek kümesi ölçek görüntülerinde güncelleştirebilirsiniz. İşlemi görüntüyü güncelleştirmek için aşağıdaki senaryoları bağlıdır:

1. Sanal makine ölçek kümesi dağıtım şablonu **son belirtir** için *sürüm*:  

   Zaman *sürüm* olarak ayarlanmış olan **son** içinde *Imagereference* şablon için bir ölçek bölümünü ayarlamak, Ölçek kümesi kullanım işlemleri en yeni kullanılabilir sürümü ölçeklendirin Ölçek için görüntünün örnekleri ayarlayın. Bir ölçek büyütme tamamlandıktan sonra eski sanal makine ölçek kümeleri örnekleri silebilirsiniz.  (Değerlerini *yayımcı*, *teklif*, ve *sku* değişmeden kalır). 

   Aşağıdaki belirten bir örnektir *son*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Ölçek büyütme yeni bir görüntü kullanmadan önce yeni görüntü karşıdan yüklemeniz gerekir:  

   - Market görüntüde ölçek kümesini görüntüde daha yeni bir sürüme olduğunda: eski resim yerini alan yeni görüntüyü indirin. Görüntünün yerini sonra bir kullanıcı ölçeği geçebilirsiniz. 

   - Market görüntü sürümü olduğunda görüntünün ölçek kümesindeki aynı: ölçek kümesindeki kullanılıyor görüntüyü silin ve yeni görüntüyü indirin. Özgün görüntüsüne kaldırılmasını ve yeni görüntüyü indirme arasındaki süre boyunca, ölçeği olamaz. 
      
     Bu işlem gereklidir resyndicate için 1803 sürümü ile sunulan seyrek dosya biçimi olun görüntüleri kullanın. 
 

2. Sanal makine ölçek kümesi dağıtım şablonu **son belirtmiyor** için *sürüm* ve bunun yerine bir sürüm numarasını belirtir:  

     Bir görüntü (kullanılabilir sürüm Değişeni) daha yeni bir sürümüyle yüklerseniz, Ölçek kümesini ölçeği olamaz. Bu, Ölçek kümesi şablonda belirtilen görüntüyü sürüm kullanılabilir olması gerektiği tasarım gereğidir.  

Daha fazla bilgi için bkz: [işletim sistemi diskleri ve görüntüleri](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Kaldır

Bir sanal makineyi kaldırmak için kümesi galeri öğesi ölçeklendirme, aşağıdaki PowerShell komutunu çalıştırın:

```PowerShell  
    Remove-AzsVMSSGalleryItem
````

> [!NOTE]
> Galeri öğesi hemen kaldırılamaz. Gece, Market görüntüsünden kaldırılmış olarak olan öğeyi gösterir önce portal birkaç kez yenilemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığını için sık sorulan sorular](azure-stack-faq.md)