---
title: Azure Active Directory B2C'de bir B2C kiracısına geçiş | Microsoft Docs
description: Active Directory B2C kiracınızın bağlamına geçiş yapma.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/13/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: ee6d85788211315412b7cbd3c82532dcd45162f7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64697699"
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınıza geçiş yapma

Azure AD B2C’yi yapılandırmak için Azure AD B2C kiracınızın bağlamında olmanız gerekir.

## <a name="log-into-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızda oturum açın.

Azure AD B2C kiracınıza gitmek için Azure portalında Azure AD B2C kiracısının genel yöneticisi olarak oturum açmanız gerekir.

1. [Azure portal](https://portal.azure.com) oturum açın.
1. E-posta adresinize veya sağ üst köşedeki resme tıklayarak kiracılar arasında geçiş yapabilirsiniz.
1. Görünecek `Directory` listesinden, yönetmek istediğiniz Azure AD B2C kiracısını seçin.

Azure portalı yenilenir.  Azure Portal'da Azure AD B2C kiracınızın bağlamında oturum açmış olursunuz.

## <a name="navigate-to-the-b2c-features-pane"></a>B2C özellikleri bölmesine gitme

1. Gezinti bölmesinin sol tarafındaki **Gözat** seçeneğine tıklayın.
1. **Tüm hizmetler**’e tıklayın ve ardından sol gezinti bölmesinde `Azure AD B2C` seçeneğini bulun.  (Sol taraftaki başlangıç panonuza sabitlemek için Azure AD B2C’nin solundaki yıldıza tıklayın)
1. B2C özellikleri bölmesine erişmek için **Azure AD B2C** seçeneğine tıklayın.
   
    ![' In ekran görüntüsü B2C özellikleri bölmesine için Gözat](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> B2C özellikleri bölmesine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
