---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme
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
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: 2290273c1b998a2d75046fcbcf613762ddd588ee
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503215"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme


## <a name="task-3-configure-administrative-group"></a>3. Görev: Yönetici grubu yapılandırma
Bu yapılandırma görevi Azure AD dizininizde bir yönetim grubu oluşturun. Bu özel yönetim grubu adı *AAD DC Administrators*. Bu grubun üyeleri, yönetilen etki alanına etki alanı ile birleşik olan makineler üzerinde yönetim izinleri verilir. Etki alanına katılmış makinelerde, bu grubun Yöneticiler grubuna eklenir. Ayrıca, bu grubun üyeleri, etki alanına katılan makineler için uzaktan bağlanmak için Uzak Masaüstü'nü kullanabilirsiniz.

> [!NOTE]
> Azure Active Directory Domain Services'ı kullanarak oluşturduğunuz yönetilen etki alanındaki etki alanı yöneticisi veya kuruluş yöneticisi izinlerine sahip değil. Yönetilen etki alanlarında, bu izinleri hizmet tarafından ayrılmıştır ve Kiracı içindeki kullanıcılar için kullanılabilir duruma getirilmez. Ancak, bazı ayrıcalıklı işlemleri gerçekleştirmek için bu yapılandırma görevde oluşturduğunuz özel yönetici grubunu kullanabilirsiniz. Bu işlemler, bilgisayarlar etki alanına katılma, etki alanına katılmış makinelerde yönetim grubuna ait ve Grup İlkesi yapılandırma içerir.
>

Sihirbaz, yönetim grubunu Azure AD dizininizde otomatik olarak oluşturur. Bu grup, 'AAD DC Administrators' adı verilir. Bu ada sahip mevcut bir grup Azure AD dizininiz varsa, bu grubu Sihirbazı'nı seçer. Grup üyeliği kullanarak yapılandırabilirsiniz **yönetici grubuna** sihirbaz sayfası.

1. Grup üyeliğini yapılandırmak için tıklayın **AAD DC Administrators**.

    ![Grup üyeliğini yapılandır](./media/getting-started/domain-services-blade-admingroup.png)

2. Tıklayın **üye ekleme** kullanıcıları Azure AD dizininizdeki yönetici grubuna eklemek için düğme.

3. İşiniz bittiğinde tıklayın **Tamam** üzerinde taşımayı **özeti** Sihirbazı sayfası.


## <a name="deploy-your-managed-domain"></a>Yönetilen etki alanınıza dağıtma

1. Üzerinde **özeti** sayfası, yönetilen etki alanı için yapılandırma ayarlarını gözden geçirin. Ayrıca gerekirse değişiklik yapmak için sihirbazın herhangi bir adıma geri dönebilirsiniz. İşiniz bittiğinde tıklayın **Tamam** yeni yönetilen etki alanı oluşturmak için.

    ![Özet](./media/getting-started/domain-services-blade-summary.png)

2. Azure AD Domain Services dağıtımınızın ilerlemesini gösteren bir bildirim görürsünüz. Dağıtım için ayrıntılı ilerleme durumunu görmek için bildirime tıklayın.

    ![Bildirim - dağıtım devam ediyor](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="check-the-deployment-status-of-your-managed-domain"></a>Yönetilen etki alanınıza dağıtım durumunu denetleyin
Yönetilen etki alanınıza sağlama işleminin bir saate kadar sürebilir.

1. Dağıtım işlemi devam ederken 'etki alanı Hizmetleri için' arama yapabilir **kaynak Ara** arama kutusu. Seçin **Azure AD Domain Services** arama sonuç. **Azure AD Domain Services** sağlanıyor yönetilen etki alanı dikey penceresinde listelenir.

    ![Yönetilen etki alanı hala uygulanmakta Bul](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Yönetilen etki alanı hakkında daha fazla ayrıntı görmek için yönetilen etki alanı adını (örneğin, ' contoso100.com')'ye tıklayın.

    ![Etki alanı hizmetleri - sağlama durumu](./media/getting-started/domain-services-provisioning-state.png)

3. **Genel bakış** sekme gösterir yönetilen etki alanı şu anda sağlanıyor. Tam olarak sağlanana kadar yönetilen etki alanı yapılandıramazsınız. Bu tam olarak hazırlanmasını yönetilen etki alanınız için bir saate kadar sürebilir.

    ![Etki alanı hizmetleri - sağlama durumu sırasında genel bakış sekmesi ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Yönetilen etki alanı tam olarak sağlandığından, **genel bakış** sekme olarak etki alanı durumunu gösterir **çalıştıran**.

    ![Etki Alanı Hizmetleri - Tamamen hazır haldeki Genel Bakış sekmesi](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >Sağlama işlemi sırasında Azure AD Domain Services dizininiz "Etki alanı denetleyicisi Hizmetler" ve "AzureActiveDirectoryDomainControllerServices" adlı kurumsal uygulamalar oluşturur. Bu kurumsal uygulamalar, yönetilen etki alanınıza hizmet vermek için gereklidir. Bunlar herhangi bir zamanda silinmez zorunludur.
    >

5. Üzerinde **özellikleri** sekmesi, etki alanı denetleyicisi sanal ağ için kullanılabilir olan iki IP adresi görürsünüz.

    ![Etki Alanı Hizmetleri - tamamen hazır haldeki Özellikleri sekmesi](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="need-help"></a>Yardım mı gerekiyor?
Bir veya iki sağlanması yönetilen etki alanınız için her iki etki alanı denetleyicileri için bir saat sürebilir. Dağıtım başarısız oldu veya birkaç saat değerinden daha fazla bilgi için 'Bekleniyor' durumunda takılı rahatça [Yardım için ürün ekibiyle](active-directory-ds-contact-us.md).


## <a name="next-step"></a>Sonraki adım
[Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
