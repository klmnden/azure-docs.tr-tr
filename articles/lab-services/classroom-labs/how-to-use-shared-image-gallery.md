---
title: Azure Lab Services paylaşılan görüntü galerisinde kullanabilecek | Microsoft Docs
description: Bir kullanıcı başka bir görüntü paylaşın ve başka bir kullanıcı, laboratuvarda bir VM şablonu oluşturmak için görüntü kullanabilirsiniz, paylaşılan görüntü Galerisi kullanmak için bir laboratuvar hesabı yapılandırmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 93136c7d685bd9fc8ec4bcdea3a900b28029059b
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58653219"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisinde kullanın
Bu makalede, Öğretmenler/Laboratuvar Yöneticisi şablonu sanal makine görüntüsü de başkaları tarafından kullanılabilmeleri nasıl tasarruf edebilecekleri gösterilmektedir. Bu görüntüleri bir Azure kaydedilir [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md). İlk adım, Laboratuvar Yöneticisi var olan bir paylaşılan görüntü Galerisine Laboratuvar hesabına ekler. Paylaşılan görüntü Galerisi bağlandıktan sonra labs Laboratuvar hesabında oluşturulan görüntüleri paylaşılan görüntü Galerisine kaydedebilirsiniz. Diğer Öğretmenler, bu görüntü, kendi sınıfları için bir şablon oluşturmak için paylaşılan bir görüntü galerisinden seçebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Kullanarak bir paylaşılan görüntü Galerisi oluşturma [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI](../../virtual-machines/linux/shared-images.md).

## <a name="attach-a-shared-image-gallery-to-a-lab-account"></a>Bir laboratuvar hesabı için paylaşılan görüntü Galerisine ekleme
Aşağıdaki yordam nasıl bir laboratuvar hesabı paylaşılan görüntü Galerisi eklemek gösterir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde. Seçin **Lab Services** içinde **DEVOPS** bölümü. Yıldız seçerseniz (`*`) yanındaki **Lab Services**, eklenir **Sık Kullanılanlar** sol menüde bölümü. Bir sonraki zamandan ve sonraki sürümlerde, seçtiğiniz **Lab Services** altında **Sık Kullanılanlar**.

    ![Lab Services'i tüm hizmetleri ->](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Görmek için laboratuvar hesabınızı seçin **Laboratuvar hesabı** sayfası. 
4. Seçin **paylaşılan görüntü Galerisi** seçin ve soldaki menüden **iliştirme** araç. 

    ![Paylaşılan görüntü Galerisi - düğmesi ekleme](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. Üzerinde **var olan bir paylaşılan görüntü Galerisine ekleme** seçin sayfasında ve paylaşılan görüntü Galerisi **Tamam**.

    ![Mevcut bir galeriyi seçin](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Aşağıdaki ekranı görürsünüz: 

    ![Galerim listesinde](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    Bu örnekte, vardır hiçbir görüntü paylaşılan görüntü galerisinde henüz.

Azure Lab Services kimlik laboratuvara bağlı paylaşılan görüntü Galerisine katkıda bulunan olarak eklenir. Bu, Öğretmenler / sanal makine olarak kaydetmek için BT yöneticisi, paylaşılan görüntü Galerisine görüntüler. Bu Laboratuvar hesabında oluşturulan tüm labs ekli paylaşılan görüntü Galerisine erişebilirsiniz. 

Ekli paylaşılan görüntü Galerisi'ndaki tüm görüntüler varsayılan olarak etkindir. Etkinleştirebilir veya bunları listeden ve kullanarak seçilen görüntüler devre dışı **seçilen görüntüler etkinleştirme** veya **seçilen görüntüler devre dışı** düğmesi. 

## <a name="detach-a-shared-image-gallery"></a>Paylaşılan görüntü Galerisi Ayır
Tek bir paylaşılan görüntü Galerisi laboratuvara eklenebilir. Başka bir paylaşılan görüntü Galerisi eklemek istiyorsanız, önce yeni bir ekleme geçerli bir ayırma. Laboratuvarınızı paylaşılan görüntü galerisinden ayırmak için seçin **ayırma** araç çubuğunda ve ayırma işlemi onaylayın. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü Galerisine bir görüntüyü kaydedin
Paylaşılan görüntü Galerisi bağlandıktan sonra diğer Öğretmen tarafından yeniden kullanılabilir, böylece bir Öğretmen bir şablon görüntüsü paylaşılan görüntü Galerisine kaydedebilirsiniz.

![Sanal makine görüntü Galerisi'nde Kaydet](../media/how-to-use-shared-image-gallery/save-virtual-machine.png)

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Öğretmen/Profesör yeni Laboratuvar oluşturma sırasında şablonu için paylaşılan görüntü galerisinde kullanılabilir özel bir görüntü seçebilirsiniz.

![Galeriden sanal makine görüntüsünü kullanma](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
