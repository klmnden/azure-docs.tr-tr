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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: e388045e839c19c68ad2c00f31d74c73e107872c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="manage-classroom-labs-in-azure-lab-services-formerly-azure-devtest-labs"></a>Sınıf labs Azure Laboratuvar Services'de (önceki adıyla Azure DevTest Labs) yönetme
Bu makalede, oluşturma ve sınıf laboratuvarı yapılandırmak, tüm sınıf labs görüntülemek veya bir sınıf Laboratuvar silme açıklar.

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin.
2. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Sınıf laboratuvarı için bir **ad** belirtin. 
    2. Sınıfta kullanmayı planladığınız sanal makinenin **boyutunu** seçin.
    3. Sanal makineyi oluşturmak için kullanılacak **görüntüyü** seçin.
    4. Belirtin **varsayılan kimlik bilgileri** oturum açmayı laboratuvara sanal makineler için kullanılacak.
    7. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](./media/how-to-manage-classroom-labs/new-lab-window.png)
1. Laboratuvarın **giriş sayfasını** görürsünüz. 
    
    ![Sınıf laboratuvarı giriş sayfası](./media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesini yapılandırma

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](./media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Şablonu ayarlama
Bir şablon bir laboratuvarda, tüm kullanıcıların sanal makineleri oluşturulduğu temel sanal makine görüntüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Bir ad ve açıklama Laboratuvar kullanıcıların gördüğü şablonun sağlayabilir. Ortak VM şablonu örneklerini Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için şablon görünürlüğünü ayarlayabilirsiniz.  

### <a name="set-template-title-and-description"></a>Set şablon başlığı ve açıklama
1. **Şablon** bölümünde, şablon için **Düzenle**’yi (kalem simgesi) seçin. 
2. **Kullanıcı görünümü** penceresinde, şablon için bir **başlık** girin.
3. Şablon için **açıklama** girin.
4. **Kaydet**’i seçin.

    ![Sınıf laboratuvarı açıklaması](./media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Şablon örneklerini genel yapma 
Bir şablonun görünürlüğünü **Genel** olarak ayarlamanızın ardından Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan VM'ler laboratuvarı kullanım ilkesi ayarlayabilirsiniz Laboratuvar uygulamasına izin verilen kullanıcı sayısı ile aynı. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir.  

1. **Şablon** bölümünden **Görünürlük** seçeneğini belirleyin. 
2. **Kullanılabilirlik** sayfasında **Genel** seçeneğini belirleyin.
    
    > [!IMPORTANT]
    > Bir şablon genel olarak kullanılabilir olduğunda, şablonun erişimi özel erişim olarak değiştirilemez. 
3. **Kaydet**’i seçin.

    ![Kullanılabilirlik](./media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

1. **Kullanıcı kaydı** kutucuğunu seçin.
2. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](./media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Tüm sınıf labs görüntüleyin
1. Gidin [Azure Laboratuvar Hizmetleri portalı](https://labs.azure.com).
2. Seçili Laboratuvar hesaptaki tüm labs gördüğünüzü onaylayın. 

    ![Tüm Laboratuvarları](./media/how-to-manage-classroom-labs/all-labs.png)
3. Açılan listenin en üstünde farklı Laboratuvar hesabı seçmek için kullanın. Seçili Laboratuvar hesabı Labs'de bakın. 

## <a name="delete-a-classroom-lab"></a>Bir sınıf Laboratuvar Sil
1. Laboratuvar için kutucuğa köşedeki üç noktaya (...) seçin. 

    ![Laboratuvarı seçme](./media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Düğme silme](./media/how-to-manage-classroom-labs/delete-button.png)
3. Üzerinde **Delete Laboratuvar** iletişim kutusunda **silmek**. 

    ![Sil iletişim kutusu](./media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Özel bir laboratuvarı ayarlama](tutorial-create-custom-lab.md)
