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
ms.date: 04/20/2018
ms.author: spelluru
ms.openlocfilehash: 53494abead5701052f6e08f68b01ccdf1bfa211c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Azure Laboratuvar Services'de (önceki adıyla Azure DevTest Labs) Laboratuvar hesaplarını yönetme
Bu makalede, bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesapları görüntüleyin ya da bir laboratuvar hesabını silmek açıklar.

## <a name="create-a-lab-account"></a>Bir laboratuvar hesabı oluşturun
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta ana menüden seçin **kaynak oluşturma**.
3. Arama **Laboratuvar Hizmetleri** Azure Marketi ve seçin **Laboratuvar Hizmetleri** aşağı açılan listesinde. 
4. Seçin **Laboratuvar Sercices (Önizleme)** Hizmetleri flitered listesinde. 
5. İçinde **Laboratuvar hesabı oluşturma** penceresinde, seçin **oluşturma**.
7. İçinde **Laboratuvar hesabı** penceresinde, aşağıdaki eylemleri gerçekleştirebilirsiniz: 
    1. İçin **Laboratuvar hesap adı**, bir ad girin. 
    2. Seçin **Azure aboneliği** Laboratuvar hesabını oluşturmak istediğiniz.
    3. İçin **kaynak grubu**seçin **Yeni Oluştur**ve kaynak grubu için bir ad girin.
    4. İçin **konumu**, oluşturulacak Laboratuvar hesabını istediğiniz konum/bölge seçin. 
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabını penceresi oluşturma](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Sayfa Laboratuvar hesabı için görmüyorsanız seçin **bildirimleri** düğmesine tıklayın ve ardından **kaynağa gidin** bildirimleri düğmesini. 

    ![Laboratuvar hesabını penceresi oluşturma](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Aşağıdaki bakın **Laboratuvar hesabı** sayfa:

    ![Laboratuvar hesap sayfası](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Creator role bir kullanıcı ekleyin
Eğitimcilere labs kendi sınıfları için oluşturma izni sağlamak için laboratuvar Creator role ekleyin:

1. Üzerinde **Laboratuvar hesabı** sayfasında, **erişim denetimi (IAM)**, tıklatıp **+ Ekle** araç çubuğunda. 

    ![Laboratuvar hesap sayfası](./media/tutorial-setup-lab-account/access-control.png)
2. Üzerinde **izinleri eklemek** sayfasında, **Laboratuvar Creator** için **rol**, Laboratuvar oluşturucuları role ekleyin ve seçmek için istediğiniz kullanıcıyı seçin **kaydetmek**. 

    ![Laboratuvar Creator rolüne kullanıcı ekleme](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


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
Azure Laboratuvar hizmetlerini kullanarak laboratuarı ayarı ile başlayın:

- [Bir sınıf laboratuvarı ayarlamanız](tutorial-setup-classroom-lab.md)
- [Özel bir laboratuvarı ayarlamanız](tutorial-create-custom-lab.md)
