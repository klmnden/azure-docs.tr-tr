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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b47b0c5af98198d829c4658877acae2edff5455
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473728"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması nedir?

>[!Important]
>Bu içerik kullanıcılara yöneliktir. Yöneticiyseniz, Azure Active Directory (Azure AD) ortamınızı ayarlama ve yönetme hakkında daha fazla bilgi için [Azure Active Directory Belgelerine](https://docs.microsoft.com/azure/active-directory) bakabilirsiniz.

Microsoft Authenticator uygulamasını hesaplarınıza iki aşamalı doğrulama kullanırsanız oturum yardımcı olur. İki Faktörlü doğrulama özellikle hassas bilgileri görüntülerken hesaplarınızı daha güvenli bir şekilde erişmenize yardımcı olur. Parolaları bu nedenle Unutulan, çalınması veya ele geçirilen, iki Faktörlü doğrulama kesmek diğer kişiler için daha zor hale getirerek hesabınızın korunmasına yardımcı olan bir ek güvenlik adımdır.

Microsoft Authenticator uygulamasını birden çok yolla kullanabilirsiniz:

- Sonra kullanıcı adı ve parolanızla oturum kimlik doğrulaması için bir istem yanıt verir.

- Parmak izi, yüz tanıma veya PIN kullanıcı adınızı, kimlik doğrulayıcı uygulamasını ve mobil Cihazınızı kullanarak parola girmeden oturum.

- Kimlik doğrulayıcısı uygulamalarını destekleyen herhangi bir hesabı için bir kod Oluşturucu.

> [!Important]
> Microsoft Authenticator uygulamasını iki Faktörlü doğrulama kullanır ve zamana bağlı bir kerelik parola (TOTP) standartlarını destekleyen herhangi bir hesabı ile çalışır.
> 
> Kuruluşunuzun oturum açma ve kuruluş verilerini ve belgelerine erişmek için bir kimlik doğrulayıcı uygulama kullanmanızı gerektirebilir. Kullanıcı adınızı uygulamada gibi görünse de, hesap gerçekten kayıt işlemi tamamlanana kadar bir doğrulama yöntemi olarak davranacak şekilde ayarlanmamış. Daha fazla bilgi için [iş veya Okul hesabınızı eklemek](user-help-auth-app-add-work-school-account.md).
> 
> Hesabınızda oturum açarken sorun yaşıyorsanız, bkz. [olamaz oturum açtığınızda Microsoft hesabınızı](https://support.microsoft.com/help/12429) Yardım. Aldığınız olduğunda yapılması gerekenler hakkında daha fazla bilgi alın ["Bu Microsoft hesabı mevcut değil"](https://support.microsoft.com/help/13811) iletisi, Microsoft hesabınızda oturum açın.

## <a name="terminology"></a>Terminoloji

|Sözleşme Dönemi|Açıklama|
|----|-----------|
|İki Faktörlü doğrulama |Özellikle, parola ve PIN gibi doğrulama bilgilerini yalnızca iki parça kullanmanızı gerekli hale getirmiş doğrulama işlemi. Microsoft Authenticator uygulamasını hem standart iki Faktörlü doğrulama ve parolasız oturum açma destekler.|
|Çok faktörlü kimlik doğrulaması (MFA)|Multi-Factor authentication, kullanmanızı gerektiren tüm iki aşamalı doğrulama, *en az* doğrulama bilgileri, kuruluşunuzun gereksinimlerine göre iki parça.|
|Microsoft hesabı (olarak da bilinen MSA)|Erişmek için tüketici yönelimli Microsoft ürünlerinde ve bulut Hizmetleri, Outlook, OneDrive, Xbox LIVE veya Office 365 gibi kendi kişisel hesapları oluşturun. Microsoft hesabınızı oluşturulur ve Microsoft tarafından çalıştırılan Microsoft tüketici kimlik hesap sistemi depolanır.|
|İş veya okul hesabı|Kuruluşunuz iş veya Okul hesabınızı oluşturur (gibi alain@contoso.com) ve Microsoft Azure, Windows Intune ve Office 365 gibi büyük olasılıkla kısıtlı kaynaklarına erişim iç izin vermek için.|
|Doğrulama kodu|Eklenen her hesap altında kimlik doğrulayıcı uygulamasında görüntülenen altı rakamlı kodu. Birisi bir kod birden çok kez tüketmesini her 30 saniyede doğrulama kodunu değiştirir. Bir kerelik geçiş kodu (OTP) olarak da bilinen budur.|

## <a name="how-two-factor-verification-works-with-the-app"></a>İki aşamalı doğrulama uygulamayı nasıl çalışır?
İki Microsoft Authenticator uygulaması ile doğrulama çalışır aşağıdaki yollarla faktörü:

- **Bildirim.** Sizin oturum içinde iş veya Okul hesabı veya kişisel Microsoft hesabınız için cihazın kullanıcı adı ve parola yazın ve ardından Microsoft Authenticator uygulamasını isteyen bir bildirim gönderir **oturumonaylama**. Seçin **Onayla** oturum açma denemesi tanımak durumunda. Aksi takdirde seçin **Reddet**. Seçerseniz **Reddet**, ayrıca istek sahte olarak işaretleyebilirsiniz.

- **Doğrulama kodu.** Sizin oturum içinde iş veya Okul hesabı veya kişisel Microsoft hesabınız için cihazın kullanıcı adı ve parola yazın ve ardından ilişkili doğrulama kodu kopyalayın **hesapları** Microsoft ekranı Authenticator uygulaması. Doğrulama kodunu bir kerelik geçiş kodu (OTP) kimlik doğrulaması de denir.

- **Parolasız oturum açın.** Cihazın içine ya da iş veya Okul hesabı veya kişisel Microsoft hesabınızla oturum açtığınızdan ve ardından, parmak izi, yüz tanıma veya PIN kullanarak olduğunu doğrulamak için mobil Cihazınızı kullanmak adınızı yazın. Bu yöntem için parolanızı girmeniz gerekmez.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Cihazınızın biyometrik özellikleri kullanıp kullanmayacağınızı
Kimlik doğrulama işlemini tamamlamak için bir PIN kullanıyorsanız, bunun yerine cihazınızın parmak izi veya yüz tanıma (Biyometri) özelliklerini kullanmak için Microsoft Authenticator uygulamasını ayarlayabilirsiniz. Bu ilk defa ayarlayabilirsiniz authenticator uygulamasını cihaz biyometrik yeteneklerinizi PIN'iniz yerine kimlik olarak kullanmak için seçeneği belirleyerek hesabınızı doğrulamak için kullanın.

## <a name="who-decides-if-you-use-this-feature"></a>Olan bu özellik kullanırsanız karar verir?
Hesap türüne bağlı olarak, iki Faktörlü doğrulama kullanmalısınız veya kendiniz karar verebilirsiniz olabilir, kuruluşunuzun karar verebilirsiniz.

- **İş veya Okul hesabı.** Bir iş veya Okul hesabı kullanıyorsanız (örneğin, alain@contoso.com), kuruluşunuz için belirli doğrulama yöntemleriyle birlikte, iki Faktörlü doğrulama kullanmanız gerekir olduğu. İş veya Okul hesabınız için Microsoft Authenticator uygulamasını ekleme hakkında daha fazla bilgi için bkz. [, iş veya Okul hesapları ekleme](user-help-auth-app-add-work-school-account.md).

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki aşamalı doğrulama ayarlamak seçebilirsiniz (örneğin, alain@outlook.com). Kişisel Microsoft hesabınızı ekleme hakkında daha fazla bilgi için bkz. [kişisel hesaplarınız ekleme](user-help-auth-app-add-personal-ms-account.md).

- **Microsoft olmayan hesaplar.** Microsoft dışı hesaplarınız için iki aşamalı doğrulama ayarlamak seçebilirsiniz (örneğin, alain@gmail.com). Microsoft olmayan hesapların kullanamayabilir terimi, iki aşamalı doğrulama, ancak özelliğin Bul olmalıdır **güvenlik** veya **oturum** ayarları. Microsoft Authenticator uygulamasını TOTP standartlarını destekleyen herhangi bir hesaplarıyla çalışır. Microsoft olmayan hesapların ekleme hakkında daha fazla bilgi için bkz. [ekleyin, Microsoft olmayan hesapların](user-help-auth-app-add-non-ms-account.md).

## <a name="in-this-section"></a>Bu bölümde

|Makale |Açıklama |
|------|------------|
|[Uygulamayı indirme ve yükleme](user-help-auth-app-download-install.md)|Nerede tanımlar ve nasıl alınacağını ve Android ve iOS çalıştıran cihazlar için Microsoft Authenticator uygulamasını yükleyin.|
|[İş veya Okul hesapları ekleme](user-help-auth-app-add-work-school-account.md)|Çeşitli iş veya Okul ve kişisel hesaplar için Microsoft Authenticator uygulamasını eklemeyi açıklar.|
|[Kişisel hesaplarınızın Ekle](user-help-auth-app-add-personal-ms-account.md)|Kişisel Microsoft hesaplarınız için Microsoft Authenticator uygulamasını eklemeyi açıklar.|
|[Microsoft olmayan hesapların Ekle](user-help-auth-app-add-non-ms-account.md)|Microsoft dışı hesaplarınız için Microsoft Authenticator uygulamasını eklemeyi açıklar.|
|[Hesaplarınızı el ile Ekle](user-help-auth-app-add-account-manual.md)|Sağlanan QR kodunu tarayın yapamıyorsanız el ile hesaplarınızı Microsoft Authenticator uygulamasına eklemeyi açıklar.|
|[Uygulamayı kullanarak oturum açın](user-help-auth-app-sign-in.md)|Microsoft Authenticator uygulamasını kullanarak, çeşitli hesaplarında oturum açıklar.|
|[Hesap kimlik bilgilerini yedekleme ve kurtarma](user-help-auth-app-backup-recovery.md)| Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerinizi yedekleme ve kurtarma hakkında bilgi sağlar.|
|[Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)|Uygulamayla ilgili sık sorulan soruların yanıtlarını sunar.|
