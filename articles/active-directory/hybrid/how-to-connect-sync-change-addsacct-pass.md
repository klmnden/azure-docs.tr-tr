---
title: 'Azure AD Connect eşitleme:  AD DS hesap parolasını değiştirme | Microsoft Docs'
description: Bu konuda belge, Azure AD Connect AD DS hesap parolasını değiştirildikten sonra güncelleştirme işlemi açıklanmaktadır.
services: active-directory
keywords: AD DS hesabı, Active Directory hesabı parola
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
origin.date: 07/12/2017
ms.date: 11/09/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: 35e04be046e20883f60c576745a29342add68a81
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60241617"
---
# <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
AD DS hesabını Azure AD Connect tarafından şirket içi Active Directory ile iletişim kurmak için kullanılan kullanıcı hesabının ifade eder. AD DS hesap parolasını değiştirirseniz, Azure AD Connect eşitleme hizmeti yeni parolayla güncelleştirmeniz gerekir. Aksi takdirde eşitleme artık doğru şekilde şirket içi Active Directory ile eşitlenebilir ve şu hatalarla karşılaşırsınız:

- Eşitleme Hizmeti Yöneticisi, içeri aktarma veya dışarı aktarma işlemi ile şirket içi AD ile başarısız **Hayır başlangıç kimlik** hata.

- Altındaki Windows Olay Görüntüleyicisi, uygulama olay günlüğüne bir hata içeriyor. **olay kimliği 6000** ve ileti **'yönetim Aracısı "contoso.com" kimlik bilgileri geçersiz olduğundan başarısız oldu'**.


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

- [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)

- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

