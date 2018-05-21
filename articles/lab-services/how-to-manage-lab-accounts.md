---
title: Azure Laboratuvar Services'de Laboratuvar hesaplarını yönetme | Microsoft Docs
description: Bir laboratuvar hesabı oluşturma, tüm Laboratuvar hesapları görüntüleyin veya bir Azure aboneliği Laboratuvar hesabında silme öğrenin.
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
ms.openlocfilehash: 1286698fb7dd13c7568a0fa8b20c50511d5a6919
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Azure Laboratuvar Services'de (önceki adıyla Azure DevTest Labs) Laboratuvar hesaplarını yönetme
Bu makalede, bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesapları görüntüleyin ya da bir laboratuvar hesabını silmek açıklar.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u seçin.
3. Azure Market’te **Lab Services** ifadesini aratıp açılan listeden **Lab Services** uygulamasını seçin. 
4. Seçin **Laboratuvar Sercices (Önizleme)** Hizmetleri flitered listesinde. 
5. **Laboratuvar hesabı oluştur** penceresinde **Oluştur**’u seçin.
7. **Laboratuvar hesabı** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Laboratuvar hesabının sayfasını görmüyorsanız, **bildirimler** düğmesini seçin ve sonra bildirimlerdeki **Kaynağa git** düğmesine tıklayın. 

    ![Laboratuvar hesabı oluştur penceresi](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Eğitimcilere, sınıfları için laboratuvar oluşturma ve Laboratuvar Oluşturan rolüne bunları ekleme izni sağlamak için:

1. **Laboratuvar Hesabı** sayfasında **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ Ekle** düğmesine tıklayın. 

    ![Laboratuvar hesabı sayfası](./media/tutorial-setup-lab-account/access-control.png)
2. **İzin ekle** sayfasında, **Rol** için **Laboratuvar Oluşturan**’ı seçin, Laboratuvar Oluşturan rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet** seçeneğini belirleyin. 

    ![Laboratuvar Oluşturan rolüne kullanıcı ekleme](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="view-lab-accounts"></a>Görünüm Laboratuvar hesapları
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm kaynakları** menüsünde. 
3. Seçin **Laboratuvar Hizmetleri** için **türü**. 
    Abonelik, kaynak grubu, konumları ve etiketleri göre de filtreleyebilirsiniz. 

## <a name="delete-a-lab-account"></a>Laboratuvar hesabını silme
Laboratuvar hesapları bir listede görüntüler önceki bölümdeki yönergeleri izleyin. Bir laboratuvar hesabını silmek için aşağıdaki yönergeleri kullanın: 

1. Seçin **Laboratuvar hesabı** silmek istediğiniz. 
2. Seçin **silmek** araç çubuğundan. 
3. Tür **Evet** onay.
4. **Sil**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)
- [Özel bir laboratuvarı ayarlama](tutorial-create-custom-lab.md)
