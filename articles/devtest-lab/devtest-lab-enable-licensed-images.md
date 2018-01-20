---
title: "Azure DevTest Labs laboratuvarınızda lisanslı bir görüntüde etkinleştirme | Microsoft Docs"
description: "Azure portalını kullanarak Azure DevTest Labs lisanslı bir görüntüde etkinleştirmeyi öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: v-craic
ms.openlocfilehash: 022b6340a8d2748624ba292fb4a28a956d28c6f9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarınızda lisanslı bir görüntüde etkinleştir

Azure DevTest Labs'de lisanslı bir görüntü hüküm ve koşulları – genellikle görüntünün laboratuvara kullanıcılar için erişilebilir hale gelmeden önce kabul edilmesi gereken bir üçüncü taraf – içeren biridir. Aşağıdaki bölümlerde, sanal makineler oluşturmak için kullanılabilir; böylece lisanslı görüntülerle çalışma konusunda açıklanmaktadır.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Lisanslı bir görüntü kullanıcılar için kullanılabilir olup olmadığını belirleme
Kullanıcıların lisanslı bir görüntüden sanal makineleri oluşturmasını sağlayan ilk adımı, hüküm ve koşulları için lisanslı görüntü benimsenmiş emin olmaktır. Aşağıdaki adımlar nasıl lisanslı bir görüntü teklif durumunu görüntüleyebilir ve, gerekirse, onun hüküm ve koşulları kabul gösterir.

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Sol bölmede altında **ayarları**seçin **yapılandırma ve ilkeleri**.

1. Sol bölmede altında **sanal makine TABANLARI**seçin **Market görüntülerini**. 

    ![Market görüntülerini menü öğesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Dahil olmak üzere tüm kullanılabilir Market görüntülerini listesi gösterilir **teklif durumu** her görüntü için.

    ![Her görüntü için teklif durumu gösteren Market görüntülerini listesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Lisanslı bir görüntü bir teklif durumunu gösterir 
    
    - **Koşulları kabul:** lisanslı görüntü VM'ler oluşturmak için kullanıcılar tarafından kullanılabilir. 
    - **Gözden geçirme gereken koşulları:** lisanslı görüntü kullanıcılar için şu anda kullanılabilir değil. Laboratuvar kullanıcıların sanal makineleri oluşturmak için kullanmadan önce hüküm ve lisans koşullarını kabul edilmesi gerekir. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Lisanslı bir görüntü Laboratuvar kullanıcılar için kullanılabilir hale getirme
Lisanslı bir görüntü Laboratuvar kullanıcılar için kullanılabilir olduğundan emin olmak için yönetici izinlerine sahip bir laboratuvar sahibi önce hüküm ve koşulları lisanslı bu görüntü için kabul etmeniz gerekir. Lisanslı bir görüntüyle otomatik olarak ilişkili abonelik için programlı dağıtımı etkinleştirmek, yasal koşulları ve gizlilik bildirimlerini bu görüntü için kabul eder. [Azure Resource Manager Market görüntülerle çalışma](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) Market görüntülerini programlı dağıtımı hakkında ek bilgi sağlar.

Aşağıdaki adımları izleyerek lisanslı bir görüntü için programlı dağıtımı etkinleştirebilirsiniz:

1. İçinde [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)listesine gidin **Market görüntülerini**.

1. Hangi kullanıcıların erişim sahibi olmasını istediğiniz ancak, koşulları kabul edilmemiş için lisanslı bir görüntü tanımlayın. Örneğin, ya da durumunu gösteren bir veri bilimi sanal makine görebilirsiniz **koşullarını kabul** veya **koşulları gözden geçirme gereken**.

    ![Programlamayla dağıtımın penceresini yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Veri bilimi VM'ler veri analizi, machine learning ve AI eğitim için yaygın olarak kullanılan birkaç popüler araçları ile test önceden yüklenmiş ve yapılandırılmış Azure sanal makine görüntüleri, bağımsızdır. [Linux ve Windows için Azure veri bilimi sanal makine için giriş](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) büyük bir bölümünü DSVMs hakkında bilgi sağlar.
   >
   >

1. İçinde **teklif durumu** sütunu select görüntü için **koşulları gözden geçirme gereken**.

1. Programlı dağıtımı Yapılandır penceresinde seçin **etkinleştirmek**.

    ![Programlamayla dağıtımın penceresini yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > Birden çok abonelik programlı dağıtımı Yapılandır penceresinde listelenen görebilirsiniz. Yalnızca hedeflenen abonelik için programlı dağıtımı etkinleştirmek emin olun.
   >
   >


1. **Kaydet**’i seçin. 

    Market görüntülerini listesinde şimdi gösterir görüntüye **koşullarını kabul** ve sanal makineler oluşturmak kullanıcılar için kullanılabilir.

> [!NOTE]
> Kullanıcılar, lisanslı bir görüntüden özel bir görüntü oluşturabilirsiniz. Bkz: [bir VHD dosyasındaki özel bir görüntü oluşturun](devtest-lab-create-template.md) daha fazla bilgi için.
>
>


## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Bir sanal makineden özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [Bir VHD dosyasındaki özel bir görüntü oluşturun](devtest-lab-create-template.md)
- [Laboratuvarınızı için bir VM ekleme](devtest-lab-add-vm.md)