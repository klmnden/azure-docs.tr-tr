---
title: Azure DevTest Labs laboratuarda bir iç destek ifade ekleyin | Microsoft Docs
description: Azure DevTest Labs'de Laboratuvar için bir iç destek deyimi sonrası öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 2d12ca26fb2aa5abddcf44b2e634b2f08b1fb01b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787433"
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuarda bir iç destek ifade ekleyin

Azure DevTest Labs Laboratuvarınızı kullanıcıların Laboratuvar hakkında destek bilgileri sağlayan bir iç destek ifadesiyle özelleştirmenizi sağlar. Örneğin, böylece bir kullanıcının, sorun giderme veya laboratuvara kaynaklara erişme konusunda Yardım gerektiğinde iç destek ulaşması bildiği kişi bilgilerini sağlayın. İç Web siteleri veya ekibiniz destek ile irtibat kurmadan önce erişebilmeniz için sık sorulan sorular için bağlantılar sağlar.

Bir iç destek deyimi genellikle çok sık değişmeyen Laboratuvar bilgileri postalamak izin verecek şekilde tasarlanmıştır. Kullanıcıları yapısı – Laboratuvar ilkeleri – en son güncelleştirmeleri gibi daha geçicidir Laboratuvar bilgisi hakkında bilgilendirmek için bkz: [bir laboratuvar Post Duyuruda](devtest-lab-announcements.md).

Kolayca devre dışı veya artık geçerli olmadığı sonra bir destek bildirimiyle düzenleyin.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Varolan bir laboratuvar için bir destek bildirimiyle ekleme adımları

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı altında bir Pano üzerinde zaten görüntülenebilir **tüm kaynakları**).
1. Bir destek bildirimiyle eklemek istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar 's üzerinde **genel bakış** alanında **yapılandırma ve ilkeleri**.  

    ![Yapılandırma ve ilkeleri düğmesi](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Solda'nin altında **ayarları**seçin **iç destek**.

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Etkin kullanıcılar için bir iç destek iletisi bu laboratuvarda oluşturmak için kümesine **Evet**.

1. İçinde **destek iletisini** alan, Laboratuvar kullanıcılarınıza sunmak istediğiniz iç destek deyimi girin. Destek iletisini Markdown kabul eder. İleti metni girerken, görüntüleyebileceğiniz **Önizleme** iletinin kullanıcılara nasıl göründüğünü görmek için ekranın alanında.

    ![İletiyi oluşturmak için dahili destek ekranı.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Seçin **kaydetmek** destek deyiminizde göndermeye hazır olduğunda.

Artık bu destek iletisini Laboratuvar kullanıcılara göstermek istediğinizde, geri dönüp **iç destek** sayfasında ve ayarlama **etkin** için **Hayır**.

## <a name="steps-for-users-to-view-the-support-message"></a>Kullanıcıların destek iletisini görüntülemek adımları

1. Gelen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin.

1. Laboratuvar 's üzerinde **genel bakış** alanında **iç destek**.  

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Kullanıcı desteği iletiye gönderilen, iç destek bölmesinden görüntüleyebilirsiniz.

    ![Destek gönderilme gösteren iç destek bölmesi](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* İç destek bildirimleri genellikle sık değiştirmez destek bilgileri sağlamak için kullanılır. Ayrıca öğrenebilirsiniz nasıl [duyuru laboratuvara sonrası](devtest-lab-announcements.md) geçici değişiklikler veya Laboratuvar güncelleştirmeleri kullanıcıları bilgilendirmek için.
* [İlkeleri ve zamanlamaları ayarlama](devtest-lab-set-lab-policy.md) nasıl, diğer kısıtlamaları ve kuralları aboneliğinizi arasında özelleştirilmiş ilkeler kullanarak uygulayabilirsiniz hakkında bilgi sağlar.