---
title: "Azure DevTest Labs'de Laboratuvar için bir announcment gönderme | Microsoft Docs"
description: "Duyuru Azure DevTest Labs laboratuvarda eklemeyi öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/29/2018
ms.author: v-craic
ms.openlocfilehash: 99b0938d5f4c8b022ead3473a0367de5d75cd6ff
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de Laboratuvar için bir duyuru sonrası

Laboratuar Yöneticisi olarak, kullanıcıları son değişiklikler veya Laboratuvar eklemeler hakkında bilgilendirmek için varolan bir laboratuvar özel bir duyuru nakledebilirsiniz. Örneğin, kullanıcılar hakkında bilgilendirmek isteyebilirsiniz:

- Kullanılabilir yeni VM boyutları
- Şu anda kullanılamaz durumda görüntüleri
- Laboratuvar ilkeleri güncelleştirmeleri

Sonra gönderilen, duyuruyu Laboratuvar ait genel bakış sayfasında görüntülenir ve kullanıcı bunu daha fazla ayrıntı için seçebilirsiniz.

Duyuru özelliği geçici bildirimler için kullanılmak üzere tasarlanmıştır.  Artık gerekli olmadığında duyuru kolayca devre dışı bırakabilirsiniz.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Var olan bir laboratuar ortamında duyuru sonrası adımlar

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı altında bir Pano üzerinde zaten görüntülenebilir **tüm kaynakları**).
1. Duyuru sonrası istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar 's üzerinde **genel bakış** alanında **yapılandırma ve ilkeleri**.  

    ![Yapılandırma ve ilkeleri düğmesi](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Solda'nin altında **ayarları**seçin **Laboratuvar duyuru**.

    ![Laboratuvar duyuru düğmesi](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Kullanıcılar için bir mesaj bu laboratuvarda oluşturmak için **etkin** için **Evet**.

1. Girdiğiniz bir **sona erme tarihi** bir tarih ve saat sonra duyuruyu artık gösterilen kullanıcılara belirtmek için. Sona erme tarihi girmezseniz, devre dışı kadar duyuruyu kalır.

   > [!NOTE]
   > Duyurunun sona erdikten sonra artık kullanıcılara gösterilir, ancak hala var. **Laboratuvar duyuru** bölmesi. Kendisine düzenlemeleri yapın ve yeniden etkinleştirmek için yeniden etkinleştirin.
   >
   >

1. Girin bir **duyuru başlık** ve **duyuru metin**.

   Başlık en fazla 100 karakter olabilir ve Laboratuvar ait genel bakış sayfasında kullanıcıya gösterilir. Kullanıcı başlık seçerse, duyuru metin görüntülenir.

   Duyuru metin markdown kabul eder. Duyuru metin girerken, ekranın altındaki önizleme alanında ileti görüntüleyebilirsiniz.

    ![İletiyi oluşturmak için laboratuvar duyuru ekranı.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Seçin **kaydetmek** duyurunuzun göndermeye hazır olduğunda.

Artık bu duyuru Laboratuvar kullanıcılara göstermek istediğinizde, geri dönüp **Laboratuvar duyuru** sayfasında ve ayarlama **etkin** için **Hayır**. Sona erme tarihi belirtilen duyuru otomatik olarak en, tarih ve saat devre dışı bırakılır.

## <a name="steps-for-users-to-view-an-announcement"></a>Duyuru görüntülemek kullanıcılar için adımları

1. Gelen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin.

1. Laboratuvar için gönderilen bir duyuru varsa, bir bilgi uyarısı Laboratuvar ait genel bakış sayfanın en üstünde gösterilir. Bu bilgi bildirimin duyuruyu oluşturduğunuzda belirttiğiniz duyuru başlık ' dir.

    ![Genel bakış sayfasında Laboratuvar Duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Kullanıcı tüm duyuru görüntülemek için ileti seçebilirsiniz.

    ![Laboratuvar duyuru için daha fazla bilgi](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Değiştirme ya da bir laboratuvar İlkesi ayarlama, kullanıcıları bilgilendirmek için duyuru sonrası isteyebilirsiniz. [İlkeleri ve zamanlamaları ayarlama](devtest-lab-set-lab-policy.md) kısıtlamaları ve kuralları aboneliğinizi arasında özelleştirilmiş ilkelerini kullanarak uygulama hakkında bilgi sağlar.
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
