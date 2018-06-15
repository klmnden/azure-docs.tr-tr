---
title: Bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs
description: Azure DevTest Labs Azure portalını kullanarak bir VHD'yi dosyasından içinde özel bir görüntü oluşturmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: a961565815ca0d89dc98a8d6a3e14b338b649398
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787468"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasındaki özel bir görüntü oluşturun

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, Azure portalını kullanarak bir VHD'yi dosyasından özel görüntü oluşturmada size yol:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar ait ana bölmede, seçin **yapılandırma ve ilkeleri**. 

1. Üzerinde **yapılandırma ve ilkeleri** bölmesinde, **özel görüntüleri**.

1. Üzerinde **özel görüntüleri** bölmesinde, **+ Ekle**.

    ![Özel görüntü ekleme](./media/devtest-lab-create-template/add-custom-image.png)

1. Özel görüntü adı girin. Bu ad, bir VM oluşturulurken temel görüntü listesinde görüntülenir.

1. Özel görüntü açıklamasını girin. Bu açıklama, bir VM oluşturulurken temel görüntü listesinde görüntülenir.

1. İçin **işletim sistemi türü**, şunlardan birini seçin **Windows** veya **Linux**.

    - Seçerseniz **Windows**, onay kutusu belirtin olup olmadığını *sysprep* makinede çalıştırın. 
    - Seçerseniz **Linux**, onay kutusu belirtin olup olmadığını *deprovision* makinede çalıştırın. 

1. Seçin bir **VHD** açılır menüsünden. Yeni özel görüntü oluşturmak için kullanılan VHD budur. Gerekirse, seçin **PowerShell kullanarak bir VHD'yi karşıya**.

1. Özel görüntü oluşturmak için kullanılan görüntü (Microsoft tarafından yayımlanan) lisanslı bir görüntü değilse plan adı, planı teklif ve planı yayımcı da girebilirsiniz.

   - **Plan adı:** bu özel görüntü oluşturulurken gelen Market görüntüsü (SKU) adını girin 
   - **Teklif planlama:** bu özel görüntü oluşturulduğu Market görüntüsü ürününü (teklif) girin 
   - **Yayımcı planlama:** bu özel görüntü oluşturulduğu Market görüntüsü yayımcısı girin

   > [!NOTE]
   > Özel bir görüntü oluşturmak için kullandığınız görüntü olup olmadığını **değil** lisanslı bir görüntü, ardından bu alanları boş ve seçerseniz doldurulabilir. Varsa görüntü **olan** lisanslı bir görüntü sonra alanları planı bilgilerle doldurulmuş otomatik ayarlanır. Bu durumda değiştirmeye çalışırsanız, bir uyarı iletisi görüntülenir.
   >
   >

1. Seçin **Tamam** özel görüntüsü oluşturulamadı.

Birkaç dakika sonra özel görüntü oluşturulur ve Laboratuvar ait depolama hesabı içinde depolanır. Yeni bir VM oluşturmak bir laboratuvar kullanıcı istediği zaman, görüntü temel görüntü listesinde tarafından kullanılabilir.

![Özel görüntü temel görüntü listesinde kullanılabilir](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızı için bir VM ekleme](./devtest-lab-add-vm.md)
