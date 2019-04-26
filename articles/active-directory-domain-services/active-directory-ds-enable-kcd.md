---
title: 'Azure Active Directory etki alanı Hizmetleri: Kısıtlı kerberos temsilcisi seçmeyi etkinleştirme | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarında kerberos kısıtlanmış temsili etkinleştir
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 5569344a2df560036b99dea40c466302f5e6fe4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359385"
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Yönetilen bir etki alanında Kerberos Kısıtlı temsilci (KCD) yapılandırma
Birçok uygulama, kullanıcı bağlamında kaynaklara erişmek gerekir. Active Directory, bu kullanım örneği tanıyan Kerberos temsilcisi seçme adlı bir mekanizmayı destekler. Ayrıca, böylece yalnızca belirli kaynaklara kullanıcı bağlamında erişilebilir temsilci kısıtlayabilirsiniz. Azure AD Domain Services yönetilen etki alanlarını geleneksel Active Directory etki alanlarından farklı olduğundan, daha güvenli bir şekilde kilitlendiğini.

Bu makalede bir Azure AD Domain Services yönetilen etki alanında kısıtlı Kerberos temsilcisi seçmeyi yapılandırma gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos Kısıtlı temsilci (KCD)
Kerberos temsilcisi, bir hesap kimliğine bürün kaynaklara erişmek için başka bir güvenlik sorumlusu (örneğin, bir kullanıcı) sağlar. Bir kullanıcı bağlamında bir arka uç web API'sine erişen bir web uygulamasını göz önünde bulundurun. Bu örnekte, (bir hizmet hesabı veya bilgisayar/makine hesabı bağlamında çalışır) web uygulamasının kaynak (arka uç web API'si) erişirken kullanıcının kimliğine bürünür. Kerberos temsilcisi seçmeyi güvenli olduğu özellikleri alınırken hesabı kullanıcı bağlamında erişebildiği kaynakları kısıtlamaz.

Kerberos Kısıtlı temsilci (KCD), belirtilen sunucunun kullanıcı adına hareket edebilecekleri Hizmetleri/kaynakları kısıtlar. Geleneksel KCD, bir hizmet için bir etki alanı hesabı yapılandırmak için etki alanı yöneticisi ayrıcalıkları gerektirir ve bu hesabın tek bir etki alanına kısıtlar.

Geleneksel KCD, ayrıca kendisiyle ilişkilendirilmiş bir bazı sorunlar vardır. Etki alanı yöneticisi hesabı tabanlı KCD hizmet için yapılandırılmış önceki işletim sistemlerinde hizmet yöneticisinin hangi ön uç hizmetlerinin sahip oldukları kaynak hizmetlerin temsilcisi bilmek bilmesinin yolu yoktu. Ve bir kaynak hizmetine temsilci seçebilecek tüm ön uç Hizmetleri olası bir saldırı noktası temsil edilir. Ön uç hizmeti barındırılan bir sunucunun güvenliği aşılırsa ve bu kaynak Hizmetleri temsilci seçmek üzere yapılandırılmışsa, kaynak hizmetlerin de tehlikeye girebilir.

> [!NOTE]
> Bir Azure AD Domain Services yönetilen etki alanında, etki alanı yönetici ayrıcalıklarına sahip değil. Bu nedenle, **geleneksel hesabı tabanlı KCD, yönetilen bir etki alanında yapılandırılamaz**. Kaynak tabanlı KCD, bu makalede anlatıldığı gibi kullanın. Bu mekanizma ayrıca daha güvenlidir.
>
>

## <a name="resource-based-kcd"></a>Kaynak tabanlı KCD
Windows Server 2012'den başlayarak, hizmet yöneticileri, hizmet için kısıtlanmış temsili yapılandırma olanağı elde edin. Bu modelde, arka uç hizmet yöneticisine izin verebilir veya KCD kullanarak özel ön uç Hizmetleri reddet. Bu model olarak da bilinen **kaynak tabanlı KCD**.

Kaynak tabanlı KCD, PowerShell kullanılarak yapılandırılır. Kullandığınız `Set-ADComputer` veya `Set-ADUser` özellikleri alınırken hesabı bir kullanıcı hesabı/hizmet hesabı veya bilgisayar hesabını olmasına bağlı olarak, cmdlet'ler.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Yönetilen bir etki alanında bilgisayar hesabı için kaynak tabanlı KCD yapılandırın
Bilgisayarda çalışan bir web uygulamasına sahip ' contoso100-webapp.contoso100.com'. Kaynağa erişmek ihtiyaç duyduğu (üzerinde çalışan bir web API ' contoso100-api.contoso100.com') etki alanı kullanıcıları bağlamında. İşte nasıl kaynak tabanlı KCD bu senaryo için ayarlamanız:

1. [Özel bir OU oluşturmanız](active-directory-ds-admin-guide-create-ou.md). Kullanıcılar yönetilen etki alanı içinde özel bu OU'ya yönetmek için izinleri verebilirsiniz.
2. Her iki sanal makine (bir web uygulaması çalıştıran ve bir web API'sini çalıştırma), yönetilen etki alanına katılın. Bu özel kuruluş içinde bilgisayar hesapları oluşturun.
3. Artık, aşağıdaki PowerShell komutunu kullanarak kaynak tabanlı KCD yapılandırın:

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Web uygulaması ve web API'si için bilgisayar hesaplarının kaynak tabanlı KCD yapılandırma izinlerine sahip olduğu özel bir OU'da olması gerekir. Yerleşik 'AAD DC bilgisayarlar' kapsayıcısında kaynak tabanlı KCD bilgisayar hesabı için yapılandıramazsınız.
>

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Yönetilen bir etki alanında bir kullanıcı hesabı için kaynak tabanlı KCD yapılandırın
Bir hizmet hesabı 'appsvc' olarak çalışan bir web uygulaması olduğu varsayılır ve etki alanı kullanıcıları bağlamında (bir hizmet hesabı - 'backendsvc' olarak çalışan bir web API'si) kaynağa erişmek gerekiyor. İşte nasıl kaynak tabanlı KCD bu senaryo için ayarlamanız.

1. [Özel bir OU oluşturmanız](active-directory-ds-admin-guide-create-ou.md). Kullanıcılar yönetilen etki alanı içinde özel bu OU'ya yönetmek için izinleri verebilirsiniz.
2. Arka uç web API'si/kaynak yönetilen etki alanında çalışan sanal makinenin katılın. Kendi özel kuruluş içinde bilgisayar hesabı oluşturun.
3. Özel kuruluş içinde web uygulamasını çalıştırmak için kullanılan hizmet hesabı (örneğin, ' appsvc') oluşturun.
4. Artık, aşağıdaki PowerShell komutunu kullanarak kaynak tabanlı KCD yapılandırın:

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Arka uç web API'si için bilgisayar hesabı ve kaynak tabanlı KCD yapılandırma izinlerine sahip olduğu özel bir OU'da olması hizmet hesabı gerekir. Yerleşik 'AAD DC kullanıcılar' kapsayıcısında kaynak tabanlı KCD'yerleşik 'AAD DC bilgisayarlar' kapsayıcısındaki bilgisayar hesabı veya kullanıcı hesapları için yapılandıramazsınız. Bu nedenle, kaynak tabanlı KCD ' ayarlamak için Azure AD'den eşitlenen kullanıcı hesapları kullanamazsınız.
>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Kerberos Kısıtlı temsilci seçmeye genel bakış](https://technet.microsoft.com/library/jj553400.aspx)
