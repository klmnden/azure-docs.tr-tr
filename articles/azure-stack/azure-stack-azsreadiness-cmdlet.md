---
title: Başlangıç AzsReadinessChecker cmdlet başvurusu | Microsoft Docs
description: Azure Stack hazırlık denetleyicisi modülü için PowerShell cmdlet Yardımı.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/04/2018
ms.openlocfilehash: f920059f97f43a2ac3c48dad1c8f999833f6add1
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757778"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Başlangıç AzsReadinessChecker cmdlet başvurusu

Modül: **Microsoft.AzureStack.ReadinessChecker**

Bu modül, yalnızca tek bir cmdlet içerir. Cmdlet, Azure Stack için bir veya daha fazla dağıtım öncesi veya önceden bakım işlemleri gerçekleştirir.

## <a name="syntax"></a>Sözdizimi

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

## <a name="description"></a>Açıklama

**Başlangıç AzsReadinessChecker** cmdlet'i, sertifikaları, Azure hesapları, Azure abonelikleri ve Azure Active dizin doğrular. Azure Stack dağıtmadan önce ya da Azure Stack gizli döndürme gibi eylemleri bakım önce doğrulama çalıştırın. Cmdlet, altyapı sertifikalarına ilişkin istekleri imzalama sertifikası ve isteğe bağlı olarak PaaS sertifikaları oluşturmak için de kullanılabilir. Son olarak, cmdlet ortak paketleme sorunlarını düzeltmek için PFX sertifikaları paketleyebilirsiniz.

## <a name="examples"></a>Örnekler

### <a name="example-generate-certificate-signing-request"></a>Örnek: sertifika imzalama isteği oluşturma

```powershell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Bu örnekte, `Start-AzsReadinessChecker` bir bölge adı ile AD FS Azure Stack dağıtımı için uygun sertifikaların için birden çok sertifika imzalama isteği (CSR) oluşturan **Doğu** ve dış FQDN'si  **azurestack.contoso.com**.

### <a name="example-validate-certificates"></a>Örnek: Sertifika doğrulama

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Bu örnekte, güvenlik için PFX parolası gereklidir ve `Start-AzsReadinessChecker` göreli klasör denetler **sertifikaları** geçerli bir bölge adı ile AAD dağıtımı için sertifikaları **Doğu** ve Dış FQDN'sini **azurestack.contoso.com**.

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>Örnek: sertifikaları dağıtım verileri (dağıtım ve destek) ile doğrulama

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

PFX parolası güvenlik için dağıtım ve Destek Bu örnekte gereklidir ve `Start-AzsReadinessChecker` göreli klasör denetler **sertifikaları** kimlik, bölge ve dış FQDN olduğu bir dağıtım için geçerli sertifika dağıtım için oluşturulan dağıtım verileri JSON dosyasından okuyun.

### <a name="example-validate-paas-certificates"></a>Örnek: PaaS sertifikalarını doğrulama

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

Bu örnekte, bir karma tablosu yolları ve her bir PaaS sertifikanın parolaları ile oluşturulur. Sertifikaları atlanabilir. `Start-AzsReadinessChecker` Her PFX yolun mevcut olduğunu ve bölge kullanarak bunları doğrular denetler **Doğu** ve dış FQDN **azurestack.contoso.com**.

### <a name="example-validate-paas-certificates-with-deployment-data"></a>Örnek: PaaS sertifikalarla dağıtım verileri doğrulama

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

Bu örnekte, bir karma tablosu yolları ve her bir PaaS sertifikanın parolaları ile oluşturulur. Sertifikaları atlanabilir. `Start-AzsReadinessChecker` Her bir PFX yolunun var olduğundan ve bölgeyi kullanarak bunları doğrular ve dış FQDN dağıtımı için oluşturulan dağıtım verileri JSON dosyasından okuma olduğunu denetler.

### <a name="example-validate-azure-identity"></a>Örnek: Azure kimlik doğrulama

```powershell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

Bu örnekte, hizmet yönetici hesabının kimlik bilgilerini güvenlik için gereklidir ve `Start-AzsReadinessChecker` Azure hesabı ve Azure Active Directory Kiracı dizin adı ile AAD dağıtımı için geçerli olup olmadığını kontrol eder  **azurestack.contoso.com**.

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>Örnek: Veri dağıtımı (dağıtım desteği) ile Azure kimlik doğrulama

```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

Bu örnekte, hizmet yönetici hesabının kimlik bilgilerini güvenlik için gereklidir ve `Start-AzsReadinessChecker` Azure Active Directory ve Azure hesabı bir AAD dağıtım için geçerli olduğunu denetler. burada **AzureCloud** ve **Kiracıadı** dağıtımı için oluşturulan dağıtım verileri JSON dosyasından okuyun.

### <a name="example-validate-azure-registration"></a>Örnek: Azure kaydı doğrula

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenlik için gereklidir ve `Start-AzsReadinessChecker` belirli hesap ve Azure Stack kayıt için kullanılabileceği emin olmak için abonelik karşı doğrulama gerçekleştirir.

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>Örnek: Veri dağıtımı (dağıtım ekibi) ile Azure kaydı doğrula

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenlik için gereklidir ve `Start-AzsReadinessChecker` burada ek ayrıntılar okuma, Azure Stack kayıt için kullanılabileceği emin olmak için abonelik ve belirtilen hesabın karşı doğrulama gerçekleştirir dağıtım için oluşturulan dağıtım verileri JSON dosyası.

### <a name="example-importexport-pfx-package"></a>Örnek: içeri/dışarı aktarma PFX paketi

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Bu örnekte, güvenlik için PFX parolası gereklidir. Ssl.pfx dosyası yerel makine sertifika deposuna içe, aynı parola ile yeniden dışarı ve Ssl_new.pfx kaydedilir. Bu yordamı bir özel anahtarı olmayan sertifika doğrulama bayrağı kullanıldığında **yerel makine** öznitelik kümesi, sertifika zinciri kopmuş, ilgisiz sertifikaların PFX'e mevcut olduğunu veya sertifika zinciri yanlış sırada.

### <a name="example-view-validation-report-deployment-support"></a>Örnek: görünüm doğrulama raporu (dağıtım desteği)

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Bu örnekte, dağıtım veya destek ekibi hazırlık raporu (Contoso) müşteriden alır ve kullandığı `Start-AzsReadinessChecker` Contoso gerçekleştirilen doğrulama yürütme durumunu görüntülemek için.

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>Örnek: sertifika doğrulaması yalnızca (dağıtım ve destek) Özet doğrulama raporunu görüntüle

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Bu örnekte, dağıtım veya destek ekibi hazırlık raporu (Contoso) müşteriden alır ve kullandığı `Start-AzsReadinessChecker` özetlenen Contoso yapılan sertifika doğrulama yürütme durumunu görüntülemek için.

## <a name="required-parameters"></a>Gerekli Parametreler

### <a name="-regionname"></a>-RegionName

Azure Stack dağıtım bölge adını belirtir.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |String        |
|Konum:                   |adlı         |
|Varsayılan değer:              |None          |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

### <a name="-fqdn"></a>-FQDN

Azure Stack dağıtımı belirten dış FQDN, ayrıca diğer adlı olarak **ExternalFQDN** ve **ExternalDomainName**.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |String        |
|Konum:                   |adlı         |
|Varsayılan değer:              |ExternalFQDN, ExternalDomainName |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

### <a name="-identitysystem"></a>-IdentitySystem

Azure Stack dağıtım kimlik sistemi geçerli değerleri, AAD veya ADFS, Azure Active Directory ve Active Directory Federasyon Hizmetleri için sırasıyla belirtir.

|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |String        |
|Konum:                   |adlı         |
|Varsayılan değer:              |None          |
|Geçerli değerler:               |'AAD', 'ADFS'  |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

### <a name="-pfxpassword"></a>-PfxPassword

PFX sertifika dosyaları ile ilişkili parolayı belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |SecureString |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-paascertificates"></a>-PaaSCertificates

Yollar ve parolaları PaaS sertifikaları içeren karma tablo belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Hashtable |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-deploymentdatajsonpath"></a>-DeploymentDataJSONPath

Azure Stack dağıtım verileri JSON yapılandırma dosyasını belirtir. Bu dosya, dağıtım için oluşturulur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-pfxpath"></a>-PfxPath

Bu araç, sertifika doğrulama tarafından belirtildiği şekilde düzeltmek için bir içeri/dışarı aktarma yordamı gerektiren sorunlu bir sertifika yolunu belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-exportpfxpath"></a>-ExportPFXPath  

Sonuç PFX dosyasından içeri/dışarı aktarma yordamı için hedef yolu belirtir.  

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-subject"></a>-Konu

Bir sıralanmış sözlük konu için sertifika isteği oluşturma belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |OrderedDictionary   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-requesttype"></a>-RequestType

Sertifika isteği SAN türünü belirtir. Geçerli değerler **MultipleCSR**, **SingleCSR**.

- **MultipleCSR** birden çok sertifika istekleri, her hizmet için bir tane oluşturur.
- **SingleCSR** tüm hizmetler için bir sertifika isteği oluşturur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'MultipleCSR', 'SingleCSR' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-outputrequestpath"></a>-OutputRequestPath

Sertifika isteği dosyaları hedef yolunu belirtir. Dizin zaten mevcut olmalıdır.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-aadserviceadministrator"></a>-AADServiceAdministrator

Azure Stack dağıtımı için kullanılacak Azure Active Directory Hizmet Yöneticisi belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |PSCredential   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-aaddirectorytenantname"></a>-AADDirectoryTenantName

Azure Stack dağıtımı için kullanılacak Azure Active Directory adını belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-azureenvironment"></a>-AzureEnvironment

Azure Stack dağıtım ve kayıt için kullanılacak Azure hesapları, dizinler ve abonelikler içeren Services örneğini belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'AzureCloud', 'AzureChinaCloud', 'AzureUSGovernment' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-registrationaccount"></a>-RegistrationAccount

Azure Stack kayıt için kullanılacak kayıt hesabı belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-registrationsubscriptionid"></a>-RegistrationSubscriptionID

Azure Stack kayıt için kullanılacak kayıt abonelik Kimliğini belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Guid     |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-reportpath"></a>-ReportPath

Hazırlık raporunu yolunu belirtir, varsayılan olarak geçerli dizin ve varsayılan rapor adı.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |Tümü      |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

## <a name="optional-parameters"></a>İsteğe bağlı parametreler

### <a name="-certificatepath"></a>-CertificatePath

Sertifika klasörlerin var olduğundan, altında yalnızca sertifikası gerekli yolunu belirtir.

Azure Active Directory kimlik sistemi ile Azure Stack dağıtımı için gerekli klasörler şunlardır:

Genel, anahtar kasası, KeyVaultInternal, genel kullanıma açık portala ACSBlob, ACSQueue, ACSTable, Yönetim Portalı, ARM yönetici ARM

Klasörü dağıtım Active Directory Federasyon Hizmetleri kimlik sistemi olan Azure Stack için gerekli:

ACSBlob, ACSQueue, ACSTable, ADFS, Yönetim Portalı, ARM yönetici, ARM genel, grafik, anahtar kasası, KeyVaultInternal, genel kullanıma açık portala

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |.\Certificates |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-includepaas"></a>-IncludePaaS  

PaaS Hizmetleri/konak adları için sertifika istekleri eklenmesi gerekip gerekmediğini belirtir.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

### <a name="-reportsections"></a>-ReportSections

Ayrıntılı rapor yalnızca Özet, gösterecek şekilde olmadığını atlar belirtir.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |String   |
|Konum:                   |adlı    |
|Varsayılan değer:              |Tümü      |
|Geçerli değerler:               |'Sertifika', 'AzureRegistration', 'AzureIdentity', 'İşler', 'All' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

### <a name="-summary"></a>-Özet

Ayrıntılı rapor yalnızca Özet, gösterecek şekilde olmadığını atlar belirtir.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

### <a name="-cleanreport"></a>-CleanReport

Önceki yürütme ve doğrulamayı geçmişini kaldırır ve Doğrulamalar için yeni bir rapor yazar.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

### <a name="-outputpath"></a>-OutputPath

Özel bir hazırlık JSON raporu kaydedip ayrıntılı günlük dosyası yolu belirtir. Yol zaten mevcut değilse, bir dizin oluşturmak komut çalışır.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |String            |
|Konum:                   |adlı             |
|Varsayılan değer:              |$ENV: TEMP\AzsReadinessChecker  |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

### <a name="-confirm"></a>-Onaylayın

Cmdlet'i çalıştırmadan önce onay ister.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

### <a name="-whatif"></a>-WhatIf

Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |Wi                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |
