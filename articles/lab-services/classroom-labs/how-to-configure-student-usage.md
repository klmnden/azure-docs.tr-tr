---
title: Azure Lab Services classroom Labs'de kullanım ayarlarını yapılandırma | Microsoft Docs
description: Laboratuvar için kullanıcıların sayısını yapılandırın, lab ile kayıtlı alın, VM'yi ve daha fazlasını kullanabilirsiniz saat sayısını denetlemek hakkında bilgi edinin.
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
ms.openlocfilehash: 834674eb63af75088434db0f614b11c7a36e7adf
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964823"
---
# <a name="configure-usage-settings-and-policies"></a>Kullanım ayarları ve ilkeleri yapılandırma
Bu makalede kullanıcıların laboratuvara ekleme, bunları laboratuvarla kayıtlı almak için VM ve daha fazlasını kullanabilirsiniz saat sayısını denetlemek nasıl açıklar. 


## <a name="add-users-to-the-lab"></a>Kullanıcılar laboratuvara ekleme
Varsa **erişimi kısıtlama** etkin kullanıcılar (e-posta adresleri) listeye ekleyin.

1. Seçin **kullanıcılar** sol menüsünde.
2. Seçin **kullanıcı ekleme** araç. 

    ![Kullanıcı düğmesi ekleme](../media/how-to-configure-student-usage/add-users-button.png)
1. Üzerinde **kullanıcı ekleme** sayfasında, ayrı bir satırda veya tek bir satırda noktalı virgülle ayırarak kullanıcılar e-posta adreslerini girin. 

    ![Kullanıcı e-posta ekleyin](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. E-posta adreslerini kullanıcıları ve bunların durumlarını (veya kayıtlı) listesinde görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme
Aşağıdaki yordam bir kayıt bağlantı kullanıcılara göndermek için adımları yok. Varsa **erişimi kısıtlama** yalnızca kullanıcılar listesindeki kullanıcıları için laboratuvar kaydetmek için kayıt bağlantısını kullanabilir Laboratuvar için etkinleştirilir. 

1. Geçiş **kullanıcılar** görünümü seçerek **kullanıcılar** sol menüsünde. 
2. Seçin **kayıt bağlantı alma** Döşe.

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
2. **Kullanıcı kaydı** iletişim kutusunda **Kapat**'ı seçin. 
4. Kayıt bağlantısını sınıfa kaydolmasını istediğiniz öğrencilerle paylaşın. Varsa **seçeneği kısıtlama** ayarı etkin ve kullanıcıların listesini listesinde olması, aşağıdaki eylemleri gerçekleştirin:
    1. Seçin **e-posta adresi** listesinde kullanıcının. 
    2. Bir pencere varsayılan e-posta programınızı görmek **Kime** adresi doldurulur. 
    3. Yapıştırma **kayıt URL'si** daha önce kopyaladığınız. 
    4. Gönderme **e-posta**. 

## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

Seçin **kullanıcılar** kullanıcıların listesini görmek için sol taraftaki menüde laboratuvarla kayıtlı. 

![Laboratuvarla kayıtlı kullanıcıların bir listesi](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Kullanıcı başına kota ayarlama
Aşağıdaki adımları kullanarak, kullanıcı başına kotaları ayarlayabilirsiniz: 

1. Seçin **kullanıcılar** sol menüsünde.
2. Seçin **kullanıcı başına kota: sınırsız** araç. 
3. Üzerinde **kullanıcı başına kotası** sayfasında, aşağıdaki seçeneklerden birini seçin: 
    1. **None**. Kullanıcıların sanal makinelerinde yalnızca zamanlanmış süre boyunca veya Laboratuvar sahibi için sanal makinelerde döndüğünde kullanabilirsiniz.
    2. **Sınırsız (varsayılan)**. Kullanıcılar, herhangi bir zaman kısıtlaması olmadan sanal makinelerini kullanabilirsiniz.
    3. **Kullanıcı başına saat sayısını belirtin**. Kullanıcıların sanal makinelerinde kümesi kaç saat (aşağıda zamanlanan süreden ek olarak belirtilen) için kullanabilirsiniz. Bu seçeneği belirlerseniz, girin **saat sayısı** metin kutusuna. 

        ![Saat başına kullanıcı sayısı](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. **Kaydet**’i seçin. 
5. Değiştirilmiş değerlere araç göreceksiniz: **Kullanıcı başına kota: &lt;saat sayısı&gt;**. 

    ![Kullanıcı başına kotası](../media/how-to-configure-student-usage/quota-per-user.png)

> [!IMPORTANT]
> [VM'lerin çalışmasını zamanlanmış](how-to-create-schedules.md) bir kullanıcı için ayrılan kota karşı sayılmaz. Bir öğrenci Vm'lerde geçirdiği zamanlama saatleri dışında saat kota içindir. 

### <a name="add-users-by-uploading-a-csv-file"></a>Bir CSV dosyasını karşıya yükleyerek kullanıcı ekleme
Kullanıcıların e-posta adreslerini içeren bir CSV dosyası karşıya yükleyerek, kullanıcılar da ekleyebilirsiniz.

1. Bir sütun kullanıcıların e-posta adreslerine sahip bir CSV dosyası oluşturabilirsiniz.

    ![Kullanıcı başına kotası](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Üzerinde **kullanıcılar** Laboratuvar seçin sayfasında **karşıya CSV** araç.

    ![Karşıya Yükle CSV düğmesi](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Kullanıcı e-posta adreslerini içeren CSV dosyası seçin. Seçtiğinizde, **açık** CSV dosyasını seçtikten sonra aşağıdaki görüntüyle **kullanıcı ekleme** penceresi. E-posta adresi listesi, CSV dosyasından e-posta adresleriyle doldurulur. 

    ![CSV dosyasından e-posta adresleriyle doldurulur kullanıcılar pencere Ekle](../media/how-to-configure-student-usage/add-users-window.png)
4. Seçin **Kaydet** içinde **kullanıcı ekleme** penceresi. 
5. Kullanıcılar listesindeki kullanıcıları gördüğünüzü onaylayın. 

    ![Eklenen kullanıcıların listesi](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Kullanıcı Vm'leri yönetme
Azure Lab Services'i kullanarak kayıt Öğrenciler kaydı bağladıktan sonra Öğrenciler için tarihinde atanmış Vm'leri, gördüğünüz sağlanan **sanal makineler** sekmesi. 

![Öğrenciler için atanan sanal makineler](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Bir öğrenci VM üzerinde aşağıdaki görevleri gerçekleştirebilirsiniz: 

- Bir VM, sanal makine çalışıyorsa durdurun. 
- Sanal makine durdurulduysa bir VM'yi başlatın. 
- VM’ye bağlanın. 
- VM'yi silin. 
- Kullanıcıların sanal makine kullanılan saat sayısını görüntüleyin. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Laboratuvarındaki sanal makinelerde güncelleştirme sayısı
Laboratuvar sanal makinelerin sayısını güncelleştirmek için aşağıdaki adımları uygulayın **sanal makineler** sayfası:

1. Seçin **sanal makineler** sol menüsünde. 
2. Seçin **Laboratuvar Kapasite: &lt;numarası&gt; makine** araç. 
3. Girin **numarası** sanal makinelerin.
4. **Kaydet**’i seçin.

    ![Laboratuvarındaki sanal makinelerde](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)
