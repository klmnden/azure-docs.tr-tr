---
title: "Bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs"
description: "Azure DevTest Labs Azure portalını kullanarak bir VHD'yi dosyasından içinde özel bir görüntü oluşturmayı öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: v-craic
ms.openlocfilehash: d1f1b9948fb591484c107818a01e141932effbba
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
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

1. Özel görüntü oluşturmak için kullanılan görüntü, Microsoft tarafından yayımlanmayan varsa planı adı, planı teklif ve planı yayımcı da girebilirsiniz.

1. Seçin **Tamam** özel görüntüsü oluşturulamadı.

Birkaç dakika sonra özel görüntü oluşturulur ve Laboratuvar ait depolama hesabı içinde depolanır. Yeni bir VM oluşturmak bir laboratuvar kullanıcı istediği zaman, görüntü temel görüntü listesinde tarafından kullanılabilir.

![Özel görüntü temel görüntü listesinde kullanılabilir](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızı için bir VM ekleme](./devtest-lab-add-vm.md)
