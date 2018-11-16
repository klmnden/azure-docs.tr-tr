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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 7b8c0e2a4df59baecc3aac7e56789cef8f8985d3
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705829"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Lab Services içinde sınıf laboratuvarlarını yönetme 
Bu makalede, oluşturma ve bir sınıf laboratuvarına silme açıklar. Ayrıca, bir laboratuvar hesabı tüm sınıf laboratuvarlarını görüntülemek nasıl gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Laboratuvar sahibi, şu makaledeki adımları kullanarak Laboratuvar Oluşturan rolüne kullanıcı ekleyebilir: [Laboratuvar Oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. Seçin **oturum**. Seçin veya girin bir **kullanıcı kimliği** diğer bir deyişle üyesi **Laboratuvar oluşturan** Laboratuvardaki rol hesap ve parolayı girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Belirtin **kullanıcıların sayısı** laboratuara izin verilir. 
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Laboratuvarda oluşturulan sanal makineler (VM) için bir **boyut** seçin. 
    2. VM'lerin oluşturulmasını istediğiniz **bölgeyi** seçin. 
    3. Laboratuvardaki VM'leri oluşturmak için kullanılacak **VM görüntüsünü** seçin. 
    4. **İleri**’yi seçin.

        ![VM özellikleri belirtme](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. **Kimlik bilgilerini ayarla** sayfasında laboratuvardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. 
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarlama](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın.  

    ![Şablonu yapılandır](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon yapılandırma işlemleri tamamlandıktan sonra şu sayfayı görürsünüz: 

    ![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Bu öğreticide aşağıdaki adımlar isteğe bağlıdır: 
    1. **Başlat**'ı seçerek şablon VM'sini başlatın.
    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. 
    3. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    4. VM'yi **durdurun**.  
    5. Şablon için bir **açıklama** girin

        ![Şablon yapılandırma sayfasında İleri](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Şablon sayfasında **İleri**'yi seçin. 
10. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablonu hemen yayımlamak için *Şablonu yayımladıktan sonra değiştiremeyeceğimi anlıyorum. Bu işlem yalnızca bir kez gerçekleştirilebilir ve bir saat sürebilir* onay kutusunu işaretleyip **Yayımla**'yı seçin.  Şablon sanal makinesinin laboratuvar kullanıcılarınız tarafından kullanılabilmesi için şablonu yayımlayın.

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlayın, yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlama hakkında ayrıntılı bilgi için bkz hakkında daha fazla bilgi için bkz. [şablon yayımlama](#publish-the-template) konusundaki [sınıf laboratuvarlarını yönetme ](how-to-manage-classroom-labs.md) makalesi.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="view-all-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleyin
1. Gidin [Azure Lab Services portalı](https://labs.azure.com).
2. Seçin **oturum**. Seçin veya girin bir **kullanıcı kimliği** diğer bir deyişle üyesi **Laboratuvar oluşturan** Laboratuvardaki rol hesap ve parolayı girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. Seçili bir laboratuvar hesabı tüm Labs'de gördüğünüzü onaylayın. 

    ![Tüm Laboratuvarları](../media/how-to-manage-classroom-labs/all-labs.png)
3. Açılan listenin en üstünde farklı bir laboratuvar hesabı seçmek için kullanın. Seçili bir laboratuvar hesabı Labs'de görürsünüz. 

## <a name="delete-a-classroom-lab"></a>Bir sınıf laboratuvarına Sil
1. Laboratuvar için bir kutucuğa üst köşedeki üç nokta (...) seçin. 

    ![Laboratuvarı seçme](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Sil düğmesi](../media/how-to-manage-classroom-labs/delete-button.png)
3. Üzerinde **silme Laboratuvar** iletişim kutusunda **Sil**. 

    ![Sil iletişim kutusu](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)

