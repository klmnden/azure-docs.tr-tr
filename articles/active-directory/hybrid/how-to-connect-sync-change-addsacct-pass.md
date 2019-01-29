---
title: 'Azure AD Connect eşitleme:  AD DS hesap parolasını değiştirme | Microsoft Docs'
description: Bu konuda belge, Azure AD Connect AD DS hesap parolasını değiştirildikten sonra güncelleştirme işlemi açıklanmaktadır.
services: active-directory
keywords: AD DS hesabı, Active Directory hesabı parola
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: ac0237ebc9c6d9ffc6e66291f648b0176f386205
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168714"
---
# <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
AD DS hesabını Azure AD Connect tarafından şirket içi Active Directory ile iletişim kurmak için kullanılan kullanıcı hesabının ifade eder. AD DS hesap parolasını değiştirirseniz, Azure AD Connect eşitleme hizmeti yeni parolayla güncelleştirmeniz gerekir. Aksi takdirde eşitleme artık doğru şekilde şirket içi Active Directory ile eşitlenebilir ve şu hatalarla karşılaşırsınız:

* Eşitleme Hizmeti Yöneticisi, içeri aktarma veya dışarı aktarma işlemi ile şirket içi AD ile başarısız **Hayır başlangıç kimlik** hata.

* Altındaki Windows Olay Görüntüleyicisi, uygulama olay günlüğüne bir hata içeriyor. **olay kimliği 6000** ve ileti **'yönetim Aracısı "contoso.com" kimlik bilgileri geçersiz olduğundan başarısız oldu'**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Eşitleme hizmeti ile yeni AD DS hesap parolasını güncelleştirme
Eşitleme hizmeti yeni parolayı güncelleştirmek için:

1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-change-addsacct-pass/startmenu.png)  

2. Git **Bağlayıcılar** sekmesi.

3. Seçin **AD Bağlayıcısı** parolası değiştirildi AD DS hesaba karşılık gelir.

4. Altında **eylemleri**seçin **özellikleri**.

5. Açılan iletişim kutusunda, seçmek **Active Directory ormanına Bağlan**:

6. Yeni AD DS hesap parolasını girin **parola** metin.

7. Tıklayın **Tamam** yeni parolayı kaydedin ve açılır iletişim kutusunu kapatın.

8. Yeniden başlatma Azure AD eşitleme hizmeti Windows Hizmet Denetimi Yöneticisi altında bağlanın. Eski parola herhangi bir referans bellek önbelleğinden kaldırıldığından emin olmak için budur.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
