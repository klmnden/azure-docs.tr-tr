---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde yapılandırma | Microsoft Docs
description: Bir Azure AD Domain Services yönetilen etki alanı için güvenli LDAP (LDAPS) yapılandırın
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: ergreenl
ms.openlocfilehash: e961b6c4b97103668c591e319a81d20f533b4acb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246382"
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Güvenli LDAP (LDAPS) bir Azure AD Domain Services yönetilen etki alanı için yapılandırma
Bu makalede, Azure AD Domain Services yönetilen etki alanınıza Güvenli Basit Dizin Erişim Protokolü (LDAPS) nasıl olanak sağlayabileceğiniz açıklanmaktadır. Güvenli LDAP olan olarak da bilinen ' Basit Dizin Erişim Protokolü (LDAP) Güvenli Yuva Katmanı (SSL) üzerinden / Aktarım Katmanı Güvenliği (TLS)'.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. A **güvenli LDAP'yi etkinleştirmek için kullanılacak sertifikayı**.

   * **Önerilen** -bir güvenilir bir ortak sertifika yetkilisinden bir sertifika edinin. Bu yapılandırma seçeneği daha güvenlidir.
   * Alternatif olarak, siz de seçebilirler [otomatik olarak imzalanan bir sertifika oluşturmak](#task-1---obtain-a-certificate-for-secure-ldap) bu makalenin sonraki bölümlerinde gösterilen şekilde.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Güvenli LDAP sertifikası gereksinimleri
Güvenli LDAP etkinleştirmeden önce aşağıdakilere başına geçerli bir sertifika edinin. Geçersiz/yanlış bir sertifika ile yönetilen etki alanınız için güvenli LDAP'yi etkinleştirme çalışırsanız hatalarıyla karşılaşırsanız.

1. **Güvenilen veren** -sertifika güvenli LDAP kullanarak yönetilen etki alanına bağlanma bilgisayarlar tarafından güvenilen bir yetkili tarafından verilmiş olması gerekir. Bu yetki, bir ortak sertifika yetkilisi (CA) veya bu bilgisayar tarafından güvenilen bir kuruluş CA olabilir.
2. **Yaşam süresi** -en az bir sonraki 3-6 ay boyunca sertifika geçerli olmalıdır. Sertifikanın süresi dolduğunda, yönetilen etki alanınıza güvenli LDAP erişimini bozulur.
3. **Konu adı** -sertifika üzerindeki konu adı, yönetilen etki alanınız olmalıdır. Örneğin, 'contoso100.com' etki alanınızı adlandırılmışsa, sertifikanın konu adı 'contoso100.com' olmalıdır. DNS adı (konu alternatif adı), yönetilen etki alanınız için bir joker karakter adına ayarlayın.
4. **Anahtar kullanımı** -için aşağıdakileri kullanır - dijital imzalar ve anahtar şifreleme sertifikası yapılandırılmalıdır.
5. **Sertifika amacı** -sertifikayı SSL sunucu kimlik doğrulaması için geçerli olmalıdır.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Görev 1 - güvenli LDAP için sertifika edinme
İlk görev, yönetilen etki alanı için güvenli LDAP erişimi için kullanılan bir sertifika edinme içerir. İki seçeneğiniz vardır:

* Bir ortak CA ya da kuruluş CA bir sertifika edinin.
* Otomatik olarak imzalanan bir sertifika oluşturun.

> [!NOTE]
> Güvenli LDAP sertifikasını verenin güvenli LDAP kullanarak yönetilen etki alanına bağlanmak için gereken istemci bilgisayarların güvenmesi gerekir.
>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>(Önerilen). seçenek - bir sertifika yetkilisinden bir güvenli LDAP sertifikası
Kuruluşunuz bir ortak CA sertifikalarını alırsa, bu genel bir CA'dan güvenli LDAP sertifikasını edinin. Bir kuruluş sertifika yetkilisi dağıtırsanız, kuruluş CA'sından güvenli LDAP sertifikasını edinin.

> [!TIP]
> **İle yönetilen etki alanları için otomatik olarak imzalanan sertifikaları kullanmak '. onmicrosoft.com' etki alanı sonekleri.**
> Yönetilen etki alanınızın DNS etki alanı adı ile biter, '. onmicrosoft.com', bir ortak sertifika yetkilisinden bir güvenli LDAP sertifikası alınamıyor. Microsoft 'onmicrosoft.com' etki alanı sahibi olduğu bir güvenli LDAP sertifikası için bir etki alanı için bu sonekiyle vermek ortak sertifika yetkilileri reddeder. Bu senaryoda, otomatik olarak imzalanan bir sertifika oluşturabilir ve bunu güvenli LDAP yapılandırmak için kullanabilirsiniz.
>

Sertifikanın genel sertifika yetkilisinden elde bölümünde açıklanan tüm gereksinimlerini karşılayan olun [gereksinimleri için güvenli LDAP sertifikasını](#requirements-for-the-secure-ldap-certificate).


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>B seçeneği - için güvenli LDAP otomatik olarak imzalanan bir sertifika oluşturma
Bir genel sertifika yetkilisinden bir sertifika kullanmayı düşünmüyorsanız, otomatik olarak imzalanan bir sertifika için güvenli LDAP oluşturmayı da seçebilirsiniz. Yönetilen etki alanınızın DNS etki alanı adı ile biter, bu seçeneği belirleyin. '. onmicrosoft.com'.

**PowerShell kullanarak otomatik olarak imzalanan bir sertifika oluşturma**

Windows bilgisayarınızda olarak yeni bir PowerShell penceresi açın **yönetici** ve yeni bir otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki komutları yazın.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.com, contoso100.com
```

Yukarıdaki örnekte, 'contoso100.com' yönetilen etki alanınızın DNS etki alanı adıyla değiştirin. Örneğin, 'contoso100.onmicrosoft.com' adında yönetilen bir etki alanı oluşturduysanız, konu özniteliğinde ' contoso100.com' 'contoso100.onmicrosoft.com' ile değiştirin ve ' *. contoso100.com' ile DnsName özniteliğindeki '* . contoso100.onmicrosoft.com').

![Azure AD Dizini Seçme](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Yeni oluşturulan otomatik olarak imzalanan sertifika, yerel makinenin sertifika depolama alanına yerleştirilir.


## <a name="next-step"></a>Sonraki adım
[Görev 2 - için güvenli LDAP sertifikasını dışarı aktarma bir. PFX dosyası](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
