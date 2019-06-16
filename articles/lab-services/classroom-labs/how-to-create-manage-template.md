---
title: Azure Lab Services içinde bir sınıf laboratuvarına şablonu yönetme | Microsoft Docs
description: Oluşturma ve Azure Lab Services classroom laboratuvar şablonu yönetme hakkında bilgi edinin.
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
ms.date: 02/07/2019
ms.author: spelluru
ms.openlocfilehash: b287a67c470cc1697065838e52916c285a2233a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60704467"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Oluşturma ve bir sınıf şablonunda Azure Lab Services'ı yönetme
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Ardından, VM şablonu örneklerini Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için şablonu yayımlayın. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir.

Bu makalede, oluşturmak ve bir sınıf laboratuvarına Azure Lab Services'ın bir şablon sanal makinede yönetmek açıklar. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Bir sınıf laboratuvarına oluşturulurken bir şablon yayımlama
İlk olarak ayarlayın ve bir sınıf laboratuvarına oluşturulurken bir şablon yayımlama.

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Laboratuvara katılmasına izin verilen maksimum **kullanıcı sayısını** belirtin. 
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
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. 

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
    1. Şablonu hemen yayımlamak için *Şablonu yayımladıktan sonra değiştiremeyeceğimi anlıyorum. Bu işlem yalnızca bir kez gerçekleştirilebilir ve bir saat sürebilir* onay kutusunu işaretleyip **Yayımla**'yı seçin.  

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlama hakkında ayrıntılı bilgi için bkz: yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlama hakkında ayrıntılı bilgi için şablon konusundaki Yayımla bkz [sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesi.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

 
## <a name="set-or-update-template-title-and-description"></a>Ayarlayın veya şablon başlığı ve açıklamayı güncelleştirme
Başlık ve açıklama ilk kez ayarlamak için aşağıdaki adımları kullanın ve daha sonra güncelleştirebilirsiniz. 

1. İçinde **şablon** fareyi hareket ettirin bölümünde **adı** şablonunun veya **açıklama** şablonunun ve bu seçeneği belirleyin. 
2. Girin **yeni adı** veya **yeni açıklama** tuşuna basın ve şablon için **ENTER**.

    ![Şablon adı ve açıklaması](../media/how-to-create-manage-template/template-name-description.png)

## <a name="set-up-or-update-a-template-vm"></a>Ayarlayın veya bir VM şablonunu güncelleştirme
 Şablon VM'yi öğrencilerinizin kullanımına sunmadan önce bağlanıp gerekli yazılımları yüklemeniz gerekir. VM şablon ilk kez ayarlama veya VM güncelleştirmek için aşağıdaki adımları kullanın. 

1. Şablon sanal makine hazır olana kadar bekleyin. Hazır olduktan sonra **Başlat** düğmesinin etkinleştirilmesi gerekir. VM'yi başlatmak için **Başlat**'ı seçin.

    ![Şablon VM'yi başlatma](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Uyarıyı gözden geçirin ve seçin **Başlat**. 

    ![Başlangıç şablonu - uyarı](../media/how-to-create-manage-template/start-template-warning.png)
2. Laboratuvar durumu kutucuğu göreceğiniz **şablon** bölümü.

    ![Başlangıç şablonu - durum](../media/how-to-create-manage-template/template-start-status.png)
1. Bir VM'ye bağlanmak için başlatıldıktan sonra seçin **Connect**ve yönergeleri izleyin. 

    ![Bağlanma veya VM şablonu Durdur](../media/how-to-create-manage-template/connect-stop-vm.png)
1. Öğrencilerinizin laboratuvarda ihtiyaç duyacağı uygulamaları (Visual Studio, Azure Depolama Gezgini gibi) yükleyin. 
2. Şablon VM bağlantısını kesin (uzak masaüstü oturumunuzu kapatın). 
3. **Durdur**'u seçerek şablon VM'yi **durdurun**. 

## <a name="publish-the-template-vm"></a>Şablon VM'yi yayımlama  
Laboratuvar oluşturulurken bir şablon yayımlama, daha sonra yayımlayabilirsiniz. Yayımlamadan önce şablona VM bağlanmak ve herhangi bir yazılım güncelleştirme isteyebilirsiniz. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. 

1. **Şablon** bölümünden **Yayımla**'yı seçin. 

    ![Şablon VM'yi yayımlama](../media/tutorial-setup-classroom-lab/public-access.png)
1. Üzerinde **şablon yayımlama** mesaj kutusunda iletisini gözden geçirin ve seçin **Yayımla**. Bu işlem VM sayısına bağlı olarak biraz zaman alabilir oluşturuluyor.
    
    > [!IMPORTANT]
    > Şablon yayımlama işlemi geri alınamaz. Şablon yine de yayımlayabilirsiniz. 
4. Durumu değiştirmek için şablonu bekleyin **yayımlanan**. 

    ![Yayımlama durumu](../media/how-to-create-manage-template/publish-status.png)
1. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. VM'ler oluşturulana kadar bekleyin. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. Bu sayfada başlatın veya VM'lerin başlatma Öğrencilerinize olanak tanır. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="republish-the-template"></a>Şablonu yeniden yayımlayın 
Şablon yayımlama sonra hala şablona VM bağlanmak, güncelleştirin ve sonra bunu yeniden yayınlayacaksınız. Bir VM şablonu yayımladığınızda, tüm kullanıcı VM bağlantısı kesildi ve güncelleştirilmiş şablonu temel alan yeniden oluşturulur. 

1. Laboratuvarınızı Pano sayfasında **yeniden yayımlamanız** şablonu bölümünde. 

    ![Düğmeyi yeniden yayımlayın](../media/how-to-create-manage-template/republish-button.png)
2. Üzerinde **şablonu yeniden yayımlamanız** mesaj kutusunda, metni gözden geçirip seçin **yeniden yayımlamanız** devam etmek için. Aksi takdirde seçin **iptal**. 

    ![Şablonun iletinin yeniden yayımlayın](../media/how-to-create-manage-template/republish-template-message.png)
3. Yeniden yayımlama durumu kutucuğunda gördüğünüz **şablon** bölümü.

    ![Yeniden yayımlama durumu](../media/how-to-create-manage-template/republish-status-publishing.png)
4. Şablon yayımlandıktan sonra durumu kümesine **yayımlanan**. 

    ![Yeniden yayımlama başarılı](../media/how-to-create-manage-template/republish-success.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)
