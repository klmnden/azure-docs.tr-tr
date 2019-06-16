---
title: Azure Lab Services içinde sınıf laboratuvarlarını yönetme | Microsoft Docs
description: Oluşturma ve bir sınıf laboratuvarı yapılandırmak, tüm sınıf laboratuvarlarını görüntülemek, kayıt bağlantısını bir laboratuvar kullanıcıyla paylaşın veya bir laboratuvar silme öğrenin.
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
ms.date: 06/07/2019
ms.author: spelluru
ms.openlocfilehash: 6ba41132c93ebdb2578bafb100416ca3fe579298
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123286"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Lab Services içinde sınıf laboratuvarlarını yönetme 
Bu makalede, oluşturma ve bir sınıf laboratuvarına silme açıklar. Ayrıca, bir laboratuvar hesabı tüm sınıf laboratuvarlarını görüntülemek nasıl gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Laboratuvar sahibi diğer kullanıcılara şu makalede adımları kullanarak Laboratuvar Oluşturucusu rolüne ekleyebilirsiniz: [Laboratuvar oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11 henüz desteklenmediğini unutmayın. 
2. Seçin **oturum**. Seçin veya girin bir **kullanıcı kimliği** diğer bir deyişle üyesi **Laboratuvar oluşturan** Laboratuvardaki rol hesap ve parolayı girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Maksimum belirtin **sanal makinelerin sayısı** Laboratuvardaki. Artırmak ya da daha sonra Laboratuvardaki sanal makinelerin sayısını azaltın. 
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Laboratuvarda oluşturulan sanal makineler (VM) için bir **boyut** seçin. Şu anda **küçük**, **orta**, **Orta (sanallaştırma)** , **büyük**, ve **GPU** boyutları izin verilir. Ayrıntılar için bkz [VM boyutları](#vm-sizes) bölümü.
    1. VM'lerin oluşturulmasını istediğiniz **bölgeyi** seçin. 
    1. Laboratuvardaki VM'leri oluşturmak için kullanılacak **VM görüntüsünü** seçin. Bir Linux görüntüsü seçerseniz, Uzak Masaüstü bağlantı etkinleştirmek için bir seçenek görürsünüz. Ayrıntılar için bkz [Linux için Uzak Masaüstü Bağlantısı etkinleştirme](how-to-enable-remote-desktop-linux.md).
    1. **İleri**’yi seçin.

        ![VM özellikleri belirtme](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. **Kimlik bilgilerini ayarla** sayfasında laboratuvardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. 
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. Devre dışı **tüm sanal makineler için kullanılan parolayı kullan** Öğrenciler kendi parolalarını ayarlamak için isterseniz seçeneği. Bu adım **isteğe bağlıdır**. 

        Bir Öğretmen Laboratuvardaki tüm sanal makineler için aynı parolayı kullanın veya kendi Vm'leri için parolanın öğrencilerin izin seçebilirsiniz. Varsayılan olarak, Ubuntu hariç tüm Windows ve Linux görüntüleri için bu ayar etkinleştirilir. Seçtiğinizde, **Ubuntu** VM, bu ayarı devre dışıysa, böylece Öğrenciler ilk kez oturum açtığında bir parola ayarlamanız istenir.
    1. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarlama](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın.  

    ![Şablonu yapılandır](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon yapılandırma işlemleri tamamlandıktan sonra şu sayfayı görürsünüz: 

    ![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Bu öğreticide aşağıdaki adımlar isteğe bağlıdır: 
    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Linux VM şablon varsa (RDP etkinse), SSH veya RDP kullanarak bağlanmasını isteyip istemediğinizi seçin.
    1. Seçin **parolayı Sıfırla** VM için parolayı sıfırlamak için. 
    1. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    1. VM'yi **durdurun**.  
    1. Şablon için bir **açıklama** girin
9. Şablon sayfasında **İleri**'yi seçin. 
10. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablonu hemen yayımlamak için *Şablonu yayımladıktan sonra değiştiremeyeceğimi anlıyorum. Bu işlem yalnızca bir kez gerçekleştirilebilir ve bir saat sürebilir* onay kutusunu işaretleyip **Yayımla**'yı seçin.  Şablon sanal makinesinin laboratuvar kullanıcılarınız tarafından kullanılabilmesi için şablonu yayımlayın.

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
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

### <a name="vm-sizes"></a>VM boyutları  

| Boyut | Çekirdek | RAM | Açıklama | 
| ---- | ----- | --- | ----------- | 
| Küçük | 2 | 3,5 GB | Bu boyut komut satırı, web tarayıcısı, düşük trafikli web sunucuları, Orta boyutlu veritabanları küçük açmak için idealdir. |
| Orta | 4 | 7 GB | Bu boyut ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir | 
| Orta (iç içe sanallaştırma) | 4 | 16 GB | Bu boyut ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir. Bu boyut, iç içe sanallaştırmayı da destekler. <p>Bu boyut, her öğrencinin birden çok VM gereken yere senaryolarda kullanılabilir. Öğretmenler, iç içe sanallaştırma, birkaç küçük boyut iç içe sanal makineleri sanal makinenin içinde ayarlamak için kullanabilirsiniz. </p> |
| Büyük | 8 | 32 GB | Bu boyut daha hızlı CPU'lar, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekler gerek duyan uygulamalar için idealdir. Bu boyut iç içe sanallaştırmayı da destekler. |  
| GPU | 12 | 112 GB | Bu boyut görselleştirme yoğun işlem gücü kullanımlı ve grafik kullanımlı iş yükleri için idealdir | 

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

## <a name="switch-to-another-classroom-lab"></a>Geçiş yapmak için başka bir sınıf laboratuvarı
Başka bir sınıf laboratuvarı için geçerli geçiş yapmak için üst Laboratuvar hesabı labs açılan listesini seçin.

![Laboratuvar üstündeki aşağı açılan listeden seçin](../media/how-to-manage-classroom-labs/switch-lab.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)

