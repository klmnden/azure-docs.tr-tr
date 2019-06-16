---
title: Azure portal eğitim Kılavuzu - Azure uygulama kayıtları
description: Cihaz kodu verme kullanan katıştırılmış ve tarayıcı olmayan kimlik doğrulaması akışlar oluşturun.
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 118c6ecb16d325a384246a0b3d9e685f6f6f04ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870125"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>Eğitim Kılavuzu: Azure portalında uygulama kayıtları  

Çok sayıda geliştirmeleri yeni bulabilirsiniz [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) Azure portalında karşılaşırsınız. Eski deneyimi ile daha biliyorsanız, yeni deneyimi kullanarak başlamanıza yardımcı olmak için bu eğitim kılavuzu kullanın.

## <a name="key-changes"></a>Önemli değişiklikler

- Uygulama kayıtları ya da olmaya sınırlı olmayan bir **web uygulaması/API'si** veya **yerel** uygulama. Tüm ilgili yeniden yönlendirme URI'leri kaydederek bu aynı uygulama kaydı'nı kullanabilirsiniz.
- Oturum eski deneyimi desteklenen uygulamalar yalnızca kuruluş (Azure AD) hesaplar. Tek kiracılı (yalnızca Kurumsal hesapları uygulama kaydedildi dizininden destekleme) olarak kayıtlı uygulamalar ve çok kiracılı (destekleyen tüm kurumsal hesaplar) olacak şekilde değiştirilmesi. Yeni deneyimi destek bu seçeneklerin yanı sıra üçüncü bir seçenek uygulamaları kaydetme sağlar: Kişisel Microsoft hesaplarının yanı sıra tüm kurumsal hesaplar.
- Eski deneyimi yalnızca bir kurumsal hesap kullanarak Azure portalında oturum olduğunda kullanılabilir. Yeni deneyimi, bir dizin ile ilişkili olmayan kişisel Microsoft hesapları kullanabilirsiniz.

## <a name="list-of-applications"></a>Uygulamaların listesi

- Yeni uygulama listesini eski uygulama üzerinden kaydedilen uygulamalar gösterilmektedir ancak kayıtlı uygulamaların yanı sıra Azure portal (Azure AD hesaplarının oturum uygulamaları) kaydı deneyimi [uygulama kayıt portalı](https://apps.dev.microsoft.com/) (Azure AD'de oturum uygulamaları ve kişisel Microsoft hesapları).
- Yeni uygulama listesinde yok bir **uygulama türü** (tek uygulama kaydı birden fazla olabileceği) sütun ve iki ek sütunlar vardır: bir **oluşturulan** sütun ve **sertifikaları & Gizli dizileri** uygulamanın kayıtlı kimlik bilgileri, durum (geçerli, süresi yakında dolacak ve süresi) gösteren bir sütun.

## <a name="new-app-registration"></a>Yeni uygulama kaydı

Eski deneyimi, bir uygulamayı kaydetme, sağlamak için korumaları gerekir: **Adı**, **uygulama türü**, ve **oturum açma URL'si/yeniden yönlendirme URI'si**. Oluşturulan uygulamalar Azure AD yalnızca tek kiracılı uygulamalar bunlar yalnızca uygulama kaydedildiği dizininden Kurumsal hesaplar desteklenen anlamı yoktu.

Yeni deneyim, sağlamanız gereken bir **adı** uygulama ve **desteklenen hesap türleri**. İsteğe bağlı olarak sağlayan bir **yeniden yönlendirme URI'si**. Yeniden yönlendirme URI'sini sağlarsanız, web/public (Mobil ve Masaüstü) olup olmadığını belirlemek gerekir. Deneyimi yeni uygulama kayıtları kullanarak bir uygulamayı kaydetme konusunda daha fazla bilgi için bkz: [Bu hızlı başlangıçta](quickstart-register-app.md).

## <a name="the-legacy-properties-page"></a>Eski Özellikler sayfası

Eski bir deneyim yaşıyordu bir **özellikleri** sayfasını yeni deneyime sahip değil. **Özellikleri** dikey penceresi aşağıdaki alanları sahipti: **Adı**, **nesne kimliği**, **uygulama kimliği**, **uygulama kimliği URI'si**, **logosu**, **giriş sayfası URL'si** , **Oturum kapatma URL'si**, **hizmet kullanım koşulları URL'si**, **gizlilik bildirimi URL'si**, **uygulama türü**, ve  **Çok kiracılı.**

Yeni deneyimde eşdeğer bir işlevselliği bulabileceğiniz aşağıda verilmiştir:

- **Adı**, **logosu**, **giriş sayfası URL'si**, **hizmet kullanım koşulları URL'si**, ve **gizlilik bildirimi URL'si** uygulamanın üzerinde sunulmuştur **Marka** sayfası.
- **Nesne Kimliği** ve **uygulama (istemci) kimliği** açıktır **genel bakış** sayfası.
- Denetlenen işlevselliğini **çok kiracılı** eski deneyiminde geçiş tarafından değiştirilmiştir **desteklenen hesap türleri** üzerinde **kimlik doğrulaması** sayfası. Çok kiracılı eşlemelerini nasıl desteklenen hesap türü seçenekleri hakkında daha fazla bilgi için bkz. [Bu hızlı başlangıçta](quickstart-modify-supported-accounts.md).
- **Oturum kapatma URL'si** şu anda etkin **kimlik doğrulaması** sayfası.
- **Uygulama türü** artık geçerli bir alan değil. Bunun yerine, yeniden yönlendirme URI'leri (üzerinde bulabileceğiniz **kimlik doğrulaması** sayfası) hangi uygulama türlerini desteklendiğini belirlemek.
- **Uygulama Kimliği URI'si** artık adlı **uygulama kimliği URI'si** ve bunu şirket bulabilirsiniz **bir API'yi kullanıma sunmak** dikey penceresi. Eski deneyimi, bu özelliğin otomatik-aşağıdaki biçimi kullanarak kaydedildiği: `https://{tenantdomain}/{appID}` (örneğin, `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). Yeni biçiminde olarak otomatik olarak oluşturulan `api://{appID}`, ancak bunu açıkça kaydedilmiş olması gerekir.

## <a name="reply-urlsredirect-urls"></a>Yanıt URL'leri/yeniden yönlendirme URL'leri

Uygulama eski deneyimi olan bir **yanıt URL'leri** sayfası. Yeni deneyim, bir uygulamanın üzerinde yanıt URL'leri bulunabilir **kimlik doğrulaması** bölümü. Ayrıca, bunlar denir **yeniden yönlendirme URI'leri**. Ayrıca, biçimi için yeniden yönlendirme URI'leri değişti. Uygulama türü (web veya genel) ile ilişkilendirilmesi için gereklidirler. Ayrıca, güvenlik nedenleriyle, joker karakterler ve http:// düzeni desteklenmiyor (dışında http://localhost).

## <a name="keyscertificates--secrets"></a>Anahtarları/sertifikaları & Gizli dizileri

Uygulama eski deneyimi olan **anahtarları** sayfası. Yeni deneyimde, yeniden adlandırıldı **sertifikaları ve parolaları**. Ayrıca, **ortak anahtarları** denir **sertifikaları** ve **parolaları** denir **istemci gizli dizileri**.

## <a name="required-permissionsapi-permissions"></a>Gerekli izinleri/API izinleri

- Uygulama eski deneyimi olan bir **gerekli izinler** sayfası. Yeni deneyimde, yeniden adlandırıldı **API izinleri**.
- Bir API eski deneyimi seçerken, Microsoft APIs veya b kiracısındaki Sorumlular hizmeti aracılığıyla aramayı küçük bir listeden seçebilirsiniz. Yeni deneyim, birden çok sekmelerden birini seçebilirsiniz: **Microsoft APIs**, **Kuruluşum kullandığı API'leri**, veya **Apı'lerim**. Arama çubuğunda **kuruluşumun API'leri** kiracıda hizmet sorumluları için sekmesinde arar kullanır. 

   > [!NOTE]
   > Bu sekme, uygulamanızın bir kiracı ile ilişkili değilse görmezsiniz. Yeni deneyimi kullanarak izin isteme konusunda daha fazla bilgi için bkz. [Bu hızlı başlangıçta](quickstart-configure-app-access-web-apis.md).

- Eski bir deneyim yaşıyordu bir **izinleri verin** üst kısmındaki düğmeye **istenen izinleri** sayfası. Olduğunda, yeni deneyimi bir **onay verme** ile bölümünde bir **yönetici onayı vermek** uygulamanın düğmesinde **API izinleri** bölümü. Ayrıca, düğmeler işlevi yollarla bazı farklar vardır:
   - Eski deneyimi, oturum açmış olan kullanıcının ve istenen izinler bağlı olarak mantıksal değiştirilen. Mantıksal şöyleydi:
      - Yalnızca kullanıcı onayı mümkün izinleri istendi ve oturum açmış olan kullanıcının, bir yönetici değil, kullanıcının istenen izinler için kullanıcı onayı verebilir.
      - Yönetici onayı gerektiren en az bir izin istendi ve oturum açmış olan kullanıcının, bir yönetici değil, kullanıcı onayı vermek çalışılırken bir hata ile karşılaştı.
      - Oturum açmış olan kullanıcının yönetici, yönetici onayı için istenen tüm izinleri verildi.
   - Yeni deneyim, yalnızca bir yönetici izni verebilirsiniz. Bir yönetici seçtiğinde **yönetici onayı vermek** düğmesi, yönetici onayı için istenen tüm izinler verilir.

## <a name="deleting-an-app-registration"></a>Bir uygulama kaydı siliniyor

Eski deneyimi, bir uygulama silinecek tek kiracılı olması gerekiyordu. Sil düğmesini, çok kiracılı uygulamalar için devre dışı bırakıldı. Yeni deneyimde uygulamaları herhangi bir durumu silinebilir ancak eylemi onaylamanız gerekir. Uygulama kayıtları silme hakkında daha fazla bilgi için bkz. [Bu hızlı başlangıçta](quickstart-remove-app.md).

## <a name="application-manifest"></a>Uygulama bildirimi

Eski ve yeni deneyimler, bildirim düzenleyicisini JSON biçimi için farklı sürümlerini kullanın. Daha fazla bilgi için bkz. [uygulama bildirimini](reference-app-manifest.md).

## <a name="new-ui"></a>Yeni kullanıcı Arabirimi

Daha önce yalnızca bildirim düzenleyicisini veya API kullanarak ayarlanabilir veya yoktu özellikler için yeni kullanıcı Arabirimi yoktur.

- **Örtük izin akışı** (oauth2AllowImplicitFlow) bulunabilir **kimlik doğrulaması** sayfası. Farklı olarak eski deneyimi etkinleştirebilirsiniz **erişim belirteçlerini** veya **kimlik belirteçlerini**, veya her ikisini de.
- **Kapsamları bu API tarafından tanımlanan** (oauth2Permissions) ve **istemci uygulamaları yetkili** (preAuthorizedApplications) aracılığıyla yapılandırılabilir **bir API'yi kullanıma sunmak** sayfası. Web API'si olması ve izinleri/kapsamları ortaya çıkarmak için bir uygulama yapılandırma hakkında daha fazla bilgi için bkz. [Bu hızlı başlangıçta](quickstart-configure-app-expose-web-apis.md).
- **Yayımcı etki alanı** (görüntülendiği kullanıcılara üzerinde [uygulamanın onay istemi](application-consent-experience.md)) bulunabilir **markalama dikey** sayfası. Bir yayımcı etki alanını yapılandırma hakkında daha fazla bilgi için bkz. [bu nasıl yapılır](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Sınırlamalar

Yeni deneyimi aşağıdaki sınırlamalara sahiptir:

- Yeni deneyim şu anda Azure AD B2C kiracıları içinde kullanılabilir değil.
- İstemci gizli anahtarları (uygulama parolaları) biçimini eski deneyimi farklı ve CLI keser.
- Kullanıcı Arabiriminde, desteklenen hesapları değerini değiştirme desteklenmiyor. Azure AD arasında tek kiracılı ve çok kiracılı geçiş yapıyorsanız sürece, uygulama bildirimi kullanmanız gerekir.
