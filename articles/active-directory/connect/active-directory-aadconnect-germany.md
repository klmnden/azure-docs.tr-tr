---
title: "Microsoft Bulut Almanya’da Azure AD Connect"
description: "Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz."
keywords: "Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme, Almanya, Black Forest"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4c55b116c0dc080ae316caca873a7b693caa793b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Microsoft Bulut Almanya’da Azure AD Connect - Genel Önizleme
## <a name="introduction"></a>Giriş
Azure AD Connect, şirket içi Active Directory’niz ile Azure Active Directory arasında eşitleme sağlar.
Şu anda [Microsoft Bulut Almanya](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx)’daki senaryoların çoğunun operatör tarafından yürütülmesi gerekmektedir. Microsoft Bulut Almanya’yı kullanırken aşağıdakilerin farkında olmanız gerekir:

* Eşitlemenin başarıyla gerçekleştirilebilmesi için aşağıdaki URL'lerin bir ara sunucuda açılması gerekir:
  
  * *. microsoftonline.de
  * *.windows.net
  * * Sertifika İptal Listeleri
* Azure AD dizininizde oturum açarken, onmicrosoft.de etki alanında yer alan bir hesap kullanmanız gerekir.
* Aşağıdaki özellikler kullanılamaz:
  * Azure AD Connect Health
  * Otomatik güncelleştirmeler
 
## <a name="download"></a>İndirme
Azure AD Connect’i portalın içerisindeki Azure AD Connect dikey penceresinden indirebilirsiniz.  Azure AD Connect dikey penceresini bulmak için aşağıdaki yönergeleri kullanın.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect Dikey Penceresi
Azure portalda oturum açtıktan sonra şunları yapın:

1. Gözat’a gidin
2. Azure Active Directory'yi seçin
3. Azure AD Connect'i seçin

Şunları görmeniz gerekir:

![Azure AD Connect Dikey Penceresi](media/active-directory-aadconnect-germany/germany1.png)

Aşağıdaki tabloda, dikey pencerede gösterilen özellikler açıklanmaktadır.

| Başlık | Açıklama |
| --- | --- |
| EŞİTLEME DURUMU |Eşitlemenin etkin mi yoksa devre dışı mı olduğunu gösterir. |
| SON EŞİTLEME |Başarılı bir eşitlemenin tamamlandığı en son zaman. |
| FEDERASYON ETKİ ALANLARI |Şu anda yapılandırılmış durumda olan federasyon etki alanlarının sayısını gösterir. |

## <a name="installation"></a>Yükleme
Azure AD Connect'i yüklemek için [buradaki](active-directory-aadconnect.md#install-azure-ad-connect) belgeleri kullanabilirsiniz.

## <a name="advanced-features-and-additional-information"></a>Gelişmiş özellikler ve Ek Bilgiler
Özel ayarlar veya gelişmiş yapılandırmalar hakkında ek bilgiler ya da kılavuzluk edinmek için [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md) (Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme) makalesinden başlayın.  Bu sayfada, bilgiler ve ek rehberlik sağlayan diğer sayfalara bağlantılar bulabilirsiniz.

