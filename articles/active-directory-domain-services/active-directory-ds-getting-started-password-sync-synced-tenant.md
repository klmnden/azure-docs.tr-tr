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
    ms.date="09/20/2016"
    ms.author="maheshu"/>


# Azure AD Domain Services için parola eşitlemeyi etkinleştirme
Önceki görevlerde Azure AD kiracınız için Azure AD Etki Alanı Hizmetleri’ni etkinleştirdiniz. Bir sonraki göreviniz, Azure AD Domain Services'de parola eşitlemeyi etkinleştirmektir. Kimlik bilgisi eşitlemesi ayarlandıktan sonra kullanıcılar, şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilir.

Uygulanan adımlar, kuruluşunuzun yalnızca bulutta yer alan bir Azure AD kiracısına sahip olmasına veya Azure AD Connect yoluyla şirket içi dizininizle eşitlenmek üzere ayarlanmış olmasına göre değişiklik gösterir.

<br>

> [AZURE.SELECTOR]
- [Yalnızca bulutta yer alan Azure AD kiracısı](active-directory-ds-getting-started-password-sync.md)
- [Eşitlenmiş Azure AD kiracısı](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## Görev 5: Eşitlenmiş Azure AD kiracısı için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme
Eşitlenen bir Azure AD kiracısı, Azure AD Connect kullanarak kuruluşunuzun şirket içi dizini ile eşitlenecek şekilde ayarlanır. Azure AD Connect, varsayılan olarak NTLM ve Kerberos kimlik bilgisi karmalarını Azure AD ile eşitlemez. Azure AD Etki Alanı Hizmetleri’ni kullanmak için, Azure AD Connect’i NTLM ve Kerberos kimlik doğrulamasında gereken kimlik bilgisi karmalarını eşitleyecek şekilde yapılandırmanız gerekir. Aşağıdaki adımlar, gerekli kimlik bilgisi karmalarının Azure AD kiracınız ile eşitlenmesini etkinleştirir.


### Azure AD Connect'i yükleme veya güncelleştirme 
Etki alanına katılan bir bilgisayara, Azure AD Connect'in önerilen en son sürümünü yükleyin. Azure AD Connect kurulumunun var olan bir örneğine sahipseniz, bu örneği Azure AD Connect’in en yeni sürümünü kullanacak şekilde güncelleştirmelisiniz. Düzeltilmiş olabilecek bilinen sorunlardan/hatalardan kaçınmak için her zaman Azure AD Connect'in en yeni sürümünü kullandığınızdan emin olun.

**[Azure AD Connect'i indirme](http://www.microsoft.com/download/details.aspx?id=47594)**

Önerilen sürüm: **1.1.281.0** - 7 Eylül 2016'da yayımlanmıştır.

  > [AZURE.WARNING] Eski parola kimlik bilgilerinin (NTLM ve Kerberos kimlik doğrulaması için gereklidir), Azure AD kiracınız ile eşitlenebilmesini sağlamak için Azure AD Connect'in en yeni önerilen sürümünü yüklemeniz GEREKİR. Bu işlev, Azure AD Connect'in önceki sürümlerinde veya eski DirSync aracında kullanılamaz.

Azure AD Connect'i yükleme talimatları şu makalede bulunabilir - [Azure AD Connect ile çalışmaya başlama](../active-directory/active-directory-aadconnect.md)


### NTLM ve Kerberos kimlik bilgisi karmalarını Azure AD'ye eşitlemeyi etkinleştirme
Tam parola eşitlemesini zorlamak ve tüm şirket içi kullanıcılara ait kimlik bilgisi karmalarının Azure AD kiracınız ile eşitlenmesini sağlamak için her AD ormanında aşağıdaki PowerShell betiğini yürütün. Bu betik, NTLM/Kerberos kimlik doğrulaması için gerekli olan kimlik bilgisi karmalarının Azure AD kiracınız ile eşitlenmesini sağlar.

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

Dizininizin boyutuna bağlı olarak (kullanıcıların, grupların vb. sayısı), kimlik bilgisi karmalarının Azure AD ile eşitlenmesi zaman alır. Kimlik bilgisi karmalarının Azure AD'ye eşitlenmesinden kısa bir süre sonra, parolalar Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanında kullanılabilir olur.


<br>

## İlgili İçerik

- [Sadece bulutta yer alan Azure AD dizini için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)

- [Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)

- [Windows sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)

- [Red Hat Enterprise Linux sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!--HONumber=Sep16_HO3-->


