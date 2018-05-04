---
title: Azure Active Directory portalında bulunan oturum açma etkinlik raporundaki hata kodları | Microsoft Docs
description: Oturum açma etkinlik raporu hata kodları başvurusu.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 47894f0a3d7b74c8d0e271afb7d2657ce7d3be0c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında bulunan oturum açma etkinlik raporundaki hata kodları

Kullanıcı oturum açma işlemlerinin raporuyla sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Kimler Azure Active Directory kullanarak oturum açtı?
- Hangi uygulamalarda oturum açıldı?
- Hangi oturum açma girişimleri başarısız oldu ve neden?

Bu konu başlığı altında, hata kodları ve ilgili açıklamaları listelenir. 

## <a name="how-can-i-display-failed-sign-ins"></a>Başarısız oturum açma girişimlerini nasıl görüntüleyebilirim? 

Tüm oturum açma etkinliği verilerine ilk giriş noktanız, **Azure Active**’in **Etkinlik** bölümündeki **[Oturum açma işlemleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** kısmıdır.


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/61.png "oturum açma etkinliği")

Oturum açma raporunuzda tüm başarısız oturum açma işlemlerini görüntülemek için, **Oturum açma durumu** olarak **Başarısız**'ı seçebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/06.png "oturum açma etkinliği")

Görüntülenen listedeki bir öğeye tıklarsanız **Etkinlik Ayrıntıları: Oturum açma işlemleri** dikey penceresi açılır. Bu görünüm size Azure Active Directory’nin oturum açma işlemleriyle ilgili olarak izlediği tüm ayrıntıları gösterir; bunlara **oturum açma hata kodu** ve **hatanın nedeni** de dahildir.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/05.png "oturum açma etkinliği")


Oturum açma verilerine erişmek için Azure portalını kullanmaya alternatif olarak [raporlama API'sini](active-directory-reporting-api-getting-started-azure-portal.md) de kullanabilirsiniz.


Aşağıdaki bölümde, tüm olası hataları ve ilgili açıklamalarını kapsayan bir genel bakış sağlanır. 

## <a name="error-codes"></a>Hata kodları

|Hata|Açıklama|
|---|---|
|50001|X adlı hizmet sorumlusu Y adlı kiracıda bulunamadı. Uygulama, kiracının yöneticisi tarafından yüklenmediyse bu durum ortaya çıkabilir. Ayrıca, kaynak sorumlusu dizinde bulunamamış veya geçersiz de olabilir.|
|50008|SAML onay deyimi eksik veya belirteçte yanlış yapılandırılmış.|
|50011|Yanıt adresi eksik, yanlış yapılandırılmış veya uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor.|
|50012|Kullanıcı Çok Faktörlü kimlik doğrulaması sırasında sahtekarlık bildirdi.|
|50053|Kullanıcı, yanlış kullanıcı kimliği veya parola ile çok fazla kez oturum açmaya çalıştığı için hesap kilitlendi.|
|50054|Kimlik doğrulaması için eski parola kullanıldı.|
|50055|Geçersiz parola, süresi dolmuş parola girildi.|
|50057|Kullanıcı hesabı devre dışı bırakıldı.|
|50058|Sağlanan kimlik bilgilerinde kullanıcının kimliğiyle ilgili hiçbir bilgi bulunamadı veya Kullanıcı, kiracıda bulunamadı veya Sessiz bir oturum açma isteği gönderildi ancak hiçbir kullanıcı oturum açmadı veya Hizmet, kullanıcının kimliğini doğrulayamadı.|
|50072|Kullanıcının ikinci faktör kimlik doğrulamasına kaydolması gerekir (etkileşimli)|
|50074|Kullanıcı MFA testini geçemedi.|
|50079|Kullanıcının ikinci faktör kimlik doğrulamasına kaydolması gerekir.|
|50089|Akış belirteci doğrulaması, akış belirteci süre sonu nedeniyle başarısız oldu.|
|50097|Cihazın kimliği doğrulanamadı.|
|50105|Oturum açmış olan kullanıcı, bu uygulama için bir role atanmadı.|
|50126|Geçersiz kullanıcı adı veya parola ya da Geçersiz şirket içi kullanıcı adı veya parola.|
|50131|Çeşitli koşullu erişim hatalarında kullanılır. Örneğin: Hatalı Windows cihazı durumu, şüpheli etkinlik nedeniyle istek engellendi, erişim ilkesi ve güvenlik ilkesi kararları.|
|50133|Süresi dolduğu veya yakın zamanda parola değiştirildiği için oturum geçersiz.|
|50144|Kullanıcının Active Directory parolasının süresi doldu.|
|65001|X uygulamasının Y uygulamasına erişim izni yok veya erişim izni iptal edildi. Veya Kullanıcı ya da yönetici X kimliğiyle uygulamanın kullanılmasını onaylamadı. Bu kullanıcı veya kaynak için etkileşimli yetkilendirme isteği gönderin. Veya Kullanıcı ya da yönetici X kimliğiyle uygulamanın kullanılmasını onaylamadı. Kaynak: Z için Uygulama: Y adına işlem yapmak üzere kiracı yöneticinize bir yetkilendirme isteği gönderin.|
|65005|Uygulamaya gereken kaynak erişim listesi, kaynak tarafından bulunabilen uygulamaları içermiyor veya İstemci uygulaması kendi gerekli kaynak erişim listesinde belirtilmemiş bir kaynağa erişim isteğinde bulundu veya Graph hizmeti hatalı istek döndürdü veya kaynak bulunamadı.|
|70001|X adlı uygulama Y adlı kiracıda bulunamadı. Uygulama, kiracının yöneticisi tarafından yüklenmediyse veya kiracıdaki herhangi bir kullanıcı tarafından onaylanmadıysa bu durum ortaya çıkabilir. Kimlik doğrulaması isteğinizi yanlış kiracıya göndermiş olabilirsiniz.|
|80001|Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı.|
|80002|Kimlik Doğrulama Aracısı'nın parola doğrulama isteği zaman aşımına uğradı.|
|80003|Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı.|
|80004|Oturum açma isteğinde yanlış Kullanıcı Asıl Adı (UPN) kullanıldı.|
|80005|Kimlik Doğrulama Aracısı: Hata oluştu.|
|80007|Kimlik Doğrulama Aracısı kullanıcının parolasını doğrulayamıyor.|
|80010|Kimlik Doğrulama Aracısı parolanın şifresini çözemedi.|
|80011|Kimlik Doğrulama Aracısı şifre çözme anahtarını alamıyor.|
|81001|Kullanıcının Kerberos anahtarı fazla büyük.|
|81002|Kullanıcının Kerberos anahtarı doğrulanamadı.|
|81003|Kullanıcının Kerberos anahtarı doğrulanamadı.|
|81004|Kerberos kimlik doğrulaması girişimi başarısız oldu.|
|81008|Kullanıcının Kerberos anahtarı doğrulanamadı.|
|81009|Kullanıcının Kerberos anahtarı doğrulanamadı.|
|81010|Kullanıcının Kerberos anahtarının süresi dolduğu veya anahtar geçersiz olduğu için sorunsuz SSO başarısız oldu.|
|81011|Kullanıcının Kerberos anahtarındaki bilgiler temel alınarak kullanıcı nesnesi bulunamadı.|
|81012|Azure AD'de oturum açmaya çalışan kullanıcı, cihazda oturum açmış olan kullanıcıdan farklıdır.|
|81013|Kullanıcının Kerberos anahtarındaki bilgiler temel alınarak kullanıcı nesnesi bulunamadı.|
|90014|Kimlik bilgilerinde beklenen bir alanın bulunamadığı çeşitli durumlarda kullanılır.|
|90093|Graph istek için yasak hata kodu döndürdü.|


## <a name="next-steps"></a>Sonraki adımlar

Daha ayrıntılı bilgi için bkz. [Azure Active Directory portalındaki oturum açma etkinlik raporları](active-directory-reporting-activity-sign-ins.md).
