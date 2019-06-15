---
title: Azure Lab Services Laboratuvar hesaplarını yönetme | Microsoft Docs
description: Bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesaplarını görüntülemek veya bir Azure aboneliğinde bir laboratuvar hesabı silme öğrenin.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 6f283ce007e96547e01a01a3753ddcb60574bfc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412797"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Azure Lab Services Laboratuvar hesaplarını yönetme 
Azure Lab Services içinde bir laboratuvar hesabı sınıf laboratuvarlarını gibi yönetilen Laboratuvar türleri için bir kapsayıcıdır. Yönetici Azure Lab Services ile bir laboratuvar hesabı ayarlama ayarlar ve Laboratuvarları hesabı oluşturabilirsiniz Laboratuvar sahipleri erişim sağlar. Bu makalede bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesaplarını görüntülemek veya bir laboratuvar hesabı silme işlemini açıklamaktadır.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde. Seçin **Laboratuvar hesaplarını** içinde **DEVOPS** bölümü. Yıldız seçerseniz (`*`) yanındaki **Laboratuvar hesaplarını**, eklenir **Sık Kullanılanlar** sol menüde bölümü. Bir sonraki zamandan ve sonraki sürümlerde, seçtiğiniz **Laboratuvar hesaplarını** altında **Sık Kullanılanlar**.

    ![Laboratuvar hesaplarını tüm hizmetleri ->](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Üzerinde **Laboratuvar hesaplarını** sayfasında **Ekle** araç. 

    ![Laboratuvar hesaplarını sayfada Ekle'yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Üzerinde **Laboratuvar hesabı** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. Mevcut bir seçin **paylaşılan görüntü Galerisi** veya bir tane oluşturabilirsiniz. VM şablonu, başkaları tarafından kullanılabilmeleri de paylaşılan görüntü galerisinde kaydedebilirsiniz. Paylaşılan resim galerileri hakkında ayrıntılı bilgi için bkz. [Azure Lab Services paylaşılan görüntü galerisinde kullanabilecek](how-to-use-shared-image-gallery.md).
    6. İçin **eş sanal ağ**, Laboratuvar ağı için bir eş sanal ağ (VNet) seçin. Bu hesap içinde oluşturduğunuz laboratuvarlar, seçilen sanal ağa bağlı ve seçili sanal ağ kaynaklara erişime sahip. 
    7. Belirtin bir **adres aralığı** Vm'leri Laboratuvardaki için. Adres aralığı içinde classless Inter-Domain yönlendirme (CIDR) gösterimi olmalıdır (örnek: 10.20.0.0/23). Bu adres aralığındaki laboratuvarındaki sanal makinelerde oluşturulur. Daha fazla bilgi için [bir adres aralığı, laboratuvarda VM'ler için belirtin](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).    
    8. İçin **Laboratuvar konumunu seçmek için izin Laboratuvar oluşturan** alanında, Laboratuvar için bir konum seçmek için laboratuvar creators isteyip istemediğinizi belirtin. Varsayılan olarak, seçenek devre dışıdır. Bunu devre dışı bırakıldığında, Laboratuvar creators oluşturmakta olduğunuz Laboratuvar için bir konum belirtilemez. Labs Laboratuvar hesabına en yakın coğrafi konumda oluşturulur. Etkinleştirildiğinde, Laboratuvar oluşturan bir laboratuvar oluşturma sırasında bir konum seçebilirsiniz.      
    9. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Seçin **zil simgesine** araç çubuğunda (**bildirimleri**), dağıtım başarılı oldu ve ardından onaylamak **kaynağa Git**. 

    Alternatif olarak, seçin **Yenile** üzerinde **Laboratuvar hesaplarını** sayfasında ve oluşturduğunuz Laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Laboratuvar hesaplarını görüntüle
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm kaynakları** menüsünde. 
3. Seçin **Laboratuvar hesaplarını** için **türü**. 
    Ayrıca, abonelik, kaynak grubu, konumları ve etiketleri göre de filtreleyebilirsiniz. 

    ![Tüm kaynaklar Laboratuvar hesaplarını ->](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Görüntüleme ve Labs'de bir laboratuvar hesabı yönetme

1. Üzerinde **Laboratuvar hesabı** sayfasında **Labs** sol menüsünde.

    ![Hesap Labs'de](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Gördüğünüz bir **labs listesi** hesabında aşağıdaki bilgilerle: 
    1. Laboratuvar adı.
    2. Laboratuvar oluşturulduğu tarih. 
    3. Laboratuvar oluşturan kullanıcının e-posta adresi. 
    4. Laboratuvarda izin verilen kullanıcıların sayısı. 
    5. Laboratuvar durumu. 

## <a name="delete-a-lab-in-the-lab-account"></a>Bir laboratuvar hesabı laboratuvarda Sil
Labs'de bir laboratuvar hesabı listesini görmek için önceki bölümdeki yönergeleri uygulayın.

1. Seçin **... (üç nokta)** seçip **Sil**. 

    ![Bir laboratuvar delete - düğme](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Seçin **Evet** uyarı iletisinde. 

    ![Laboratuvar Silmeyi Onayla](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Bir laboratuvar hesabı Sil
Laboratuvar hesaplarını bir listede görüntüler önceki bölümde yönergeleri izleyin. Bir laboratuvar hesabı silmek için aşağıdaki yönergeleri kullanın: 

1. Seçin **Laboratuvar hesabı** silmek istediğiniz. 
2. Seçin **Sil** araç çubuğundan. 

    ![Laboratuvar hesaplarını Sil düğmesini ->](../media/how-to-manage-lab-accounts/delete-button.png)
1. Tür **Evet** onay.
1. **Sil**’i seçin. 

    ![Laboratuvar hesabı - onayını Sil](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)


## <a name="next-steps"></a>Sonraki adımlar
Şu makaleye bakın: [Laboratuvar hesaplarını yapılandırmak nasıl](how-to-configure-lab-accounts.md).