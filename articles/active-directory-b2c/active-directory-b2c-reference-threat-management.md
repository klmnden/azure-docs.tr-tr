---
title: Kaynaklar ve Azure Active Directory B2C verilerinde tehditleri yönetme
description: Hizmet reddi saldırılarını ve Azure Active Directory B2C parola saldırıları algılama ve Önleme teknikleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7232917df6018c9c8afc7e7edd3730a277b193f4
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798256"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Kaynaklar ve Azure Active Directory B2C verilerinde tehditleri yönetme

Azure Active Directory (Azure AD) B2C, kaynaklarınıza ve verilerin tehditlere karşı korumanıza yardımcı olur yerleşik özelliklere sahiptir. Bu tehditler, hizmet reddi saldırılarını ve parola saldırılarını içerir. Hizmet reddi saldırılarını kaynakları hedeflenen kullanıcılara kullanımdan. Kaynaklara yetkisiz erişim için parola saldırılarına yol.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırılarını

Azure AD B2C SYN tanımlama bilgisi kullanılarak SYN baskın saldırılarına karşı felaketlerden koruyor. Azure AD B2C, hizmet reddi saldırılarına karşı da hızları ve bağlantıları için sınırları kullanarak korur.

## <a name="password-attacks"></a>Parola saldırıları

Kullanıcılar tarafından ayarlanan parolaların makul karmaşık olması gerekmez. Azure AD B2C parola saldırılarını yürürlükte olan azaltma teknikleri var. Algılama parola deneme yanılma saldırıları ve sözlük parola saldırılarını azaltma içerir. Çeşitli sinyalleri kullanarak, Azure AD B2C isteklerinin bütünlüğünü analiz eder. Azure AD B2C, hedeflenen kullanıcıların bilgisayar korsanlarının ve botnet akıllı bir şekilde ayırt etmek için tasarlanmıştır.

Azure AD B2C hesapları kilitlemek için karmaşık bir strateji kullanır. Hesapları istek ve girilen parolaları IP kilitlenir. Kilitleme süresi ayrıca girişimin bir saldırı olma olasılığına göre artar. Bir parola başarısız 10 kez denendi sonra (varsayılan girişimi eşik), bir dakikalık kilitleme gerçekleşir. (Kilitleme süresi dolduktan sonra diğer bir deyişle, hesap otomatik olarak hizmet tarafından kilidi sonra) sonra hesap bir oturum açma başarısız sonraki açışınızda kilidi açıldı, başka bir dakikalık kilitleme oluşur ve her hizmette oturum açma için devam eder. Aynı parolayı tekrar tekrar girilmesi olarak birden çok başarısız oturum açma bilgileri sayılmaz.

İlk 10 kilitleme bir dakikadan uzun olur. Sonraki 10 kilitleme dönemleri biraz daha uzun ve her 10 kilitleme nokta sonra süresi artar. Hesap kilitli değil, başarılı bir oturum açma işleminden sonra sıfır kilidi açma sayacı sıfırlanır. Kilitleme nokta beş saate kadar sürebilir.

## <a name="manage-password-protection-settings"></a>Parola koruma ayarlarını yönetme

Kilitleme eşiği dahil olmak üzere parola koruması ayarlarını yönetmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Seçin **dizin + abonelik** filtrelemenize portalın sağ üst menüsünde ve ardından Azure AD B2C kiracınızı seçin.
1. Seçin **Azure Active Directory** soldaki menüden (veya **tüm hizmetleri** portalın sol üst kısmında öğesini arayın ve seçin *Azure Active Directory*).
1. Altında **güvenlik**seçin **kimlik doğrulama yöntemleri**, ardından **parola koruması**.
1. İstenilen parola koruması ayarlarınızı girdikten sonra belirleyin **Kaydet**.

    ![Azure portalındaki parola koruması sayfasında bulunan Azure AD ayarları](media/active-directory-b2c-reference-threat-management/portal-02-password-protection.png)
    <br />*5 olarak kilitleme eşiği ayarlama **parola koruması** ayarları*.

## <a name="view-locked-out-accounts"></a>Görünüm kilitli hesaplar

Kilitlenmiş hesaplar hakkında bilgi edinmek için Active Directory denetleyebilirsiniz [oturum açma etkinliği raporunu](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md). Altında **durumu**seçin **hatası**. Başarısız oturum açma denemesi ile bir **oturum açma hata kodu** , `50053` kilitli bir hesap belirtin:

![Kilitli hesabını gösteren Azure AD oturum açma rapor bölümü](media/active-directory-b2c-reference-threat-management/portal-01-locked-account.png)

Azure Active Directory'de oturum açma etkinliği raporunu görüntüleme hakkında bilgi edinmek için [oturum açma etkinlik raporundaki hata kodları](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md).
