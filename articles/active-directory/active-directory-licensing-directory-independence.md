---
title: "Azure Active Directory özelliklerini Kiracı etkileşim | Microsoft Docs"
description: "Azure Active Kiracı kiracılarınız kiracılarınız tamamen bağımsız kaynaklar olarak anlayarak yönetme"
services: active-tenant
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 2c00f382546a0e5f1e9da9dc90777dcf9622ec94
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Birden çok Azure Active Directory kiracıları etkileşim anlama

Azure Active Directory (Azure AD), her bir kiracı tamamen bağımsız bir kaynaktır: eşe yönettiğiniz diğer kiracılardan mantıksal olarak bağımsızdır. Kiracılar arasında üst-alt ilişkisi yoktur. Kiracılar arasındaki bu bağımsızlığa kaynak bağımsızlığı, yönetim bağımsızlığı ve eşitleme bağımsızlığı dahildir.

## <a name="resource-independence"></a>Kaynak bağımsızlığı
* Oluşturmak veya bir kiracı kaynak silerseniz dış kullanıcılar kısmi özel başka bir kiracı herhangi bir kaynak üzerinde hiçbir etkisi yoktur. 
* Bir kiracı ile etki alanı adlarından birini kullanırsanız, başka hiçbir kiracıyla kullanılamaz.

## <a name="administrative-independence"></a>Yönetim bağımsızlığı
Kiracı 'Contoso' yönetici olmayan bir kullanıcı bir test Kiracı 'Test' ardından oluşturursa:

* Varsayılan olarak, bir kiracı oluşturan kullanıcının yeni Kiracı bir dış kullanıcı olarak eklendi ve Kiracı genel Yönetici rolüne atanmış.
* Bir yönetici 'Test' özellikle vermediği sürece 'Contoso' Kiracı Yöneticiler hiçbir Kiracı 'Test' için doğrudan yönetici ayrıcalıklarına sahip. Ancak, 'Test' oluşturulduğu kullanıcı hesabı denetimi yöneticileri olarak 'Contoso' Kiracı 'Test' na erişimi denetleyebilir
* Eklemek veya bir kullanıcı bir kiracı Yönetici rolü Kaldır, Değiştir kullanıcının başka bir kiracıya sahip yönetici rolleri etkilemez.

## <a name="synchronization-independence"></a>Eşitleme bağımsızlığı
Bağımsız olarak herhangi birinin tek bir örneğinden eşitlenen veri almak için her bir Azure AD Kiracı yapılandırabilirsiniz:

* Verileri tek bir AD ormanıyla eşitlemek için Azure AD Connect aracı.
* Bağlayıcı için Forefront Identity bir veya daha fazla şirket içi ormanları ve/veya Azure olmayan AD veri kaynakları ile veri eşitlemek için Manager, Azure Active Kiracı.

## <a name="add-an-azure-ad-tenant"></a>Azure AD kiracısı ekleme
Azure portalında Azure AD kiracısı eklemek için oturumu [Azure portalı](https://portal.azure.com) Azure AD genel yönetici olan bir hesapla, sol taraftaki, seçin ve **yeni**.

> [!NOTE]
> Diğer Azure kaynaklarının aksine, kiracılar bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğinizin süresi doldu veya iptal edilirse, Kiracı verilerinizi Azure PowerShell, Azure grafik API'sini veya Office 365 Yönetim merkezini kullanarak erişmeye devam edebilirsiniz. Ayrıca [başka bir abonelik Kiracı ile ilişkilendirmek](active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Sonraki adımlar
Azure AD lisans sorunları ve en iyi yöntemler genel bakış için bkz: [ne olduğu Azure Active Kiracı lisans?](active-directory-licensing-whatis-azure-portal.md).
