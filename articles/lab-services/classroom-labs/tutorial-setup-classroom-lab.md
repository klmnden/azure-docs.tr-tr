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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 803fe6eff8804dbd407642386865fe975c8db524
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123253"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı ayarlayacaksınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Kullanıcılar laboratuvara ekleme
> * Öğrencilere kayıt bağlantısı gönderme

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabı içinde bir sınıf laboratuvarı kurmak için laboratuvar hesabında bu rollerden birinin üyesi olması gerekir: Sahip, Laboratuvar oluşturan veya katkıda bulunan. Bir laboratuvar hesabı oluşturmak için kullanılan hesap sahibi rolüne otomatik olarak eklenir.

Laboratuvar sahibi diğer kullanıcılara ekleyebilirsiniz **Laboratuvar oluşturan** rol. Örneğin, bir laboratuvar sahibi profesörlerinin Laboratuvar oluşturan rolüne ekler. Ardından, profesörlerinin kendi sınıfları için Vm'lerle laboratuvarlar oluşturun. Öğrenciler için laboratuvar kaydedilecek profesörlerinin almak kayıt bağlantıyı kullanın. Kullanıcılar kaydedildikten sonra Vm'leri Labs'de sınıfı yapmak için kullanabileceklerini iş ve ev iş. Laboratuvar oluşturan rolüne kullanıcı eklemek için ayrıntılı adımlar için bkz. [Laboratuvar oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11 henüz desteklenmediğini unutmayın. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Maksimum belirtin **sanal makinelerin sayısı** Laboratuvardaki. Laboratuvarı oluşturduktan sonra veya varolan bir laboratuvar içindeki VM sayısını decreate veya artırabilirsiniz. Daha fazla bilgi için [bir laboratuvar içindeki sanal makine sayısını güncelleştir](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Laboratuvarda oluşturulan sanal makineler (VM) için bir **boyut** seçin. Şu anda **küçük**, **orta**, **Orta (sanallaştırma)** , **büyük**, ve **GPU** boyutları izin verilir.
    3. Laboratuvardaki VM'leri oluşturmak için kullanılacak **VM görüntüsünü** seçin. Bir Linux görüntüsü seçerseniz, Uzak Masaüstü bağlantı etkinleştirmek için bir seçenek görürsünüz. Ayrıntılar için bkz [Linux için Uzak Masaüstü Bağlantısı etkinleştirme](how-to-enable-remote-desktop-linux.md).
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
8. Üzerinde **yapılandırma şablonu** sayfasında, aşağıdaki adımları uygulayın: Bu adımlar **isteğe bağlı** öğretici.
    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Linux VM şablon varsa (RDP etkinse), SSH veya RDP kullanarak bağlanmasını isteyip istemediğinizi seçin.
    1. Seçin **parolayı Sıfırla** VM için parolayı sıfırlamak için. 
    1. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    1. VM'yi **durdurun**.  
    1. Şablon için bir **açıklama** girin
9. Şablon sayfasında **İleri**'yi seçin. 
10. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablon hemen yayımlamak için seçin **Yayımla**.  

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Sihirbaz tamamlandıktan sonra VM şablonunu yayımlayabilirsiniz. Yapılandırma ve Sihirbaz sonlandıktan sonra yayımlama hakkında daha fazla bilgi için bkz [şablon yayımlama](how-to-create-manage-template.md#publish-the-template-vm) konusundaki [sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesi.

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

    ![Kullanıcı düğmesi ekleme](../media/how-to-configure-student-usage/add-users-button.png)
1. Üzerinde **kullanıcı ekleme** sayfasında, ayrı bir satırda veya tek bir satırda noktalı virgülle ayırarak kullanıcılar e-posta adreslerini girin. 

    ![Kullanıcı e-posta ekleyin](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. E-posta adreslerini kullanıcıları ve bunların durumlarını (veya kayıtlı) listesinde görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Kullanıcılar için kota ayarlama
Aşağıdaki adımları kullanarak, kullanıcı başına kotaları ayarlayabilirsiniz: 

1. Seçin **kullanıcılar** sayfası zaten etkin değilse sol menüsünde. 
2. Seçin **kullanıcı başına kota:** araç. 
3. Üzerinde **kullanıcı başına kotası** sayfasında, her bir kullanıcı (Öğrenci) vermek istediğiniz süreyi belirtin: 
    1. **0 saat (yalnızca zamanlama)** . Kullanıcılar Vm'lerini yalnızca zamanlanmış süre boyunca veya Laboratuvar sahibi olarak, Vm'lerde için bunları kullanabilirsiniz.

        ![Sıfır saat - yalnızca zamanlanan saati](../media/how-to-configure-student-usage/zero-hours.png)
    1. **Toplam Laboratuvar saat başına kullanıcı sayısı**. Kullanıcıların kümesi kaç saat (Bu alan için belirtilen) için Vm'lerini kullanabilir **zamanlanan süreden yanı sıra**. Bu seçeneği belirlerseniz, girin **saat sayısı** metin kutusuna. 

        ![Saat başına kullanıcı sayısı](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. **Kaydet**’i seçin. 
5. Değiştirilmiş değerlere araç göreceksiniz: **Kullanıcı başına kota: &lt;saat sayısı&gt;** . 

    ![Kullanıcı başına kotası](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Laboratuvar için bir zamanlama
Kota ayarı yapılandırılmışsa **0 saat (yalnızca zamanlama)** , Laboratuvar için bir zamanlama ayarlamanız gerekir. Bu öğreticide, bir programa haftalık olarak çalıştırılması için zamanlamayı ayarlayın.

1. Geçiş **zamanlamaları** sayfasında ve seçin **Ekle zamanlama** araç. 

    ![Zamanlama sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/add-schedule-button.png)
2. Üzerinde **Ekle zamanlama** sayfasında, geçiş **haftalık** en üstünde. 
3. İçin **zamanlama gün (gereklidir)** , zamanlama etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte, Pazartesi-Cuma seçilir. 
4. İçin **gelen** alanına **zamanlama başlangıç tarihi** ya da bir tarih seçerek çekme **Takvim** düğmesi. Bu alan gereklidir. 
5. İçin **zamanlama bitiş tarihi**kapatma için VM'ler üzerinde olan bir bitiş tarihi seçin veya girin. 
6. İçin **başlangıç zamanı**, başlatılacak Vm'leri istediğiniz saati seçin. Başlangıç zamanı, bitiş zamanı ayarlanmamışsa gereklidir. Seçin **kaldırma olay başlangıç** durdurma saati belirtmek istiyorsanız. varsa **başlangıç zamanı** olduğundan devre dışı seçin **Ekle başlangıç olayı** etkinleştirmek için açılır listenin yanındaki. 
7. İçin **durdurma saati**, kapatma için Vm'leri istediğiniz saati seçin. Bitiş zamanı başlangıç zamanından ayarlanmamışsa gereklidir. Seçin **Kaldır durdurma olayını** yalnızca başlangıç saatini belirtmek istiyorsanız. varsa **durdurma saati** olduğundan devre dışı seçin **durdurma olay Ekle** etkinleştirmek için açılır listenin yanındaki.
8. İçin **saat dilimi (gerekli)** , başlangıç için saat dilimini seçin ve bitiş zamanlarını belirttiğiniz.  
9. İçin **notları**, zamanlama için herhangi bir açıklama veya notları girin. 
10. **Kaydet**’i seçin. 

    ![Haftalık Zamanlama](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Kayıt bağlantısı içeren bir e-posta Gönder

1. Geçiş **kullanıcılar** sayfada zaten kök kullanıcı değilseniz görüntüleyin. 
2. Belirli veya tüm kullanıcılar listeden seçin. Belirli kullanıcıları seçmek için listedeki ilk sütunda onay kutularını seçin. Tüm kullanıcıları seçmek için ilk sütun başlığının önüne onay kutusunu seçin (**adı**) veya listeden tüm kullanıcılar için tüm onay kutularını seçin. Durumunu görebilirsiniz **davet durumu** bu listede.  Aşağıdaki görüntüde tüm Öğrenciler için davet durumu ayarlanır **davet gönderilmedi**. 

    ![Öğrencileri seçme](../media/tutorial-setup-classroom-lab/select-students.png)
1. Seçin **e-posta simgesine (Zarf)** satır (veya) select biriyle **Davet Gönder** araç. Fare, e-posta simgesini görmek için listedeki bir öğrenci adının üzerine de gelebilirsiniz. 

    ![Kayıt bağlantıyı e-posta ile gönderin](../media/tutorial-setup-classroom-lab/send-email.png)
4. Üzerinde **e-posta ile gönderme kayıt bağlantı** sayfasında, aşağıdaki adımları izleyin: 
    1. Türü bir **isteğe bağlı iletisini** Öğrenciler göndermek istediğiniz. E-posta kayıt bağlantıyı otomatik olarak içerir. 
    2. Üzerinde **e-posta ile gönderme kayıt bağlantı** sayfasında **Gönder**. Davet değiştirerek durumunu görmek **davet gönderiliyor** ve sonra **davet gönderildi**. 
        
        ![Davet gönderildi](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sınıf laboratuvarı oluşturdunuz ve laboratuvarı yapılandırdınız. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

