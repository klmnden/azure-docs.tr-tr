---
title: Sanal makine ölçek kümeleri Azure Stack kullanılabilmesini | Microsoft Docs
description: Bir bulut operatörü sanal makine ölçek kümeleri için Azure Stack Marketini nasıl ekleyebileceğinizi öğrenin
services: azure-stack
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: d7b2c0a39d6d7287b3f956d824239a40e373ea36
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594772"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Sanal makine ölçek kümeleri Azure Stack'te kullanılabilir yapın

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*
  
Sanal makine ölçek kümeleri Azure Stack işlem kaynaklarıdır. Bir özdeş sanal makine kümesini dağıtıp yönetmek için kullanabilirsiniz. Tüm aynı şekilde yapılandırılmış sanal makineler ile ölçek kümeleri, sanal makinelerin önceden hazırlanması gerekmez. Büyük işlem, büyük veri ve kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolaylaştırır.

Bu makalede ölçek kümeleri Azure Stack Market'te kullanılabilir hale getirmek için işleminde size rehberlik eder. Bu yordamı tamamladıktan sonra kullanıcılarınızın abonelikleri için sanal makine ölçek kümeleri ekleyebilirsiniz.

Azure Stack üzerinde sanal makine ölçek kümeleri, sanal makine ölçek kümeleri Azure üzerinde benzerdir. Daha fazla bilgi için aşağıdaki videolarda bakın:

* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure Stack üzerinde sanal makine ölçek kümelerini otomatik ölçeklendirme desteklemez. Daha fazla örnek Resource Manager şablonları, CLI veya PowerShell kullanarak bir ölçek kümesine ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* **Market:** Azure Stack öğeleri Market'te kullanılabilirliğini etkinleştirmek için genel Azure ile kaydedin. Bölümündeki yönergeleri [kaydetme Azure Stack Azure ile](azure-stack-registration.md).
* **İşletim sistemi yansıması:** Bir sanal makine ölçek kümesi (VMSS) oluşturulabilmesi için önce VMSS kullanmak için VM görüntüleri indirin [Azure Stack Marketini](azure-stack-download-azure-marketplace-item.md). Görüntüleri zaten bir kullanıcı yeni bir VMSS oluşturabilmeniz için önce mevcut olması gerekir.

## <a name="use-the-azure-stack-portal"></a>Azure Stack portalını kullanın

>[!IMPORTANT]  
> Bu bölümdeki bilgiler, Azure Stack 1808 veya sonraki bir sürümü kullandığınızda uygulanır. Sürümünüzü 1807 veya önceki bir sürümü olup olmadığını [(önce 1808) sanal makine ölçek kümesi ekleme](#add-the-virtual-machine-scale-set-prior-to-version-1808).

1. Azure Stack portalında oturum açın. Ardından, Git **tüm hizmetleri**, ardından **sanal makine ölçek kümeleri**ve ardından altındaki **işlem**seçin **sanal makine ölçek kümeleri**.
   ![Sanal makine ölçek kümeleri](media/azure-stack-compute-add-scalesets/all-services.png)

2. Oluştur'u seçin ***sanal makine ölçek kümeleri***.
   ![Sanal makine ölçek kümesi oluşturma](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. Boş alanları doldurun, için ve pencerelerden seçin **işletim sistemi disk görüntüsü**, **abonelik**, ve **örnek boyutu**. Seçin **Evet** için **yönetilen diskleri kullan**. Sonra, **Oluştur**’a tıklayın.
    ![Oluşturma ve yapılandırma](media/azure-stack-compute-add-scalesets/create.png)

4. Yeni sanal makine ölçek kümesi, Git görmek için **tüm kaynakları**, sanal makine ölçek kümesi adı için arama yapın ve ardından aramaya adını seçin.
   ![Ölçek kümesini görüntüleme](media/azure-stack-compute-add-scalesets/search.png)

## <a name="add-the-virtual-machine-scale-set-prior-to-version-1808"></a>Sanal makine ölçek kümesi (önceki sürüm 1808) Ekle

>[!IMPORTANT]  
> Bu bölümdeki bilgiler, Azure Stack bir sürümünü 1808 önce kullandığınızda uygulanır. 1808 veya sonraki bir sürümü kullanıyorsanız bkz [Azure Stack portalını](#use-the-azure-stack-portal).

1. Azure Stack Marketini açın ve Azure'a bağlanın. Seçin **Market Yönetim**, ardından **+ Ekle azure'dan**.

    ![Market Yönetimi](media/azure-stack-compute-add-scalesets/image01.png)

2. Ekleyin ve sanal makine ölçek kümesi Market öğesi indirin.

    ![Sanal Makine Ölçek Kümesi](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi'ndeki görüntüleri güncelleştirme

Bir sanal makine ölçek kümesi oluşturduktan sonra kullanıcılar görüntüleri ölçek kümesindeki ölçek oluşturulması gerek kalmadan kümesini güncelleştirebilirsiniz. İşlem bir görüntüsünü güncelleştirmek için aşağıdaki senaryoları bağlıdır:

1. Sanal makine ölçek kümesi dağıtım şablonu belirtir **son** için **sürüm**:  

   Zaman `version` ayarlanır **son** içinde `imageReference` bölümünde şablonu bir ölçek kümesi, Ölçek kümesi örnekleri için ölçek kümesi kullanılması ile ilgili işlemler görüntünün kullanılabilir en yeni sürümünü ölçeğini. Ölçek büyütme işlemi tamamlandıktan sonra eski sanal makine ölçek kümesi örneklerine silebilirsiniz. Değerleri `publisher`, `offer`, ve `sku` değişmeden kalır.

   Aşağıdaki JSON örneği belirtir `latest`:  

    ```json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Ölçek büyütme yeni görüntüsünü kullanabilmeniz için bu yeni görüntüyü karşıdan yüklemeniz gerekir:  

   * Görüntü Marketi'nde ölçek kümesindeki bir görüntüden yeni bir sürüme olduğunda eski görüntünün yerini alan yeni bir görüntü indirin. Görüntü değiştirilir sonra bir kullanıcı ölçeklendirilebilecek şekilde geçebilirsiniz.

   * Market'te görüntü sürümü ölçek kümesindeki bir görüntü ile aynı olduğunda, Ölçek kümesindeki kullanılıyor seçerek görüntüyü silin ve ardından yeni görüntüyü indirin. Özgün resmin kaldırılmasını ve yeni görüntüyü indirilmesini arasındaki süre boyunca, ölçeği olamaz.

     Bu işlem gereklidir yeniden yayınlamak için 1803 sürümü ile sunulan seyrek dosya biçimi olun görüntüleri kullanın.

2. Sanal makine ölçek kümesi dağıtım şablonu **son belirtmiyor** için **sürüm** ve bunun yerine bir sürüm numarası belirtir:  

    Bir görüntü (kullanılabilir sürüm Değişeni) daha yeni bir sürümüyle yüklerseniz, Ölçek kümesinin ölçeği olamaz. Ölçek kümesi şablonunuzda belirtilen görüntü sürümü kullanılabilir olmalıdır. Bu, tasarım gereğidir.  

Daha fazla bilgi için [işletim sistemi diskleri ve görüntüleri](./user/azure-stack-compute-overview.md#operating-system-disks-and-images).  

## <a name="scale-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini ölçeklendirme

Boyutu ölçeklendirilebilir bir *sanal makine ölçek kümesi* büyütmek veya küçültmek yapma.  

1. Portalda ölçek kümenizi seçin ve ardından **ölçeklendirme**.

2. Bu sanal makine ölçek kümesi için ölçeklendirme yeni düzeyini ayarlamak için kaydırma çubuğunu kullanın ve ardından **Kaydet**.

     ![Ölçek kümesi](media/azure-stack-compute-add-scalesets/scale.png)

## <a name="remove-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Kaldır

Bir sanal makine ölçek kümesi galeri öğesini kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell  
Remove-AzsGalleryItem
```

> [!NOTE]
> Galeri öğesi hemen kaldırılmayabilir. Marketten kaldırıldı olarak öğeyi gösterir önce birkaç kez portalı yenilemeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)