---
title: 'Azure AD Connect: yükleme türünü seçin. | Microsoft Docs'
description: Bu konuda, Azure AD Connect için kullanılacak yükleme türü seçme açıklanmaktadır
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 76f1ce12ab149f57ec6e995d132de83105c5e0ca
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a>Azure AD Connect için kullanmak üzere hangi yükleme türünü seçin
Azure AD Connect, yeni yükleme için iki yükleme türleri vardır: Express ve özelleştirilebilir. Bu konuda, yükleme sırasında kullanmak için hangi seçeneği karar vermenize yardımcı olur.

## <a name="express"></a>Express
Express en yaygın kullanılan bir seçenektir ve yaklaşık % 90'ını tarafından tüm yeni yüklemeler kullanılır. En yaygın müşteri senaryoları için çalışan bir yapılandırma sağlamak için tasarlanmıştır.

Bunu varsayılır:

- Şirket içi orman tek bir Active Directory var.
- Yükleme için kullanabileceğiniz bir kuruluş yöneticisi hesabına sahip.
- Şirket içi Active Directory 100. 000'den az nesneler var.

Şunları alırsınız:

- [Parola karma eşitlemesi](active-directory-aadconnectsync-implement-password-hash-synchronization.md) şirket içi Azure ad çoklu oturum açma için.
- Eşitleyen bir yapılandırma [kullanıcılar, gruplar, kişiler ve Windows 10 bilgisayarlar](active-directory-aadconnectsync-understanding-default-configuration.md).
- Tüm etki alanlarını ve tüm OU'larda tüm uygun nesneleri eşitlenmesi.
- [Otomatik yükseltmeyi](active-directory-aadconnect-feature-automatic-upgrade.md) en son sürüme her zaman kullandığınızdan emin olmak için etkinleştirilir.

Seçenekler burada Express kullanmaya devam edebilirsiniz:

- Tüm OU'larda eşitlemek istemediğiniz yaparsanız hala Express kullanın ve son sayfasında seçimini kaldırın **... eşitleme işlemini başlatmak ***. Yükleme Sihirbazı'nı yeniden çalıştırın ve OU'larda değiştirme [yapılandırma seçenekleri](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) ve zamanlanmış eşitleme etkinleştirin.
- Azure AD Premium, parola geri yazma gibi özelliklerden birini, etkinleştirmek istediğiniz. İlk ilk yükleme almak için express gidin. Yükleme Sihirbazı'nı yeniden çalıştırın ve değiştirme [yapılandırma seçenekleri](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Özel
Özelleştirilmiş yolu express daha pek çok seçenek sağlar. Burada express için önceki bölümde açıklanan yapılandırma, kuruluşunuz için temsili olmadığı tüm durumlarda kullanılmalıdır.

Şu durumlarda kullanın:

- Active Directory içinde bir kuruluş yöneticisi hesabı için erişim sahip değil.
- Birden çok orman veya birden çok orman gelecekte eşitlemek planlama.
- Connect sunucudan ulaşılabilir değil, ormanınızdaki etki alanları sahiptir.
- Kullanıcı oturum açma için Federasyon veya doğrudan kimlik doğrulama kullanmayı planlayın.
- 100. 000'den fazla nesneleri ve tam bir SQL Server kullanmanız gerekir.
- Grup tabanlı filtreleme ve yalnızca etki alanı veya OU tabanlı filtreleme kullanmayı planlayın.

## <a name="upgrade-from-dirsync"></a>DirSync'ten yükseltme
Şu anda DirSync kullanıyorsanız, ardından adımları [Dirsync'ten yükseltme](active-directory-aadconnect-dirsync-upgrade-get-started.md) varolan yapılandırmanızı yükseltmek için. İki farklı yükseltme seçenekleri şunlardır:

- Connect aynı sunucuya yüklemek için yerinde yükseltme.
- Mevcut DirSync sunucu hala durumdayken Bağlan yeni bir sunucuya yüklemek için paralel dağıtım.

## <a name="upgrade-from-azure-ad-sync"></a>Azure AD eşitleme'den yükseltme
Azure AD eşitleme kullanmakta olduğunuz sonra izleyebilirsiniz [aynı adımları](active-directory-aadconnect-upgrade-previous-version.md) bir Connect sürümünden yeni bir yükseltme yaptığınızda olarak. İki farklı yükseltme seçenekleri şunlardır:

- Connect aynı sunucuya yüklemek için yerinde yükseltme.
- Esnek geçiş Bağlan varolan sırasında yeni bir sunucuya yüklemek için Azure AD eşitleme sunucusunun hala çalışır durumda.

## <a name="migrate-from-fim2010-or-mim2016"></a>Fım2010 veya MIM2016 geçirmek
Şu anda Forefront Identity Manager 2010 veya Microsoft Identity Manager 2016 Azure AD Bağlayıcısı ile kullanıyorsanız, tek seçeneğiniz bir geçiş değil. Bölümünde açıklanan adımları [esnek geçiş](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). Aşağıdaki adımlarda, Azure AD eşitleme herhangi Bahsetme fım2010/MIM2016 ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Kullanmak için seçtiğiniz seçeneğine bağlı olarak, içerik sol tablo Makalenizi ayrıntılı adımları bulmak için kullanın.
