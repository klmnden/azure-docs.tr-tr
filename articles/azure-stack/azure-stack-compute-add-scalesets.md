---
title: Sanal makine ölçek kümeleri Azure Stack kullanılabilmesini | Microsoft Docs
description: Bir bulut operatörü sanal makine ölçek kümeleri için Azure Stack Marketini nasıl ekleyebileceğinizi öğrenin
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 06/05/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: 4e77e187d969af7ea2a12754b18d4a218daceed6
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39411915"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Sanal makine ölçek kümeleri Azure Stack'te kullanılabilir yapın

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Sanal makine ölçek kümeleri Azure Stack işlem kaynaklarıdır. Bir özdeş sanal makine kümesini dağıtıp yönetmek için kullanabilirsiniz. Ölçek kümeleri, sanal makinelerin önceden sağlama gerektirmeyen tüm sanal makinelerle aynı şekilde yapılandırılmış. Büyük işlem, büyük veri ve kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolaylaştırır.

Bu makalede ölçek kümeleri Azure Stack Market'te kullanılabilir hale getirmek için işleminde size rehberlik eder. Bu yordamı tamamladıktan sonra kullanıcılarınızın abonelikleri için sanal makine ölçek kümeleri ekleyebilirsiniz.

Sanal makine ölçek kümeleri Azure üzerinde sanal makine ölçek kümeleri Azure Stack'te gibidir. Daha fazla bilgi için aşağıdaki videolarda bakın:
* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure Stack üzerinde sanal makine ölçek kümelerini otomatik ölçeklendirme desteklemez. Daha fazla örnek Resource Manager şablonları, CLI veya PowerShell kullanarak bir ölçek kümesine ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- **Market**  
    Azure Stack öğeleri Market'te kullanılabilirliğini etkinleştirmek için genel Azure ile kaydedin. Bölümündeki yönergeleri [kaydetme Azure Stack Azure ile](azure-stack-registration.md).
- **İşletim sistemi görüntüsü**  
    Bir işletim sistemi görüntüsü için Azure Stack Marketini eklemediniz olup [Azure'dan bir Azure Stack Market öğesi ekleme](asdk/asdk-marketplace-item.md).

## <a name="add-the-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi ekleme

1. Azure Stack Marketini açın ve Azure'a bağlanın. Seçin **Market Yönetim**> **+ Ekle azure'dan**.

    ![Market Yönetimi](media/azure-stack-compute-add-scalesets/image01.png)

2. Ekleyin ve sanal makine ölçek kümesi Market öğesi indirin.

    ![Sanal Makine Ölçek Kümesi](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi'ndeki görüntüleri güncelleştirme

Bir sanal makine ölçek kümesi oluşturduktan sonra kullanıcılar görüntüleri ölçek kümesindeki ölçek oluşturulması gerek kalmadan kümesini güncelleştirebilirsiniz. İşlem bir görüntüsünü güncelleştirmek için aşağıdaki senaryoları bağlıdır:

1. Sanal makine ölçek kümesi dağıtım şablonu **son belirtir** için *sürüm*:  

   Zaman *sürüm* olarak ayarlandığından **son** içinde *Imagereference* bölümünde şablonu bir ölçek kümesi, en yeni sürüme ölçek kümesi kullanımı üzerinde işlem ölçeğini görüntünün ölçek kümesi örnekleri. Ölçeği artırma işlemi tamamlandıktan sonra eski sanal makine ölçek kümesi örneklerine silebilirsiniz.  (Değerlerini *yayımcı*, *teklif*, ve *sku* değişmeden kalır). 

   Belirten bir örnek verilmiştir *son*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Ölçeği artırma yeni görüntüsünü kullanabilmeniz için bu yeni görüntüyü karşıdan yüklemeniz gerekir:  

   - Ölçek kümesindeki bir görüntüden yeni bir sürüme olduğunda görüntü Marketi'nde: eski görüntünün yerini alan yeni bir görüntü indirin. Görüntü değiştirilir sonra bir kullanıcı ölçeklendirilebilecek şekilde geçebilirsiniz. 

   - Market'te görüntü sürümü olduğunda ölçek kümesindeki bir görüntü ile aynı: ölçek kümesindeki kullanılıyor seçerek görüntüyü silin ve ardından yeni bir görüntü indirin. Özgün resmin kaldırılmasını ve yeni görüntüyü indirilmesini arasındaki süre boyunca, ölçeği olamaz. 
      
     Bu işlem gereklidir resyndicate için 1803 sürümü ile sunulan seyrek dosya biçimi olun görüntüleri kullanın. 
 

2. Sanal makine ölçek kümesi dağıtım şablonu **son belirtmiyor** için *sürüm* ve bunun yerine bir sürüm numarası belirtir:  

    Bir görüntü (kullanılabilir sürüm Değişeni) daha yeni bir sürümüyle yüklerseniz, Ölçek kümesinin ölçeği olamaz. Ölçek kümesi şablonunuzda belirtilen görüntü sürümü kullanılabilir olması gerekir, bu tasarım gereğidir.  

Daha fazla bilgi için [işletim sistemi diskleri ve görüntüleri](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Kaldır

Bir sanal makineyi kaldırmak için ölçek kümesi galeri öğesi, aşağıdaki PowerShell komutunu çalıştırın:

```PowerShell  
    Remove-AzsGalleryItem
````

> [!NOTE]
> Galeri öğesi hemen kaldırılmayabilir. Gece, gibi marketten kaldırılan öğeyi gösterir önce birkaç kez portalı yenilemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack için sık sorulan sorular](azure-stack-faq.md)