---
title: Azure DevTest Labs özel görüntü VHD dosyasından oluşturma | Microsoft Docs
description: Azure DevTest labs'teki Azure portalını kullanarak bir VHD dosyasından özel bir görüntü oluşturmayı öğrenin
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
ms.openlocfilehash: 853c138c8cf73b41b0cebb6c1d349865e18eab6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61296167"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasından özel bir görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, Azure portalını kullanarak bir VHD dosyasından bir özel görüntü oluşturma işleminde size yol:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar ana bölmeden **yapılandırması ve ilkelerini**. 

1. Üzerinde **yapılandırması ve ilkelerini** bölmesinde **özel görüntüleri**.

1. Üzerinde **özel görüntüleri** bölmesinde **+ Ekle**.

    ![Özel görüntü ekleme](./media/devtest-lab-create-template/add-custom-image.png)

1. Özel görüntü adını girin. Bu ad, bir VM oluştururken, temel görüntüleri listesinde görüntülenir.

1. Özel görüntü açıklamasını girin. Bu açıklama, VM oluşturduğunuz sırada temel görüntüleri listesinde görüntülenir.

1. İçin **işletim sistemi türü**, şunlardan birini seçin **Windows** veya **Linux**.

    - Seçerseniz **Windows**, onay kutusu belirtin olmadığını *sysprep* makinede çalıştırın. 
    - Seçerseniz **Linux**, onay kutusu belirtin olmadığını *sağlamayı kaldırma* makinede çalıştırın. 

1. Seçin bir **VHD** aşağı açılan menüden. Yeni özel görüntü oluşturmak için kullanılan VHD budur. İçin gerekirse seçin **PowerShell kullanarak bir VHD'yi karşıya yükleme**.

1. Özel görüntü oluşturma için kullanılan görüntü (Microsoft tarafından yayımlanan) lisanslı bir görüntü değilse bir plan adı, teklif ve plan yayımcı girebilirsiniz.

   - **Plan adı:** Bu özel görüntüyü oluşturan gelen Market görüntüsü (SKU) adını girin 
   - **Teklif:** Bu özel görüntüyü oluşturulduğu Market görüntüsü ürününü (teklif) girin 
   - **Yayımcı planlayın:** Bu özel görüntüyü oluşturulduğu Market görüntü yayımcısı girin

   > [!NOTE]
   > Özel bir görüntü oluşturmak için kullandığınız görüntü olup olmadığını **değil** lisanslı görüntü daha sonra bu alanları boş olan ve seçerseniz doldurulabilir. Varsa görüntüyü **olduğu** lisanslı görüntü ve alanların otomatik plan bilgileriyle doldurulmuş olan. Bu durumda değiştirmeye çalışırsanız, bir uyarı iletisi görüntülenir.
   >
   >

1. Seçin **Tamam** özel görüntü oluşturmak için.

Birkaç dakika sonra özel bir görüntü oluşturulur ve Laboratuvar depolama hesabı içinde depolanır. Yeni bir VM oluşturmak bir laboratuvar kullanıcı istediği zaman, temel görüntüleri listesinde kullanılabilir bir görüntüsüdür.

![Temel görüntüleri listesinde kullanılabilen özel görüntü](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınız için bir VM ekleme](./devtest-lab-add-vm.md)
