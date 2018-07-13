---
title: Azure DevTest Labs özel görüntü bir VM oluşturma | Microsoft Docs
description: Sağlanan VM'den Azure portalını kullanarak Azure DevTest labs'deki bir özel görüntü oluşturmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 22f1579b2df2acdc736ed4c1d5cee64d096c320a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38635346"
---
# <a name="create-a-custom-image-from-a-vm"></a>VM'den özel görüntü oluşturma

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Sağlanan VM'den özel görüntü oluşturma ve daha sonra bu özel görüntüyü, birbirinin aynısı olan Vm'leri oluşturmak için kullanın. Aşağıdaki adımlar, bir VM'den özel görüntü oluşturma işlemini göstermektedir:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar ana bölmeden **sanal makinelerim**.
 
1. Üzerinde **sanal makinelerim** bölmesinde, özel görüntü oluşturmak istediğiniz VM'yi seçin.

1. Sanal makinenin yönetim bölmeden **özel görüntü oluşturma (VHD)**.

    ![Menü öğesi özel görüntü oluşturma](./media/devtest-lab-create-template/create-custom-image.png)

1. Üzerinde **özel görüntü** bölmesinde, bir ad ve özel görüntü için açıklama girin. Bir VM oluşturduğunuzda, bu bilgileri tabanları listesinde görüntülenir. Özel görüntü, işletim sistemi diski ve sanal makineye bağlı veri diskleri dahil edilir.

    ![Özel görüntü bölmesinde oluşturma](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Sanal makinede Sysprep çalıştırıldığı olup olmadığını seçin. Sanal makinede sysprep çalıştırılmadı, sysprep bu özel görüntüden bir VM oluşturulduğunda çalıştırmak isteyip istemediğinizi belirtin.

1. Seçin **Tamam** özel görüntü oluşturma tamamlandığında.

Birkaç dakika sonra özel bir görüntü oluşturulur ve Laboratuvar depolama hesabı içinde depolanır. Yeni bir VM oluşturmak bir laboratuvar kullanıcı istediği zaman, temel görüntüleri listesinde kullanılabilir bir görüntüsüdür.

![Temel görüntüleri listesinde kullanılabilen özel görüntü](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınız için bir VM ekleme](devtest-lab-add-vm.md)
