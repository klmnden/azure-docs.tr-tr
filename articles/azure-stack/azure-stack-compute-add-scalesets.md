---
title: "Sanal makine ölçek kümeleri Azure yığın kullanılabilmesini"
description: "Bulut yöneticisine Azure yığın Marketinde sanal makine ölçek nasıl ekleyebileceğinizi öğrenin"
services: azure-stack
author: anjayajodha
ms.service: azure-stack
ms.topic: article
ms.date: 9/25/2017
ms.author: anajod
keywords: 
ms.openlocfilehash: 31aeb963bdf4fd32712bc6f29f64060ec1c77cb8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Sanal makine ölçek kümeleri Azure yığın kullanılabilmesini

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Sanal makine ölçek kümeleri Azure yığın işlem kaynaktır. Bunları dağıtmak ve aynı sanal makineler kümesini yönetmek için kullanabilirsiniz. Tüm sanal makinelerle aynı ölçek kümeleri sanal makinelerin önceden sağlama gerektirmeyen yapılandırılmış. Big compute, büyük veri ve kapsayıcılı iş yükleri hedef büyük ölçekli hizmetler oluşturmak kolaydır.

Bu konu Azure yığın Marketi'nde ölçek kümeleri kullanılabilir hale getirmek sürecinde size kılavuzluk eder. Bu yordamı tamamladıktan sonra kullanıcılarınızın kendi abonelikler için sanal makine ölçek ayarlar ekleyebilirsiniz.

Sanal makine ölçek kümeleri Azure yığında azure'da sanal makine ölçek kümeleri gibidir. Daha fazla bilgi için aşağıdaki videoları bakın:
* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure yığında otomatik ölçek sanal makine ölçek kümeleri desteklemez. Azure yığın portal, Resource Manager şablonları veya PowerShell kullanılarak ayarlanan bir ölçek başka örnekler ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* **PowerShell ve araçları**

   Yükleme ve Azure yığını için yapılandırılmış PowerShell ve Azure yığın araçları. Bkz: [Azure yığınında PowerShell ile başlamak ve çalıştırmak](azure-stack-powershell-configure-quickstart.md).

   Azure yığın araçları yüklendikten sonra aşağıdaki PowerShell modülünü içeri aktardığınız emin olun (göreli yolu. \ComputeAdmin AzureStack araçları ana klasöründe):

        Import-Module .\AzureStack.ComputeAdmin.psm1

* **İşletim sistemi görüntüsü**

   Bir işletim sistemi görüntüsü, Azure yığın Market eklemediyseniz, bkz: [Azure yığın Market Windows Server 2016 VM görüntüsü eklemek](azure-stack-add-default-image.md).

   Linux desteği için Ubuntu Server 16.04 karşıdan yükleyip kullanarak eklemek ```Add-AzsVMImage``` aşağıdaki parametrelerle: ```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```.

## <a name="add-the-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi Ekle

Ortamınız için aşağıdaki PowerShell komut dosyasını düzenleyin ve ardından çalıştırın, Azure yığın Market ayarlamak bir sanal makine ölçek eklemek için. 

``$User``Yönetici portalı'nı bağlanmak için kullandığınız hesaptır. Örneğin, serviceadmin@contoso.onmicrosoft.com.

```
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds
Select-AzureRmProfile -Profile $AzsEnvContext

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
```

## <a name="remove-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Kaldır

Bir sanal makineyi kaldırmak için kümesi galeri öğesi ölçeklendirme, aşağıdaki PowerShell komutunu çalıştırın:

    Remove-AzsVMSSGalleryItem

> [!NOTE]
> Galeri öğesi hemen kaldırılamaz. Marketten kaldırılmadan önce portal birkaç kez yenilemeniz gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığını için sık sorulan sorular](azure-stack-faq.md)

