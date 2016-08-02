<properties
    pageTitle="Azure AD Etki Alanı Hizmetleri: Parola eşitlemeyi etkinleştirme | Microsoft Azure"
    description="Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Azure AD Etki Alanı Hizmetleri *(Önizleme)* - Azure AD Etki Alanı Hizmetleri için parola eşitlemeyi etkinleştirme

## Görev 5: Eşitlenmiş Azure AD kiracısı için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme
Azure AD dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra, sıradaki görev Azure AD Etki Alanı Hizmetleri'ne parolaların eşitlenmesini etkinleştirmektir. Bu işlem, kullanıcıların etki alanında kurumsal kimlik bilgilerini kullanarak oturum açmalarını sağlar.

Uygulanan adımlar, kuruluşunuzun yalnızca bulutta yer alan bir Azure AD dizinine sahip olmasına veya Azure AD Connect yoluyla şirket içi dizininizle eşitlenmek üzere ayarlanmış olmasına göre değişiklik gösterir.

<br>

> [AZURE.SELECTOR]
- [Yalnızca bulutta yer alan Azure AD dizini](active-directory-ds-getting-started-password-sync.md)
- [Eşitlenmiş Azure AD dizini](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Eşitlenmiş kiracılar - NTLM ve Kerberos kimlik bilgisi karmalarının Azure AD'ye eşitlenmesini etkinleştirme
Kuruluşunuzun Azure AD kiracısı Azure AD Connect yoluyla şirket içi dizininizle eşitlenmek üzere ayarlanmışsa Azure AD Connect'i yapılandırarak NTLM ve Kerberos kimlik doğrulaması için gerekli olan kimlik bilgisi karmalarını eşitlemeniz gerekir. Bu karmalar varsayılan olarak Azure AD ile eşitlenmemiştir, aşağıdaki adımlar bu karmaların Azure AD kiracınızla eşitlenmesini etkinleştirmenize yardımcı olur.

#### Azure AD Connect'i yükleme veya güncelleştirme 

Azure AD Connect'in en son önerilen sürümünü etki alanına katılan bir bilgisayara yüklemeniz gerekir. Azure AD Connect kurulumunun var olan bir örneğine sahipseniz Azure AD Connect GA yapısını kullanmak için bu örneği güncelleştirmeniz gerekir. Düzeltilmiş olabilecek hataları/bilinen sorunları önlemek için Azure AD Connect'in en son sürümünü kullandığınızdan emin olun.

**[Azure AD Connect'i indirme](http://www.microsoft.com/download/details.aspx?id=47594)**

Önerilen sürüm: **1.1.130.0** - 12 Nisan, 2016'da yayımlanmıştır.

  > [AZURE.WARNING] Eski parola kimlik bilgilerini (NTLM ve Kerberos kimlik doğrulaması için gereklidir) Azure AD kiracınıza eşitlemek için Azure AD Connect'in en son önerilen sürümünü yüklemeniz GEREKİR. Bu işlev, Azure AD Connect'in önceki sürümlerinde veya eski DirSync aracında kullanılamaz.

Azure AD Connect'i yükleme talimatları şu makalede bulunabilir - [Azure AD Connect ile çalışmaya başlama](../active-directory/active-directory-aadconnect.md)


#### Azure AD için tam parola eşitlemesini zorlama

Tam parola eşitlemesini zorlamak ve tüm şirket içi kullanıcıların parola karmalarını (NTLM/Kerberos kimlik doğrulaması için gerekli olan kimlik bilgisi karmaları dahil olmak üzere) Azure AD kiracınıza eşitlemek için her AD ağacında aşağıdaki PowerShell betiğini yürütün.

```
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

Dizininizin boyutuna bağlı olarak (kullanıcıların, grupların vb. sayısı) kimlik bilgilerinin Azure AD'ye eşitlenmesi zaman alır. Kimlik bilgisi karmalarının Azure AD'ye eşitlenmesinden kısa bir süre sonra, parolalar Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanında kullanılabilir olur.


<br>

## İlgili İçerik

- [Sadece bulutta yer alan Azure AD dizini için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)

- [Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)

- [Windows sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)

- [Red Hat Enterprise Linux sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!---HONumber=Jun16_HO2-->


