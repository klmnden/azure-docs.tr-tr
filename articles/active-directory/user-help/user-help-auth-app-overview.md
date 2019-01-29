---
title: Microsoft Authenticator uygulamasına genel bakış - Azure Active Directory | Microsoft Docs
description: Nedir, dahil olmak üzere Microsoft Authenticator uygulaması hakkında nasıl çalıştığını ve hangi bilgilerin içeriğin bu bölüme dahil öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.openlocfilehash: 2379f1ff4fb4385015cc6077cb923cab998d1d11
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175222"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması nedir?
Microsoft Authenticator uygulamasını hesaplarınıza iki aşamalı doğrulama kullanırsanız oturum yardımcı olur. İki Faktörlü doğrulama özellikle hassas bilgileri görüntülerken hesaplarınızı daha güvenli bir şekilde erişmenize yardımcı olur. Parolaları bu nedenle Unutulan, çalınması veya ele geçirilen, iki Faktörlü doğrulama kesmek diğer kişiler için daha zor hale getirerek hesabınızın korunmasına yardımcı olan bir ek güvenlik adımdır.

Microsoft Authenticator uygulamasını birden çok yolla kullanabilirsiniz:

- Sonra kullanıcı adı ve parolanızla oturum ikinci bir doğrulama yöntemi için bir istem sağlama.

- Oturum açma olmadan parola gerektirme, parmak izi, yüz tanıma veya PIN kullanıcı adınızı ve mobil Cihazınızı kullanarak sağlama.

>[!Important]
>Bu içerik kullanıcılara yöneliktir. Yöneticiyseniz, Azure Active Directory (Azure AD) ortamınızı ayarlama ve yönetme hakkında daha fazla bilgi için [Azure Active Directory Belgelerine](https://docs.microsoft.com/azure/active-directory) bakabilirsiniz.<br><br>Hesabınızda oturum açarken sorun yaşıyorsanız, bkz. [olamaz oturum açtığınızda Microsoft hesabınızı](https://support.microsoft.com/help/12429) Yardım.  Aldığınız olduğunda yapılması gerekenler hakkında daha fazla bilgi alın ["Bu Microsoft hesabı mevcut değil"](https://support.microsoft.com/help/13811) iletisi, Microsoft hesabınızda oturum açın.

## <a name="terminology"></a>Terminoloji
|Sözleşme Dönemi|Açıklama|
|----|-----------|
|İki Faktörlü doğrulama |Özellikle, parola ve PIN gibi doğrulama bilgilerini yalnızca iki parça kullanmanızı gerekli hale getirmiş doğrulama işlemi. Microsoft Authenticator uygulamasını hem standart iki Faktörlü doğrulama ve parolasız oturum açma destekler.|
|Çok faktörlü kimlik doğrulaması (MFA)|Multi-Factor authentication, kullanmanızı gerektiren tüm iki aşamalı doğrulama, *en az* doğrulama bilgileri, kuruluşunuzun gereksinimlerine göre iki parça.|
|Microsoft hesabı (olarak da bilinen MSA)|Erişmek için tüketici yönelimli Microsoft ürünlerinde ve bulut Hizmetleri, Outlook, OneDrive, Xbox LIVE veya Office 365 gibi kendi kişisel hesapları oluşturun. Microsoft hesabınızı oluşturulur ve Microsoft tarafından çalıştırılan Microsoft tüketici kimlik hesap sistemi depolanır.|
|İş veya okul hesabı|Kuruluşunuz iş veya Okul hesabınızı oluşturur (gibi alain@contoso.com) ve Microsoft Azure, Windows Intune ve Office 365 gibi büyük olasılıkla kısıtlı kaynaklarına erişim iç izin vermek için.|

## <a name="how-two-factor-verification-works-with-the-app"></a>İki aşamalı doğrulama uygulamayı nasıl çalışır?
İki Microsoft Authenticator uygulaması ile doğrulama çalışır aşağıdaki yollarla faktörü:

- **Bildirim.** Sizin oturum içinde iş veya Okul hesabı veya kişisel Microsoft hesabınız için cihazın kullanıcı adı ve parola yazın ve ardından Microsoft Authenticator uygulamasını isteyen bir bildirim gönderir **oturumonaylama**. Seçin **Onayla** oturum açma denemesi tanımak durumunda. Aksi takdirde seçin **Reddet**. Seçerseniz **Reddet**, ayrıca istek sahte olarak işaretleyebilirsiniz.

- **Doğrulama kodu.** Sizin oturum içinde iş veya Okul hesabı veya kişisel Microsoft hesabınız için cihazın kullanıcı adı ve parola yazın ve ardından ilişkili doğrulama kodu kopyalayın **hesapları** Microsoft ekranı Authenticator uygulaması.

- **Parolasız oturum açın.** Cihazın içine ya da iş veya Okul hesabı veya kişisel Microsoft hesabınızla oturum açtığınızdan ve ardından, parmak izi, yüz tanıma veya PIN kullanarak olduğunu doğrulamak için mobil Cihazınızı kullanmak adınızı yazın. Bu yöntem için parolanızı girmeniz gerekmez.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Cihazınızın biyometrik özellikleri kullanıp kullanmayacağınızı
Kimlik doğrulama işlemini tamamlamak için bir PIN kullanıyorsanız, bunun yerine cihazınızın parmak izi veya yüz tanıma (Biyometri) özelliklerini kullanmak için Microsoft Authenticator uygulamasını ayarlayabilirsiniz. Bu ilk defa ayarlayabilirsiniz authenticator uygulamasını cihaz biyometrik yeteneklerinizi PIN'iniz yerine kimlik olarak kullanmak için seçeneği belirleyerek hesabınızı doğrulamak için kullanın.

## <a name="who-decides-if-you-use-this-feature"></a>Olan bu özellik kullanırsanız karar verir?
Hesap türüne bağlı olarak, iki Faktörlü doğrulama kullanmalısınız veya kendiniz karar verebilirsiniz olabilir, kuruluşunuzun karar verebilirsiniz.

- **İş veya Okul hesabı.** Bir iş veya Okul hesabı kullanıyorsanız (örneğin, alain@contoso.com), kuruluşunuz için belirli doğrulama yöntemleriyle birlikte, iki Faktörlü doğrulama kullanmanız gerekir olduğu. Çok faktörlü doğrulama hakkında daha fazla bilgi için bkz: [Azure multi-Factor Authentication benim için ne demektir](multi-factor-authentication-end-user.md). Microsoft Authentication uygulamasını kullanmak için güvenlik bilgileri ' ayarlama hakkında daha fazla bilgi için bkz. [güvenlik bilgileri bir doğrulayıcı uygulama (Önizleme) kullanacak şekilde](security-info-setup-auth-app.md).

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki aşamalı doğrulama ayarlamak seçebilirsiniz (örneğin, alain@outlook.com).

- **Microsoft olmayan kişisel hesaplar.** Microsoft olmayan kişisel hesaplarınız için iki aşamalı doğrulama ayarlamak seçebilirsiniz (örneğin, alain@gmail.com). Microsoft olmayan hesapların kullanamayabilir terimi, iki aşamalı doğrulama, ancak özelliğin Bul olmalıdır **güvenlik** veya **oturum** ayarları.

## <a name="in-this-section"></a>Bu bölümde

|Makale |Açıklama |
|------|------------|
|[Uygulamayı indirme ve yükleme](microsoft-authenticator-app-how-to.md)|Nerede tanımlar ve nasıl alınacağını ve Android, iOS ve Windows Phone çalıştıran cihazlar için Microsoft Authenticator uygulamasını yükleyin.|
|[İş veya Okul hesapları ekleme](microsoft-authenticator-app-add-work-account.md)|Çeşitli iş veya Okul ve kişisel hesaplar için Microsoft Authenticator uygulamasını eklemeyi açıklar.|
|[Kişisel hesaplarınızın Ekle](microsoft-authenticator-app-add-personal-account.md)|Kişisel Microsoft ve Microsoft dışı hesaplar için Microsoft Authenticator uygulamasını eklemeyi açıklar.|
|[Hesaplarınızı el ile Ekle](microsoft-authenticator-app-add-account-manual.md)|Sağlanan QR kodunu tarayın yapamıyorsanız el ile hesaplarınızı Microsoft Authenticator uygulamasına eklemeyi açıklar.|
|[Uygulamayı kullanarak oturum açın](microsoft-authenticator-app-phone-signin-faq.md)|Microsoft Authenticator uygulamasını kullanarak, çeşitli hesaplarında oturum açıklar.|
|[Hesap kimlik bilgilerini yedekleme ve kurtarma](microsoft-authenticator-app-backup-and-recovery.md)| Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerinizi yedekleme ve kurtarma hakkında bilgi sağlar.|
|[Microsoft Authenticator uygulaması hakkında SSS](microsoft-authenticator-app-faq.md)|Uygulamayla ilgili sık sorulan soruların yanıtlarını sunar.|
