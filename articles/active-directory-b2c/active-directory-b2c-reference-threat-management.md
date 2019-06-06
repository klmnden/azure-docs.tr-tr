---
title: Kaynaklar ve Azure Active Directory B2C verilerinde tehditleri yönetme | Microsoft Docs
description: Hizmet reddi saldırılarını ve Azure Active Directory B2C parola saldırıları algılama ve Önleme teknikleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f961e873bb230e4e6d7e14b03e2664b7f987974e
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508893"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Kaynaklar ve Azure Active Directory B2C verilerinde tehditleri yönetme

Azure Active Directory (Azure AD) B2C, kaynaklarınıza ve verilerin tehditlere karşı korumanıza yardımcı olur yerleşik özelliklere sahiptir. Bu tehditler, hizmet reddi saldırılarını ve parola saldırılarını içerir. Hizmet reddi saldırılarını kaynakları hedeflenen kullanıcılara kullanımdan. Kaynaklara yetkisiz erişim için parola saldırılarına yol. 

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırılarını

Azure AD B2C SYN tanımlama bilgisi kullanılarak SYN baskın saldırılarına karşı felaketlerden koruyor. Azure AD B2C, hizmet reddi saldırılarına karşı da hızları ve bağlantıları için sınırları kullanarak korur.

## <a name="password-attacks"></a>Parola saldırıları

Kullanıcılar tarafından ayarlanan parolaların makul karmaşık olması gerekmez. Azure AD B2C parola saldırılarını yürürlükte olan azaltma teknikleri var. Parola deneme yanılma saldırıları ve sözlük parola saldırılarını azaltma içerir. Çeşitli sinyalleri kullanarak, Azure AD B2C isteklerinin bütünlüğünü analiz eder. Azure AD B2C, hedeflenen kullanıcıların bilgisayar korsanlarının ve botnet akıllı bir şekilde ayırt etmek için tasarlanmıştır. 

Azure AD B2C hesapları kilitlemek için karmaşık bir strateji kullanır. Hesapları istek ve girilen parolaları IP kilitlenir. Kilitleme süresi ayrıca girişimin bir saldırı olma olasılığına göre artar. Bir parola başarısız 10 kez denendi sonra bir dakikalık kilitleme gerçekleşir. Sonra Hesap kilidi, bir oturum açma başarısız, sonraki açışınızda başka bir dakikalık kilitleme oluşur ve her hizmette oturum açma için devam eder. Aynı parolayı tekrar tekrar girilmesi olarak birden çok başarısız oturum açma bilgileri sayılmaz. 

İlk 10 kilitleme bir dakikadan uzun olur. Sonraki 10 kilitleme dönemleri biraz daha uzun ve her 10 kilitleme nokta sonra süresi artar. Hesap kilitli değil, başarılı bir oturum açma işleminden sonra sıfır kilidi açma sayacı sıfırlanır. Kilitleme nokta beş saate kadar sürebilir. 

Şu anda, şunları yapamazsınız:

- 10'dan az başarısız oturum açma bilgileri ile bir kilitleme tetikleyin
- Çıkış kilitli hesaplarının bir listesini alma
- Kilitleme ilkesi yapılandırma

Daha fazla bilgi için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/default.aspx).
