---
title: "Azure portalında çok faktörlü kimlik doğrulaması raporlama başvurusu | Microsoft Docs"
description: "Azure portalında çok faktörlü kimlik doğrulaması raporlama için başvuru bilgileri"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3b817c59658f4a5d102a3d65a17fade254e0257c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="reference-for-multi-factor-authentication-reporting-in-the-azure-portal"></a>Azure portalında çok faktörlü kimlik doğrulaması raporlama için başvuru

[Azure portalında](https://portal.azure.com) [Azure Active Directory (Azure AD) raporlama](active-directory-reporting-azure-portal.md) özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

[Oturum açma etkinliği raporları](active-directory-reporting-activity-sign-ins.md) çok faktörlü kimlik doğrulaması (MFA) kullanımı hakkında bilgiler dahil olmak üzere yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sunar. 

MFA verileri MFA'nın kuruluşunuzdaki kullanımı hakkında öngörüler sunar. Aşağıdaki gibi sorulara yanıt bulmanızı sağlar: 

- Oturum açma sırasında MFA kullanıldı mı? 

- Kullanıcı MFA'yı nasıl tamamladı? 

- Kullanıcının MFA'yı tamamlayamadığı durumlar oldu mu?  

Tüm oturum açma verilerini toplayarak kuruluşunuzdaki MFA deneyimi hakkında daha iyi bir anlayışa sahip olabilirsiniz. Veriler aşağıdaki gibi sorulara yanıt bulmanıza yardımcı olur: 

- Kaç kullanıcıdan MFA istendi?  

- Kaç kullanıcı MFA isteğini tamamlayamadı? 

- Son kullanıcıların karşılaştığı ortak MFA sorunları nelerdir? 


Bu verilere Azure portalından ve [raporlama API'sinden](active-directory-reporting-api-getting-started-azure-portal.md) ulaşılabilir. 


## <a name="data-structure"></a>Veri yapısı


MFA hakkındaki oturum açma etkinliği raporları aşağıdaki bilgilere erişmenizi sağlar:

**MFA gerekli:** Oturum açma işlemi için MFA'nın gerekli olup olmadığı. MFA; kullanıcı için MFA, koşullu erişim veya diğer nedenlerle gerekli olabilir. Olası değerler: `Yes` veya `No`.

**MFA kimlik doğrulama yöntemi:** Kullanıcının MFA'yı tamamlaması için kullandığı kimlik doğrulama yöntemi. Olası değerler şunlardır: 

- Kısa mesaj 

- Mobil uygulama bildirimi 

- Telefon araması (Kimlik doğrulama telefonu) 

- Mobil uygulama doğrulama kodu 

- Telefon araması (Ofis telefonu) 

- Telefon araması (Alternatif kimlik doğrulama telefonu) 

**MFA kimlik doğrulama ayrıntısı:** Telefon numarasının temizlenmiş sürümü. Örnek: +X XXXXXXXX64. 

**MFA Sonucu:** MFA'nın tamamlanıp tamamlanmadığı hakkında daha fazla bilgi:

- MFA tamamlandıysa bu sütunda MFA'nın tamamlanma şekli hakkında daha fazla bilgi yer alır. 

- MFA reddedildiyse bu sütunda reddedilme nedeni yer alır. Olası değerler: `Satisfied` veya `Denied`. 

Aşağıdaki bölümde MFA sonuç alanı için olası dize değerleri listelenmiştir.

## <a name="status-strings"></a>Durum dizeleri

Bu bölümde MFA sonuç durumu dizesi için olası değerler listelenmiştir.

### <a name="satisfied-status-strings"></a>Tamamlanma durumu dizeleri


- Azure Multi-Factor Authentication

    - bulutta tamamlandı 

    - kiracıda yapılandırılmış ilkeler nedeniyle süresi doldu 

    - kayıt istendi 

    - belirteç talebiyle gerçekleştirildi 

    - belirteç talebiyle gerçekleştirildi 

    - belirteç talebiyle gerçekleştirildi 

    - belirteç talebiyle gerçekleştirildi 

    - harici sağlayıcı tarafından sağlanan taleple gerçekleştirildi 

    - güçlü kimlik doğrulamasıyla gerçekleştirildi 

    - gerçekleştirilen akış Windows aracısı oturum açma akışı tarafı olduğundan atlandı 

    - gerçekleştirilen akış Windows aracısı oturum açma akışı tarafı olduğundan atlandı 

    - uygulama parolası nedeniyle atlandı 

    - konum nedeniyle atlandı 

    - kayıtlı cihaz nedeniyle atlandı 
    
    - hatırlanan cihaz nedeniyle atlandı 

    - başarıyla tamamlandı 

- Çok faktörlü kimlik doğrulaması için harici sağlayıcıya yönlendirildi 

 
### <a name="denied-status-strings"></a>Reddetme durumu dizeleri

Azure Multi-Factor Authentication tarafından reddedildi; 

- kimlik doğrulaması devam ediyor 

- yinelenen kimlik doğrulaması girişimi 

- çok fazla kez yanlış kod girildi 

- geçersiz kimlik doğrulaması 

- geçersiz mobil uygulama doğrulama kodu 

- yanlış yapılandırma 

- telefon araması sesli mesaja düştü 

- telefon numarası biçimi geçersiz 

- hizmet hatası 

- kullanıcının telefonuna ulaşılamadı 

- cihaza mobil uygulama bildirimi gönderilemedi 

- mobil uygulama bildirimi gönderilemedi 

- kullanıcı kimlik doğrulamasını reddetti 

- kullanıcı mobil uygulama bildirimine yanıt vermedi 

- kullanıcının kayıtlı doğrulama yöntemi yok 

- kullanıcı hatalı kod girdi 

- kullanıcı hatalı PIN girdi 

- kullanıcı kimlik doğrulaması başarılı olmadan telefon aramasını sonlandırdı 

- kullanıcı engellendi 

- kullanıcı doğrulama kodunu hiç girmedi 

- kullanıcı bulunamadı 
 
- doğrulama kodu zaten bir kez kullanıldı 



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](active-directory-reporting-azure-portal.md).




























