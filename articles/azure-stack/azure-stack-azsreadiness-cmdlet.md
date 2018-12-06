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
ms.topic: get-started-article
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 1dbfd668c2d233d299ee673da92ca203e72942fe
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957435"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Başlangıç AzsReadinessChecker cmdlet başvurusu

Modül: Microsoft.AzureStack.ReadinessChecker

Bu modül, yalnızca tek bir cmdlet içerir.  Bu cmdlet, Azure Stack için bir veya daha fazla dağıtım öncesi veya önceden bakım işlemleri gerçekleştirir.

## <a name="syntax"></a>Sözdizimi

```PowerShell
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

```PowerShell
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

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
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

```PowerShell
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

```PowerShell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
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

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
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

```PowerShell
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

```PowerShell
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

**Başlangıç AzsReadinessChecker** cmdlet'i, sertifikaları, Azure hesapları, Azure abonelikleri ve Azure Active dizin doğrular. Azure Stack dağıtmadan önce ya da Azure Stack gizli döndürme gibi eylemleri bakım önce doğrulama çalıştırın. Cmdlet, altyapı sertifikaları ve isteğe bağlı olarak PaaS sertifikalar için sertifika imzalama isteği oluşturmak için de kullanılabilir.  Son olarak, cmdlet ortak paketleme sorunlarını düzeltmek için PFX sertifikaları paketleyebilirsiniz.

## <a name="examples"></a>Örnekler

### <a name="example-generate-certificate-signing-request"></a>Örnek: Sertifika imzalama isteği oluşturma

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Bu örnekte, birden çok sertifika imzalama isteği (CSR) bölge adı "Doğu" ile ADFS Azure Stack dağıtımı için uygun sertifikaların ve "azurestack.contoso.com" dış FQDN'si başlangıç AzsReadinessChecker oluşturur

### <a name="example-validate-certificates"></a>Örnek: Sertifika doğrulama

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Bu örnekte, PFX parolasını güvenli bir şekilde istenir ve geçerli bir AAD dağıtımı "Doğu" ve "azurestack.contoso.com" dış FQDN'si için bir bölge adı ile sertifikalar için göreli klasörü "Sertifikalar" Başlangıç AzsReadinessChecker denetler

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>Örnek: sertifikaları dağıtım verileri (dağıtım ve destek) ile doğrulama

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

Bu dağıtım ve Destek örnekte PFX parolasını güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker sertifikalar burada kimlik, bölge ve dış FQDN okunur gelen bir dağıtım için geçerli göreli klasörü "Sertifikalar" denetler dağıtım için oluşturulan dağıtım verileri JSON dosyası. 

### <a name="example-validate-paas-certificates"></a>Örnek: PaaS sertifikalarını doğrulama

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

Bu örnekte, bir karma tablosu yolları ve her bir PaaS sertifikanın parolaları ile oluşturulur. Sertifikaları atlanabilir. Başlangıç AzsReadinessChecker her PFX yolun mevcut olduğunu ve bölge 'Doğu' kullanarak bunları ve dış FQDN 'azurestack.contoso.com' doğrular denetler.

### <a name="example-validate-paas-certificates-with-deployment-data"></a>Örnek: PaaS sertifikalarla dağıtım verileri doğrulama

```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

Bu örnekte, bir karma tablosu yolları ve her bir PaaS sertifikanın parolaları ile oluşturulur. Sertifikaları atlanabilir. Başlangıç AzsReadinessChecker her PFX yolun mevcut olduğunu ve bölge kullanarak bunları doğrular ve dağıtım için oluşturulan dağıtım verileri JSON dosyasından dış FQDN okuma denetler. 

### <a name="example-validate-azure-identity"></a>Örnek: Azure kimlik doğrulama

```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

Bu örnekte, hizmet yönetici hesabının kimlik bilgilerini güvenli bir şekilde istenir ve Azure Active Directory ve Azure hesabı, geçerli bir AAD dağıtımı "azurestack.contoso.com" için bir kiracı dizin adı ile başlangıç AzsReadinessChecker denetler.

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>Örnek: Veri dağıtımı (dağıtım desteği) ile Azure kimlik doğrulama

```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

Bu örnekte, hizmet yönetici hesabının kimlik bilgilerini güvenli bir şekilde istenir ve Azure hesabı ve Azure Active Directory AzureCloud ve Kiracıadı dağıtım verileri nerede okumak bir AAD dağıtım için geçerli olan başlangıç AzsReadinessChecker denetler Dağıtım için oluşturulan JSON dosyası.

### <a name="example-validate-azure-registration"></a>Örnek: Azure kaydı doğrula

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

Bu örnekte, abonelik sahibi kimlik bilgilerini güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker belirli bir hesaba karşı doğrulama gerçekleştirir ve abonelik emin olmak için Azure Stack kayıt için kullanılabilir. 

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>Örnek: Veri dağıtımı (dağıtım ekibi) ile Azure kaydı doğrula

```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Bu örnekte, abonelik sahibi kimlik bilgilerini güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker belirli bir hesaba karşı doğrulama gerçekleştirir ve abonelik emin olmak için ek ayrıntılar olduğu Azure Stack kayıt için kullanılabilir dağıtım için oluşturulan dağıtım verileri JSON dosyasından okuyun.

### <a name="example-importexport-pfx-package"></a>Örnek: İçeri/dışarı aktarma PFX paketi

```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Bu örnekte, PFX parolasını güvenli bir şekilde istenir. Ssl.pfx dosya yerel makine sertifika deposuna ve aynı parola ile yeniden dışarı ve ssl_new.pfx kaydedilir.  Sertifika doğrulaması yerel makine öznitelik kümesi özel bir anahtara sahip değil, sertifika zinciri bozuk, ilgisiz sertifikaların PFX'e mevcut olduğunu veya sertifika zinciri yanlış sırada bayrak eklendiğinde bu yordamı için kullanılır.

### <a name="example-view-validation-report-deployment-support"></a>Örnek: Görünüm doğrulama raporu (dağıtım desteği)

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Bu örnekte, dağıtım veya destek ekibi hazırlık raporu (Contoso) müşteriden almak ve başlangıç AzsReadinessChecker Contoso gerçekleştirilen doğrulama yürütme durumunu görüntülemek için kullanın.

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>Örnek: sertifika doğrulaması yalnızca (dağıtım ve destek) Özet doğrulama raporunu görüntüle

```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Bu örnekte, dağıtım veya destek ekibi müşteriden Contoso Hazırlık raporunu almak ve başlangıç AzsReadinessChecker özetlenen Contoso yapılan sertifika doğrulama yürütme durumunu görüntülemek için kullanın.

## <a name="required-parameters"></a>Gerekli Parametreler

> -RegionName

Azure Stack dağıtımın bölge adı belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |adlı         |
|Varsayılan değer:              |None          |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

> -FQDN

Azure Stack dağıtımın dış FQDN, ayrıca diğer adlı ExternalFQDN ve ExternalDomainName olarak belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |adlı         |
|Varsayılan değer:              |ExternalFQDN, ExternalDomainName |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

> -IdentitySystem

Azure Stack dağıtımın kimlik sistemi geçerli değerler, AAD veya ADFS, Azure Active Directory ve Active Directory Federasyon Hizmetleri için sırasıyla belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |adlı         |
|Varsayılan değer:              |None          |
|Geçerli değerler:               |'AAD', 'ADFS'  |
|Ardışık giriş yapılabilir:      |False         |
|Joker karakterler kabul edin: |False         |

> -PfxPassword

PFX sertifika dosyaları ile ilişkili parolayı belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |SecureString |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -PaaSCertificates

Yollar ve parolaları PaaS sertifikaları içeren karma tablo belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Hashtable |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -DeploymentDataJSONPath

Azure Stack dağıtım verileri JSON yapılandırma dosyasını belirtir. Bu dosya, dağıtım için oluşturulur.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -PfxPath

Bu araç, sertifika doğrulama tarafından belirtildiği şekilde düzeltmek için içeri/dışarı aktarma yordamı gerektiren sorunlu bir sertifika yolunu belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -ExportPFXPath  

Sonuç PFX dosyasından içeri/dışarı aktarma yordamı için hedef yolu belirtir.  
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -Konu

Bir sıralanmış sözlük konu için sertifika isteği oluşturma belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |OrderedDictionary   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -RequestType

Sertifika isteği SAN türünü belirtir. Geçerli değerler, MultipleCSR SingleCSR.

- *MultipleCSR* birden çok sertifika istekleri, her hizmet için bir tane oluşturur.
- *SingleCSR* tüm hizmetler için bir sertifika isteği oluşturur.

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'MultipleCSR', 'SingleCSR' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -OutputRequestPath

Hedef yolu belirtir sertifika isteği dosyaları için dizin zaten mevcut olmalıdır.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -AADServiceAdministrator

Azure Stack dağıtımı için kullanılacak Azure Active Directory Hizmet Yöneticisi belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |PSCredential   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -AADDirectoryTenantName

Azure Stack dağıtımı için kullanılacak Azure Active Directory adını belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -AzureEnvironment

Azure Stack dağıtım ve kayıt için kullanılacak Azure hesapları, dizinler ve abonelikler içeren Services örneğini belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'AzureCloud', 'AzureChinaCloud', 'AzureUSGovernment' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -RegistrationAccount

Azure Stack kayıt için kullanılacak kayıt hesabı belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -RegistrationSubscriptionID

Azure Stack kayıt için kullanılacak kayıt abonelik Kimliğini belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Guid     |
|Konum:                   |adlı    |
|Varsayılan değer:              |None     |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -ReportPath

Hazırlık raporunu yolunu belirtir, varsayılan olarak geçerli dizin ve varsayılan rapor adı.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |Tümü      |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

## <a name="optional-parameters"></a>İsteğe bağlı parametreler

> -CertificatePath

Sertifika klasörlerin var olduğundan, altında yalnızca sertifikası gerekli yolunu belirtir.

Azure Active Directory kimlik sistemi ile Azure Stack dağıtımı için gerekli klasörler şunlardır:

Genel, anahtar kasası, KeyVaultInternal, genel kullanıma açık portala ACSBlob, ACSQueue, ACSTable, Yönetim Portalı, ARM yönetici ARM

Klasörü dağıtım Active Directory Federasyon Hizmetleri kimlik sistemi olan Azure Stack için gerekli:

ACSBlob, ACSQueue, ACSTable, ADFS, Yönetim Portalı, ARM yönetici, ARM genel, grafik, anahtar kasası, KeyVaultInternal, genel kullanıma açık portala

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |. \Certificates |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -IncludePaaS  

PaaS Hizmetleri/ana bilgisayar adları için sertifika istekleri eklenip eklenmeyeceğini belirtir.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

> -ReportSections

Ayrıntılı rapor yalnızca Özet, gösterecek şekilde olmadığını atlar belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |adlı    |
|Varsayılan değer:              |Tümü      |
|Geçerli değerler:               |'Sertifika', 'AzureRegistration', 'AzureIdentity', 'İşler', 'All' |
|Ardışık giriş yapılabilir:      |False    |
|Joker karakterler kabul edin: |False    |

> -Özet

Ayrıntılı rapor yalnızca Özet, gösterecek şekilde olmadığını atlar belirtir.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

> -CleanReport

Önceki yürütme ve doğrulamayı geçmişini kaldırır ve Doğrulamalar için yeni bir rapor yazar.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

> -OutputPath

Özel JSON hazırlık raporu kaydedip ayrıntılı günlük dosyası yolu belirtir.  Yol zaten mevcut değilse, aracı dizini dener.

|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |Dize            |
|Konum:                   |adlı             |
|Varsayılan değer:              |$ENV: TEMP\AzsReadinessChecker  |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

> -Onaylayın

Cmdlet'i çalıştırmadan önce onay ister.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |

> -WhatIf

Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |Wi                |
|Konum:                   |adlı             |
|Varsayılan değer:              |False             |
|Ardışık giriş yapılabilir:      |False             |
|Joker karakterler kabul edin: |False             |
