---
title: Geri yükleme veya yakın zamanda silinmiş bir kullanıcı Azure Active Directory'de kalıcı olarak kaldırma | Microsoft Docs
description: Silinmiş bir kullanıcı geri yükleme, geri yüklenebilen kullanıcılar görüntüleme veya Azure Active Directory'de bir kullanıcıyı kalıcı olarak sil
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/28/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 9eea61cc1b735c6505df87526cd526a9a4a6eed8
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Silinmiş bir kullanıcı Azure Active Directory'de geri yükleme

Bu makale, geri yüklemek veya önceden silinmiş bir kullanıcı kalıcı olarak silmek için yönergeler içerir. Azure Active Directory'de (Azure AD) kullanıcı sildiğinizde, silinen kullanıcı silme tarihten itibaren 30 gün süreyle tutulur. Bu işlem sırasında kullanıcı ve özelliklerini geri yüklenebilir. 

> [!wARNING]
> Kalıcı olarak silindikten sonra kullanıcı geri yüklenemez.


## <a name="how-to-restore-a-recently-deleted-user"></a>Yakın zamanda silinmiş bir kullanıcı geri yükleme
Kullanıcı son silindiğinde, tüm dizin bilgilerinin korunur. Kullanıcı geri yüklediyseniz, bu bilgileri de geri yüklendi.

1. İçinde [Azure AD Yönetim Merkezi](https://aad.portal.azure.com)seçin **kullanıcılar ve gruplar** &gt; **tüm kullanıcılar**. 
2. Altında **Göster**, sayfanın göstermek için filtre **son kullanıcılar'ı silinmiş**. 
3. Bir veya daha fazla yakın zamanda silinen kullanıcılar'ı seçin.
4. Seçin **geri kullanıcı**.

## <a name="how-to-permanently-delete-a-recently-deleted-user"></a>Yakın zamanda silinmiş bir kullanıcı kalıcı olarak silmek nasıl

1. İçinde [Azure AD Yönetim Merkezi](https://aad.portal.azure.com)seçin **kullanıcılar ve gruplar** &gt; **tüm kullanıcılar**. 
2. Altında **Göster**, sayfanın göstermek için filtre **son kullanıcılar'ı silinmiş**. 
3. Bir veya daha fazla yakın zamanda silinen kullanıcılar'ı seçin.
4. Seçin **kalıcı olarak silmek**.

## <a name="required-permissions"></a>Gerekli izinler
Aşağıdaki izinler kullanıcı geri yüklemek yeterli değil.

Rol  | İzinler 
--------- | ---------
Şirket Yöneticisi<p>Partner Tier1 Desteği<p>Partner Tier2 Desteği<p>Kullanıcı Hesabı Yöneticisi | Silinen kullanıcılar geri yükleyebilirsiniz 
Şirket Yöneticisi<p>Partner Tier1 Desteği<p>Partner Tier2 Desteği<p>Kullanıcı Hesabı Yöneticisi | Kullanıcıların kalıcı olarak silebilir

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure Active Directory kullanıcı yönetimi hakkında ek bilgiler sağlar.

* [Hızlı Başlangıç: Ekleme veya Azure Active Directory Kullanıcıları silme](add-users-azure-active-directory.md)
* [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
* [Başka bir dizinden Konuk kullanıcılar ekleme](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Bir kullanıcı, Azure AD'de rol atama](active-directory-users-assign-role-azure-portal.md)
