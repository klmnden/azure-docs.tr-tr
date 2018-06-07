---
title: 'Azure AD Connect eşitleme: AD DS hesap parolasını değiştirme | Microsoft Docs'
description: Bu konuda belge, AD DS hesabı için parola değiştirildikten sonra Azure AD Connect güncelleştirme açıklar.
services: active-directory
keywords: AD DS hesabı, Active Directory hesap parolası
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a4d0d062b28b03de7f1e606202dddae28bf6a2f3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592468"
---
# <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
AD DS hesabı şirket içi Active Directory ile iletişim kurmak için Azure AD Connect tarafından kullanılan kullanıcı hesabına başvuruyor. AD DS hesabı için parola değiştirirseniz, Azure AD Connect eşitleme hizmeti yeni parolayla güncelleştirmeniz gerekir. Aksi takdirde eşitleme artık doğru şekilde şirket içi Active Directory ile eşitleyebilir ve şu hatalarla karşılaşır:

* Eşitleme Hizmeti Yöneticisi, içeri aktarma veya dışarı aktarma işlemi şirket içi AD ile başarısız **Hayır başlangıç kimlik** hata.

* Windows Olay Görüntüleyicisi'ni altında uygulama olay günlüğüne bir hata ile içeren **olay kimliği 6000** ve ileti **'yönetim Aracısı "contoso.com" kimlik bilgileri geçersiz olduğundan çalıştırılamadı'**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>AD DS hesabı için yeni parolayla eşitleme hizmetini güncelleştirme
Yeni bir parola ile eşitleme hizmeti güncelleştirmek için:

1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Git **Bağlayıcılar** sekmesi.

3. Seçin **AD Bağlayıcısı** parolası değiştirildi AD DS hesabına karşılık gelir.

4. Altında **Eylemler**seçin **özellikleri**.

5. Açılan iletişim kutusunda seçin **Active Directory ormanına Bağlan**:

6. Yeni AD DS hesap parolasını girin **parola** metin kutusu.

7. Tıklatın **Tamam** yeni parolayı Kaydet ve açılır iletişim kutusunu kapatın.

8. Yeniden başlatma Azure AD Connect eşitleme hizmeti altında Windows Hizmet Denetimi Yöneticisi. Bu eski parola herhangi bir referans bellek önbelleğinden kaldırıldığından emin olmak için yapılır.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
