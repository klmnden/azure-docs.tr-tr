---
title: 'Azure AD Connect: AD DS bağlayıcı hesap izinlerini yapılandırma | Microsoft Docs'
description: Bu belgede yeni ADSyncConfig PowerShell modülü ile AD DS bağlayıcı hesabı yapılandırma ayrıntıları
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/14/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6911b19c680c2fdb8c372347c4dd0fca60bb0e0b
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60007568"
---
# <a name="azure-ad-connectconfigure-ad-ds-connector-account-permissions"></a>Azure AD Connect: AD DS bağlayıcı hesap izinlerini yapılandırma 

PowerShell modülü adlı [ADSyncConfig.psm1](reference-connect-adsyncconfig.md) cmdlet'leri Azure AD'niz için doğru Active Directory izinlerini yapılandırmanıza yardımcı olması için bir koleksiyon içerir (Ağustos 2018'de yayımlanan) 1.1.880.0 yapı ile kullanılmaya başlandı Dağıtım bağlanın. 

## <a name="overview"></a>Genel Bakış 
Aşağıdaki PowerShell cmdlet'lerini, Azure AD Connect'e bağlanan etkinleştirmek için seçtiğiniz her bir özellik için AD DS bağlayıcı hesabının, Kurulum Active Directory izinlerini kullanılabilir. Ormanınıza bağlanmak için bir özel etki alanı hesabı kullanarak Azure AD Connect'i yüklemek istediğiniz her sorunları önlemek için Active Directory izinlerini önceden hazırlamanız. Bu ADSyncConfig modülü, Azure AD Connect dağıtıldıktan sonra izinlerini yapılandırmak için de kullanılabilir.

![](media/how-to-connect-configure-ad-ds-connector-account/configure1.png)

İzinleri engellemedikçe bu ADSyncConfig modülü kullanın gerekmez. Bu nedenle Azure AD Connect Express yüklemesi için otomatik olarak oluşturulan bir hesap (MSOL_nnnnnnnnnn) Active Directory'de tüm gerekli izinlerle oluşturulur Azure AD ile eşitlemek istediğiniz belirli Active Directory nesneleri veya kuruluş birimleri üzerinde devralma. 
 
### <a name="permissions-summary"></a>İzin özeti 
Aşağıdaki tabloda, AD nesneler üzerinde gerekli izinleri bir özetini sunar: 

| Özellik | İzinler |
| --- | --- |
| MS-DS-ConsistencyGuid özelliği |Belirtilmiştir ms-DS-ConsistencyGuid özniteliği için yazma izinleri [tasarım kavramları - ms-DS-Consistencyguid'i sourceAnchor olarak kullanma](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). | 
| Parola Karması eşitleme |<li>Dizin Değişikliklerini Çoğaltma</li>  <li>Çoğaltma Directory yapılan tüm değişiklikler |
| Exchange karma dağıtımı |Özniteliklere açıklandığı yazma izinleri [Exchange karma geri yazma](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) kullanıcıları, grupları ve kişileri için. |
| Exchange posta ortak klasör |İçinde belirtilen öznitelikler için Okuma izinleri [Exchange posta ortak klasör](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder) ortak klasörleri için. | 
| Parola geri yazma |Özniteliklere açıklandığı yazma izinleri [parola yönetimine Başlarken](../authentication/howto-sspr-writeback.md) kullanıcılar için. |
| Cihaz geri yazma |Cihaz nesneleri ve kapsayıcıları belirtilmiştir yazma izinleri [cihaz geri yazmayı](how-to-connect-device-writeback.md). |
| Grup geri yazma |Okuma, oluşturma, güncelleştirme ve silme grubu eşitlenmesi için nesneleri **Office 365 grupları**.  Daha fazla bilgi için [grup geri yazma](how-to-connect-preview.md#group-writeback).|

## <a name="using-the-adsyncconfig-powershell-module"></a>ADSyncConfig PowerShell modülünü kullanma 
ADSyncConfig modülü gerektirir [Uzak Sunucu Yönetim Araçları (RSAT) AD DS için](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) beri AD DS PowerShell modülü ve araçları bağlıdır. RSAT için AD DS'yi yüklemek için 'Yönetici olarak çalıştır' ile bir Windows PowerShell penceresi açın ve yürütün: 

``` powershell
Install-WindowsFeature RSAT-AD-Tools 
```
![Yapılandırma](media/how-to-connect-configure-ad-ds-connector-account/configure2.png)

>[!NOTE]
>Ayrıca, dosyayı kopyalayabilirsiniz **C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\ADSyncConfig.psm1** AD DS yüklü ve bu PowerShell modülünü buradan kullanmak için RSAT zaten sahip olan bir etki alanı denetleyicisi.

ADSyncConfig kullanmaya başlamak için bir Windows PowerShell penceresinde modülü yüklemeniz gerekir: 

``` powershell
Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1" 
```

Tüm kontrol etmek için bu modülündeki cmdlet'ler şunu yazabilirsiniz:  

``` powershell
Get-Command -Module AdSyncConfig  
```

![İşaretli](media/how-to-connect-configure-ad-ds-connector-account/configure3.png)

Her cmdlet, AD DS bağlayıcı hesabı ve bir AdminSDHolder anahtarı girmek için aynı parametrelere sahip. AD DS bağlayıcı hesabınızın belirtmek için hesap adı ve etki alanı veya hesap sağlayabilir ayırt edici ad (DN),

Örneğin:

```powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <ADAccountName> -ADConnectorAccountDomain <ADDomainName>
```

veya;

```powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <ADAccountDN>
```

Değiştirdiğinizden emin olun `<ADAccountName>`, `<ADDomainName>` ve `<ADAccountDN>` ortamınız için uygun değerleri.

AdminSDHolder kapsayıcı izinlerini değiştirmek istemediğiniz durumlarda anahtarını kullanın `-SkipAdminSdHolders`. 

Varsayılan olarak, tüm küme izinleri cmdlet'leri PowerShell oturumu çalıştıran kullanıcının ormandaki her etki alanında etki alanı yönetici hakları gerekir yani ormandaki her etki alanı kökünde AD DS izinleri dener.  Bu gereksinimden dolayı Kurumsal Yönetici ormanın kök kullanmak için önerilir. Azure AD Connect Dağıtımınızda birden çok AD DS bağlayıcı varsa, bir AD DS bağlayıcı sahip her ormanda aynı cmdlet'i çalıştırmak için gerekir. 

Parametresini kullanarak belirli bir OU veya AD DS nesnesi izinleri ayarlayabilirsiniz `-ADobjectDN` izinlerini ayarlamak istediğiniz hedef nesnenin DN tarafından izlenen. Bir hedef ADobjectDN kullanırken, cmdlet izinler bu nesne üzerinde AdminSDHolder kapsayıcı ve etki alanı kökü değil, yalnızca ve ayarlar. Bazı OU'lar varsa veya AD DS izin devralma işlemi (izin devralma işlemi devre dışı nesneleriyle bakın bulun AD DS) devre dışı olan nesneler, bu parametre yararlı olabilir 

Bu ortak parametreleri istisnaları `Set-ADSyncRestrictedPermissions` kendisi, AD DS bağlayıcı hesapta izinleri ayarlamak için kullanılan cmdlet ve `Set-ADSyncPasswordHashSyncPermissions` parola karması eşitleme için gerekli izinleri yalnızca etki alanı kökünde, bu nedenle ayarlandığı cmdlet'i Bu cmdlet içermemesi `-ObjectDN` veya `-SkipAdminSdHolders` parametreleri.

### <a name="determine-your-ad-ds-connector-account"></a>AD DS Bağlayıcınızı belirlemek hesabı 
Azure AD Connect zaten yüklüyse ve ne AD DS bağlayıcı hesabı Azure AD Connect tarafından kullanılmakta olduğunu kontrol etmek istediğiniz durumlarda cmdlet'ini çalıştırabilirsiniz: 

``` powershell
Get-ADSyncADConnectorAccount 
```
### <a name="locate-ad-ds-objects-with-permission-inheritance-disabled"></a>AD DS nesnelerini izin devralmayı devre dışı bulun 
Herhangi bir AD DS nesnesi ile izin devralma işlemi devre dışı olup olmadığı denetlenecek istemeniz durumunda, çalıştırabilirsiniz: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' 
```
Varsayılan olarak, bu cmdlet'i yalnızca OU'lar için devre dışı kalıtım görünür ancak diğer AD DS nesnesi sınıflarda belirtebilirsiniz `-ObjectClass` parametresi veya Kullan ' *' gibi tüm sınıflar, nesne için: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' -ObjectClass * 
```
 
### <a name="view-ad-ds-permissions-of-an-object"></a>Bir nesnenin AD DS izinlerini görüntüleme 
Kendi DistinguishedName sağlayarak, şu anda bir Active Directory nesne üzerinde izinlere listesini görüntülemek için aşağıdaki cmdlet'i kullanabilirsiniz: 

``` powershell
Show-ADSyncADObjectPermissions -ADobjectDN '<DistinguishedName>' 
```

## <a name="configure-ad-ds-connector-account-permissions"></a>AD DS bağlayıcı hesap izinlerini yapılandırma 
 
### <a name="configure-basic-read-only-permissions"></a>Basit salt okunur izinlerini yapılandırma 
Herhangi bir Azure AD Connect özelliği kullanmadığınız durumlarda AD DS bağlayıcı hesabı için temel salt okunur izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```


Bu cmdlet şu izinler ayarlanır: 
 

|Type |Name |Access |Şunun İçin Geçerli| 
|-----|-----|-----|-----|
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt cihaz nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı|Tüm özellikleri oku |Alt InetOrgPerson nesnelerine| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt bilgisayar nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt foreignSecurityPrincipal nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt grup nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt kullanıcı nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt kişi nesneler| 

 
### <a name="configure-ms-ds-consistency-guid-permissions"></a>MS-DS-tutarlılık-GUID izinlerini yapılandırma 
Ms-Ds-tutarlılık-Guid özniteliği kaynak bağlantısı (diğer adıyla "Kaynak bağlantısını benim için Azure yönetsin" seçeneği) kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```

veya; 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver|AD DS bağlayıcı hesabı|Okuma/yazma özelliği|Alt kullanıcı nesneleri|

### <a name="permissions-for-password-hash-synchronization"></a>Parola Karması eşitleme için izinleri 
Parola Karması eşitleme kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [<CommonParameters>] 
```

Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |AD DS bağlayıcı hesabı |Dizin Değişikliklerini Çoğaltma |Yalnızca bu nesne (etki alanı kökü)| 
|İzin Ver |AD DS bağlayıcı hesabı |Dizini çoğaltmak yapılan tüm değişiklikler |Yalnızca bu nesne (etki alanı kökü)| 
  
### <a name="permissions-for-password-writeback"></a>Parola geri yazma izinleri 
Parola geri yazma özelliğini kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya;

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |AD DS bağlayıcı hesabı |Parola Sıfırlama |Alt kullanıcı nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Özellik lockoutTime yazma |Alt kullanıcı nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Özellik pwdLastSet yazma |Alt kullanıcı nesneleri| 

### <a name="permissions-for-group-writeback"></a>Grup geri yazma izinleri 
Grup geri yazma kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```
veya; 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>]
```
 
Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |AD DS bağlayıcı hesabı |Genel okuma/yazma |Nesne türü grubunu ve alt nesnelerinin tüm öznitelikleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Alt nesne oluşturma/silme |Nesne türü grubunu ve alt nesnelerinin tüm öznitelikleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Ağaç nesneleri silme/silme|Nesne türü grubunu ve alt nesnelerinin tüm öznitelikleri|

### <a name="permissions-for-exchange-hybrid-deployment"></a>Exchange karma dağıtımı için izinleri 
Exchange karma dağıtımı kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Bu cmdlet şu izinler ayarlanır:  
 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |AD DS bağlayıcı hesabı |Okuma/yazma tüm özellikleri |Alt kullanıcı nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Okuma/yazma tüm özellikleri |Alt InetOrgPerson nesnelerine| 
|İzin Ver |AD DS bağlayıcı hesabı |Okuma/yazma tüm özellikleri |Alt grup nesneleri| 
|İzin Ver |AD DS bağlayıcı hesabı |Okuma/yazma tüm özellikleri |Alt kişi nesneler| 

### <a name="permissions-for-exchange-mail-public-folders-preview"></a>Exchange posta ortak klasörleri (Önizleme) için izinleri 
Exchange posta ortak klasörleri özelliğini kullanırken AD DS bağlayıcı hesabının izinlerini ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |AD DS bağlayıcı hesabı |Tüm özellikleri oku |Alt PublicFolder nesneler| 

### <a name="restrict-permissions-on-the-ad-ds-connector-account"></a>AD DS bağlayıcı izinleri kısıtla hesabı 
Bu PowerShell Betiği, bir parametre olarak sağlanan AD Bağlayıcısı hesabı için izinler sıkılaştıran. İzinleri sıkılaştırma aşağıdaki adımları içerir: 

- Belirtilen nesnenin devralma devre dışı bırak 
- Kendi KENDİNE söz konusu olduğunda, varsayılan izinleri korumak istediğimiz için kendi KENDİNE özgü ACE dışında belirli nesne üzerindeki tüm ACE kaldırın. 
 
  -ADConnectorAccountDN izinlerini sıkılaştırıldığını gereken AD hesabın parametredir. Bu genellikle, AD DS bağlayıcısında yapılandırılmış MSOL_nnnnnnnnnnnn etki alanı hesabı (AD DS bağlayıcı hesabınızın belirleme bakın). Credential parametresi, hedef AD nesne için Active Directory izinlerini kısıtlamak için gerekli ayrıcalıklara sahip bir yönetici hesabı belirtmeniz gerekir. Genellikle Kurumsal veya etki alanı yöneticisi budur.  

``` powershell
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential> [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>] 
```
 
Örneğin: 

``` powershell
$credential = Get-Credential 
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN'CN=ADConnectorAccount,CN=Users,DC=Contoso,DC=com' -Credential $credential  
```

Bu cmdlet şu izinler ayarlanır: 

|Type |Name |Access |Şunun İçin Geçerli|
|-----|-----|-----|-----| 
|İzin Ver |SİSTEM |Tam Denetim |Bu nesne 
|İzin Ver |Kuruluş Yöneticileri |Tam Denetim |Bu nesne 
|İzin Ver |Etki alanı yöneticileri |Tam Denetim |Bu nesne 
|İzin Ver |Yöneticiler |Tam Denetim |Bu nesne 
|İzin Ver |Kuruluş etki alanı denetleyicileri |İçeriği Listele |Bu nesne 
|İzin Ver |Kuruluş etki alanı denetleyicileri |Tüm özellikleri oku |Bu nesne 
|İzin Ver |Kuruluş etki alanı denetleyicileri |Okuma izinleri |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |İçeriği Listele |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |Tüm özellikleri oku |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |Okuma izinleri |Bu nesne 

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect: Hesaplar ve izinler](reference-connect-accounts-permissions.md)
- [Hızlı yükleme](how-to-connect-install-express.md)
- [Özel yükleme](how-to-connect-install-custom.md)
- [ADSyncConfig başvurusu](reference-connect-adsyncconfig.md)

