---
title: Azure DevTest Labs'de bir laboratuvar için bir Duyurusu gönderin | Microsoft Docs
description: Duyuru Azure DevTest labs'deki bir laboratuvara ekleme hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 3282a90069ecd119154df492ac6dc366d26b5300
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611214"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir laboratuvar için bir duyuru gönderin

Laboratuvar Yöneticisi olarak, son değişiklikleri veya laboratuvara ekleme konusunda kullanıcıları bilgilendirmek üzere var olan bir laboratuvar içindeki özel bir duyuru gönderebilir. Örneğin, hakkında kullanıcılara bildirmek isteyebilirsiniz:

- Kullanılabilir olan yeni sanal makine boyutları
- Şu anda kullanılamaz durumda görüntüleri
- Laboratuvar ilkeleri güncelleştirmeleri

Sonra gönderilen, duyuru Laboratuvar genel bakış sayfasında görüntülenir ve kullanıcı bunu daha fazla ayrıntı için seçebilirsiniz.

Duyuru özellik geçici bildirimler için kullanılmak üzere tasarlanmıştır.  Artık gerekli olmadığında bir duyuru kolayca devre dışı bırakabilirsiniz.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Mevcut bir laboratuvarda duyuru göndermek için adımları

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı zaten altında Panoda görüntülenebilir **tüm kaynakları**).
1. Duyuru gönderin istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar'ın **genel bakış** alanında **yapılandırması ve ilkelerini**.  

    ![Yapılandırması ve ilkelerini düğmesi](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Soldaki altında **ayarları**seçin **Laboratuvar duyuru**.

    ![Laboratuvar duyuru düğmesi](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Bu laboratuvarda kullanıcılar için bir ileti oluşturmak için **etkin** için **Evet**.

1. Girebileceğiniz bir **sona erme tarihi** bir tarih ve saat sonra bildiri artık gösterilen kullanıcıları belirtmek için. Sona erme tarihi girmezseniz, bunu devre dışı kadar duyuruyu kalır.

   > [!NOTE]
   > Duyurunun sona erdikten sonra artık kullanıcılara gösterilir, ancak hala var. **Laboratuvar duyuru** bölmesi. Düzenlemeler yapmak ve yeniden etkinleştirmek için yeniden etkinleştirin.
   >
   >

1. Girin bir **Duyurunun başlığını** ve **duyuru metin**.

   Başlık en fazla 100 karakter olabilir ve Laboratuvar genel bakış sayfasında kullanıcıya gösterilir. Kullanıcı başlığı seçerse, duyuru metni görüntülenir.

   Duyuru metin markdown kabul eder. Duyuru metin girerken, ekranın alt kısmındaki Önizleme alanında ileti görüntüleyebilirsiniz.

    ![İleti oluşturmak üzere Laboratuvar duyuru ekranı.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Seçin **Kaydet** , duyuru gönderilmeye hazır olduğunda.

Artık Laboratuvar kullanıcılara bu duyuru göstermek istediğiniz zaman dönün **Laboratuvar duyuru** sayfasında ve ayarlayın **etkin** için **Hayır**. Sona erme tarihi belirtilen duyuruyu otomatik olarak en, tarih ve saat devre dışı bırakılır.

## <a name="steps-for-users-to-view-an-announcement"></a>Bir duyurunun görüntülemek kullanıcılar için adımları

1. Gelen [Azure portalında](http://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin.

1. Laboratuvar için gönderilen bir duyuru varsa, bir bilgi uyarısı Laboratuvar genel bakış sayfasının en üstünde gösterilir. Bu bilgi uyarısı duyuruyu oluştururken belirttiğiniz Duyurunun başlığını ' dir.

    ![Genel bakış sayfasında Laboratuvar Duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Kullanıcının, iletinin tamamı duyuruyu görüntüleyin seçebilirsiniz.

    ![Daha fazla bilgi için laboratuvar Duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Laboratuvar ilkesini ayarlamak veya değiştirirseniz, kullanıcılara bildiren bir duyuru gönderin isteyebilirsiniz. [İlke ve zamanlamalar ayarlama](devtest-lab-set-lab-policy.md) özelleştirilmiş ilkeler kullanılarak aboneliğiniz kısıtlamaları ve kurallarını uygulama hakkında bilgi sağlar.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
