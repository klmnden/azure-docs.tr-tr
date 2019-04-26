---
title: 'Azure AD Connect: ADSyncTools PowerShell başvurusu | Microsoft Docs'
description: Bu belge ADSyncTools.psm1 PowerShell modülü için başvuru bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
origin.date: 10/19/2018
ms.date: 03/15/2019
ms.subservice: hybrid
ms.author: v-junlch
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a1b8abf15233c06e8ff9e507b315cc8a3703970
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60454668"
---
# <a name="azure-ad-connect--adsynctools-powershell-reference"></a>Azure AD Connect:  ADSyncTools PowerShell başvurusu
Aşağıdaki belgeler Azure AD Connect ile birlikte sağlanan ADSyncTools.psm1 PowerShell modülü için başvuru bilgileri sağlar.

## <a name="clear-adsynctoolsconsistencyguid"></a>ADSyncToolsConsistencyGuid Temizle

### <a name="synopsis"></a>ÖZET
MS-Ds-consistencyguid içinde AD kullanıcıdan Temizle

### <a name="syntax"></a>SÖZ DİZİMİ

```
Clear-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Hedef AD kullanıcı için mS-Ds-ConsistencyGuid değeri Temizle

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Hedef kullanıcı kümesi için ad

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-adsynctoolsadmoduleloaded"></a>ADSyncToolsADModuleLoaded onaylayın

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ADSyncToolsADModuleLoaded
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

## <a name="connect-adsyncdatabase"></a>Connect-AdSyncDatabase

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Connect-AdSyncDatabase [-Server] <String> [[-Instance] <String>] [[-Database] <String>] [[-UserName] <String>]
 [[-Password] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-database"></a>-Veritabanı
{{Veritabanı açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-instance"></a>-Örneği
{{Örneği açıklaması dolgu}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-password"></a>-Password
{{Parola açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
{{Sunucusu açıklaması dolgu}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName
{{Kullanıcıadı açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="export-adsynctoolsconsistencyguidmigration"></a>Dışarı aktarma ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>ÖZET
Consistencyguid içinde raporu dışarı aktarın

### <a name="syntax"></a>SÖZ DİZİMİ

```
Export-ADSyncToolsConsistencyGuidMigration [-AlternativeLoginId] [-UserPrincipalName] <String>
 [-ImmutableIdGUID] <String> [-Output] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bir içeri aktarma CSV dosyasından içeri aktarma ADSyncToolsImmutableIdMigration göre consistencyguid içinde bir rapor oluşturur

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-Csv .\AllSyncUsers.csv | Export-ADSyncToolsConsistencyGuidMigration -Output ".\AllSyncUsers-Report"
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-alternativeloginid"></a>-AlternativeLoginId
Alternatif oturum açma kimliği (posta) kullanın

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

#### <a name="-userprincipalname"></a>-UserPrincipalName
userPrincipalName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Çıkış
CSV ve günlük dosyaları için çıkış dosyası adı

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsyncsqlbrowserinstances"></a>Get-ADSyncSQLBrowserInstances

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncSQLBrowserInstances [[-hostName] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Ana bilgisayar adı, açıklamayı doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="get-adsynctoolsaduser"></a>Get-ADSyncToolsADuser

### <a name="synopsis"></a>ÖZET
AD'den kullanıcı alma

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsADuser [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bir AD nesne yapılacak döndürür: Çoklu orman desteği

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Hedef kullanıcı consistencyguid içinde ayarlamak için ad

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsconsistencyguid"></a>Get-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>ÖZET
MS-Ds-consistencyguid içinde AD kullanıcıdan Al

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
MS-Ds-ConsistencyGuid özniteliği hedef AD kullanıcının GUID biçiminde bulunan değeri döndürür

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Hedef kullanıcı kümesi için ad

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsobjectguid"></a>Get-ADSyncToolsObjectGuid

### <a name="synopsis"></a>ÖZET
ObjectGUID AD kullanıcıdan Al

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsObjectGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
ObjectGUID özniteliği GUID biçiminde hedef AD kullanıcının bulunan değeri döndürür

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Hedef kullanıcı kümesi için ad

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsrunhistory"></a>Get-ADSyncToolsRunHistory

### <a name="synopsis"></a>ÖZET
Get AAD Connect çalıştırma geçmişi

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsRunHistory [[-Days] <Int32>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
AAD Connect çalıştırma geçmişi XML biçiminde döndüren bir işlev

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Get-ADSyncToolsRunHistory
```

#### <a name="example-2"></a>ÖRNEK 2
```
Get-ADSyncToolsRunHistory -Days 1
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-days"></a>-Gün
{{Gün dolgu açıklama}}

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 3
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolssourceanchorchanged"></a>Get-ADSyncToolsSourceAnchorChanged

### <a name="synopsis"></a>ÖZET
Kullanıcılar değiştirilmiş SourceAnchor hatalarla Al

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsSourceAnchorChanged [-sourcePath] <Object> [-outputPath] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
AAD Connect çalıştırma geçmişi sorguları işlev ve hata raporlama tüm kullanıcıları dışarı aktarır: "SourceAnchor özniteliği değişti."

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
#Required Parameters
```

$sourcePath = Read-Host - komut istemi "girin, günlük dosyası yolu dosya adıyla" #"\<Source_Path\>" $outputPath = Read-Host-Prompt ", çıkış girin dosya adına sahip bir dosya yolu" #"\<Out_Path\>"
 
 Get-ADSyncToolsUsersSourceAnchorChanged - sourcePath $sourcePath - outputPath $outputPath

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-sourcepath"></a>-sourcePath
{{SourcePath açıklamasını doldurun}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-outputpath"></a>-outputPath
{{OutputPath açıklamasını doldurun}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="import-adsynctoolsimmutableidmigration"></a>İçeri aktarma ADSyncToolsImmutableIdMigration

### <a name="synopsis"></a>ÖZET
AAD'den Immutableıd alma

### <a name="syntax"></a>SÖZ DİZİMİ

```
Import-ADSyncToolsImmutableIdMigration [-Output] <String> [-IncludeSyncUsersFromRecycleBin]
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Tüm Azure AD Synchronızed kullanıcılarla GUID biçiminde gereksinimleri Immutableıd değerini içeren bir dosya oluşturur: MSOnline PowerShell Modülü

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-ADSyncToolsImmutableIdMigration -OutputFile '.\AllSyncUsers.csv'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-output"></a>-Çıkış
Çıkış CSV dosyası

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

#### <a name="-includesyncusersfromrecyclebin"></a>-IncludeSyncUsersFromRecycleBin
Eşitleneceğini kullanıcıları Azure ad geri dönüşüm kutusu

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

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


## <a name="invoke-adsyncdatabasequery"></a>Invoke-AdSyncDatabaseQuery

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Invoke-AdSyncDatabaseQuery [-SqlConnection] <SqlConnection> [[-Query] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-query"></a>-Sorgu
{{Sorgu açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-sqlconnection"></a>-SqlConnection
{{Fill SqlConnection Description}}

```yaml
Type: SqlConnection
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="remove-adsynctoolsexpiredcertificates"></a>Remove-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>ÖZET
Süresi dolmuş sertifikaları UserCertificate özniteliğinden kaldırmak için komut dosyası

### <a name="syntax"></a>SÖZ DİZİMİ

```
Remove-ADSyncToolsExpiredCertificates [-TargetOU] <String> [[-BackupOnly] <Boolean>] [-ObjectClass] <String>
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bu betik, nesne sınıfı (kullanıcı/bilgisayar) göre filtrelenmiş bir Active Directory etki alanınızdaki - kuruluş birimi hedef tüm nesneleri alır ve UserCertificate özniteliğinde mevcut tüm süresi dolan sertifikaları siler.
Varsayılan olarak (BackupOnly modu), yalnızca yedekleyecektir süresi dolan sertifikaları bir dosyaya ve herhangi bir değişiklik AD'de yapın.
-BackupOnly $false kullandığınız sonra bu nesneleri UserCertificate özniteliğinde mevcut tüm süresi sertifika kopyalanan sonra AD kaldırılacak. dosya için.
Her sertifika için ayrı bir dosya adı yedeklenir: ObjectClass_ObjectGUID_CertThumprint.cer komut dosyası, ayrıca ya da geçersiz veya süresi dolmuş gerçekleştirilen gerçek eylem dahil olmak üzere olan sertifikalar ile tüm kullanıcıları gösteren CSV biçiminde (dışarı aktarılan/atlandı/silinen) bir günlük dosyası oluşturulur.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Check all users in target OU - Expired Certificates will be copied to separated files and no certificates will be removed
```

Remove-ADSyncToolsExpiredCertificates-{TargetOU "OU Kullanıcılar, OU = Corp, DC = Contoso, DC = com" - ObjectClass kullanıcı

#### <a name="example-2"></a>ÖRNEK 2
```
Delete Expired Certs from all Computer objects in target OU - Expired Certificates will be copied to files and removed from AD
```

Remove-ADSyncToolsExpiredCertificates-{TargetOU "OU = bilgisayarlar, OU = Corp, DC = Contoso, DC = com" - ObjectClass bilgisayar - BackupOnly $false

### <a name="parameters"></a>PARAMETRELER

#### <a name="-targetou"></a>-TargetOU
Arama için AD nesnelerini OU hedefine

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

#### <a name="-backuponly"></a>-BackupOnly
AD'den BackupOnly sertifikalarını silmez

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Nesne sınıfı filtresi

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="repair-adsynctoolsautoupgradestate"></a>Onarım ADSyncToolsAutoUpgradeState

### <a name="synopsis"></a>ÖZET
Kısa açıklama

### <a name="syntax"></a>SÖZ DİZİMİ

```
Repair-ADSyncToolsAutoUpgradeState
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

## <a name="resolve-adsynchostaddress"></a>Resolve-ADSyncHostAddress

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Resolve-ADSyncHostAddress [[-hostName] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Ana bilgisayar adı, açıklamayı doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="restore-adsynctoolsexpiredcertificates"></a>Geri yükleme-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>ÖZET
(YAPMAK İÇİN) AD, UserCertificate özniteliğinin yol açtığı alınan sertifika dosyası geri yükler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Restore-ADSyncToolsExpiredCertificates
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

## <a name="set-adsynctoolsconsistencyguid"></a>Set-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>ÖZET
MS-Ds-consistencyguid içinde AD Kullanıcı Ayarla

### <a name="syntax"></a>SÖZ DİZİMİ

```
Set-ADSyncToolsConsistencyGuid [-User] <Object> [-Value] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
MS-Ds-ConsistencyGuid özniteliği ' % s'hedef AD kullanıcısı için bir değer kümesindeki

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Hedef kullanıcı consistencyguid içinde ayarlamak için ad

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-value"></a>-Değer
Immutableıd (bayt dizisi, GUID, GUID veya Base64 dize)

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="test-adsyncnetworkport"></a>Test-ADSyncNetworkPort

### <a name="synopsis"></a>ÖZET
{{İçinde doğrulanır dolgu}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Test-ADSyncNetworkPort [[-hostName] <String>] [[-port] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Bir açıklama girin}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Örnek açıklamayı buraya ekleyin}}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Ana bilgisayar adı, açıklamayı doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-bağlantı noktası
{{Bağlantı noktası açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="trace-adsynctoolsadimport"></a>İzleme ADSyncToolsADImport

### <a name="synopsis"></a>ÖZET
Bir izleme dosyasından ve AD alma adımı oluşturur

### <a name="syntax"></a>SÖZ DİZİMİ

```
Trace-ADSyncToolsADImport [[-ADConnectorXML] <String>] [[-dc] <String>] [[-rootDN] <String>]
 [[-filter] <String>] [-SkipCredentials] [[-ADwatermark] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
İzlemeleri AAD Connect AD içeri aktarma tüm ldap sorguları bir verilen AD Filigran denetim noktasından (bölüm tanımlama bilgisi) çalıştırın. Bir izleme dosyası '.\ADimportTrace_yyyyMMddHHmmss.log' üzerinde geçerli bir klasör oluşturur.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectorxml"></a>-ADConnectorXML
{{ADConnectorXML açıklamasını doldurun}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dc"></a>-dc
XML dosyasının AD bağlayıcı dışarı aktarma

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: DC1.domain.contoso.com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-rootdn"></a>-rootDN
Hedef etki alanı denetleyicisi

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: DC=Domain,DC=Contoso,DC=com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-Filtre
Orman kök DN

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: (&(objectClass=*))
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipcredentials"></a>-SkipCredentials
İzleme için AD nesne türlerini \> * = tüm nesne türleri

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

#### <a name="-adwatermark"></a>-ADwatermark
Etki alanı yöneticisi olarak zaten çalışıyorsa AD kimlik bilgilerini sağlamaya gerek yoktur.
Filigran, XML yerine, el ile giriş dosyası örn $ADwatermark "TVNEUwMAAAAXyK9ir1zSAQAAAAAAAAAA(...)" =

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 5
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="trace-adsynctoolsldapquery"></a>İzleme ADSyncToolsLdapQuery

### <a name="synopsis"></a>ÖZET
Kısa açıklama

### <a name="syntax"></a>SÖZ DİZİMİ

```
Trace-ADSyncToolsLdapQuery [-Context] <String> [-Server] <String> [-Port] <Int32> [-Filter] <String>
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-context"></a>-İçerik
Param1 Yardım açıklaması

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

#### <a name="-server"></a>-Server
Param2 Yardım açıklaması

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: Localhost
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-Bağlantı noktası
Param2 Yardım açıklaması

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: 389
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-Filter
Param2 Yardım açıklaması

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: (objectClass=*)
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi için bkz. about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="update-adsynctoolsconsistencyguidmigration"></a>Update-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>ÖZET
Güncelleştirmeleri kullanıcılarla yeni consistencyguid içinde (Immutableıd)

### <a name="syntax"></a>SÖZ DİZİMİ

```
Update-ADSyncToolsConsistencyGuidMigration [[-DistinguishedName] <String>] [-ImmutableIdGUID] <String>
 [-Action] <String> [-Output] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Kullanıcılar WhatIf anahtarı Not consistencyguid içinde rapor bu işlevi destekleyen alınan yeni consistencyguid içinde (Immutableıd) değeriyle güncelleştirir: Consistencyguid içinde rapor ile sekmesindeki ayırıcı alınması gerekir

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2 -WhatIf
```

#### <a name="example-2"></a>ÖRNEK 2
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-distinguishedname"></a>-DistinguishedName
DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: False
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-action"></a>-Eylem
Eylem

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Çıkış
Günlük dosyaları için çıkış dosyası adı

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
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

<!-- Update_Description: wording update -->