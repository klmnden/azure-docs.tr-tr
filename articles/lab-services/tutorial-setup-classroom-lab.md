---
title: Azure Lab Services kullanarak sınıf laboratuvarı ayarlama | Microsoft Docs
description: Bu öğreticide, bir sınıfta kullanılacak laboratuvar ayarlarsınız.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/19/2018
ms.author: spelluru
ms.openlocfilehash: 3e9f8d118c4413ec93b7dffcfa41b6ad4381b052
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makine kümesiyle bir sınıf laboratuvarı ayarlarsınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Sınıf laboratuvarını yapılandırma
> * Öğrencilere kayıt bağlantısı gönderme

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin.
2. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Sınıf laboratuvarı için bir **ad** belirtin. 
    2. Sınıfta kullanmayı planladığınız sanal makinenin **boyutunu** seçin.
    3. Sanal makineyi oluşturmak için kullanılacak **görüntüyü** seçin.
    4. Laboratuvardaki sanal makinelerde oturum açmak için **varsayılan kimlik bilgilerini** belirtin. 
    7. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](./media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Laboratuvarın **giriş sayfasını** görürsünüz. 
    
    ![Sınıf laboratuvarı giriş sayfası](./media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesini yapılandırma

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](./media/tutorial-setup-classroom-lab/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Şablonu ayarlama 
Tüm kullanıcıların sanal makineleri, laboratuvardaki bir şablondan oluşturulur. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının gördüğü şablonun adını ve açıklamasını sağlayabilir, şablon sanal makinesinin örneklerini laboratuvar kullanıcılarınızın kullanımına sunmak için görünürlüğü “Genel” olarak ayarlayabilirsiniz. 

### <a name="set-title-and-description"></a>Başlık ve açıklama ayarlama
1. **Şablon** bölümünde, şablon için **Düzenle**’yi (kalem simgesi) seçin. 
2. **Kullanıcı görünümü** penceresinde, şablon için bir **başlık** girin.
3. Şablon için **açıklama** girin.
4. **Kaydet**’i seçin.

    ![Sınıf laboratuvarı açıklaması](./media/tutorial-setup-classroom-lab/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Şablon örneklerini genel yapma
Bir şablonun görünürlüğünü **Genel** olarak ayarlamanızın ardından Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. 

1. **Şablon** bölümünden **Görünürlük** seçeneğini belirleyin. 
2. **Kullanılabilirlik** sayfasında **Genel** seçeneğini belirleyin.
    
    > [!IMPORTANT]
    > Bir şablon genel olarak kullanılabilir olduğunda, şablonun erişimi özel erişim olarak değiştirilemez. 
3. **Kaydet**’i seçin.

    ![Kullanılabilirlik](./media/tutorial-setup-classroom-lab/public-access.png)

## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

1. **Kullanıcı kaydı** kutucuğunu seçin.
2. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](./media/tutorial-setup-classroom-lab/registration-link.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sınıf laboratuvarı oluşturdunuz ve laboratuvarı yapılandırdınız. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

