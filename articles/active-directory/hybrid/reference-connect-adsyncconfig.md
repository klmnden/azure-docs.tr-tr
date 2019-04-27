---
title: 'Azure AD Connect: ADSyncConfig PowerShell başvurusu | Microsoft Docs'
description: Bu belge ADSyncConfig.psm1 PowerShell modülü için başvuru bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 01/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 554bb99121190198982f64deb6ee0674aa8831ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381204"
---
# <a name="azure-ad-connect--adsyncconfig-powershell-reference"></a>Azure AD Connect:  ADSyncConfig PowerShell başvurusu
Aşağıdaki belgeler Azure AD Connect ile birlikte sağlanan ADSyncConfig.psm1 PowerShell modülü için başvuru bilgileri sağlar.


## <a name="get-adsyncadconnectoraccount"></a>Get-ADSyncADConnectorAccount

### <a name="synopsis"></a>ÖZET
Her AD bağlayıcısında yapılandırılmış etki alanı ve hesabı adını alır

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncADConnectorAccount
```

### <a name="description"></a>AÇIKLAMA
Bu işlev, AD Bağlayıcısı hesabını gösteren bir tablo bağlantı parametrelerini almak için AAD Connect mevcut olduğundan 'Get-ADSyncConnector' cmdlet kullanır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Get-ADSyncADConnectorAccount
```

## <a name="get-adsyncobjectswithinheritancedisabled"></a>Get-ADSyncObjectsWithInheritanceDisabled

### <a name="synopsis"></a>ÖZET
İzin devralma devre dışı AD nesnelerini alır

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncObjectsWithInheritanceDisabled [-SearchBase] <String> [[-ObjectClass] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
SearchBase parametresinden itibaren AD arar ve şu anda devre dışı ACL kalıtıma sahip ObjectClass parametresi tarafından filtrelenen tüm nesneleri döndürür.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Find objects with disabled inheritance in 'Contoso' domain (by default returns 'organizationalUnit' objects only)
```

Get-ADSyncObjectsWithInheritanceDisabled - SearchBase 'Contoso'

#### <a name="example-2"></a>ÖRNEK 2
```
Find 'user' objects with disabled inheritance in 'Contoso' domain
```

Get-ADSyncObjectsWithInheritanceDisabled - SearchBase 'Contoso' - ObjectClass 'user'

#### <a name="example-3"></a>ÖRNEK 3
```
Find all types of objects with disabled inheritance in a OU
```

Get-ADSyncObjectsWithInheritanceDisabled - SearchBase OU AzureAD, DC = Contoso, DC = com - ObjectClass = ' *'

### <a name="parameters"></a>PARAMETRELER

#### <a name="-searchbase"></a>-SearchBase
Bir AD etki alanı DistinguishedName ya da bir FQDN LDAP sorgusu için SearchBase

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Sınıfı, aranacak nesne olabilir ' *' (tüm nesne sınıfı için), 'user', 'group', 'kapsayıcısı' vb. Varsayılan olarak, bu işlev, 'organizationalUnit' nesne sınıfı için arama yapar.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: OrganizationalUnit
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncbasicreadpermissions"></a>Set-ADSyncBasicReadPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için temel Okuma izinleri başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncBasicReadPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Tüm alt bilgisayar nesneleri için tüm öznitelikler üzerinde okuma özellik erişimi
2.
Tüm alt cihaz nesneler için tüm öznitelikler üzerinde okuma özellik erişimi
3.
Tüm alt foreignsecurityprincipal nesneler için tüm öznitelikler üzerinde okuma özellik erişimi
5.
Tüm alt kullanıcı nesneleri için tüm öznitelikler üzerinde okuma özellik erişimi
6.
Tüm alt inetorgperson nesnelerine için tüm öznitelikleri okuma özellik erişimi
7.
Tüm alt grubu nesnelerinin tüm öznitelikler üzerinde okuma özellik erişimi
8.
Tüm öznitelikler kişi tüm alt nesneleri için okuma özelliği erişimi

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncexchangehybridpermissions"></a>Set-ADSyncExchangeHybridPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için Exchange karma özelliğini başlatır.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeHybridPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Tüm alt kullanıcı nesneleri için tüm öznitelikler üzerinde okuma/yazma özellik erişimi
2.
Tüm alt inetorgperson nesnelerine için tüm öznitelikler özelliği okuma/yazma erişimi
3.
Tüm alt grubu nesnelerinin tüm öznitelikler üzerinde okuma/yazma özellik erişimi
4.
Tüm öznitelikler kişi tüm alt nesneleri için okuma/yazma özelliği erişimi

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncexchangemailpublicfolderpermissions"></a>Set-ADSyncExchangeMailPublicFolderPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için Exchange posta ortak klasör özelliğini başlatır.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String>
 -ADConnectorAccountDomain <String> [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeMailPublicFolderPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Tüm alt publicfolder nesneler için tüm öznitelikler üzerinde okuma özellik erişimi

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncmsdsconsistencyguidpermissions"></a>Set-ADSyncMsDsConsistencyGuidPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için mS-DS-ConsistencyGuid özelliğini başlatır.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncMsDsConsistencyGuidPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Tüm alt kullanıcı nesneleri için mS-DS-ConsistencyGuid özniteliği özelliği okuma/yazma erişimi

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncpasswordhashsyncpermissions"></a>Set-ADSyncPasswordHashSyncPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için parola karması eşitlemeyi başlatır.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordHashSyncPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Dizin Değişikliklerini Çoğaltma
2.
Dizini çoğaltmak yapılan tüm değişiklikler

Bu izinler, ormandaki tüm etki alanları için verilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName directory içindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncpasswordwritebackpermissions"></a>Set-ADSyncPasswordWritebackPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için parola geri yazma özelliğini Azure AD'den başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordWritebackPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Alt kullanıcı nesneleri üzerinde parola sıfırlama
2.
Özellik erişimini lockoutTime özniteliği tüm alt kullanıcı nesneleri için yazma
3.
Özellik erişimini pwdLastSet özniteliği tüm alt kullanıcı nesneleri için yazma

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncrestrictedpermissions"></a>Set-ADSyncRestrictedPermissions

### <a name="synopsis"></a>ÖZET
Aksi takdirde herhangi bir AD korumalı güvenlik grubunda yer almayan bir AD nesne izinleri sıkılaştıran.
Genel bir örnek AAD Connect tarafından otomatik olarak oluşturulan AD Connect'i (MSOL) hesaptır.
Bu hesap tüm etki alanları üzerinde çoğaltma izinlerine sahip, korumalı gibi ancak kolayca tehlikeye girebilir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential>
 [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncRestrictedPermissions işlevi sağlanan hesabın izinleri paylaşımlarınızda sıkılaştıran.
İzinleri sıkılaştırma aşağıdaki adımları içerir:
1.
Belirtilen nesnenin devralma devre dışı bırak
2.
Kendi KENDİNE özgü ACE dışında belirli nesne üzerindeki tüm ACE kaldırın.
Kendi KENDİNE söz konusu olduğunda, varsayılan izinleri korumak istiyoruz.
3.
Bu özel izinler atayın:

        Type    Name                                        Access              Applies To
        =============================================================================================
        Allow   SYSTEM                                      Full Control        This object
        Allow   Enterprise Admins                           Full Control        This object
        Allow   Domain Admins                               Full Control        This object
        Allow   Administrators                              Full Control        This object

        Allow   Enterprise Domain Controllers               List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

        Allow   Authenticated Users                         List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN "CN=TestAccount1,CN=Users,DC=Contoso,DC=com" -Credential $(Get-Credential)
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName Active Directory hesabının izinlerini sıkılaştırıldığını gerekir.
Bu, genellikle MSOL_nnnnnnnnnn hesabın veya AD Bağlayıcınızı yapılandırılan özel etki alanı hesabı olacaktır.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credential"></a>-Credential
ADConnectorAccountDN hesap izinlerini kısıtlamak için gerekli ayrıcalıklara sahip yönetici kimlik bilgileri. Genellikle kuruluş veya etki alanı yönetici budur. Hesap arama hatalarını önlemek için yönetici hesabının tam etki alanı adını kullanın.
Örnek: CONTOSO\admin

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-disablecredentialvalidation"></a>-DisableCredentialValidation
DisableCredentialValidation kullanıldığında, işlev-sağlanan kimlik bilgileri, kimlik bilgisi AD'de geçerli ve sağlanan hesabı ADConnectorAccountDN hesap izinlerini kısıtlamak için gerekli ayrıcalıklara sahip olmadığını denetlemez.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncunifiedgroupwritebackpermissions"></a>Set-ADSyncUnifiedGroupWritebackPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanı ve etki alanı için Azure ad grup geri yazma'yı başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncUnifiedGroupWritebackPermissions işlevi şunlardır AD eşitleme hesabı için gerekli izinleri verecektir:
1.
Genel okuma/yazma, silme, ağaç silin ve tüm Create\Delete alt nesne türlerini ve alt nesnelerinin Grup

Bu izinleri, ormandaki tüm etki alanları için uygulanır.
İsteğe bağlı olarak, yalnızca (dahil devralma alt nesneleri için), AD nesnesi üzerinde bu izinleri ayarlamak için bir DistinguishedName ADobjectDN parametresi olarak sağlayabilirsiniz.
Bu durumda, ADobjectDN GroupWriteback özelliğiyle bağlamak istediğiniz kapsayıcının ayırt edici adı olacaktır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının adıdır.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabının etki alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName veya Azure AD Connect Sync tarafından directory içindeki nesneleri yönetmek için kullanılan Active Directory hesabı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
(İsteğe bağlı) izinlerini ayarlamak için hedef AD nesnenin DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcı bu izinlere sahip güncelleştirilmemelidir olmadığını belirtmek için isteğe bağlı parametre

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="show-adsyncadobjectpermissions"></a>ADSyncADObjectPermissions Göster

### <a name="synopsis"></a>ÖZET
Belirtilen bir AD nesne izinleri gösterilir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Show-ADSyncADObjectPermissions [-ADobjectDN] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bu işlev parametre sağlanan belirli bir AD nesne için ayarlanmış olan tüm AD izinleri döndürür - ADobjectDN.
ADobjectDN DistinguishedName biçiminde sağlanmalıdır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Show-ADSyncADObjectPermissions -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adobjectdn"></a>-ADobjectDN
{{ADobjectDN açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).
