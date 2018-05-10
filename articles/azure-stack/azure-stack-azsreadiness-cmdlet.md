---
title: Start-AzsReadinessChecker cmdlet başvurusu | Microsoft Docs
description: Azure yığın hazırlık denetleyicisi modülü için PowerShell cmdlet Yardımı.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 8481fbd6c7cb82b34070f9bc8cc6d7f3f4b2518c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Start-AzsReadinessChecker cmdlet başvurusu

Modül: Microsoft.AzureStack.ReadinessChecker

Bu modül, yalnızca tek bir cmdlet içerir.  Bu cmdlet bir veya daha fazla dağıtım öncesi veya önceden bakım işlevleri için Azure yığın gerçekleştirir.

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
**Başlangıç AzsReadinessChecker** cmdlet, sertifikaları, Azure hesapları, Azure abonelikleri ve Azure Active dizinleri doğrular. Doğrulama Azure yığın dağıtmadan önce ya da Azure gizli döndürme gibi eylemleri bakım yığını önce çalıştırın. Cmdlet, altyapı sertifikaları ve isteğe bağlı olarak PaaS sertifikaları için sertifika imzalama istekleri oluşturmak için de kullanılabilir.  Son olarak, cmdlet paketleme ile ilgili genel sorunları düzeltmek için PFX sertifikaları paketleyebilirsiniz.

## <a name="examples"></a>Örnekler
**Örnek: Sertifika imzalama isteği oluştur**

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subjectName $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Bu örnekte, birden çok sertifika imzalama isteği (CSR) için bir bölge adı "Doğu" ile bir ADFS Azure yığın dağıtım için uygun sertifikaların ve "azurestack.contoso.com" dış FQDN'si başlangıç AzsReadinessChecker oluşturur

**Örnek: sertifikaları doğrular**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Bu örnekte, PFX parola için güvenli bir şekilde istenir ve geçerli bir AAD dağıtımı "Doğu" ve "azurestack.contoso.com" dış FQDN'si için bir bölge adı ile sertifikalar için "Sertifikalar" göreli klasör başlangıç AzsReadinessChecker denetler 

**Örnek: dağıtım verilerle (dağıtım ve destek) sertifika doğrulama**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```
Bu dağıtım ve Destek örnekte PFX parola için güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker sertifikalar burada kimlik, bölge ve dış FQDN okunur gelen bir dağıtım için geçerli göreli klasör "Sertifikalar" denetler dağıtımı için oluşturulan dağıtım veri JSON dosyası. 

**Örnek: PaaS sertifikaları doğrular**
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

Bu örnekte, bir karma tablosu yolları ve her PaaS sertifika için parola ile oluşturulur. Sertifikaları atlanabilir. Başlangıç AzsReadinessChecker her PFX yolunun var olduğunu ve bölge 'Doğu' kullanarak bunları ve dış FQDN 'azurestack.contoso.com' doğrular denetler.

**Örnek: Dağıtım verilerle PaaS sertifika doğrulama**
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

Bu örnekte, bir karma tablosu yolları ve her PaaS sertifika için parola ile oluşturulur. Sertifikaları atlanabilir. Başlangıç AzsReadinessChecker her PFX yolunun var olduğunu ve bölge kullanarak bunları doğrular ve dış FQDN dağıtımı için oluşturulan dağıtım veri JSON dosyasından okunan denetler. 

**Örnek: Azure kimlik doğrulama**
```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AzureDirectoryTenantName azurestack.contoso.com
```

Bu örnekte, Hizmet Yöneticisi hesap kimlik bilgilerini güvenli bir şekilde istenir ve Azure Active Directory ve Azure hesabın geçerli bir AAD dağıtımı "azurestack.contoso.com" için bir kiracı dizin adı ile başlangıç AzsReadinessChecker denetler


**Örnek: Veri dağıtımı (dağıtım desteği) ile Azure kimlik doğrulama**
```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

Bu örnekte, Hizmet Yöneticisi hesap kimlik bilgilerini güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker Azure hesabı ve Azure Active Directory AzureCloud ve TenantName dağıtım verilerinden burada okunan bir AAD dağıtım için geçerli olup olmadığını denetler Dağıtımı için oluşturulan JSON dosyası.


**Örnek: Azure kaydı doğrula**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment AzureCloud
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker verilen hesap karşı doğrulama gerçekleştirir ve emin olmak için abonelik Azure yığın kaydı için kullanılabilir. 


**Örnek: Veri dağıtımı (dağıtım ekibi) ile Azure kaydı doğrula**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Bu örnekte, abonelik sahibi kimlik bilgileri güvenli bir şekilde istenir ve başlangıç AzsReadinessChecker verilen hesap karşı doğrulama gerçekleştirir ve abonelik emin olmak için ek ayrıntılar nerede Azure yığın kaydı için kullanılabilir dağıtımı için oluşturulan dağıtım verileri JSON dosyası okunamıyor.

**Örnek: İçeri/dışarı aktarma PFX paketi**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Bu örnekte, PFX parolası için güvenli bir şekilde istenir. Ssl.pfx dosyası yerel makine sertifika deposuna içe ve aynı parolayı yeniden dışarı ve ssl_new.pfx kaydedilir.  Bu yordamı bir özel anahtar yerel makine özniteliği kümesine sahip değildir, sertifika zinciri bozuk olduğundan, ilgisiz sertifikaların PFX mevcut olduğunu veya sertifika zinciri yanlış sırayla, sertifika doğrulama bayrak eklendiğinde kullanımı içindir.


**Örnek: Görünümü doğrulama raporu (dağıtım desteği)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Bu örnekte, dağıtım veya destek ekibi müşteri (Contoso) hazırlık raporu alır ve başlangıç AzsReadinessChecker Contoso gerçekleştirilen doğrulama yürütmeleri durumunu görüntülemek için kullanın.

**Örnek: Sertifika doğrulama yalnızca (dağıtım ve destek) Özet doğrulama raporunu görüntüle**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Bu örnekte, dağıtım veya destek ekibi müşteri Contoso hazırlık raporu alır ve başlangıç AzsReadinessChecker Contoso yapılan sertifika doğrulama yürütmeleri özetlenen durumunu görüntülemek için kullanın.



## <a name="required-parameters"></a>Gerekli Parametreler
> -RegionName

Azure yığın dağıtımın bölge adı belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |Adlı         |
|Varsayılan değer:              |None          |
|Ardışık Düzen giriş kabul edin:      |False         |
|Joker karakterler kabul edin: |False         |

> -FQDN    

Azure yığın dağıtımın dış FQDN, ayrıca diğer ExternalFQDN ve ExternalDomainName olarak belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |Adlı         |
|Varsayılan değer:              |ExternalFQDN, ExternalDomainName |
|Ardışık Düzen giriş kabul edin:      |False         |
|Joker karakterler kabul edin: |False         |

 

> -IdentitySystem    

Azure yığın dağıtımın kimlik sistemi geçerli değerler, AAD veya ADFS, Azure Active Directory ve Active Directory Federasyon Hizmetleri için sırasıyla belirtir.
|  |  |
|----------------------------|--------------|
|Şunu yazın:                       |Dize        |
|Konum:                   |Adlı         |
|Varsayılan değer:              |None          |
|Geçerli değerler:               |'AAD', 'ADFS'  |
|Ardışık Düzen giriş kabul edin:      |False         |
|Joker karakterler kabul edin: |False         |

> -PfxPassword    

PFX sertifika dosyaları ile ilişkili parolayı belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |SecureString |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -PaaSCertificates

Yollar ve PaaS sertifikalar için parola içeren hashtable belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Hashtable |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -DeploymentDataJSONPath

Azure yığın dağıtım veri JSON yapılandırma dosyası belirtir. Bu dosya, dağıtım için oluşturulur.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -PfxPath

İçeri/dışarı aktarma yordamı düzeltmek için bu aracı sertifika doğrulama belirtildiği gibi gerektirir sorunlu bir sertifika yolunu belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -ExportPFXPath  

İçeri/dışarı aktarma yordamı sonuç PFX dosyasından hedef yolunu belirtir.  
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -Konu

Konu sıralı bir sözlüğü için sertifika isteği oluşturma belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |OrderedDictionary   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -RequestType

Sertifika isteği SAN türünü belirtir. Geçerli değerler MultipleCSR, SingleCSR.
- *MultipleCSR* birden çok sertifika isteklerini, her hizmet için bir tane oluşturur.
- *SingleCSR* tüm hizmetler için tek bir sertifika isteği oluşturur.   

|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'MultipleCSR', 'SingleCSR' |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -OutputRequestPath

Hedef yolu belirtir sertifika isteği dosyaları için dizin zaten mevcut olmalıdır.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -AADServiceAdministrator

Azure yığın dağıtımı için kullanılacak Azure Active Directory Hizmet Yöneticisi belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |PSCredential   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -AADDirectoryTenantName

Azure yığın dağıtımı için kullanılacak Azure Active Directory adını belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -AzureEnvironment

Azure yığın dağıtım ve kayıt için kullanılacak Azure Hizmetleri hesapları, dizinler ve abonelikleri içeren örneğini belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Geçerli değerler:               |'AzureCloud', 'AzureChinaCloud', 'AzureGermanCloud' |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -RegistrationAccount

Kayıt Azure yığın kaydı için kullanılacak hesabı belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -RegistrationSubscriptionID

Azure yığın kaydı için kullanılacak kayıt abonelik Kimliğini belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Guid     |
|Konum:                   |Adlı    |
|Varsayılan değer:              |None     |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |

> -ReportPath

İçin geçerli dizini ve varsayılan rapor adı varsayılan olarak, Hazırlık raporunu yolunu belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |Tümü      |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |



## <a name="optional-parameters"></a>İsteğe bağlı parametreler
> -CertificatePath     

Sertifika klasörleri mevcut altında yalnızca sertifikası gerekli yolunu belirtir.

Azure Active Directory kimlik sistemi ile Azure yığın dağıtımı için gerekli klasörleri şunlardır:

ACSBlob, ACSQueue, ACSTable, Yönetim Portalı, ARM yönetici ARM ortak, KeyVault, KeyVaultInternal, ortak portalı

Klasör için Azure Active Directory Federasyon Hizmetleri kimlik sistemi dağıtımı olan yığınına gerekli:

ACSBlob, ACSQueue, ACSTable, ADFS, Admin portalı, ARM yönetici, ARM ortak, grafik, KeyVault, KeyVaultInternal, ortak portalı


|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |. \Certificates |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |


> -IncludePaaS  

PaaS Hizmetleri/ana bilgisayar adları için sertifika istekleri eklenip eklenmeyeceğini belirler.


|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |Adlı             |
|Varsayılan değer:              |False             |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |


> -ReportSections        

Ayrıntı raporu Özet, yalnızca göstermek için olup olmadığını atlar belirtir.
|  |  |
|----------------------------|---------|
|Şunu yazın:                       |Dize   |
|Konum:                   |Adlı    |
|Varsayılan değer:              |Tümü      |
|Geçerli değerler:               |'Sertifika', 'AzureRegistration', 'AzureIdentity', 'İşler', 'All' |
|Ardışık Düzen giriş kabul edin:      |False    |
|Joker karakterler kabul edin: |False    |


> -Özet 

Ayrıntı raporu Özet, yalnızca göstermek için olup olmadığını atlar belirtir.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Konum:                   |Adlı             |
|Varsayılan değer:              |False             |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |


> -CleanReport  

Önceki yürütme ve doğrulama geçmişi kaldırır ve yeni bir rapor doğrulamaları yazar.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |Adlı             |
|Varsayılan değer:              |False             |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |


> -OutputPath    

Hazırlık JSON raporunu ve ayrıntılı günlük dosyası kaydetmek için özel yolunu belirtir.  Yolu zaten mevcut değilse aracı dizin oluşturmayı deneyecek.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |Dize            |
|Konum:                   |Adlı             |
|Varsayılan değer:              |$ENV: TEMP\AzsReadinessChecker  |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |


> -Onayla  

Cmdlet'i çalıştırmadan önce onay ister.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |cf                |
|Konum:                   |Adlı             |
|Varsayılan değer:              |False             |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |


> -WhatIf  

Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz.
|  |  |
|----------------------------|------------------|
|Şunu yazın:                       |SwitchParameter   |
|Diğer adlar:                    |Wi                |
|Konum:                   |Adlı             |
|Varsayılan değer:              |False             |
|Ardışık Düzen giriş kabul edin:      |False             |
|Joker karakterler kabul edin: |False             |

 
