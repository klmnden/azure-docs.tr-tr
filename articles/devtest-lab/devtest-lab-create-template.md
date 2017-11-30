---
title: "Bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs"
description: "Azure DevTest Labs Azure portalını kullanarak bir VHD'yi dosyasından içinde özel bir görüntü oluşturmayı öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: a421327ab8794315005327833b873dc5ebd57e9e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasındaki özel bir görüntü oluşturun

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, Azure portalını kullanarak bir VHD'yi dosyasından özel görüntü oluşturmada size yol:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **More services**’i (Daha fazla hizmet’i) seçip ardından listeden **DevTest Labs**’i seçin.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar 's dikey penceresinde, seçin **yapılandırma**. 

1. Laboratuvar üzerinde **yapılandırma** dikey penceresinde, select **özel görüntülerini (VHD)**.

1. Üzerinde **özel görüntüleri** dikey penceresinde, select **+ Ekle**.

    ![Özel görüntü ekleme](./media/devtest-lab-create-template/add-custom-image.png)

1. Özel görüntü adı girin. Bu ad, bir VM oluşturulurken temel görüntü listesinde görüntülenir.

1. Özel görüntü açıklamasını girin. Bu açıklama, bir VM oluşturulurken temel görüntü listesinde görüntülenir.

1. Seçin **VHD**.

1. Gelen **VHD** dikey penceresinde istenen VHD dosyasını seçin.

1. Seçin **Tamam** kapatmak için **VHD** dikey.

1. Seçin **işletim sistemi yapılandırması**.

1. Üzerinde **işletim sistemi yapılandırması** sekmesinde, ya da seçin **Windows** veya **Linux**.

1. Varsa **Windows** olan onay kutusunu seçildiyse olup olmadığını *Sysprep* makinede çalıştırın. 

1. Seçin **Tamam** kapatmak için **işletim sistemi yapılandırması** dikey.

1. Seçin **Tamam** özel görüntüsü oluşturulamadı.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızı için bir VM ekleme](./devtest-lab-add-vm.md)
