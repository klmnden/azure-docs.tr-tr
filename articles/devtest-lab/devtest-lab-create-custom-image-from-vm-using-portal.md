---
title: "Bir sanal makineden Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs"
description: "Azure DevTest Labs Azure portalını kullanarak sağlanan bir VM'den içinde özel bir görüntü oluşturmayı öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Bir sanal makineden özel bir görüntü oluşturun

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Sağlanan bir sanal makineden özel bir görüntü oluşturun ve daha sonra aynı VM'ler oluşturmak için özel görüntü kullanın. Aşağıdaki adımlar, bir VM'den özel bir görüntü oluşturmak nasıl çalışılacağını:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **More services**’i (Daha fazla hizmet’i) seçip ardından listeden **DevTest Labs**’i seçin.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar 's dikey penceresinde, seçin **My sanal makineleri**.
 
1. Üzerinde **My sanal makineleri** dikey penceresinde istediğiniz özel görüntü oluşturmak VM seçin.

1. VM'ın dikey penceresinde, seçin **oluşturma özel görüntü (VHD)**.

    ![Özel görüntü menü öğesi oluşturma](./media/devtest-lab-create-template/create-custom-image.png)

1. Üzerinde **oluşturma görüntü** dikey penceresinde, bir ad ve özel görüntünüzü açıklamasını girin. Bir VM oluşturduğunuzda, bu bilgileri tabanları listesinde görüntülenir.

    ![Özel görüntü dikey penceresi oluşturma](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Sysprep VM üzerinde çalışan olup olmadığını seçin. Sysprep VM çalıştırılmadı, sysprep bu özel görüntüsünü bir VM oluşturulduğunda çalıştırmak isteyip istemediğinizi belirtin.

1. Seçin **Tamam** özel görüntü oluşturmak için bitirdikten sonra.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızı için bir VM ekleme](./devtest-lab-add-vm-with-artifacts.md)
