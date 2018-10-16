---
title: Azure Stack'te bir doğrulama sınamasını çalıştırmanızı | Microsoft Docs
description: Azure Stack'te tanılama günlük dosyaları toplamak nasıl.
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 10/15/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 3f4dc6e4136d8d2e3eb1ca5e822306aae2217e3b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340860"
---
# <a name="run-a-validation-test-for-azure-stack"></a>Azure Stack için bir doğrulama testi Çalıştır

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*
 
Azure Stack durumunu doğrulayabilirsiniz. Bir sorun varsa, Microsoft Müşteri Hizmetleri desteği ile iletişime geçin. Destek çalıştırmanızı isteyen **Test AzureStack** , yönetim düğümünden. Doğrulama testi başarısız yalıtır. Destek sonra ayrıntılı günlükleri analiz etmek, hatanın oluştuğu alan odaklanın ve çalışmak istediğiniz sorun giderme ile.

## <a name="run-test-azurestack"></a>Test-AzureStack çalıştırın

Bir sorun varsa, Microsoft Müşteri Hizmetleri desteği ile iletişime geçin ve ardından çalıştırın **çalıştırma Test AzureStack**.

1. Bir sorun var.
2. Kişi Microsoft Müşteri Destek Hizmetleri.
3. Çalıştırma **Test AzureStack** ayrıcalıklı uç noktasından.
    1. Ayrıcalıklı uç noktaya erişebilirsiniz. Yönergeler için [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md). 
    2. Yönetim ana bilgisayarı olarak ASDK üzerinde oturum **AzureStack\CloudAdmin**.  
    Tümleşik bir sistem üzerinde ayrıcalıklı-uç-noktası için OEM donanım satıcınız tarafından sağlanan Yönetim için IP adresini kullanmanız gerekir.
    3. PowerShell'i yönetici olarak açın.
    4. Çalıştırın: `Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint`
    5. Çalıştırın: `Test-AzureStack`
4. Herhangi bir test başarısız olarak rapor çalıştırırsanız: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` cmdlet Test-AzureStack günlükleri toplar. Tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure Stack'te tanılama araçları](azure-stack-diagnostics.md).
5. Gönderme **SeedRing** günlükleri Microsoft Müşteri Hizmetleri desteği. Microsoft Müşteri Hizmetleri desteği sorunu çözmek için sizinle birlikte çalışır.

## <a name="reference-for-test-azurestack"></a>Test-AzureStack için başvuru

Bu bölüm, Test AzureStack cmdlet ve doğrulama raporu özeti için genel bir bakış içerir.

### <a name="test-azurestack"></a>Test-AzureStack

Azure Stack durumunu doğrular. Cmdlet'i, Azure Stack donanımı ve yazılımı durumunu raporlar. Destek personeli, Azure Stack desteği durumları çözümleme süresini azaltmak için bu raporu kullanabilirsiniz.

> [!Note]  
> **Test-AzureStack** tek bir disk veya bir tek bir fiziksel ana düğüm hatasından başarısız oldu gibi bulut kesintilerine sonuçlanmadığını hatalarını algılayabilir.

#### <a name="syntax"></a>Sözdizimi

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parametreler

| Parametre               | Değer           | Gerekli | Varsayılan |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | Dize    | Hayır       | FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Hayır       | FALSE   |
| AdminCredential         | PSCredential    | Hayır       | NA      |
| Liste                    | SwitchParameter | Hayır       | FALSE   |
| Yoksayma                  | Dize          | Hayır       | NA      |
| Ekle                 | Dize          | Hayır       | NA      |
| BackupSharePath         | Dize          | Hayır       | NA      |
| BackupShareCredential   | PSCredential    | Hayır       | NA      |


Test AzureStack cmdlet genel parametreleri destekler: ayrıntılı, hata ayıklama, ErrorAction ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable ve OutVariable. Daha fazla bilgi için [ortak parametreleri hakkında](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Test-AzureStack örnekleri

Aşağıdaki örneklerde, oturum açtınız olarak varsayılmaktadır **CloudAdmin** ve ayrıcalıklı uç noktası (CESARETLENDİRİCİ) erişim. Yönergeler için [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Test-AzureStack bulut senaryoları etkileşimli olarak çalıştır

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Test-AzureStack bulut senaryoları ile çalıştırın.

Kullanabileceğiniz **Test AzureStack** bulut senaryolarına karşı Azure Stack çalıştırılacak. Bu senaryolar şunlardır:

 - Kaynak grupları oluşturma
 - Planları oluşturma
 - Teklifler oluşturma
 - Depolama hesabı oluşturma
 - Bir sanal makine oluşturma
 - Testi senaryosunda oluşturduğunuz depolama hesabına kullanarak blob işlemleri
 - Testi senaryosunda oluşturduğunuz depolama hesabıyla kuyruk işlemleri gerçekleştirin
 - Testi senaryosunda oluşturduğunuz depolama hesabına kullanarak tablo işlemleri gerçekleştirme

Bulut senaryosu, bulut Yöneticisi kimlik bilgileri gerektirir. 
> [!Note]  
> Active Directory Federasyon Hizmetleri'nde (AD FS) kimlik bilgilerini kullanarak bulut senaryolarına çalıştıramazsınız. **Test AzureStack** cmdlet'i, yalnızca CESARETLENDİRİCİ erişilebilir. Ancak, CESARETLENDİRİCİ AD FS kimlik bilgilerini desteklemez.

Bulut yönetici kullanıcı adı UPN biçiminde yazın serviceadmin@contoso.onmicrosoft.com (Azure AD). İstendiğinde, bulut yönetici hesabının parolasını yazın.

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Test-AzureStack bulut senaryoları çalıştırın

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Kullanılabilir test senaryolar listelenmiştir:

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Belirtilen test çalıştırması

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Belirli testleri hariç tutmak için:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="run-test-azurestack-to-test-infrastructure-backup-settings"></a>Altyapı Yedekleme ayarlarını test etmek için test-AzureStack çalıştırın

Altyapı yedeklemeyi yapılandırmadan önce yedekleme paylaşım yolu test edebilir ve kullanarak kimlik bilgisi **AzsBackupShareAccessibility** test edin.

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
````
Çalıştırabileceğiniz yedekleme yapılandırdıktan sonra paylaşıma doğrulamak için AzsBackupShareAccessibility CESARETLENDİRİCİ oturumundan çalıştırın ERCS aracılığıyla erişilebilir:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility
````

Test etmek için yeni kimlik bilgileriyle yapılandırılmış yedekleme paylaşımına CESARETLENDİRİCİ oturumundan çalıştırın:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential <PSCredential for backup share>
````

### <a name="validation-test"></a>Doğrulama testi

Aşağıdaki tabloda özetlenmiştir tarafından çalıştırılan doğrulama testlerini **Test AzureStack**.

| Ad                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Azure Stack bulut barındırma altyapısını özeti                                                                                  |
| Azure Stack depolama hizmetler özeti                                                                                              |
| Azure Stack altyapısını rol örneği özeti                                                                                  |
| Azure Stack bulut barındırma altyapısını kullanımı                                                                              |
| Azure Stack altyapısını kapasite                                                                                               |
| Azure Stack Portal ve API özeti                                                                                                |
| Azure Stack, Azure Resource Manager Sertifika Özeti                                                                                               |
| Altyapı yönetim denetleyicisi, Ağ denetleyicisi, depolama hizmetleri ve ayrıcalıklı uç altyapısı rollerinin          |
| Altyapı yönetim denetleyicisi, Ağ denetleyicisi, depolama hizmetleri ve ayrıcalıklı uç nokta altyapı rol örnekleri |
| Azure Stack altyapısını Rol Özeti                                                                                           |
| Azure Stack bulut Service Fabric Hizmetleri                                                                                         |
| Azure Stack altyapısı rol örneği performansı                                                                              |
| Azure Stack bulut ana performans özeti                                                                                        |
| Azure Stack hizmet kaynak tüketimi özeti                                                                                  |
| Azure Stack ölçek birimi kritik olayları (son 8 saat)                                                                             |
| Azure Stack depolama hizmetlerini fiziksel diskler özeti                                                                               |
|Azure Stack yedekleme paylaşımına erişilebilirlik özeti                                                                                     |

## <a name="next-steps"></a>Sonraki adımlar

 - Azure Stack'te tanılama araçları ve sorunu günlüğe kaydetme hakkında daha fazla bilgi için bkz: [ Azure Stack'te tanılama araçları](azure-stack-diagnostics.md).
 - Sorun giderme hakkında daha fazla bilgi için bkz: [Microsoft Azure Stack sorunlarını giderme](azure-stack-troubleshooting.md)
