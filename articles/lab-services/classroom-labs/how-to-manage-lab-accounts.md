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
ms.date: 02/07/2018
ms.author: spelluru
ms.openlocfilehash: 6cd06778ad54fa698c5bc2fe4ccf02f4be2ee2ec
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807073"
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
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Seçin **zil simgesine** araç çubuğunda (**bildirimleri**), dağıtım başarılı oldu ve ardından onaylamak **kaynağa Git**. 

    Alternatif olarak, seçin **Yenile** üzerinde **Laboratuvar hesaplarını** sayfasında ve oluşturduğunuz Laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

1. Üzerinde **Laboratuvar hesabı** sayfasında **erişim denetimi (IAM)**, tıklatıp **+ rol ataması Ekle** araç. 

    ![Erişim Denetimi'ne rol ataması Ekle düğmesi](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Üzerinde **rol ataması Ekle** sayfasında **Laboratuvar oluşturan** için **rol**, Laboratuvar Creators role ekleyin ve istediğiniz kullanıcıyı seçin **Kaydet**. 

    ![Laboratuvar oluşturan Ekle](../media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="specify-marketplace-images-available-to-lab-creators"></a>Market görüntüleri için laboratuvar creators kullanılabilir belirtin
Laboratuvar sahibi olarak laboratuvar oluşturucuların laboratuvar hesabında laboratuvar oluşturmak için kullanacağı Market görüntülerini belirtebilirsiniz. 

1. Sol taraftaki menüden **Market görüntüleri**'ni seçin. Listede varsayılan olarak tüm görüntüler (hem etkin hem devre dışı) yer alır. Yalnızca etkin/devre dışı görüntüleri görmek için en üstteki açılan listeden **Yalnızca etkin**/**Yalnızca devre dışı** seçeneğini belirleyerek listeyi filtreleyebilirsiniz. 
    
    ![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Listede görüntülenen Market görüntüleri yalnızca aşağıdaki koşulları karşılayan görüntülerdir:
        
    - Tek bir VM oluşturur.
    - VM'leri sağlamak için Azure Resource Manager'ı kullanır
    - Ek lisans planı satın alınmasını gerektirmez
2. Etkin durumdaki bir Market görüntüsünü **devre dışı bırakmak** için aşağıdaki eylemlerden birini gerçekleştirin: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü devre dışı bırak**'ı seçin. 

        ![Tek bir görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri devre dışı bırak**'a tıklayın. 

        ![Birden fazla görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Benzer şekilde bir Market görüntüsünü **etkinleştirmek** için aşağıdaki eylemlerden birini gerçekleştirebilirsiniz: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü etkinleştir**'i seçin. 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri etkinleştir**'e tıklayın. 

## <a name="view-lab-accounts"></a>Laboratuvar hesaplarını görüntüle
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm kaynakları** menüsünde. 
3. Seçin **Laboratuvar hesaplarını** için **türü**. 
    Ayrıca, abonelik, kaynak grubu, konumları ve etiketleri göre de filtreleyebilirsiniz. 

    ![Tüm kaynaklar Laboratuvar hesaplarını ->](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)


## <a name="delete-a-lab-account"></a>Bir laboratuvar hesabı Sil
Laboratuvar hesaplarını bir listede görüntüler önceki bölümde yönergeleri izleyin. Bir laboratuvar hesabı silmek için aşağıdaki yönergeleri kullanın: 

1. Seçin **Laboratuvar hesabı** silmek istediğiniz. 
2. Seçin **Sil** araç çubuğundan. 

    ![Laboratuvar hesaplarını Sil düğmesini ->](../media/how-to-manage-lab-accounts/delete-button.png)
1. Tür **Evet** onay.
1. **Sil**’i seçin. 

    ![Laboratuvar hesabı - onayını Sil](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

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

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)