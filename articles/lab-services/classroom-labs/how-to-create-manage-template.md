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
ms.date: 10/25/2018
ms.author: spelluru
ms.openlocfilehash: 3ecbef3b3063ceb413b852f8000b44a85d28d08e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142513"
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
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlayın, yapılandırma ve Sihirbaz tamamlandıktan sonra yayımlama hakkında ayrıntılı bilgi için bkz hakkında daha fazla bilgi için bkz. [şablon yayımlama](#publish-the-template) konusundaki [sınıf laboratuvarlarını yönetme ](how-to-manage-classroom-labs.md) makalesi.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="set-up-a-template-after-creating-a-lab"></a>Bir laboratuvarı oluşturduktan sonra bir şablonu ayarlama 
Laboratuvarı oluşturduktan sonra şablon de ayarlayabilirsiniz.   

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
Laboratuvar oluşturulurken bir şablon yayımlama, daha sonra yayımlayabilirsiniz. Yayımlamadan önce şablona VM bağlanmak ve herhangi bir yazılım güncelleştirme isteyebilirsiniz. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. 

1. **Şablon** bölümünden **Yayımla**'yı seçin. 

    ![Şablon VM'yi yayımlama](../media/tutorial-setup-classroom-lab/public-access.png)
1. Üzerinde **şablon yayımlama** mesaj kutusunda iletisini gözden geçirin ve seçin **Yayımla**. Bu işlemin süresi oluşturulan VM sayısına (laboratuvara katılan kullanıcı sayısıyla aynıdır) bağlı olarak değişebilir.
    
    > [!IMPORTANT]
    > Şablon yayımlama işlemi geri alınamaz. Şablon yine de yayımlayabilirsiniz. 
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. 

    ![Sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. VM'ler oluşturulana kadar bekleyin. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

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
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)
