---
title: 'Azure AD etki alanı Hizmetleri: Parola eşitlemeyi etkinleştirme | Microsoft Docs'
description: Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: ergreenl
ms.openlocfilehash: 74ad811481aea83454d7e3179652e68d4c406521
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60417257"
---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme
Önceki görevlerde Azure Active Directory (Azure AD) kiracınız için Azure Active Directory Domain Services hizmetini etkinleştirdiniz. Sıradaki görev, NT LAN Manager (NTLM) ve Kerberos kimlik doğrulaması için gereken kimlik bilgisi karmalarının Azure AD Domain Services ile eşitlemesini etkinleştirmektir. Kimlik bilgisi eşitlemesini ayarladıktan sonra kullanıcılar, şirket kimlik bilgileri ile yönetilen etki alanında oturum açabilir.

İşlemin adımları, yalnızca bulutta yer alan kullanıcı hesapları ile Azure AD Connect kullanılarak şirket içi dizininizden eşitlenmiş hesaplar arasında farklılık gösterir.

| **Kullanıcı hesabı türü** | **Gerçekleştirilecek adımlar** |
| --- | --- |
| **Şirket içi dizinden eşitlenen kullanıcı hesapları** |**&#x2713;** [Bu makaledeki talimatları izleyin](active-directory-ds-getting-started-password-sync-synced-tenant.md#task-5-enable-password-synchronization-to-your-managed-domain-for-user-accounts-synced-with-your-on-premises-ad) |
| **Azure AD'de oluşturulan bulut kullanıcı hesapları** |**&#x2713;** [Yönetilen etki alanınıza yalnızca bulutta yer alan kullanıcı hesaplarının parolalarını eşitleyin](active-directory-ds-getting-started-password-sync.md) |

> [!TIP]
> **İki kümedeki adımları da gerçekleştirmeniz gerekir.**
> Azure AD kiracınızda yalnızca bulut kullanıcılarıyla ve şirket içi AD kullanıcılarının bir bileşimi varsa her iki işlemin de adımlarını takip etmeniz gerekir.
>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>5. Görev: Şirket içi AD’nizden eşitlenmiş kullanıcı hesapları için yönetilen etki alanınıza parolaların eşitlemeyi etkinleştirme
Eşitlenen bir Azure AD kiracısı, Azure AD Connect kullanarak kuruluşunuzun şirket içi dizini ile eşitlenecek şekilde ayarlanır. Azure AD Connect, varsayılan olarak NTLM ve Kerberos kimlik bilgisi karmalarını Azure AD ile eşitlemez. Azure AD Domain Services’ı kullanmak için, Azure AD Connect’i NTLM ve Kerberos kimlik doğrulamasında gereken kimlik bilgisi karmalarını eşitleyecek şekilde yapılandırmanız gerekir. Aşağıdaki adımlar, şirket içi dizininizden gerekli kimlik bilgisi karmalarının Azure AD kiracınız ile eşitlenmesini etkinleştirir.

> [!NOTE]
> **Kuruluşunuzda şirket içi dizininizden eşitlenmiş kullanıcı hesapları varsa, yönetilen etki alanını kullanmak için NTLM ve Kerberos karmalarının eşitlenmesini etkinleştirmeniz gerekir.** Eşitlenmiş kullanıcı hesabı, şirket içi dizininizde oluşturulmuş ve Azure AD Connect kullanarak Azure AD kiracınıza eşitlenmiş bir hesaptır.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Azure AD Connect'i yükleme veya güncelleştirme 
Etki alanına katılan bir bilgisayara, Azure AD Connect'in önerilen en son sürümünü yükleyin. Azure AD Connect kurulumunun var olan bir örneğine sahipseniz, bu örneği Azure AD Connect’in en yeni sürümünü kullanacak şekilde güncelleştirmelisiniz. Düzeltilmiş olabilecek bilinen sorunlardan/hatalardan kaçınmak için her zaman Azure AD Connect'in en yeni sürümünü kullanın.

**[Azure AD Connect'i indirme](https://www.microsoft.com/download/details.aspx?id=47594)**

Önerilen sürüm: **1.1.614.0** - 5 Eylül 2017 tarihinde yayımlanmış.

> [!WARNING]
> Eski parola kimlik bilgilerinin (NTLM ve Kerberos kimlik doğrulaması için gereklidir), Azure AD kiracınız ile eşitlenebilmesini sağlamak için Azure AD Connect'in en yeni önerilen sürümünü yüklemeniz GEREKİR. Bu işlev, Azure AD Connect'in önceki sürümlerinde veya eski DirSync aracında kullanılamaz.
>
>

Azure AD Connect'i yükleme talimatları şu makalede bulunabilir - [Azure AD Connect ile çalışmaya başlama](../active-directory/hybrid/whatis-hybrid-identity.md)

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>NTLM ve Kerberos kimlik bilgisi karmalarını Azure AD'ye eşitlemeyi etkinleştirme
Aşağıdaki PowerShell betiğini tüm AD ormanlarında çalıştırın. Bu betik tüm şirket içi kullanıcıların NTLM ve Kerberos parola karmalarının Azure AD kiracınızla eşitlenmesini sağlar. Betik ayrıca Azure AD Connect'te tam eşleşme başlatır.

```powershell
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true
```

Dizininizin boyutuna bağlı olarak (kullanıcıların, grupların vb. sayısı), kimlik bilgisi karmalarının Azure AD ile eşitlenmesi zaman alır. Kimlik bilgisi karmalarının Azure AD'ye eşitlenmesinden kısa bir süre sonra, parolalar Azure AD Domain Services tarafından yönetilen etki alanında kullanılabilir olur.

## <a name="related-content"></a>İlgili İçerik
* [Sadece bulutta yer alan Azure AD dizini için AAD Domain Services’a parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Windows sanal makinesini Azure AD Domain Services tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Red Hat Enterprise Linux sanal makinesini Azure AD Domain Services tarafından yönetilen bir etki alanına ekleme](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
