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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 163763bf1203a045326c7163b5f6da9aa417d8cf
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081865"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı ayarlayacaksınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Sınıf laboratuvarını yapılandırma
> * Öğrencilere kayıt bağlantısı gönderme

## <a name="prerequisites"></a>Ön koşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar sahibi, şu makaledeki adımları kullanarak Laboratuvar Oluşturan rolüne kullanıcı ekleyebilir: [Laboratuvar Oluşturan rolüne bir kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin.
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Sınıf laboratuvarı için bir **ad** belirtin. 
    2. Sınıfta kullanmayı planladığınız sanal makinenin **boyutunu** seçin.
    3. Sanal makineyi oluşturmak için kullanılacak **görüntüyü** seçin.
    4. Laboratuvardaki sanal makinelerde oturum açmak için **varsayılan kimlik bilgilerini** belirtin. 
    7. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesini yapılandırma

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)


## <a name="set-up-the-template"></a>Şablonu ayarlama 
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Şablon sanal makinesinin laboratuvar kullanıcılarınız tarafından kullanılabilmesi için şablonu yayımlayın. 

### <a name="set-title-and-description"></a>Başlık ve açıklama ayarlama
1. **Şablon** bölümünde, şablon için **Düzenle**’yi (kalem simgesi) seçin. 
2. **Kullanıcı görünümü** penceresinde, şablon için bir **başlık** girin.
3. Şablon için **açıklama** girin.
4. **Kaydet**’i seçin.

    ![Sınıf laboratuvarı açıklaması](../media/tutorial-setup-classroom-lab/lab-description.png)

### <a name="set-up-the-template-vm"></a>Şablon VM'yi ayarlama
 Şablon VM'yi öğrencilerinizin kullanımına sunmadan önce bağlanıp gerekli yazılımları yüklemeniz gerekir. 

1. Şablon sanal makine hazır olana kadar bekleyin. Hazır olduktan sonra **Başlat** düğmesinin etkinleştirilmesi gerekir. VM'yi başlatmak için **Başlat**'ı seçin.

    ![Şablon VM'yi başlatma](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. VM'ye bağlanmak için **Bağlan**'ı seçip yönergeleri izleyin. 

    ![Şablon VM'ye bağlanma](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Öğrencilerinizin laboratuvarda ihtiyaç duyacağı uygulamaları (Visual Studio, Azure Depolama Gezgini gibi) yükleyin. 
2. Şablon VM bağlantısını kesin (uzak masaüstü oturumunuzu kapatın). 
3. **Durdur**'u seçerek şablon VM'yi **durdurun**. 

    ![Şablon VM'yi durdurma](../media/tutorial-setup-classroom-lab/stop-template-vm.png)

### <a name="publish-the-template"></a>Şablonu yayımlama 
Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. 

1. **Şablon** bölümünden **Yayımla**'yı seçin. 

    ![Şablon VM'yi yayımlama](../media/tutorial-setup-classroom-lab/public-access.png)
1. **Yayımla** penceresinde **Yayımlanmış** seçeneğini belirleyin. 
2. Şimdi **Yayımla** düğmesini seçin. Bu işlemin süresi oluşturulan VM sayısına (laboratuvara katılan kullanıcı sayısıyla aynıdır) bağlı olarak değişebilir.
    
    > [!IMPORTANT]
    > Şablon yayımlama işlemi geri alınamaz. 
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. 

    ![Sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. VM'ler oluşturulana kadar bekleyin. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

1. **Pano** görünümüne geçin. 
2. **Kullanıcı kaydı** kutucuğunu seçin.

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
2. **Kullanıcı kaydı** iletişim kutusunda **Kapat**'ı seçin. 


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sınıf laboratuvarı oluşturdunuz ve laboratuvarı yapılandırdınız. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

