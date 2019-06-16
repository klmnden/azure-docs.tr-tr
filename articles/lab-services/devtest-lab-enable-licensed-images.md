---
title: Azure DevTest labs'deki laboratuvarınızda lisanslı görüntü etkinleştirme | Microsoft Docs
description: Azure portalını kullanarak Azure DevTest labs'teki lisanslı görüntü etkinleştirme hakkında bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 11b6553fe8aceef0d3d15977998dd870c275128a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61294344"
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki laboratuvarınızda lisanslı görüntü etkinleştirme

Azure DevTest Labs'de lisanslı görüntü hüküm ve koşulları – genellikle görüntünün Laboratuvar kullanıcıların erişebileceği önce bu kabul edilmesi gereken bir üçüncü taraf – içeren biridir. Aşağıdaki bölümlerde, sanal makineler oluşturmak için kullanılacak kullanılabilir lisanslı görüntüler ile çalışacak şekilde açıklanmıştır.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Lisanslı görüntü kullanıcılar için kullanılabilir olup olmadığını belirleme
Lisanslı bir görüntüden VM oluşturma izin vererek ilk adımı, hüküm ve koşulları için lisanslı görüntü benimsenmiş emin olmaktır. Aşağıdaki adımlar, nasıl lisanslı görüntü teklifi durumunu görüntüleyebilir ve, gerekirse, sözleşmenin hüküm ve koşulları kabul gösterir.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Sol bölmede altında **ayarları**seçin **yapılandırması ve ilkelerini**.

1. Sol bölmede altında **sanal makine TABANLARI**seçin **Market görüntüleri**. 

    ![Market görüntüleri menü öğesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Dahil olmak üzere tüm kullanılabilir Market görüntüleri listesi gösterilir **teklif durumu** her görüntü için.

    ![Market görüntüleri gösteren her görüntü için teklif durumu listesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Lisanslı görüntü bir teklif durumu gösterilir. 
    
    - **Koşulları kabul:** lisanslı görüntü sanal makineler oluşturmak için kullanıcılar tarafından kullanılabilir. 
    - **Koşulları gözden geçirme gerekli:** lisanslı görüntü kullanıcılarına şu anda kullanılamıyor. Laboratuvar kullanıcıları sanal makineler oluşturmak için kullanabilmeniz için önce lisans koşullarını ve koşulları kabul edilmesi gerekir. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Lisanslı görüntü Laboratuvar kullanıcıları için kullanılabilir hale getirme
Lisanslı görüntü Laboratuvar kullanıcıları için kullanılabilir olduğundan emin olmak için yönetici izinlerine sahip bir laboratuvar sahibi önce hüküm ve koşulları lisanslı o yansıma için kabul etmeniz gerekir. Programlı dağıtım için lisanslı bir görüntü ile otomatik olarak ilişkili aboneliği etkinleştirme, yasal koşulları ve bu görüntüyü yönelik gizlilik bildirimlerini kabul eder. [Market görüntüleri ile Azure Resource Manager'a bağlı çalışma](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) Market görüntüleri programlamalı dağıtım hakkında ek bilgi sağlar.

Lisanslı görüntü için programlamalı dağıtım aşağıdaki adımları izleyerek etkinleştirebilirsiniz:

1. İçinde [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)listesine gidin **Market görüntüleri**.

1. Lisanslı görüntü kullanıcıların erişim sahibi izin istiyor ancak, koşulları kabul edilmemiş belirleyin. Örneğin, ya da durumunu gösteren bir veri bilimi sanal makinesi görebilirsiniz **koşulları kabul** veya **koşulları gözden geçirme gerekli**.

    ![Programlı dağıtım penceresine yapılandırın](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Veri bilimi sanal makineleri önceden yüklenmiş, yapılandırılmış ve veri analizi, makine öğrenimi ve yapay ZEKA eğitimi için yaygın olarak kullanılan çeşitli popüler araçlarla test edilen Azure sanal makinesi görüntüleridir. [Linux ve Windows için Azure veri bilimi sanal Makinesi'ne giriş](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) büyük ölçüde Dsvm'leri hakkında bilgi sağlar.
   >
   >

1. İçinde **teklif durumu** seçme görüntüsü için sütun **koşulları gözden geçirme gerekli**.

1. Programlamalı dağıtımı Yapılandır penceresinde **etkinleştirme**.

    ![Programlı dağıtım penceresine yapılandırın](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > Programlamalı dağıtımı Yapılandır penceresinde listelenen birden çok abonelik görebilirsiniz. Yalnızca hedeflenen abonelik için programlamalı dağıtım etkinleştiriyorsanız emin olun.
   >
   >


1. **Kaydet**’i seçin. 

    Market görüntüleri listesinde, görüntüde artık **koşulları kabul** ve sanal makineler oluşturmak, kullanıcılar için kullanılabilir.

> [!NOTE]
> Kullanıcıların lisanslı bir görüntüyü özel görüntü oluşturabilirsiniz. Bkz: [bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-template.md) daha fazla bilgi için.
>
>


## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [VM'den özel görüntü oluşturma](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [Bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-template.md)
- [Laboratuvarınız için bir VM ekleme](devtest-lab-add-vm.md)