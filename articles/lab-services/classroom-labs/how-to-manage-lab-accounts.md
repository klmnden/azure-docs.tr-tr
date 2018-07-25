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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: ff2968f8e2fa9a705817b020f2daa6582d78029c
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225311"
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
Bir laboratuvar hesabı içinde bir sınıf laboratuvarı ayarlamak için kullanıcı bir üyesi olmanız gerekir **Laboratuvar oluşturan** Laboratuvar hesabındaki rol. Laboratuvar hesabı oluşturmak için kullanılan hesap otomatik olarak bu role eklenir. Bir sınıf laboratuvarı oluşturmak için aynı kullanıcı hesabı kullanmayı planlıyorsanız, bu adımı atlayabilirsiniz. Bir sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak için aşağıdaki adımları uygulayın: 

1. **Laboratuvar Hesabı** sayfasında **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ Ekle** düğmesine tıklayın. 

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/access-control.png)
2. **İzin ekle** sayfasında, **Rol** için **Laboratuvar Oluşturan**’ı seçin, Laboratuvar Oluşturan rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet** seçeneğini belirleyin. 

    ![Laboratuvar Oluşturan rolüne kullanıcı ekleme](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Laboratuvar sahibi Market görüntülerinden belirtin
Bu bölümde, Laboratuvar sahibini sınıf laboratuvarlarını oluşturmak için kullanabileceğiniz bir Market görüntülerini belirtin. 

1. Seçin **Market görüntüleri** sol menüsünde. Varsayılan olarak, görüntü (etkin ve devre dışı) tam listesini görürsünüz. Yalnızca etkin/devre dışı görüntüleri seçerek görmek için listeyi filtreleyebilirsiniz **yalnızca etkin**/**yalnızca devre dışı** üstündeki aşağı açılan listeden seçeneği. 

    ![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)
2. İçin **devre dışı** etkinleştirildiğinde, bir Market görüntüsü aşağıdaki işlemlerden birini yapın: 
    1. Seçin **... (üç nokta)**  son sütun ve select **devre dışı bırakma görüntü**. 

        ![Bir görüntü devre dışı bırak](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Bir veya daha fazla görüntü listesinden görüntü adlarını önce onay kutularını listeden seçim yaparak seçip **seçilen görüntüler devre dışı**. 

        ![Birden fazla görüntü devre dışı bırak](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Benzer şekilde, için **etkinleştirme** bir Market görüntüsü, aşağıdaki işlemlerden birini yapın: 
    1. Seçin **... (üç nokta)**  son sütun ve select **etkinleştir görüntü**. 
    2. Bir veya daha fazla görüntü listesinden görüntü adlarını önce onay kutularını listeden seçim yaparak seçip **seçilen görüntüler etkinleştirme**. 

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

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)
