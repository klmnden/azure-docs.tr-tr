---
title: "Aracılığıyla parolası olmayan kimlikleri Windows Hello iş ve Azure AD için kimlik doğrulaması | Microsoft Docs"
description: "İş ve iş için Windows Hello dağıtma hakkında ek bilgi için Windows Hello genel bakış sağlar."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: f907bb90-8776-46ca-9e12-279949af66ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 62adf8a9fd4400a056e2c0f59c79431acbad5865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authenticating-identities-without-passwords-through-windows-hello-for-business"></a>Kimlik doğrulama aracılığıyla parolası olmayan kimlikleri iş için Windows Hello
Geçerli parolalar tek başına ile kimlik doğrulama yöntemlerinin kullanıcıların güvenli tutmak yeterli değildir. Kullanıcıların yeniden kullanmak ve parolaları unutmayın. Parolalar breachable, phishable, kırık yatkın ve guessable. Ayrıca unutmayın zor ve saldırıları gibi yatkın Al "[karma değer geçişi](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-windows-hello-for-business"></a>İş için Windows Hello hakkında
İş için Windows Hello bir özel/ortak anahtarı veya parolaları gider sertifika tabanlı kimlik doğrulaması yaklaşım kuruluşlar ve tüketicileri için kullanılabilir. Bu form kimlik doğrulaması parolaları değiştirebilir ve ihlallerinden, thefts ve kimlik avı dayanıklıdır anahtar çifti kimlik bilgilerini kullanır.

 İş için Windows Hello bir Microsoft hesabı, bir Windows Server Active Directory hesabı, bir Microsoft Azure Active Directory (Azure AD) hesabı veya hızlı kimlik çevrimiçi (FIDO) kimlik doğrulamasını destekleyen Microsoft dışı hizmet kimlik doğrulaması sağlar. İşletme kaydı için bir ilk iki aşamalı doğrulama sırasında Windows Hello sonra iş için Windows Hello kullanıcının cihazında ayarlanır ve kullanıcı Windows Hello veya bir PIN olabilir bir hareketi ayarlar. Kullanıcı kimliklerini doğrulamak için hareketi sağlar. Windows sonra iş için Windows Hello kullanıcının kimliğini doğrulamak ve korumalı kaynaklar ve hizmetlerine erişmek için yardımcı olmak için kullanır.

Özel anahtar yalnızca bir PIN, Biyometri veya cihaza oturum açmak için kullanıcının kullandığı bir akıllı kart gibi uzak bir aygıtı gibi "kullanıcı hareketi" yoluyla kullanılabilir hale getirilir. Bu bilgiler bir sertifika veya değiştirip asimetrik anahtar çifti bağlanır. Donanım cihazı bir Güvenilir Platform Modülü (TPM) yongası varsa Onaylandı özel anahtarıdır. Özel anahtarı hiçbir zaman sürücüden ayrılmaz.

Ortak anahtar, Azure Active Directory ve Windows Server Active Directory (şirket içi) kaydedilir. Kimlik sağlayıcısı (IDPs) özel anahtara kullanıcının ortak anahtarı eşleyerek kullanıcıyı doğrulamak ve oturum açma bilgileri bir saat parola (OTP), PhoneFactor veya farklı bildirim mekanizması aracılığıyla sağlayın.

## <a name="why-enterprises-should-adopt-windows-hello-for-business"></a>Neden kuruluşların iş için Windows Hello benimsemeye
İş için Windows Hello etkinleştirerek, kuruluşların kaynaklarına göre daha da güvenli yapabilirsiniz:

* İş için Windows Hello donanım tercih edilen bir seçeneğiyle ayarlama. Bu anahtarlar TPM 1.2 veya TPM 2.0 kullanılabilir olduğunda oluşturulması anlamına gelir. TPM mevcut olmadığında, yazılım anahtarı oluşturur.
* PIN uzunluğu ve karmaşıklık tanımlama ve Hello kullanım, kuruluşunuzda etkinleştirilip etkinleştirilmediğini gösterir.
* Sertifika tabanlı güven kullanarak akıllı kart benzeri senaryolarını desteklemek üzere iş için Windows Hello yapılandırma.

## <a name="how-windows-hello-for-business-works"></a>İş için Windows Hello işleyişi
1. Anahtarları donanımda TPM veya yazılım tarafından oluşturulur. Birçok cihaz donanım cihazlarının şifreleme anahtarları ile tümleştirerek güvenliğini sağlar. yerleşik bir TPM yongası vardır. TPM 1.2 veya TPM 2.0 anahtar veya oluşturulan anahtarlarından oluşturulan sertifika oluşturur.
2. Bu donanım bağlı anahtarlar TPM gösterir.
3. Tek unlock hareketi cihaz kilidini açar. Cihaz etki alanına katılmış veya Azure ise bu hareketi AD alanına katılmış birden çok kaynaklara erişim izni verir.

## <a name="how-the-windows-hello-for-business-lifecycle-works"></a>İş yaşam döngüsü için Windows Hello işleyişi
![İş yaşam döngüsü için Windows Hello](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Önceki diyagramda özel/ortak anahtar çifti ve kimlik sağlayıcısı tarafından doğrulama gösterilmektedir. Bu adımların her biri aşağıda ayrıntılı olarak açıklanmıştır:

1. Kullanıcı kimliklerini kanıtlayan birden fazla yerleşik sağlama yöntemleri (hareketleri, fiziksel akıllı kartlar, çok faktörlü kimlik doğrulaması) için bir kimlik sağlayıcıyı (IDP) Azure Active Directory gibi bu bilgileri gönderir ve şirket içi Active Directory.
2. Cihaz ardından anahtarı oluşturur, anahtar gösterir, bu anahtarı ortak kısmını alır, istasyon ifadelerle ekler, oturum açtığında ve anahtar kaydetmek için IDP gönderir.
3. Ortak anahtar kısmını IDP kaydeder hemen IDP özel anahtar bölümünü ile imzalamak için cihaz sınar.
4. IDP doğrular ve korunan kaynaklar kullanıcı ve aygıt erişim sağlayan kimlik doğrulama belirteci verir. IDPs platformlar arası uygulamalar yazma veya oluşturmak ve Windows Hello için kullanıcılar iş kimlik bilgilerini kullanmak için tarayıcı desteği (aracılığıyla JavaScript/Webcrypto API) kullanın.

## <a name="the-deployment-requirements-for-windows-hello-for-business"></a>Dağıtım için gereksinimleri iş için Windows Hello
### <a name="at-the-enterprise-level"></a>Kurumsal düzeyde
* Kuruluş Azure aboneliği sahiptir.

### <a name="at-the-user-level"></a>Kullanıcı düzeyinde
* Kullanıcının bilgisayarı Windows 10 Professional veya Enterprise çalışır.

Ayrıntılı dağıtım yönergeleri için bkz: [etkinleştirmek için Windows Hello kuruluştaki iş](active-directory-azureadjoin-passport-deployment.md).

## <a name="additional-information"></a>Ek bilgiler
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılım ayarlama](active-directory-azureadjoin-setup.md)

