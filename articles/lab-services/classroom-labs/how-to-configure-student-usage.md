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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 30c033b487fe58d017080b02c257502f82338164
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710049"
---
# <a name="configure-usage-settings-and-policies"></a>Kullanım ayarları ve ilkeleri yapılandırma
Bu makalede, Laboratuvar için kullanıcıların sayısını yapılandırın, bunları laboratuvarla kayıtlı almak için VM ve daha fazlasını kullanabilirsiniz saat sayısını denetlemek nasıl açıklar. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Laboratuvarda izin verilen kullanıcı sayısını belirtin

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

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

![Laboratuvarla kayıtlı kullanıcıların bir listesi](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Kullanıcı başına kota ayarlama

1. Seçin **kullanıcılar** sol menüsünde.
2. Seçin **kullanıcı başına kota: sınırsız** araç. 
3. Üzerinde **kullanıcı başına kotası** sayfasında **bir kullanıcının, bir VM kullanabileceği süreyi sınırlamak**. 
4. İçin **kaç saat her kullanıcıya vermek istediğiniz**saat sayısını girin ve seçin **Kaydet**. 

    ![Saat başına kullanıcı sayısı](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Saat araç çubuğundaki göreceksiniz: **kullanıcı başına kota: &lt;saat sayısı&gt;**. 

    ![Kullanıcı başına kotası](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="add-users-to-the-lab"></a>Kullanıcılar laboratuvara ekleme
Varsa **erişimi kısıtlama** etkin kullanıcılar (e-posta adresleri) listeye ekleyin.

1. Seçin **kullanıcılar** sol menüsünde.
2. Seçin **kullanıcı ekleme** araç. 
3. Üzerinde **kullanıcı ekleme** sayfasında, ayrı bir satırda veya tek bir satırda noktalı virgülle ayırarak kullanıcılar e-posta adreslerini girin. 

    ![Kullanıcı e-posta ekleyin](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. E-posta adreslerini kullanıcıları ve bunların durumlarını (veya kayıtlı) listesinde görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Bir CSV dosyasını karşıya yükleyerek kullanıcı ekleme
Kullanıcıların e-posta adreslerini içeren bir CSV dosyası karşıya yükleyerek, kullanıcılar da ekleyebilirsiniz.

1. Seçin **karşıya CSV** araç.
2. Kullanıcı e-posta adreslerini içeren CSV dosyası seçin. Excel'de açtığınızda, bir sütundaki tüm e-posta adresleri olmalıdır. 

## <a name="manage-user-vms"></a>Kullanıcı Vm'leri yönetme
Azure Lab Services'i kullanarak kayıt Öğrenciler kaydı bağladıktan sonra Öğrenciler için tarihinde atanmış Vm'leri, gördüğünüz sağlanan **sanal makineler** sekmesi. 

![Öğrenciler için atanan sanal makineler](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Bir öğrenci VM üzerinde aşağıdaki görevleri gerçekleştirebilirsiniz: 

- Bir VM, sanal makine çalışıyorsa durdurun. 
- Sanal makine durdurulduysa bir VM'yi başlatın. 
- VM’ye bağlanın. 
- VM'yi silin. 
- Kullanıcıların sanal makine kullanılan saat sayısını görüntüleyin. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)
