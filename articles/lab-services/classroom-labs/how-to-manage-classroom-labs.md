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
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: eebdc98db5ecdf518d3b0b58e6757a2b7ecd5dd7
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043813"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Lab Services içinde sınıf laboratuvarlarını yönetme 
Bu makalede, oluşturma ve bir sınıf laboratuvarı yapılandırmak, tüm sınıf laboratuvarlarını görüntülemek veya bir sınıf laboratuvarına silme açıklar.

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Laboratuvar sahibi, şu makaledeki adımları kullanarak Laboratuvar Oluşturan rolüne kullanıcı ekleyebilir: [Laboratuvar Oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Belirtin bir **adı** laboratuvarınız için. 
    2. Maksimum belirtin **kullanıcıların sayısı** laboratuara izin verilir. 
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Üzerinde **seçin, sanal makine belirtimleri** sayfasında, aşağıdaki adımları uygulayın:
    1. Seçin bir **boyutu** laboratuvar'da oluşturulan sanal makineler (VM). 
    2. Seçin **bölge** oluşturulacak sanal makinelerin istediğiniz. 
    3. Seçin **VM görüntüsü** laboratuvarda VM oluşturmak için kullanılacak. 
    4. **İleri**’yi seçin.

        ![VM özellikleri belirtin](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Üzerinde **kimlik bilgilerini ayarla** sayfasında, Laboratuvardaki tüm sanal makineler için varsayılan kimlik bilgilerini belirtin. 
    1. Belirtin **kullanıcı adını** Laboratuvardaki tüm sanal makineler için.
    2. Belirtin **parola** kullanıcı. 

        > [!IMPORTANT]
        > Kullanıcı adınızı ve parolanızı not edin. Bunlar tekrar gösterilmeyecektir.
    3. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarla](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Üzerinde **yapılandırma şablonu** sayfasında, Laboratuvar oluşturma işleminin durumunu görürsünüz. Laboratuvar şablonu oluşturma en fazla 20 dakika sürer. 

    ![Şablon yapılandırma](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon Yapılandırma tamamlandıktan sonra aşağıdaki sayfayı görürsünüz: 

    ![Şablon sayfasına bittikten sonra yapılandırma](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Aşağıdaki adımlar, bu öğreticide isteğe bağlı adımlar şunlardır: 
    1. VM şablonu seçerek başlayın **Başlat**.
    2. VM şablonu seçerek Bağlan **Connect**. 
    3. Yazılım yükleme ve şablonunuzda VM yapılandırma. 
    4. **Durdur** VM.  
    5. Girin bir **açıklama** şablonu

        ![Yapılandırma şablon sayfasında İleri](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Seçin **sonraki** şablon sayfasında. 
10. Üzerinde **şablon yayımlama** sayfasında, aşağıdaki eylemleri gerçekleştirin. 
    1. Şablon hemen yayımlamak için onay kutusunu seçin. *miyim değiştiremeyeceğiniz şablonu yayımladıktan sonra anlıyorum. Bu işlem yalnızca bir kez yapılabilir ve bir saate kadar sürebilir*seçip **Yayımla**.  

        > [!WARNING]
        > Yayımlamadan sonra yayımdan kaldırılamıyor. 
    2. Daha sonra yayımlamak için seçin **daha sonrası için saklayın**. Sihirbaz tamamlandıktan sonra VM şablonunu yayımlayabilirsiniz. Yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlayın, yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlama hakkında ayrıntılı bilgi için bkz hakkında daha fazla bilgi için bkz. [şablon yayımlama](#publish-the-template) konusundaki [sınıf laboratuvarlarını yönetme ](how-to-manage-classroom-labs.md) makalesi.

        ![Şablon yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Gördüğünüz **yayımlama ilerlemesini** şablonu. Bu işlem bir saat kadar sürebilir. 

    ![Yayın şablonu - ilerleme durumu](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Yayın şablonu - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

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


## <a name="publish-the-template"></a>Şablonu yayımlama 
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
 
## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme
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
