---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: d5b81a6d4bdda24208673e42757807aba60fea97
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263984"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir


## <a name="task-3-configure-administrative-group"></a>Görev 3: yönetim grubunu yapılandır
Bu yapılandırma görevi, Azure AD dizininde bir yönetim grubu oluşturun. Bu özel yönetim grubu adı *AAD DC Yöneticiler*. Bu grubun üyeleri, etki alanı yönetilen etki alanına katılmış makinede yönetici izinleri verilir. Etki alanına katılmış makinelerde bu Grup administrators grubuna eklenir. Ayrıca, bu grubun üyeleri, etki alanına katılmış makinelere uzaktan bağlanmak için Uzak Masaüstü'nü kullanabilirsiniz.

> [!NOTE]
> Azure Active Directory etki alanı Hizmetleri kullanılarak oluşturulan yönetilen etki alanında etki alanı yöneticisi veya kuruluş yöneticisi izinlerine sahip değil. Yönetilen etki alanlarında, bu izinleri hizmeti tarafından ayrılmış ve Kiracı içinde kullanıcılar için kullanılabilir duruma getirilmez. Ancak, bazı ayrıcalıklı işlemleri gerçekleştirmek için bu yapılandırma görevi oluşturulan özel yönetim grubunu kullanabilirsiniz. Bu işlemler, bilgisayarları etki alanına katılma, etki alanına katılmış makinede yönetim grubuna ait ve Grup İlkesi yapılandırma içerir.
>

Sihirbaz, yönetim grubu, Azure AD dizininde otomatik olarak oluşturur. Bu grubun 'AAD DC Yöneticiler' denir. Azure AD dizininizi bu ada sahip varolan bir grubu varsa, sihirbaz bu grubu seçin. Grup üyeliği kullanarak yapılandırabilirsiniz **yönetici grubuna** sihirbaz sayfası.

1. Grup üyeliğini yapılandırmak için tıklatın **AAD DC Yöneticiler**.

    ![Grup üyeliğini yapılandır](./media/getting-started/domain-services-blade-admingroup.png)

2. Tıklatın **üye eklemek** kullanıcıları Azure AD dizininizi yönetici grubuna eklemek için düğmesi.

3. İşiniz bittiğinde tıklatın **Tamam** üzerinde taşımayı **Özet** sihirbazın.


## <a name="deploy-your-managed-domain"></a>Yönetilen etki alanınızı dağıtma

1. Üzerinde **Özet** sayfası, yönetilen etki alanı için yapılandırma ayarlarını gözden geçirin. Değişiklik yapmak için sihirbazın herhangi bir adıma gerekirse geri dönebilirsiniz. İşiniz bittiğinde tıklatın **Tamam** yeni yönetilen etki alanı oluşturmak için.

    ![Özet](./media/getting-started/domain-services-blade-summary.png)

2. Azure AD etki alanı Hizmetleri dağıtımınızın ilerlemesini gösteren bir bildirim görürsünüz. Dağıtım için ayrıntılı ilerleme durumunu görmek için bildirime tıklayın.

    ![Bildirim - dağıtımı devam ediyor](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="check-the-deployment-status-of-your-managed-domain"></a>Yönetilen etki alanınızı dağıtım durumunu denetleyin
Yönetilen etki alanınızı sağlama işleminin bir saate kadar sürebilir.

1. Dağıtımınızı sürerken içinde 'etki alanı Hizmetleri'nde' arayabilirsiniz **arama kaynakları** arama kutusu. Seçin **Azure AD etki alanı Hizmetleri** arama sonuç. **Azure AD etki alanı Hizmetleri** sağlanmakta yönetilen etki alanı dikey penceresinde listelenir.

    ![Yönetilen etki alanı sağlanacak Bul](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Yönetilen etki alanı hakkında daha fazla ayrıntı görmek için yönetilen etki alanının adını (örneğin, ' contoso100.com')'yi tıklatın.

    ![Etki alanı hizmetleri - sağlama durumu](./media/getting-started/domain-services-provisioning-state.png)

3. **Genel bakış** sekmesi, yönetilen etki alanı şu anda sağlanan olduğunu gösterir. Tam olarak sağlanana kadar yönetilen etki alanı yapılandıramazsınız. Bu tamamen sağlanması yönetilen etki alanınız için bir saate kadar sürebilir.

    ![Etki alanı hizmetleri - sağlama durumu sırasında genel bakış sekmesi ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Yönetilen etki alanı tam olarak sağlandığında **genel bakış** sekmesi olarak etki alanı durumunu gösterir **çalıştıran**.

    ![Etki Alanı Hizmetleri - Tamamen hazır haldeki Genel Bakış sekmesi](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >Sağlama işlemi sırasında Azure AD etki alanı Hizmetleri kuruluş dizininizde "Etki alanı denetleyicisi Hizmetleri" ve "AzureActiveDirectoryDomainControllerServices" adlı uygulamaları oluşturur. Bu kurumsal uygulamalar, yönetilen etki alanınızı hizmet vermek için gereklidir. Bunlar herhangi bir zamanda silinmez zorunludur.
    >

5. Üzerinde **özellikleri** sekmesine, etki alanı denetleyicileri sanal ağı için kullanılabilen iki IP adresi bakın.

    ![Etki Alanı Hizmetleri - tam olarak sağlanan sonra Özellikleri sekmesi](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="need-help"></a>Yardım mı gerekiyor?
Bir veya iki sağlanacak yönetilen etki alanınız için her iki etki alanı denetleyicileri için bir saat sürebilir. Dağıtımınızı başarısız olduysa veya birden çok birkaç saat 'Bekleniyor' durumunda takıldı, için çekinmeyin [Yardım için ürün ekibine başvurun](active-directory-ds-contact-us.md).


## <a name="next-step"></a>Sonraki adım
[Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
