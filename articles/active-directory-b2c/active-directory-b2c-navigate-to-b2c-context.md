---
title: "Azure Active Directory B2C: B2C kiracısına geçiş | Microsoft Belgeleri"
description: "Active Directory B2C kiracınızın bağlamına geçiş yapma"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 6c1fd08c52f33a062d06e0593cbbe00346bb44f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınıza geçiş yapma

Azure AD B2C’yi yapılandırmak için Azure AD B2C kiracınızın bağlamında olmanız gerekir.

## <a name="log-into-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızda oturum açın.

Azure AD B2C kiracınıza gitmek için Azure portalında Azure AD B2C kiracısının genel yöneticisi olarak oturum açmanız gerekir.

1. [Azure portal](http://portal.azure.com) oturum açın.
1. E-posta adresinize veya sağ üst köşedeki resme tıklayarak kiracılar arasında geçiş yapabilirsiniz.
1. Görünecek `Directory` listesinden, yönetmek istediğiniz Azure AD B2C kiracısını seçin.

Azure Portal yenilenir.  Azure Portal'da Azure AD B2C kiracınızın bağlamında oturum açmış olursunuz.

## <a name="navigate-to-the-b2c-features-blade"></a>B2C özellikleri dikey penceresine gitme

1. Gezinti bölmesinin sol tarafındaki **Gözat** seçeneğine tıklayın.
1. **> Daha fazla hizmet**’e tıklayın ve sol gezinti bölmesinde `Azure AD B2C` seçeneğini bulun.  (Sol taraftaki başlangıç panonuza sabitlemek için Azure AD B2C’nin solundaki yıldıza tıklayın)
1. B2C özellikleri dikey penceresine erişmek için **Azure AD B2C** seçeneğine tıklayın.
   
    ![B2C özelliklerine göz atma dikey penceresi ekran görüntüsü](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> B2C özellikleri dikey penceresine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.  Azure portalının sağ üst köşesindeki kiracı değiştiriciyi kullanarak B2C kiracınıza geçiş yapabilirsiniz.
