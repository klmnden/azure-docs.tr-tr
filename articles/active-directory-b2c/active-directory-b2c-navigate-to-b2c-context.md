---
title: Azure Active Directory B2C B2C kiracınızda geçiş | Microsoft Docs
description: Active Directory B2C kiracınızın bağlamına geçiş yapma.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 4/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 234dabe6da7f50e6102b79743f89e7bd81d55ed2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709997"
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınıza geçiş yapma

Azure AD B2C’yi yapılandırmak için Azure AD B2C kiracınızın bağlamında olmanız gerekir.

## <a name="log-into-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızda oturum açın.

Azure AD B2C kiracınıza gitmek için Azure portalında Azure AD B2C kiracısının genel yöneticisi olarak oturum açmanız gerekir.

1. [Azure portal](http://portal.azure.com) oturum açın.
1. E-posta adresinize veya sağ üst köşedeki resme tıklayarak kiracılar arasında geçiş yapabilirsiniz.
1. Görünecek `Directory` listesinden, yönetmek istediğiniz Azure AD B2C kiracısını seçin.

Azure portalı yenilenir.  Azure Portal'da Azure AD B2C kiracınızın bağlamında oturum açmış olursunuz.

## <a name="navigate-to-the-b2c-features-pane"></a>B2C özellikleri bölmesine gitme

1. Gezinti bölmesinin sol tarafındaki **Gözat** seçeneğine tıklayın.
1. **Tüm hizmetler**’e tıklayın ve ardından sol gezinti bölmesinde `Azure AD B2C` seçeneğini bulun.  (Sol taraftaki başlangıç panonuza sabitlemek için Azure AD B2C’nin solundaki yıldıza tıklayın)
1. B2C özellikleri bölmesine erişmek için **Azure AD B2C** seçeneğine tıklayın.
   
    ![B2C özelliklerine göz atma bölmesinin ekran görüntüsü](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> B2C özellikleri bölmesine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
