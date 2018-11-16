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
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: c672634e79cbc0850edfe7f7fbdb0a880d5cf0d8
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707138"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Azure Lab Services Laboratuvar hesaplarını yönetme 
Azure Lab Services içinde bir laboratuvar hesabı sınıf laboratuvarlarını gibi yönetilen Laboratuvarları için bir kapsayıcıdır. Yönetici Azure Lab Services ile bir laboratuvar hesabı ayarlama ayarlar ve Laboratuvarları hesabı oluşturabilirsiniz Laboratuvar sahipleri erişim sağlar. Bu makalede bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesaplarını görüntülemek veya bir laboratuvar hesabı silme işlemini açıklamaktadır.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u seçin.
3. Azure Market’te **Lab Services** ifadesini aratıp açılan listeden **Lab Services** uygulamasını seçin. 
4. Filtrelenen sonuç listesinden **Lab Services (Önizleme)** öğesini seçin. 
5. **Laboratuvar hesabı oluştur** penceresinde **Oluştur**’u seçin.
7. **Laboratuvar hesabı** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Laboratuvar hesabının sayfasını görmüyorsanız, **bildirimler** düğmesini seçin ve sonra bildirimlerdeki **Kaynağa git** düğmesine tıklayın. 

    ![Laboratuvar hesabı oluştur penceresi](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

1. **Laboratuvar Hesabı** sayfasında **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ Ekle** düğmesine tıklayın. 

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/access-control.png)
2. **İzin ekle** sayfasında, **Rol** için **Laboratuvar Oluşturan**’ı seçin, Laboratuvar Oluşturan rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet** seçeneğini belirleyin. 

    ![Laboratuvar Oluşturan rolüne kullanıcı ekleme](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Laboratuvar sahiplerine sunulan Market görüntülerini belirtme
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
3. Seçin **Lab Services** için **türü**. 
    Ayrıca, abonelik, kaynak grubu, konumları ve etiketleri göre de filtreleyebilirsiniz. 

## <a name="delete-a-lab-account"></a>Bir laboratuvar hesabı Sil
Laboratuvar hesaplarını bir listede görüntüler önceki bölümde yönergeleri izleyin. Bir laboratuvar hesabı silmek için aşağıdaki yönergeleri kullanın: 

1. Seçin **Laboratuvar hesabı** silmek istediğiniz. 
2. Seçin **Sil** araç çubuğundan. 
3. Tür **Evet** onay.
4. **Sil**’i seçin. 

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



## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)