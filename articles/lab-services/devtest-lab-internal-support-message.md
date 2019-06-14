---
title: Azure DevTest labs'deki bir laboratuvara iç destek deyimi ekleyin | Microsoft Docs
description: Azure DevTest Labs'de bir laboratuvar için bir dahili destek bildirimiyle gönderileceği hakkında bilgi edinin
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
ms.openlocfilehash: deb98c2c633200ab4be1d763a94fd2a04979a3b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60562347"
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara iç destek deyimi ekleyin

Azure DevTest Labs Laboratuvarınızı kullanıcıların Laboratuvar hakkında destek bilgileri sağlayan bir iç destek bildirimiyle birlikte özelleştirmenizi sağlar. Örneğin, bir kullanıcı, sorun giderme veya Laboratuvardaki kaynaklara erişme konusunda Yardım gerektiğinde iç destek erişmek nasıl bilebilmesi kişi bilgilerini sağlayın. İç Web siteleri veya desteği'ne başvurmadan önce ekibinizin erişebileceği SSS sayfaları için bağlantılar sağlar.

Bir iç destek bildirimiyle genellikle çok sık değişmeyen Laboratuvar bilgileri sağlamak için tasarlanmıştır. Daha geçici yapısı – Laboratuvar ilkeleri – en son güncelleştirmeleri gibi bir laboratuvar bilgisi konusunda kullanıcıları bilgilendirmek için bkz. [Post, laboratuvarda duyuru yayınlama](devtest-lab-announcements.md).

Kolayca devre dışı veya artık geçerli olmadığı sonra bir destek bildirimiyle düzenleyin.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Bir destek bildirimiyle var olan bir laboratuvara ekleme adımları

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı zaten altında Panoda görüntülenebilir **tüm kaynakları**).
1. Bir destek bildirimiyle eklemek istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar'ın **genel bakış** alanında **yapılandırması ve ilkelerini**.  

    ![Yapılandırması ve ilkelerini düğmesi](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Soldaki altında **ayarları**seçin **iç destek**.

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Etkin kullanıcılar için bir dahili destek iletisini bu laboratuvarda oluşturmak için kümesine **Evet**.

1. İçinde **destek iletisini** Laboratuvar kullanıcılarınıza sunmak istediğiniz iç destek deyimi girin. Destek iletisini Markdown kabul eder. İleti metni girerken, görüntüleyebileceğiniz **Önizleme** iletinin kullanıcılara nasıl göründüğünü görmek için ekranın altındaki alan.

    ![İletiyi oluşturmak için dahili destek ekranı'nı tıklatın.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Seçin **Kaydet** destek Ekstrenizi gönderi hazır olduğunda.

Artık Laboratuvar kullanıcılara bu destek iletisini göstermek istediğiniz zaman dönün **iç destek** sayfasında ve ayarlayın **etkin** için **Hayır**.

## <a name="steps-for-users-to-view-the-support-message"></a>Kullanıcıların destek iletisini görüntülemek adımları

1. Gelen [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin.

1. Laboratuvar'ın **genel bakış** alanında **iç destek**.  

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Bir destek iletisini bulunuyorsa, kullanıcı ve dahili destek bölmesinde görüntüleyebilirsiniz.

    ![Gönderilen destek iletisini gösteren iç destek bölmesi](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* İç destek bildirimleri, genellikle sık değişmeyen destek bilgileri sağlamak için kullanılır. Da bilgi edinebilirsiniz nasıl [laboratuvara duyuru gönderin](devtest-lab-announcements.md) geçici değişiklikleri veya Laboratuvar için güncelleştirmelerin kullanıcılara bildirmek için.
* [İlke ve zamanlamalar ayarlama](devtest-lab-set-lab-policy.md) nasıl, diğer kısıtlamaları ve kuralları aboneliğiniz özelleştirilmiş ilkeler kullanarak uygulayabilirsiniz hakkında bilgi sağlar.