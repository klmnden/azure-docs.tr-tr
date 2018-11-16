---
title: Microsoft Bulut Almanya’da Azure AD Connect
description: Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme, Almanya, Black Forest
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 545ec1d4f5cd817b1fa02a135d305b997c9945bc
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705404"
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Microsoft Bulut Almanya’da Azure AD Connect - Genel Önizleme
## <a name="introduction"></a>Giriş
Azure AD Connect, şirket içi Active Directory’niz ile Azure Active Directory arasında eşitleme sağlar.
Şu anda [Microsoft Bulut Almanya](https://azure.microsoft.com/global-infrastructure/germany/
)’daki senaryoların çoğunun operatör tarafından yürütülmesi gerekmektedir. Microsoft Cloud Almanya'yı kullanırken aşağıdaki bilgilerden haberdar olmanız gerekir:

* Eşitlemenin başarıyla gerçekleştirilebilmesi için aşağıdaki URL'lerin bir ara sunucuda açılması gerekir:
  
  * *. microsoftonline.de
  * *.windows.net
  * * Sertifika İptal Listeleri
* Azure AD dizininizde oturum açarken, onmicrosoft.de etki alanında yer alan bir hesap kullanmanız gerekir.

 
## <a name="download"></a>İndirme
Azure AD Connect’i portalın içerisindeki Azure AD Connect dikey penceresinden indirebilirsiniz.  Azure AD Connect dikey penceresini bulmak için aşağıdaki yönergeleri kullanın.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect Dikey Penceresi
Bir kez Azure portalında açtığınız:

1. Gözat’a gidin
2. Azure Active Directory'yi seçin
3. Azure AD Connect'i seçin

Bu ayrıntıları görürsünüz:

![Azure AD Connect Dikey Penceresi](./media/reference-connect-germany/germany1.png)

Aşağıdaki tabloda, dikey pencerede gösterilen özellikler açıklanmaktadır.

| Başlık | Açıklama |
| --- | --- |
| EŞİTLEME DURUMU |Eşitlemenin etkin mi yoksa devre dışı mı olduğunu gösterir. |
| SON EŞİTLEME |Başarılı bir eşitlemenin tamamlandığı en son zaman. |
| FEDERASYON ETKİ ALANLARI |Şu anda yapılandırılmış durumda olan federasyon etki alanlarının sayısını gösterir. |

## <a name="installation"></a>Yükleme
Azure AD Connect'i yüklemek için [buradaki](how-to-connect-install-roadmap.md) belgeleri kullanabilirsiniz.

## <a name="advanced-features-and-additional-information"></a>Gelişmiş özellikler ve Ek Bilgiler
Özel ayarlar veya gelişmiş yapılandırmalar hakkında ek bilgi için Git [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md). Bu sayfada, bilgiler ve ek rehberlik sağlayan diğer sayfalara bağlantılar bulabilirsiniz.

