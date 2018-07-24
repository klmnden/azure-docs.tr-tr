---
title: Azure Lab Services içinde sınıf laboratuvarlarını yönetme | Microsoft Docs
description: Oluşturma ve bir sınıf laboratuvarı yapılandırmak hakkında bilgi edinin, tüm sınıf laboratuvarlarını, kayıt Laboratuvar kullanıcıyla bağlantı veya bir laboratuvar silme palaşma görüntüleyin.
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
ms.openlocfilehash: 48056d6e2988dd674351aca83526032175c355b6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214403"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Lab Services içinde sınıf laboratuvarlarını yönetme 
Bu makalede, oluşturma ve bir sınıf laboratuvarı yapılandırmak, tüm sınıf laboratuvarlarını görüntülemek veya bir sınıf laboratuvarına silme açıklar.

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Bir laboratuvar hesabı oluşturmak için kullanılan hesap, bu rol için otomatik olarak eklenir. Laboratuvar sahibi diğer kullanıcılara Laboratuvar Oluşturucu rolü aşağıdaki makalede adımları kullanarak ekleyebilirsiniz: [Laboratuvar oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin.
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş ve Microsoft hesaplarını destekler.
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Sınıf laboratuvarı için bir **ad** belirtin. 
    2. Sınıfta kullanmayı planladığınız sanal makinenin **boyutunu** seçin.
    3. Sanal makineyi oluşturmak için kullanılacak **görüntüyü** seçin.
    4. Belirtin **varsayılan kimlik bilgileri** laboratuvarındaki sanal makinelerde oturum açmak için kullanılacak.
    7. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesini yapılandırma

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Şablonu ayarlama
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Şablon sanal makinesinin laboratuvar kullanıcılarınız tarafından kullanılabilmesi için şablonu yayımlayın.  

### <a name="set-template-title-and-description"></a>Set şablon başlığı ve açıklaması
1. **Şablon** bölümünde, şablon için **Düzenle**’yi (kalem simgesi) seçin. 
2. **Kullanıcı görünümü** penceresinde, şablon için bir **başlık** girin.
3. Şablon için **açıklama** girin.
4. **Kaydet**’i seçin.

    ![Sınıf laboratuvarı açıklaması](../media/how-to-manage-classroom-labs/lab-description.png)

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

## <a name="view-all-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleyin
1. Gidin [Azure Lab Services portalı](https://labs.azure.com).
2. Seçili bir laboratuvar hesabı tüm Labs'de gördüğünüzü onaylayın. 

    ![Tüm Laboratuvarları](../media/how-to-manage-classroom-labs/all-labs.png)
3. Açılan listenin en üstünde farklı bir laboratuvar hesabı seçmek için kullanın. Seçili bir laboratuvar hesabı Labs'de görürsünüz. 

## <a name="delete-a-classroom-lab"></a>Bir sınıf laboratuvarına Sil
1. Laboratuvar için bir kutucuğa üst köşedeki üç nokta (...) seçin. 

    ![Laboratuvarı seçme](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Sil düğmesi](../media/how-to-manage-classroom-labs/delete-button.png)
3. Üzerinde **silme Laboratuvar** iletişim kutusunda **Sil**. 

    ![Sil iletişim kutusu](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Öğrenci Vm'leri yönetme
Azure Lab Services'i kullanarak kayıt Öğrenciler kaydı bağladıktan sonra Öğrenciler için tarihinde atanmış Vm'leri, gördüğünüz sağlanan **sanal makineler** sekmesi. 

![Öğrenciler için atanan sanal makineler](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Bir öğrenci VM üzerinde aşağıdaki görevleri gerçekleştirebilirsiniz: 

- Bir VM, sanal makine çalışıyorsa durdurun. 
- Sanal makine durdurulduysa bir VM'yi başlatın. 
- VM’ye bağlanın. 
- VM'yi silin. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)
