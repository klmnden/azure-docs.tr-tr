---
title: 'Azure AD Connect: ADConnectivityTools PowerShell başvurusu | Microsoft Docs'
description: Bu belge ADConnectivityTools.psm1 PowerShell modülü için başvuru bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6b90ff82601acca1249c7d8c353944e39e89f95
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473792"
---
# <a name="azure-ad-connect--adconnectivitytools-powershell-reference"></a>Azure AD Connect:  ADConnectivityTools PowerShell başvurusu

Aşağıdaki belgeler Azure AD Connect ile birlikte sağlanan ADConnectivityTools.psm1 PowerShell modülü için başvuru bilgileri sağlar.

## <a name="confirm-dnsconnectivity"></a>DnsConnectivity onaylayın

### <a name="synopsis"></a>ÖZET

Yerel Dns sorunları algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-DnsConnectivity [-Forest] <String> [-DCs] <Array> [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel Dns bağlantı testleri çalıştırır.
Active Directory Bağlayıcısı'nı yapılandırmak için kullanıcı bunlar bağlanmaya de etki alanı denetleyicileri bu ormana ilişkili olduğu gibi bir orman için her iki ad ÇözümlemesiEş olması gerekir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için orman adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'leri

Sınanacak DC'leri belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanılama sonucu bir PSObject biçiminde döndürür.
Bu aracı el ile etkileşim sırasında gerekli değildir.

```yml
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

## <a name="confirm-forestexists"></a>ForestExists onaylayın

### <a name="synopsis"></a>ÖZET

Belirtilen orman olup olmadığını belirler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ForestExists [-Forest] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Bir orman ile ilişkili IP adresleri için bir DNS sunucusunu sorgular.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için orman adını belirtir.

```yml
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

## <a name="confirm-functionallevel"></a>FunctionalLevel onaylayın

### <a name="synopsis"></a>ÖZET

AD ormanı işlev düzeyi doğrular.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="samaccount"></a>SamAccount

```
Confirm-FunctionalLevel -Forest <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-FunctionalLevel -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

AD ormanı işlev düzeyi eşit veya birden çok belirli bir MinAdForestVersion (WindowsServer2003) olduğunu doğrular.
Hesabı (etki alanı\kullanıcı adı) ve parolası istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>ÖRNEK 3

```powershell
Confirm-FunctionalLevel -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.
Şu anda oturum açmış kullanıcının orman varsayılan değerdir.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

Hedef ForestFQDN nesnesi.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlevi, bilgisayarda oturum açmış kullanıcının kimlik bilgilerini değil kullanıcıdan istekte bulunan özel kimlik bilgilerini kullanır.

```yml
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

## <a name="confirm-networkconnectivity"></a>NetworkConnectivity onaylayın

### <a name="synopsis"></a>ÖZET

Yerel ağ bağlantısı sorunları algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-NetworkConnectivity [-DCs] <Array> [-SkipDnsPort] [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testleri çalıştırır.

Yerel ağ testler için AAD Connect bu testi şu anda tümleştirilmiş neden olan bağlantı noktası 53'ün (DNS), 88 (Kerberos) ve 389 çoğu kuruluş, DNS, DC'leri üzerinde çalıştırın. (LDAP) adlı etki alanı denetleyicilerinde ile iletişim kurabildiğini olması gerekir.
Başka bir DNS sunucusu belirtilirse, bağlantı noktası 53 atlanmalıdır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-NetworkConnectivity -SkipDnsPort -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-NetworkConnectivity -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM" -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-dcs"></a>-DC'leri

Sınanacak DC'leri belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipdnsport"></a>-SkipDnsPort

Kullanıcının AD Site tarafından sağlanan DNS hizmetleri kullanmıyorsa / denetimi bağlantı noktası 53 atlamak oturum açma DC sonra isteyebilirsiniz.
Kullanıcı hala _.ldap._tcp çözümleyebilmesi gerekir. \<forestfqdn\> sırada başarılı olması Active Directory Bağlayıcısı yapılandırması.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanılama sonucu bir PSObject biçiminde döndürür.
Bu aracı el ile etkileşim sırasında gerekli değildir.

```yml
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

## <a name="confirm-targetsarereachable"></a>TargetsAreReachable onaylayın

### <a name="synopsis"></a>ÖZET

Belirtilen orman ve onun ilişkili etki alanı denetleyicileri erişilebilir olup olmadığını belirler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-TargetsAreReachable [-Forest] <String> [-DCs] <Array> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Çalıştırmaları "ping testleri (olup bir bilgisayar, ağ ve/veya internet üzerinden bir hedef bilgisayara ulaşabilirsiniz)"

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için orman adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'leri

Sınanacak DC'leri belirtin.

```yml
Type: Array
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

## <a name="confirm-validdomains"></a>ValidDomains onaylayın

### <a name="synopsis"></a>ÖZET

Elde edilen orman FQDN'si etki alanlarında erişilebilir olduğunu doğrulayın

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="samaccount"></a>SamAccount

```
Confirm-ValidDomains [-Forest <String>] [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-ValidDomains -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Tüm etki alanlarının elde edilen orman FQDN'si DomainGuid ve DomainDN almaya çalışırken tarafından erişilebilir olduğunu doğrulayın.
Hesabı (etki alanı\kullanıcı adı) ve parolası istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>ÖRNEK 3

```powershell
Confirm-ValidDomains -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

Hedef ForestFQDN nesnesi.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlevi, bilgisayarda oturum açmış kullanıcının kimlik bilgilerini değil kullanıcıdan istekte bulunan özel kimlik bilgilerini kullanır.

```yml
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

## <a name="confirm-validenterpriseadmincredentials"></a>ValidEnterpriseAdminCredentials onaylayın

### <a name="synopsis"></a>ÖZET

Bir kullanıcının kuruluş yöneticisi kimlik bilgilerine sahip olup olmadığını doğrular.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ValidEnterpriseAdminCredentials [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kuruluş yöneticisi kimlik bilgilerini kullanıcının arar.
Hesabı (etki alanı\kullanıcı adı) ve parolası istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-ValidEnterpriseAdminCredentials -DomainName test.contoso.com -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-ValidEnterpriseAdminCredentials -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlevi, bilgisayarda oturum açmış kullanıcının kimlik bilgilerini değil kullanıcıdan istekte bulunan özel kimlik bilgilerini kullanır.

```yml
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

## <a name="get-domainfqdndata"></a>Get-DomainFQDNData

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola birleşimini dışında bir DomainFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-DomainFQDNData [[-DomainFQDNDataType] <String>] [-RunWithCurrentlyLoggedInUserCredentials]
 [-ReturnExceptionOnError] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kimlik bilgileri dışında bir domainFQDN nesnesi almayı dener.
DomainFQDN DomainFQDNName veya RootDomainName, kullanıcının seçimine bağlı olarak döndürülecek geçerli ise.
Hesabı (etki alanı\kullanıcı adı) ve parolası istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Get-DomainFQDNData -DomainFQDNDataType DomainFQDNName -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Get-DomainFQDNData -DomainFQDNDataType RootDomainName -RunWithCurrentlyLoggedInUserCredentials
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-domainfqdndatatype"></a>-DomainFQDNDataType

Tür alınır veri istenen.
Şu anda "DomainFQDNName" veya "RootDomainName" sınırlı.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlevi, bilgisayarda oturum açmış kullanıcının kimlik bilgilerini değil kullanıcıdan istekte bulunan özel kimlik bilgilerini kullanır.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnexceptiononerror"></a>-ReturnExceptionOnError

Başlangıç NetworkConnectivityDiagnosisTools işlevi tarafından kullanılan yardımcı parametresi

```yml
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

## <a name="get-forestfqdn"></a>Get-ForestFQDN

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola birleşimini dışında bir ForestFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ForestFQDN [-Forest] <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kimlik bilgileri dışında bir ForestFQDN almayı dener.
Hesabı (etki alanı\kullanıcı adı) ve parolası istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman. Şu anda oturum açmış kullanıcının etki alanı varsayılan değerdir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlevi, bilgisayarda oturum açmış kullanıcının kimlik bilgilerini değil kullanıcıdan istekte bulunan özel kimlik bilgilerini kullanır.

```yml
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

## <a name="start-connectivityvalidation"></a>Başlangıç ConnectivityValidation

### <a name="synopsis"></a>ÖZET

Main işlevi.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-ConnectivityValidation [-Forest] <String> [-AutoCreateConnectorAccount] <Boolean> [[-UserName] <String>]
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Çalıştırmaları AD kimlik doğrulama kullanılabilir mekanizmaları geçerlidir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Start-ConnectivityValidation -Forest "test.contoso.com" -AutoCreateConnectorAccount $True -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-autocreateconnectoraccount"></a>-AutoCreateConnectorAccount

Özel yüklemeler için: Kullanıcı "Yeni AD hesap oluştur" AADConnect'ın Sihirbazı'nın AD ormanı hesabını penceresinde seçerseniz, $True bayrak.
Kullanıcı "Var olan AD hesabını kullan" seçeneğini belirlediyseniz $False.
Express yüklemeleri için: Bu değişkenin değerini $True Express yüklemeleri için olmalıdır.

```yml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName

Kullanıcının kimlik bilgileri istendiğinde kullanıcı adı alanı önceden doldurur parametresi.

```yml
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

## <a name="start-networkconnectivitydiagnosistools"></a>Başlangıç NetworkConnectivityDiagnosisTools

### <a name="synopsis"></a>ÖZET

Main işlevi için ağ bağlantısını test eder.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-NetworkConnectivityDiagnosisTools [[-Forest] <String>] [-Credentials] <PSCredential>
 [[-LogFileLocation] <String>] [[-DCs] <Array>] [-DisplayInformativeMessage] [-ReturnResultAsPSObject]
 [-ValidCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testleri çalıştırır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM" -DCs "DC1.TEST.CONTOSO.COM", "DC2.TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Sınanacak orman adı belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credentials"></a>-Credentials

Kullanıcı adı ve bir testi çalıştıran kullanıcının parolası.
Azure AD Connect Sihirbazı'nı çalıştırmak için gerekli olan aynı izin düzeyini gerektirir.

```yml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-logfilelocation"></a>-LogFileLocation

Belirtir bir, bu işlevin çıktısı içeren bir günlük dosyasının konumu.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'leri

Sınanacak DC'leri belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-displayinformativemessage"></a>-DisplayInformativeMessage

Bu işlevin amacı hakkında bir ileti görüntüleme sağlayan bayrak.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanılama sonucu bir PSObject biçiminde döndürür.
Bu aracı el ile etkileşim sırasında belirtmek gerekli değil.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-validcredentials"></a>-ValidCredentials

Kullanıcı kimlik bilgileri geçerli olup olmadığını gösterir.
Bu aracı el ile etkileşim sırasında belirtmek gerekli değil.

```yml
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
