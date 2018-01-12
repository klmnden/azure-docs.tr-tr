---
title: "Azure yığınında bir doğrulama sınamasını çalıştırmanızı | Microsoft Docs"
description: "Azure yığınında tanılama günlük dosyaları toplamak nasıl"
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.assetid: D44641CB-BF3C-46FE-BCF1-D7F7E1D01AFA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: mabrigg
ms.openlocfilehash: 53ef19628b40c4a008143c867c9e7867ac91854d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="run-a-validation-test-for-azure-stack"></a>Azure yığını için bir doğrulama testi çalıştırma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*
 
Azure yığın durumunu doğrulayabilirsiniz. Bir sorun varsa, Microsoft Müşteri Hizmetleri desteği ile iletişime geçin. Destek, Test AzureStack yönetim düğümden çalıştırmak ister. Doğrulama testi başarısız yalıtır. Destek sonra ayrıntılı günlüklerini analiz edin, odak hatanın oluştuğu alanı ve çalışma size sorunu çözme.

## <a name="run-test-azurestack"></a>Test AzureStack çalıştırma

Bir sorun varsa, Microsoft Müşteri Hizmetleri desteği ile iletişime geçin ve ardından çalıştırın **çalıştırmak Test AzureStack**.

1. Bir sorun var.
2. Kişi Microsoft Müşteri Destek Hizmetleri.
3. Çalıştırma **Test AzureStack** ayrıcalıklı uç noktasından.
    1. Ayrıcalıklı uç noktasına erişmek. Yönergeler için bkz: [Azure yığınında kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md). 
    2. Olarak oturum **AzureStack\CloudAdmin** yönetim konaktaki.
    3. PowerShell'i yönetici olarak açın.
    4. Çalıştırın:`Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint`
    5. Çalıştırın:`Test-AzureStack`
4. Herhangi bir test başarısız rapor çalıştırırsanız: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` cmdlet günlükleri Test-AzureStack toplar. Tanılama günlükleri hakkında daha fazla bilgi için bkz: [Azure yığın tanılama araçları](azure-stack-diagnostics.md).
5. Gönderme **SeedRing** günlükleri Microsoft Müşteri Hizmetleri desteği. Microsoft Müşteri Hizmetleri desteği sorunu çözmek için sizinle birlikte çalışır.

## <a name="reference-for-test-azurestack"></a>Test-AzureStack için başvurusu

Bu bölüm, Test-AzureStack cmdlet'i ve doğrulama raporu özetini için genel bir bakış içerir.

### <a name="test-azurestack"></a>Test-AzureStack

Azure yığın durumunu doğrular. Cmdlet Azure yığın donanım ve yazılım durumunu raporlar. Destek personeli, Azure yığın destek durumları çözmek için gereken süreyi azaltmak için bu raporu kullanabilirsiniz.

> [!Note]  
> Tek bir disk veya bir tek fiziksel ana düğüm hatasından başarısız oldu gibi test AzureStack bulut kesilmelerini kaynaklanan değil hataları algılayabilir.

#### <a name="syntax"></a>Sözdizimi

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parametreler

| Parametre               | Değer           | Gerekli | Varsayılan |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | PSCredential    | Hayır       | FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Hayır       | FALSE   |
| AdminCredential         | PSCredential    | Hayır       | NA      |
| StorageConnectionString | Dize          | Hayır       | NA      |
| Liste                    | SwitchParameter | Hayır       | FALSE   |
| Yoksayma                  | Dize          | Hayır       | NA      |
| Ekle                 | Dize          | Hayır       | NA      |

Test-AzureStack cmdlet genel parametreleri destekler: ayrıntılı, hata ayıklama, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable ve OutVariable. Daha fazla bilgi için bkz: [hakkında ortak parametreler](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Test AzureStack örnekleri

Aşağıdaki örneklerde, oturum açtınız olarak varsayılmaktadır **CloudAdmin** ve ayrıcalıklı uç noktası (CESARETLENDİRİCİ) erişme. Yönergeler için bkz: [Azure yığınında kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Test AzureStack bulut senaryolarında etkileşimli olarak çalıştırma

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
      Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Test AzureStack bulut senaryoları ile Çalıştır

Bulut senaryosu, Azure yığın karşı çalıştırmak için Test AzureStack kullanabilirsiniz. Bu senaryolar şunlardır:

 - Kaynak grupları oluşturma
 - Planları oluşturma
 - Teklifler oluşturma
 - Depolama hesapları oluşturma
 - Bir sanal makine oluşturma
 - Testi senaryosunda oluşturulmuş depolama hesabı kullanarak blob işlemleri
 - Testi senaryosunda oluşturulmuş depolama hesabı kullanarak sırası işlemleri
 - Testi senaryosunda oluşturulmuş depolama hesabı kullanarak tablo işlemleri

Bulut senaryolarını bulut yönetici kimlik bilgileri gerektirir. 
> [!Note]  
> Active Directory Federasyon Hizmetleri (ADFS) kimlik bilgilerini kullanarak bulut senaryoları çalıştırılamıyor. **Test AzureStack** cmdlet'tir yalnızca CESARETLENDİRİCİ erişilebilir. Ancak CESARETLENDİRİCİ ADFS kimlik desteklemiyor.

Bulut yönetici kullanıcı adı UPN biçiminde yazın serviceadmin@contoso.onmicrosoft.com (AAD). İstendiğinde, bulut yönetici hesabı için parolayı yazın.

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Test AzureStack bulut senaryolarında çalıştırın

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Kullanılabilir test senaryoları listesi:

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Belirtilen testi çalıştırma

Bir CESARETLENDİRİCİ oturumunda çalıştırın:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Belirli testleri dışlamak için:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="validation-test"></a>Doğrulama testi

Aşağıdaki tablo doğrulama testleri Test-AzureStack tarafından Çalıştır özetler.

| Ad                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Azure yığın bulut altyapısı özeti barındırma                                                                                  |
| Azure yığın depolama hizmetler özeti                                                                                              |
| Azure yığın altyapı rol örneği özeti                                                                                  |
| Azure yığın bulut altyapısı kullanımı barındırma                                                                              |
| Azure yığın altyapı kapasite                                                                                               |
| Azure yığın Portal ve API özeti                                                                                                |
| Azure yığın Azure Resource Manager Sertifika Özeti                                                                                               |
| Altyapı Yönetimi Denetleyicisi, Ağ denetleyicisi, depolama hizmetleri ve ayrıcalıklı uç altyapısı rollerinin          |
| Altyapı Yönetimi Denetleyicisi, Ağ denetleyicisi, depolama hizmetleri ve ayrıcalıklı uç altyapısı rol örnekleri |
| Azure yığın altyapı rolünü özeti                                                                                           |
| Azure yığın bulut hizmeti doku Hizmetleri                                                                                         |
| Azure yığın altyapısı rol örneği performansı                                                                              |
| Azure yığın bulut ana bilgisayar performansı özeti                                                                                        |
| Azure yığın hizmet kaynak tüketimi özeti                                                                                  |
| Azure yığın ölçek birimi kritik olayları (son 8 saat)                                                                             |
| Azure yığın depolama hizmetleri fiziksel diskler özeti                                                                               |

## <a name="next-steps"></a>Sonraki adımlar

 - Azure yığın tanılama araçları ve sorunu günlüğe kaydetme hakkında daha fazla bilgi için bkz: [ Azure yığın tanılama araçları](azure-stack-diagnostics.md).
 - Sorun giderme hakkında daha fazla bilgi için bkz: [Microsoft Azure yığın sorunlarını giderme](azure-stack-troubleshooting.md)