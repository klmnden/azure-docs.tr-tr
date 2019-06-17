---
title: 'Azure Active Directory etki alanı Hizmetleri: Yönetim Kılavuzu | Microsoft Docs'
description: Azure AD Domain Services yönetilen etki alanlarında bir kuruluş birimi (OU) oluşturun
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: mstephen
ms.openlocfilehash: e7ff91a87377deed06fa4214cc1e5764e8b2da92
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246277"
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD Domain Services yönetilen etki alanında bir kuruluş birimi (OU) oluşturma
Azure AD Domain Services yönetilen etki alanlarını 'AADDC Computers' ve 'AADDC Users' adlı, sırasıyla iki yerleşik kapsayıcılar içerir. 'AADDC Computers' kapsayıcısı, yönetilen etki alanına katılmış olan tüm bilgisayarlar için bilgisayar nesneleri içerir. 'AADDC Users' kapsayıcı kullanıcıları ve grupları Azure AD kiracısında içerir. Bazen, iş yüklerinin dağıtımı için yönetilen etki alanında hizmet hesaplarını oluşturmak gerekli olabilir. Bu amaç için yönetilen etki alanına özel kuruluş birimi (OU) oluşturun ve o OU içinde hizmet hesapları oluşturun. Bu makalede, yönetilen etki alanında OU oluşturma işlemini gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. Bir etki alanına katılmış sanal makine, Azure AD Domain Services yönettiğiniz etki alanı yönetilebilir. Böyle bir sanal makineye sahip değilseniz, başlıklı makalede açıklanan tüm görevleri izleyin [bir Windows sanal makine için yönetilen etki alanına Katıl](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini ihtiyacınız bir **kullanıcı hesabının 'AAD DC Administrators' grubuna ait** yönetilen etki alanınızda özel bir OU oluşturmak için dizinde.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Uzaktan Yönetim için bir etki alanına katılmış sanal makine AD yönetim araçlarını yükleyin
Azure AD Domain Services yönetilen etki alanlarını, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi tanıdık Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Kiracı yöneticileri Uzak Masaüstü aracılığıyla yönetilen etki alanındaki etki alanı denetleyicisine bağlanmak için gerekli ayrıcalıklara sahip değilsiniz. Yönetilen etki alanını yönetmek için yönetilen etki alanına katılmış bir sanal makinede AD Yönetim Araçları özelliği yükleyin. Başlıklı makalesine bakabilirsiniz [bir Azure AD Domain Services etki alanını yönetme](manage-domain.md) yönergeler için.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Yönetilen etki alanında kuruluş birimi oluşturma
AD Yönetim Araçları yüklü olan göre sanal makine etki alanına katılmış, bu araçlar yönetilen etki alanında kuruluş birimi oluşturmak için kullanabiliriz. Aşağıdaki adımları gerçekleştirin:

> [!NOTE]
> Yalnızca 'AAD DC Administrators' grubunun üyeleri, özel bir OU oluşturmanız için gerekli ayrıcalıklara sahip. Bu gruba ait olan bir kullanıcı olarak aşağıdaki adımları gerçekleştirdiğinizden emin olun.
>
>

1. Başlangıç ekranından tıklayın **Yönetimsel Araçlar**. Sanal makinede yüklü AD Yönetimsel Araçlar görmeniz gerekir.

    ![Sunucuda yüklü Yönetim Araçları](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Tıklayın **Active Directory Yönetim Merkezi'ni**.

    ![Active Directory Yönetim Merkezi](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Etki alanı görüntülemek için etki alanı adı (örneğin, ' contoso100.com') sol bölmesinde tıklayın.

    ![ADAC - görünüm etki alanı](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Sağ taraftaki **görevleri** bölmesinde tıklayın **yeni** etki alanı adı düğümü altında. Bu örnekte, ı **yeni** işlecin sağ tarafındaki 'contoso100(local)' düğümünün altında **görevleri** bölmesi.

    ![ADAC - yeni OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Bir kuruluş birimi oluşturma seçeneğini görmeniz gerekir. Tıklayın **kuruluş birimi** başlatmak için **kuruluş birimi oluşturma** iletişim.
6. İçinde **kuruluş birimi oluşturma** iletişim kutusunda belirtin bir **adı** yeni OU için. OU için kısa bir açıklama sağlayın. Ayrıca ayarlayabilir **yöneten** OU için alan. Özel bir OU oluşturmak için tıklayın **Tamam**.

    ![ADAC - OU iletişim kutusu oluşturma](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. Yeni oluşturulan OU artık AD Yönetim Merkezi (ADAC) içinde görüntülenmesi gerekir.

    ![ADAC - OU oluşturuldu](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Yeni oluşturulan OU'lar için izinleri/güvenliği
Varsayılan olarak, özel OU oluşturan kullanıcının ('AAD DC Administrators' grubunun üyesi) OU üzerinde yönetim ayrıcalıkları (tam denetim) verilir. Kullanıcı daha sonra devam edin ve diğer kullanıcılara ya da istediğiniz gibi 'AAD DC Administrators' grubuna ayrıcalıkları verme. Aşağıdaki ekran görüntüsünde, kullanıcı görüldüğü gibi 'bob@domainservicespreview.onmicrosoft.com' Yeni 'MyCustomOU' kuruluş birimi oluşturan kişi, üzerinde tam denetim verilir.

 ![ADAC - yeni OU güvenlik](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Notları özel OU'ları yönetme
Özel bir OU oluşturduğunuza göre devam edin ve bu OU'da kullanıcıları, grupları, bilgisayarlar ve hizmet hesapları oluşturun. Özel OU'lara 'AADDC Users ' OU kullanıcıları veya grupları taşınamıyor.

> [!WARNING]
> Kullanıcı hesapları, grupları, hizmet hesapları ve özel OU'lar altında oluşturduğunuz bilgisayar nesnelerini Azure AD kiracınızda kullanılabilir değil. Diğer bir deyişle, bu nesneler Azure AD Graph API'sini kullanarak veya Azure AD kullanıcı arabiriminde görünmez. Bu nesneler, yalnızca Azure AD Domain Services yönetilen etki alanında kullanılabilir.
>
>

## <a name="related-content"></a>İlgili İçerik
* [Bir Azure AD Domain Services etki alanını yönetin](manage-domain.md)
* [Azure AD etki alanı Hizmetleri için Grup İlkesi yönetme](manage-group-policy.md)
* [Active Directory Yönetim Merkezi: Çalışmaya başlama](https://technet.microsoft.com/library/dd560651.aspx)
* [Hizmet hesapları adım adım kılavuzu](https://technet.microsoft.com/library/dd548356.aspx)
