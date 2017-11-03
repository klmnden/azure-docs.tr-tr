---
title: "Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde yapılandırma | Microsoft Docs"
description: "Güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yapılandırın"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 93afa49166c5b31d23237c308b9d34f6d6f3507d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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

1. **Güvenilen veren** -sertifika güvenli LDAP kullanarak yönetilen etki alanına bağlanma bilgisayarlar tarafından güvenilen bir yetkili tarafından verilmiş olması gerekir. Bu yetkilisi bu bilgisayarlar tarafından güvenilen bir genel sertifika yetkilisinin olabilir.
2. **Yaşam süresi** -sertifika en az sonraki 3-6 ay için geçerli olmalıdır. Sertifikanın süresi dolduğunda, yönetilen etki alanınız güvenli LDAP erişim bozulur.
3. **Konu adı** -yönetilen etki alanınız için joker karakter sertifika üzerindeki konu adı olmalıdır. Örneğin, etki alanınızın 'contoso100.com' adlı sertifikanın konu adı olması gerekir ' *. contoso100.com'. DNS adı (konu alternatif adı) Bu joker karakter adına ayarlayın.
4. **Anahtar kullanımı** -için aşağıdaki kullanır - dijital imzalar ve anahtar şifreleme sertifikası yapılandırılması gerekir.
5. **Sertifika amacı** -sertifikayı SSL sunucu kimlik doğrulaması için geçerli olmalıdır.

> [!NOTE]
> **Kuruluş sertifika yetkilileri:** Azure AD etki alanı Hizmetleri, kuruluşunuzun Kurumsal Sertifika yetkilisi tarafından verilen güvenli LDAP sertifikaları kullanarak desteklemez. Bu kısıtlama, hizmet kuruluş kök sertifika yetkilisi olarak CA'nız güvenmezse olmasıdır. 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Görev 1 - güvenli LDAP için bir sertifika edinin
İlk görev, yönetilen etki alanına güvenli LDAP erişim için kullanılan bir sertifika edinme içerir. İki seçeneğiniz vardır:

* Bir sertifika yetkilisinden bir sertifika edinin. Yetkili bir ortak sertifika yetkilisi olabilir.
* Kendinden imzalı bir sertifika oluşturun.

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>(Önerilen). seçenek - bir sertifika yetkilisinden bir güvenli LDAP sertifikası alın
Kuruluşunuz bir ortak sertifika yetkilisi sertifikalarını alırsa, bu ortak sertifika yetkilisinden güvenli LDAP sertifika edinmeniz gerekir.

Anlatılan tüm gereksinimleri karşılayan bir sertifika isterken olun [güvenli LDAP sertifika için gereksinimler](#requirements-for-the-secure-ldap-certificate).

> [!NOTE]
> Güvenli LDAP kullanarak yönetilen etki alanına bağlanmak için gereken istemci bilgisayarlar güvenli LDAP sertifikayı veren güvenmesi gerekir.
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Seçeneği B - güvenli LDAP için otomatik olarak imzalanan sertifika oluşturma
Bir ortak sertifika yetkilisinden bir sertifika kullanmayı düşünmüyorsanız güvenli LDAP için otomatik olarak imzalanan bir sertifika oluşturmak tercih edebilirsiniz.

**PowerShell kullanarak otomatik olarak imzalanan bir sertifika oluşturun**

Windows bilgisayarınızda olarak yeni bir PowerShell penceresi açın **yönetici** ve yeni bir otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki komutları yazın.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Önceki örnekte, Değiştir '*. contoso100.com', yönetilen etki alanınızın DNS etki alanı adına sahip. 'Contoso100.onmicrosoft.com' adında yönetilen bir etki alanı oluşturduysanız, for example, Değiştir '*. contoso100.com' ile yukarıdaki komut dosyasındaki ' *. contoso100.onmicrosoft.com').

![Azure AD Dizini Seçme](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Yeni oluşturulan otomatik olarak imzalanan sertifika, yerel makinenin sertifika deposuna yerleştirilir.


## <a name="next-step"></a>Sonraki adım
[Görev 2 - güvenli LDAP sertifika verme bir. PFX dosyası](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
