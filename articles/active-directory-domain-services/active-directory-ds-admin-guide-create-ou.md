---
title: 'Azure Active Directory etki alanı Hizmetleri: Yönetim Kılavuzu | Microsoft Docs'
description: Azure AD etki alanı Hizmetleri yönetilen etki alanlarında bir kuruluş birimi (OU) oluşturun
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 15bd837149b9856897eb83f86052a26b24a21fb0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334318"
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD etki alanı Hizmetleri yönetilen etki alanında bir kuruluş birimi (OU) oluşturun
Azure AD etki alanı Hizmetleri yönetilen etki alanı 'AADDC bilgisayarlar' ve 'AADDC kullanıcılar' sırasıyla adlı iki yerleşik kapsayıcı içerir. 'AADDC bilgisayarlar' kapsayıcısı, yönetilen etki alanına katılan tüm bilgisayarlar için bilgisayar nesneleri içerir. 'AADDC kullanıcılar' kapsayıcı Azure AD kiracısında kullanıcıları ve grupları içerir. Bazen, iş yükleri dağıtmak için yönetilen etki alanında hizmet hesaplarını oluşturmak gerekli olabilir. Bu amaç için yönetilen etki alanına özel kuruluş birimi (OU) oluşturun ve o OU içinde hizmet hesapları oluşturun. Bu makalede, yönetilen etki alanınızdaki bir OU oluşturulacağını gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Bir etki alanına katılmış sanal makine Azure AD etki alanı hizmetleri yönetmek, etki alanı yönetilen. Bu tür bir sanal makine yoksa, başlıklı makalede açıklanan tüm görevleri izleyin [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini gereken bir **'AAD DC Yöneticiler' grubuna ait olan kullanıcı hesabı** dizininizde yönetilen etki alanınızda özel bir OU oluşturmak için.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Uzaktan Yönetim için etki alanına katılmış bir sanal makine AD yönetim araçlarını yükleyin
Azure AD etki alanı Hizmetleri yönetilen etki alanları, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi bilinen Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Kiracı Yöneticiler Uzak Masaüstü aracılığıyla yönetilen etki alanında etki alanı denetleyicisine bağlanmak için ayrıcalıklara sahip değil. Yönetilen etki alanını yönetmek için yönetilen etki alanına katılan bir sanal makinede AD Yönetim Araçları özelliği yükleyin. Başlıklı makaleye bakın [Azure AD etki alanı Hizmetleri yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md) yönergeler için.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Yönetilen etki alanında kuruluş birimi oluşturma
AD Yönetim Araçları yüklü olan artık, sanal makine etki alanına katıldı, biz yönetilen etki alanında bir kuruluş birimi oluşturmak için bu araçları kullanabilirsiniz. Aşağıdaki adımları gerçekleştirin:

> [!NOTE]
> Yalnızca 'AAD DC Yöneticiler' grubunun üyeleri, özel bir OU oluşturmak için gerekli ayrıcalıklara sahip. Bu gruba ait olan bir kullanıcı olarak aşağıdaki adımları gerçekleştirdiğinizden emin olun.
>
>

1. Başlangıç ekranından tıklatın **Yönetimsel Araçlar**. Sanal makinede yüklü AD Yönetimsel Araçlar görmeniz gerekir.

    ![Sunucuda yüklü Yönetim Araçları](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Tıklatın **Active Directory Yönetim Merkezi**.

    ![Active Directory Yönetim Merkezi](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Etki alanı görüntülemek için sol bölmedeki (örneğin, ' contoso100.com') etki alanı adına tıklayın.

    ![ADAC - görünüm etki alanı](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Sağ taraftaki **görevleri** bölmesinde tıklatın **yeni** etki alanı adı düğümü altında. Bu örnekte, ı **yeni** sağ tarafındaki 'contoso100(local)' düğümünde **görevleri** bölmesi.

    ![ADAC - yeni OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Bir kuruluş birimi oluşturma seçeneğini görmeniz gerekir. Tıklatın **kuruluş birimi** başlatmak için **kuruluş birimi oluşturma** iletişim.
6. İçinde **kuruluş birimi oluşturma** iletişim kutusunda, belirtin bir **adı** yeni OU için. OU için kısa bir açıklama sağlayın. Ayrıca ayarlayabilir **yöneten** kuruluş birimi için alan. Özel OU oluşturmak için tıklatın **Tamam**.

    ![ADAC - Oluştur OU iletişim kutusu](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. Yeni oluşturulan OU artık AD Yönetim Merkezi (ADAC) olarak görünmelidir.

    ![ADAC - oluşturulan OU](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Yeni oluşturulan OU'lar için izinleri/güvenliği
Varsayılan olarak, özel OU oluşturan kullanıcının ('AAD DC Yöneticiler' grup üyesi) OU üzerinde yönetim ayrıcalıkları (tam denetim) verilir. Kullanıcı daha sonra devam edin ve diğer kullanıcılara ya da istediğiniz gibi 'AAD DC Yöneticiler' grubuna ayrıcalıkları vermek. Aşağıdaki ekran görüntüsünde kullanıcı görülen 'bob@domainservicespreview.onmicrosoft.com' Yeni 'MyCustomOU' kuruluş birimi oluşturan kişiyi tam denetime verilir.

 ![ADAC - yeni OU güvenlik](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Notlar özel OU'ları yönetme
Özel bir OU'da oluşturduğunuza göre şimdi ve kullanıcıları, grupları, bilgisayarlar ve hizmet hesapları bu OU'da oluşturun. Kullanıcıları veya grupları için özel OU 'AADDC kullanıcılardan' OU taşınamıyor.

> [!WARNING]
> Kullanıcı hesaplarını, grupları, hizmet hesapları ve özel OU'lar altında oluşturduğunuz bilgisayar nesneleri, Azure AD kiracınızda kullanılabilir değil. Diğer bir deyişle, bu nesnelerin Azure AD grafik API'sini kullanarak yukarı veya Azure AD kullanıcı Arabirimi gösterme. Bu nesneler, yalnızca Azure AD etki alanı Hizmetleri yönetilen etki alanında kullanılabilir.
>
>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Yönetilen bir etki alanında Grup İlkesi yapılandırma](active-directory-ds-admin-guide-administer-group-policy.md)
* [Active Directory Yönetim Merkezi: Başlarken](https://technet.microsoft.com/library/dd560651.aspx)
* [Hizmet hesapları adım adım kılavuzu](https://technet.microsoft.com/library/dd548356.aspx)
