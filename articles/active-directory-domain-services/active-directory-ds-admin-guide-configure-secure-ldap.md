---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde yapılandırma | Microsoft Docs
description: Güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yapılandırın
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: b130d6ac1dda93f941d4ff244c6f4513faf1ade0
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36211518"
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Güvenli LDAP (LDAPS) Azure AD etki alanı Hizmetleri yönetilen etki alanı için yapılandırma
Bu makalede, Azure AD etki alanı Hizmetleri yönetilen etki alanınız için Güvenli Basit Dizin Erişim Protokolü (LDAPS) nasıl etkinleştirebilirsiniz gösterilmektedir. Güvenli LDAP olduğu olarak da bilinen ' Basit Dizin Erişim Protokolü (LDAP) Güvenli Yuva Katmanı (SSL) üzerinden / Aktarım Katmanı Güvenliği (TLS)'.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. A **güvenli LDAP etkinleştirmek için kullanılan sertifikayı**.

   * **Önerilen** -bir güvenilir bir ortak sertifika yetkilisinden bir sertifika edinin. Bu yapılandırma seçeneği daha güvenlidir.
   * Alternatif olarak, siz de seçebilirsiniz [otomatik olarak imzalanan sertifika oluşturma](#task-1---obtain-a-certificate-for-secure-ldap) bu makalenin sonraki bölümlerinde gösterildiği gibi.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Güvenli LDAP sertifika için gereksinimler
Güvenli LDAP etkinleştirmeden önce aşağıdaki yönergeleri başına geçerli bir sertifika edinin. Güvenli LDAP geçersiz/hatalı bir sertifika ile yönetilen etki alanınız için etkinleştirmeye çalışırsanız hatalarıyla karşılaşırsanız.

1. **Güvenilen veren** -sertifika güvenli LDAP kullanarak yönetilen etki alanına bağlanma bilgisayarlar tarafından güvenilen bir yetkili tarafından verilmiş olması gerekir. Bu yetkilisi, bir ortak sertifika yetkilisi (CA) veya bu bilgisayarlar tarafından güvenilen bir kuruluş CA olabilir.
2. **Yaşam süresi** -sertifika en az sonraki 3-6 ay için geçerli olmalıdır. Sertifikanın süresi dolduğunda, yönetilen etki alanınız güvenli LDAP erişim bozulur.
3. **Konu adı** -yönetilen etki alanınız için joker karakter sertifika üzerindeki konu adı olmalıdır. Örneğin, etki alanınızın 'contoso100.com' adlı sertifikanın konu adı olması gerekir ' *. contoso100.com'. DNS adı (konu alternatif adı) Bu joker karakter adına ayarlayın.
4. **Anahtar kullanımı** -için aşağıdaki kullanır - dijital imzalar ve anahtar şifreleme sertifikası yapılandırılması gerekir.
5. **Sertifika amacı** -sertifikayı SSL sunucu kimlik doğrulaması için geçerli olmalıdır.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Görev 1 - güvenli LDAP için bir sertifika edinin
İlk görev, yönetilen etki alanına güvenli LDAP erişim için kullanılan bir sertifika edinme içerir. İki seçeneğiniz vardır:

* Bir ortak CA ya da bir kuruluş CA bir sertifika edinin.
* Kendinden imzalı bir sertifika oluşturun.

> [!NOTE]
> Güvenli LDAP kullanarak yönetilen etki alanına bağlanmak için gereken istemci bilgisayarlar güvenli LDAP sertifikayı veren güvenmesi gerekir.
>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>(Önerilen). seçenek - bir sertifika yetkilisinden bir güvenli LDAP sertifikası alın
Kuruluşunuzun genel bir CA'dan sertifikalarını alırsa, o genel CA'dan güvenli LDAP sertifika edinin. Bir kuruluş CA'sı dağıtırsanız, kuruluş CA'sı güvenli LDAP sertifika edinin.

> [!TIP]
> **İle yönetilen etki alanları için otomatik olarak imzalanan sertifikalar kullanmak '. onmicrosoft.com' etki alanı sonekleri.**
> Yönetilen etki alanınızın DNS etki alanı adı ile biten, '. onmicrosoft.com', bir ortak sertifika yetkilisinden bir güvenli LDAP sertifika elde edemiyor. Microsoft 'onmicrosoft.com' etki alanına sahip olduğundan, bir güvenli LDAP sertifikası için bir etki alanı için bu sonekiyle vermek ortak sertifika yetkilileri reddeder. Bu senaryoda, kendinden imzalı bir sertifika oluşturun ve bu güvenli LDAP yapılandırmak için kullanın.
>

Ortak sertifika yetkilisinden elde sertifika anlatılan tüm gereksinimleri karşılayan olun [güvenli LDAP sertifika için gereksinimler](#requirements-for-the-secure-ldap-certificate).


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Seçeneği B - güvenli LDAP için otomatik olarak imzalanan sertifika oluşturma
Bir ortak sertifika yetkilisinden bir sertifika kullanmayı düşünmüyorsanız güvenli LDAP için otomatik olarak imzalanan bir sertifika oluşturmak tercih edebilirsiniz. Yönetilen etki alanınızın DNS etki alanı adı ile biten varsa bu seçeneği seçin '. onmicrosoft.com'.

**PowerShell kullanarak otomatik olarak imzalanan bir sertifika oluşturun**

Windows bilgisayarınızda olarak yeni bir PowerShell penceresi açın **yönetici** ve yeni bir otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki komutları yazın.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject *.contoso100.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.com
```

Önceki örnekte, Değiştir '*. contoso100.com', yönetilen etki alanınızın DNS etki alanı adına sahip. 'Contoso100.onmicrosoft.com' adında yönetilen bir etki alanı oluşturduysanız, for example, Değiştir '*. contoso100.com' ile önceki betikteki ' *. contoso100.onmicrosoft.com').

![Azure AD Dizini Seçme](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Yeni oluşturulan otomatik olarak imzalanan sertifika, yerel makinenin sertifika deposuna yerleştirilir.


## <a name="next-step"></a>Sonraki adım
[Görev 2 - güvenli LDAP sertifika verme bir. PFX dosyası](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
