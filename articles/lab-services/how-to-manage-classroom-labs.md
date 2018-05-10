---
title: Sınıf labs Azure Laboratuvar Hizmetleri'nde yönetme | Microsoft Docs
description: Sınıf Laboratuvar oluşturulacağı ve yapılandırılacağı hakkında bilgi edinin, tüm sınıf labs, kayıt Laboratuvar kullanıcıyla bağlantı ya da bir laboratuvar silme palaşma görüntüleyin.
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
ms.openlocfilehash: 17544275f921486529518e37eb171cd0b4f26791
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="manage-classroom-labs-in-azure-lab-services-formerly-azure-devtest-labs"></a>Sınıf labs Azure Laboratuvar Services'de (önceki adıyla Azure DevTest Labs) yönetme
Bu makalede, oluşturma ve sınıf laboratuvarı yapılandırmak, tüm sınıf labs görüntülemek veya bir sınıf Laboratuvar silme açıklar.

## <a name="create-a-classroom-lab"></a>Sınıf Laboratuvar oluşturma

1. Gidin [Azure Laboratuvar Hizmetleri Web sitesine](https://labs.azure.com).
2. İçinde **yeni Laboratuvar** penceresinde, aşağıdaki eylemleri gerçekleştirebilirsiniz: 
    1. Belirtin bir **adı** sınıf Laboratuvar için. 
    2. Seçin **boyutu** sınıf kullanmayı planlıyorsanız sanal makinenin.
    3. Seçin **görüntü** sanal makine oluşturmak için kullanılacak.
    4. Belirtin **varsayılan kimlik bilgileri** oturum açmayı laboratuvara sanal makineler için kullanılacak.
    7. **Kaydet**’i seçin.

        ![Sınıf Laboratuvar oluşturma](./media/how-to-manage-classroom-labs/new-lab-window.png)
1. Gördüğünüz **giriş sayfası** Laboratuvar için. 
    
    ![Sınıf Laboratuvar giriş sayfası](./media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesi yapılandırma

1. Seçin **kullanım ilkesi**. 
2. İçinde **kullanım ilkesi**, ayarları, girin **kullanıcıların sayısı** Laboratuvar kullanılmasına izin verilir.
3. **Kaydet**’i seçin. 

    ![Kullanım İlkesi](./media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Şablon ayarlama
Bir şablon bir laboratuvarda, tüm kullanıcıların sanal makineleri oluşturulduğu gelen ' dir. Şablon sanal makineyi tam olarak ne Laboratuvar kullanıcılara sağlamak istediğiniz ile yapılandırıldığı şekilde ayarlayın. Bir ad ve açıklama Laboratuvar kullanıcıların gördüğü şablonun sağlayın ve "Genel" VM şablonu örneklerini Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için görünürlük ayarlayın.  

## <a name="set-template-title-and-description"></a>Set şablon başlığı ve açıklama
1. İçinde **şablonu** bölümünde, select **Düzenle** (Kalem simgesine) şablonu için. 
2. İçinde **kullanıcı görünümü** penceresinde, Enter bir **başlık** şablonu için.
3. Girin **açıklama** şablonu için.
4. **Kaydet**’i seçin.

    ![Sınıf Laboratuvar açıklaması](./media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Şablon örneği genel yap 
Bir şablona görünürlüğünü ayarladıktan sonra **ortak**, Azure Laboratuvar Hizmetleri oluşturur VM'ler laboratuar ortamında şablonu kullanarak. Bu işlemde oluşturulan VM'ler laboratuvarı kullanım ilkesi ayarlayabilirsiniz Laboratuvar uygulamasına izin verilen kullanıcı sayısı ile aynıdır. Tüm sanal makineler şablon olarak aynı yapılandırmaya sahip.  

1. Seçin **görünürlük** içinde **şablonu** bölümü. 
2. İçinde **kullanılabilirlik** sayfasında, **ortak**.
    
    > [!IMPORTANT]
    > Bir şablon genel olarak kullanılabilir olduğunda, uygulamaya erişim için özel değiştirilemez. 
3. **Kaydet**’i seçin.

    ![Kullanılabilirlik](./media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Öğrenciler için kayıt bağlantısı Gönder

1. Seçin **kullanıcı kaydı** döşeme.
2. İçinde **kullanıcı kaydı** iletişim kutusunda **kopyalama** düğmesi. Bağlantıyı panoya kopyalandı. Bir e-posta düzenleyiciye yapıştırın ve Öğrenci için bir e-posta gönderin. 

    ![Öğrenci kayıt bağlantı](./media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Tüm sınıf labs görüntüleyin
1. Gidin [Azure Laboratuvar Hizmetleri portalı](https://labs.azure.com).
2. Seçili Laboratuvar hesaptaki tüm labs gördüğünüzü onaylayın. 

    ![Tüm Laboratuvarları](./media/how-to-manage-classroom-labs/all-labs.png)
3. Açılan listenin en üstünde farklı Laboratuvar hesabı seçmek için kullanın. Seçili Laboratuvar hesabı Labs'de bakın. 

## <a name="delete-a-classroom-lab"></a>Bir sınıf Laboratuvar Sil
1. Laboratuvar için kutucuğa köşedeki üç noktaya (...) seçin. 

    ![Laboratuvar seçin](./media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Düğme silme](./media/how-to-manage-classroom-labs/delete-button.png)
3. Üzerinde **Delete Laboratuvar** iletişim kutusunda **silmek**. 

    ![Sil iletişim kutusu](./media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Sonraki adımlar
Azure Laboratuvar hizmetlerini kullanarak laboratuarı ayarı ile başlayın:

- [Bir sınıf laboratuvarı ayarlamanız](how-to-manage-classroom-labs.md)
- [Özel bir laboratuvarı ayarlamanız](tutorial-create-custom-lab.md)
