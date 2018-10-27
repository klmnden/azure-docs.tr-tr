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
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: a91d5826f52b2beb05f2d9a08f3646bd776b294e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142478"
---
# <a name="configure-usage-settings-and-policies"></a>Kullanım ayarları ve ilkeleri yapılandırma
Bu makalede, Laboratuvar için kullanıcıların sayısını yapılandırın, bunları laboratuvarla kayıtlı almak için VM ve daha fazlasını kullanabilirsiniz saat sayısını denetlemek nasıl açıklar. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Laboratuvarda izin verilen kullanıcı sayısını belirtin

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-users"></a>Kullanıcılara kayıt bağlantısı Gönder

1. **Pano** görünümüne geçin. 
2. **Kullanıcı kaydı** kutucuğunu seçin.

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. Öğrenciler, laboratuvarla kaydetme ile yardımcı olan bir sayfaya gitmek için bağlantıyı kullanın. 

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
2. **Kullanıcı kaydı** iletişim kutusunda **Kapat**'ı seçin. 

## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

Seçin **kullanıcılar** kullanıcıların listesini görmek için sol taraftaki menüde laboratuvarla kayıtlı. 

![Laboratuvarla kayıtlı kullanıcıların bir listesi](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Kullanıcı başına kota ayarlama

1. Seçin **kullanıcı ayarları** sol menüsünde.
2. Seçin **kullanıcı başına Quta** Döşe. 
3. Seçin **bir kullanıcının, bir VM kullanabileceği süreyi sınırlamak**. 
4. İçin **kaç saat her kullanıcıya vermek istediğiniz?** saat sayısını girin ve seçin **Kaydet**. 

    ![Saat başına kullanıcı sayısı](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Saat sayısına bakın **kullanıcı başına kotası** artık Döşe. 

    ![Kullanıcı başına kotası](../media/how-to-configure-student-usage/quota-per-user.png)

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
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)
