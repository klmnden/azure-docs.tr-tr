---
title: "Silinmiş bir kullanıcı Azure Active Directory'de geri | Microsoft Docs"
description: "Silinmiş bir kullanıcı geri yükleme, geri yüklenebilen kullanıcılar görüntüleme veya Azure Active Directory'de bir kullanıcıyı kalıcı olarak sil"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: c3b7550c2aea0e8bcb7998e0e8c732894b500403
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Silinmiş bir kullanıcı Azure Active Directory'de geri yükleme

Bu makale, geri yüklemek veya önceden silinmiş bir kullanıcı kalıcı olarak silmek için yönergeler içerir. Azure Active Directory'de (Azure AD) kullanıcı sildiğinizde, silinen kullanıcı silme tarihten itibaren 30 gün süreyle tutulur. Bu işlem sırasında kullanıcı ve özelliklerini geri yüklenebilir. 

## <a name="required-permissions"></a>Gerekli izinler
Aşağıdaki izinler kullanıcı geri yüklemek yeterli değil.

Rol  | İzinler 
--------- | ---------
Şirket Yöneticisi<p>Partner Tier1 Desteği<p>Partner Tier2 Desteği<p>Kullanıcı Hesabı Yöneticisi | Silinen kullanıcılar geri yükleyebilirsiniz 
Şirket Yöneticisi<p>Partner Tier1 Desteği<p>Partner Tier2 Desteği<p>Kullanıcı Hesabı Yöneticisi | Kullanıcıların kalıcı olarak silebilir

## <a name="how-to-restore-a-deleted-user"></a>Silinmiş bir kullanıcı geri yükleme

Azure portalında silinmiş bir kullanıcı geri hem silinmiş bir kullanıcıyı kalıcı olarak sil.

1. İçinde [Azure AD Yönetim Merkezi](https://aad.portal.azure.com)seçin **kullanıcılar ve gruplar** &gt; **tüm kullanıcılar**. 
2. Altında **Göster**, sayfanın göstermek için filtre **son kullanıcılar'ı silinmiş**. 
3. Bir veya daha fazla yakın zamanda silinen kullanıcılar'ı seçin.
4. Seçin **geri kullanıcı** veya **kalıcı olarak silmek**.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure Active Directory kullanıcı yönetimi hakkında ek bilgiler sağlar.

* [Hızlı Başlangıç: Ekleme veya Azure Active Directory Kullanıcıları silme](add-users-azure-active-directory.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Başka bir dizinden Konuk kullanıcılar ekleme](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Bir kullanıcı, Azure AD'de rol atama](active-directory-users-assign-role-azure-portal.md)
