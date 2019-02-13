---
title: Azure Stack doğrulama aracını | Microsoft Docs
description: Azure Stack'te tanılama günlük dosyaları toplamak nasıl.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: adshar
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: eca66bc2e737b0f9a9954cad21a446e82d753f84
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203820"
---
# <a name="validate-azure-stack-system-state"></a>Azure Stack sistem durumu doğrulama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack operatörü, sistem durumunu ve sistem steğe bağlı durumunu belirlemek için gereklidir. Azure Stack doğrulama aracını (**Test AzureStack**) olanak tanıyan bir PowerShell cmdlet'i bir dizi test hataları tanımlamak için sisteminizde çalıştırılır. Genellikle bu aracı istenir [ayrıcalıklı uç noktasına (CESARETLENDİRİCİ)](azure-stack-privileged-endpoint.md) , sizinle Microsoft Müşteri Hizmetleri desteği (CSS) ile ilgili bir sorun. Sistem genelinde durumunu ve durum bilgilerini en el ile CSS toplamak ve ayrıntılı günlükleri analiz ederek, burada bir hata oluştu. ve sorunu çözmek için sizinle çalışma alanı odaklanmak yapabilirsiniz.

## <a name="running-the-validation-tool-and-accessing-results"></a>Doğrulama aracını çalıştırma ve sonuçları erişme

Daha önce belirtildiği gibi doğrulama aracını CESARETLENDİRİCİ çalıştırılır. Her test döndürür bir **GEÇER/başarısız** durumu PowerShell penceresinde. Ayrıca, ayrıntılı bir HTML rapor sırasında daha sonra erişilebilen oluşturulduğunda [oturum koleksiyonu](azure-stack-diagnostics.md). Aşağıda, test etme işleminin uçtan uca doğrulama özetini verilmiştir: 

1. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) erişin. CESARETLENDİRİCİ oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > ASDK konak bilgisayardaki CESARETLENDİRİCİ erişmek için ERCS01 AzS - ComputerName için kullanın.

2. İçinde CESARETLENDİRİCİ olduktan sonra çalıştırın: 

   ```powershell
   Test-AzureStack
   ```

   Başvurmak [parametresi konuları](azure-stack-diagnostic-test.md#parameter-considerations) ve [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümlerde daha fazla bilgi için.

3. Aşağıdakilerden sınamaktadır rapor **başarısız**çalıştırın:

   ```powershell
   Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath "<path>" -OutputShareCredential $cred
   ```

   Cmdlet Test-AzureStack tarafından oluşturulan günlükleri toplar. Tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure Stack'te tanılama araçları](azure-stack-diagnostics.md). Günlükleri toplayın veya gerekir CSS başvurun testleri rapor **UYAR**.

4. CSS tarafından doğrulama aracını çalıştırmak için istendiyse, sorun giderme devam etmek için toplanan günlüklerde CSS temsilcisi ister.

## <a name="tests-available"></a>Testleri kullanılabilir

Doğrulama Aracı, bir dizi sistem düzeyindeki test çalıştırmanıza olanak tanır ve geçerli bir öngörü sağlayan temel bulut senaryolarına durum ve sisteminizdeki sorunları belirlemek.

### <a name="cloud-infrastructure-tests"></a>Bulut altyapısı testleri

Bu düşük etki testleri, bir altyapı düzeyinde çalışmak ve çeşitli sistem bileşenleri ve işlevleri hakkında bilgi sağlar. Şu anda testleri aşağıdaki kategorilere ayrılır:

| Test kategorisi                                        | Bağımsız değişken-içerir ve - yoksay |
| :--------------------------------------------------- | :-------------------------------- |
| Azure Stack uyarı özeti                            | AzsAlertSummary                   |
| Azure Stack yedekleme paylaşımına erişilebilirlik özeti       | AzsBackupShareAccessibility       |
| Azure Stack denetim düzlemi özeti                    | AzsControlPlane                   |
| Azure Stack Defender özeti                         | AzsDefenderSummary                |
| Azure Stack altyapısını bellenim özeti barındırma  | AzsHostingInfraFWSummary          |
| Azure Stack bulut barındırma altyapısını özeti     | AzsHostingInfraSummary            |
| Azure Stack bulut barındırma altyapısını kullanımı | AzsHostingInfraUtilization        |
| Azure Stack altyapısını kapasite                  | AzsInfraCapacity                  |
| Azure Stack altyapısı performansı               | AzsInfraPerformance               |
| Azure Stack altyapısını Rol Özeti              | AzsInfraRoleSummary               |
| Azure Stack güncelleştirme özeti                           | AzsInfraUpdateSummary             |
| Azure Stack Portal ve API özeti                   | AzsPortalAPISummary               |
| Azure Stack ölçek birimi VM olayları                     | AzsScaleUnitEvents                |
| Azure Stack ölçek birimi VM kaynakları                  | AzsScaleUnitResources             |
| Azure Stack SDN doğrulama özeti                   | AzsSDNValidation                  |
| Azure Stack Service Fabric Rol Özeti              | AzsSFRoleSummary                  |
| Azure Stack BMC özeti                              | AzsStampBMCSummary                |
| Azure Stack depolama hizmetler özeti                 | AzsStorageSvcsSummary             |
| Azure Stack SQL Store özeti                        | AzsStoreSummary                   |
| Azure Stack VM yerleştirme özeti                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Bulut senaryosu testleri

Yukarıdaki altyapı testleri yanı sıra ayrıca altyapı bileşenlerinde işlevselliği kontrol etmek için bulut senaryosu testleri çalıştırma olanağı vardır. Bulut Yöneticisi kimlik bilgileri, kaynak dağıtımını ilgili olarak bu testleri çalıştırmak için gereklidir. 
    > [!NOTE]
    > Currently you cannot run cloud scenario tests using Active Directory Federated Services (AD FS) credentials. 

Aşağıdaki bulut senaryoları doğrulama aracı tarafından test edilen:
- Kaynak grubu oluşturma   
- Plan oluşturma              
- Teklif oluşturma            
- Depolama hesabı oluşturma   
- Sanal makine oluşturma 
- BLOB depolama işlemi   
- Kuyruk depolama işlemi  
- Tablo depolama işlemi  

## <a name="parameter-considerations"></a>Parametre konuları

- Parametre **listesi** tüm kullanılabilir test kategorileri görüntülemek için kullanılabilir.

- Parametreleri **INCLUDE** ve **Yoksay** dahil edilecek veya hariç test kategorileri için kullanılabilir. Bkz: [testleri kullanılabilir](azure-stack-diagnostic-test.md#tests-available) bölümü bu bağımsız değişkenler ile kullanılması için Toplu özellik hakkında daha fazla bilgi için.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- VM bir parçası dağıtılan bir kiracı, bulut senaryosu sınar. Kullanabileceğiniz **DoNotDeployTenantVm** bu devre dışı bırakmak için. 

- Sağlamanız gereken **ServiceAdminCredential** açıklandığı gibi bulut senaryosu testleri çalıştırmak için parametre [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümü.

- **BackupSharePath** ve **BackupShareCredential** altyapısını Yedekleme ayarlarını gösterildiği gibi test ederken kullanılan [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümü.

- Doğrulama Aracı Ayrıca ortak PowerShell parametrelerini destekler: Ayrıntılı, hata ayıklama, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable ve OutVariable. Daha fazla bilgi için [ortak parametreleri hakkında](https://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Büyük/küçük harf örneklerini kullanın

### <a name="run-validation-without-cloud-scenarios"></a>Doğrulama bulut senaryoları olmadan çalıştırın

Olmadan doğrulama aracını çalıştırma **ServiceAdminCredential** bulut senaryosu testleri atlamak için parametre: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Doğrulama ile bulut senaryoları çalıştırın

Doğrulama Aracı ile sağlama **ServiceAdminCredentials** parametre varsayılan olarak bulut senaryosu testler çalıştırır: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

YALNIZCA bulut senaryolarına kalan testler çalıştırmadan çalıştırmak istiyorsanız, kullanabileceğiniz **INCLUDE** Bunu yapmak için parametre: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

Bulut yönetici kullanıcı adı UPN biçiminde yazılmalıdır: serviceadmin@contoso.onmicrosoft.com (Azure AD). İstendiğinde, bulut yönetici hesabının parolasını yazın.

### <a name="run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix"></a>Sistem hazırlığı güncelleştirme veya düzeltme yüklemeden önce test etmek için doğrulama aracını çalıştırma

Bir güncelleştirme veya düzeltme yüklemesini başlatmadan önce Azure Stack durumunu denetlemek için doğrulama aracını çalıştırma:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Altyapı Yedekleme ayarlarını test etmek için doğrulama aracını çalıştırma

*Önce* altyapı yedeklemeyi yapılandırma, yedekleme paylaşım yolu test edebilir ve kullanarak kimlik bilgisi **AzsBackupShareAccessibility** test: 

  ```powershell
  New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
  ```

*Sonra* çalıştırabileceğiniz yedeklemeyi yapılandırma, **AzsBackupShareAccessibility** paylaşım doğrulamak için ERCS erişilebilir:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Yeni kimlik bilgileriyle yapılandırılmış yedekleme paylaşımına test etmek için çalıştırın: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Sonraki adımlar

Azure Stack'te tanılama araçları ve sorunu günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Azure Stack'te tanılama araçları](azure-stack-diagnostics.md).

Sorun giderme hakkında daha fazla bilgi için bkz: [Microsoft Azure Stack sorunlarını giderme](azure-stack-troubleshooting.md).