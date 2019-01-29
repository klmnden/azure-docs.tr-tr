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
ms.date: 01/16/2019
ms.author: spelluru
ms.openlocfilehash: 3b425af972b0983db076ab103a33c57f7a127210
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55095762"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı ayarlayacaksınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Sınıf laboratuvarını yapılandırma
> * Öğrencilere kayıt bağlantısı gönderme

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabı içinde bir sınıf laboratuvarı kurmak için laboratuvar hesabında bu rollerden birinin üyesi gerekir: Sahip, Laboratuvar oluşturan veya katkıda bulunan. Bir laboratuvar hesabı oluşturmak için kullanılan hesap sahibi rolüne otomatik olarak eklenir.

Laboratuvar sahibi diğer kullanıcılara ekleyebilirsiniz **Laboratuvar oluşturan** rol. Örneğin, bir laboratuvar sahibi profesörlerinin Laboratuvar oluşturan rolüne ekler. Ardından, profesörlerinin kendi sınıfları için Vm'lerle laboratuvarlar oluşturun. Öğrenciler için laboratuvar kaydedilecek profesörlerinin almak kayıt bağlantıyı kullanın. Kullanıcılar kaydedildikten sonra Vm'leri Labs'de sınıfı yapmak için kullanabileceklerini iş ve ev iş. Laboratuvar oluşturan rolüne kullanıcı eklemek için ayrıntılı adımlar için bkz. [Laboratuvar oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

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
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Sihirbaz tamamlandıktan sonra yapılandırma ve yayımlama adımları için [Sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesindeki [Şablonu yayımlama](how-to-create-manage-template.md#publish-the-template-vm) bölümüne bakın.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Geçiş **sanal makineler** sayfasında soldaki menüden sanal makinelerde seçerek veya sanal makineleri kutucuğu seçtiğinizde. Bulunan sanal makineler gördüğünüzü onaylayın **atanmamış** durumu. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Kullanıcılar laboratuvara ekleme

1. Seçin **kullanıcılar** sol menüsünde. Varsayılan olarak, **erişimi kısıtlama** seçeneği etkinleştirilir. Bu ayar etkin olduğunda, kullanıcı listesinde kullanıcı değilse kullanıcı kayıt bağlantıya sahip olsa bile, bir kullanıcı laboratuvarla kaydedilemiyor. Yalnızca bu listedeki kullanıcılar laboratuvarla gönderdiğiniz kayıt bağlantıyı kullanarak kaydedebilirsiniz. Bu yordamda, kullanıcıların listesine ekleyin. Alternatif olarak, kapatabilirsiniz **erişimi kısıtlama**, kullanıcıların kayıt bağlantıya sahip oldukları sürece laboratuvarla kaydolmasını sağlar. 
2. Seçin **kullanıcı ekleme** araç. 
3. Üzerinde **kullanıcı ekleme** sayfasında, ayrı bir satırda veya tek bir satırda noktalı virgülle ayırarak kullanıcılar e-posta adreslerini girin. 

    ![Kullanıcı e-posta ekleyin](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. E-posta adreslerini kullanıcıları ve bunların durumlarını (veya kayıtlı) listesinde görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)


## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

1. Geçiş **kullanıcılar** sayfada zaten kök kullanıcı değilseniz görüntüleyin. 
2. Seçin **kayıt bağlantı alma** araç.
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır.

    ![Kayıt bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kapat**'ı seçin. 
2. Kayıt bağlantısını sınıfa kaydolmasını istediğiniz öğrencilerle paylaşın.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sınıf laboratuvarı oluşturdunuz ve laboratuvarı yapılandırdınız. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

