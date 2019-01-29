---
title: Azure Active Directory B2C'de bir B2C kiracısına geçiş | Microsoft Docs
description: Active Directory B2C kiracınızın bağlamına geçiş yapma.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/13/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9522e921b59ddc57a6039c625bf6b0af4c4b4af2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181818"
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
   
    ![B2C özelliklerine göz atma bölmesinin ekran görüntüsü](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> B2C özellikleri bölmesine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
