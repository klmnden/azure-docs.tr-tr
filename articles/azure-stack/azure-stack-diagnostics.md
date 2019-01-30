---
title: Azure Stack’te tanılama
description: Azure Stack'te tanılama günlük dosyaları toplamak nasıl
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: bd1994aca3dbbc23977b01d3511f87b5ec08b96d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251869"
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack tanılama araçları

Azure Stack, birlikte çalışan ve birbiriyle etkileşim bileşenleri büyük bir koleksiyonudur. Bu bileşenler, kendi benzersiz günlükler oluşturur. Bu, özellikle birden çok Azure Stack bileşenleri etkileşim gelen hatalara zor bir görev tanılama sorunlarını hale getirebilirsiniz. 

Tanılama araçlarımızı kolay ve etkili günlük toplama mekanizması olduğundan emin olun yardımcı olur. Aşağıdaki diyagramda gösterildiği Azure Stack iş verileri toplama araçları nasıl oturum açın:

![Azure Stack tanılama araçları](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Toplayıcısı izleme
 
İzleme toplayıcı, varsayılan olarak etkindir ve sürekli olarak Azure Stack bileşeni hizmetlerinden tüm olay izleme için Windows (ETW) günlüklerini toplamak için arka planda çalıştırır. ETW günlükleri beş gün yaş sınırı sahip ortak bir yerel paylaşıma depolanır. Bu sınıra ulaşıldığında, yeni oluşturulan eski dosyalar silinir. Her dosya için geçerli varsayılan en büyük boyutu 200 MB'dir. Bir boyut denetimi 2 dakikada bir gerçekleşir ve geçerli dosya > 200 MB = kaydedildikten ve yeni bir dosya oluşturulur. Ayrıca bir 8 GB sınırı yoktur olay oturumu oluşturulan toplam dosya boyutu. 

## <a name="log-collection-tool"></a>Günlük toplama aracı
 
PowerShell cmdlet **Get-AzureStackLog** Azure Stack ortamında tüm bileşenleri günlükleri toplamak için kullanılabilir. Bu kullanıcı tarafından tanımlanan bir konumda zip dosyaları kaydeder. Azure yığını teknik destek ekibinden bir sorunu gidermeye yardımcı olmak için günlüklerinizi gerekiyorsa, bunlar, bu aracı çalıştırmak için isteyebilir.

> [!CAUTION]
> Bu günlük dosyaları, kişisel bilgileri (PII) içerebilir. Genel olarak tüm günlük dosyalarını göndermeden önce bunu dikkate alın.
 
Toplanan bazı örnek günlük türleri şunlardır:
*   **Azure Stack dağıtım günlükleri**
*   **Windows olay günlükleri**
*   **Panther günlükleri**
*   **Küme günlükleri**
*   **Depolama, tanılama günlükleri**
*   **ETW günlükleri**

Bu dosyalar toplanan ve bir paylaşımı izleme toplayıcısı tarafından kaydedildi. **Get-AzureStackLog** PowerShell cmdlet'i ardından bunları gerekli olduğunda toplamak için kullanılabilir.

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Get-AzureStackLog Azure Stack'te çalıştırılacak tümleşik sistemleri 
Günlük toplama aracı, tümleşik bir sistemde çalıştırmak için erişim için ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) olması gerekir. Tümleşik bir sistemde günlükleri toplamak için CESARETLENDİRİCİ kullanarak çalıştırabileceğiniz bir örnek betiği şu şekildedir:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```



### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Get-AzureStackLog bir Azure Stack geliştirme Seti'ni (ASDK) sistem üzerinde çalışacak şekilde
Get-AzureStackLog bir ASDK ana bilgisayarda çalıştırmak için aşağıdaki adımları kullanın.

1. Olarak oturum **AzureStack\CloudAdmin** ASDK ana bilgisayarda.
2. Yönetici olarak yeni bir PowerShell penceresi açın.
3. Çalıştırma **Get-AzureStackLog** PowerShell cmdlet'i.

**Örnekler:**

  Tüm roller için tüm günlükleri toplayın:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred
  ```

  Günlükleri VirtualMachines ve BareMetal rollerdeki verileri toplar:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal
  ```

  Günlük VirtualMachines ve BareMetal rollerden son 8 saat boyunca günlük dosyaları için filtreleme tarihi olan toplama:
    
  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Günlük VirtualMachines ve BareMetal rollerden 8 saat önce önce 2 saat arasındaki zaman aralığı için günlük dosyaları için filtreleme tarihi olan toplama:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>ASDK hem tümleşik sistemler için parametre konuları

- Parametreleri **OutputSharePath** ve **OutputShareCredential** belirtilen bir kullanıcı günlüklerini depolamak için kullanılan konum.

- **FromDate** ve **ToDate** parametreleri, belirli bir süre için günlükleri toplamak için kullanılabilir. Günlükleri son dört saat boyunca, bu parametre belirtilmezse, varsayılan olarak toplanır.

- Kullanım **FilterByNode** günlükleri bilgisayar adına göre filtre uygulamak için parametre. Örneğin:

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByNode azs-xrp01
    ```
- Kullanım **FilterByLogType** günlükleri türüne göre filtrelemek için parametre. Dosya, paylaşım veya WindowsEvent göre filtrelemek seçebilirsiniz. Örneğin:

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByLogType File
    ```
- Kullanabileceğiniz **TimeOutInMinutes** parametresini kullanarak günlük toplama için zaman aşımını ayarlayın. 150 (2,5 saat) için varsayılan olarak ayarlanır.
- Bilgi döküm dosyası günlük koleksiyonu, varsayılan olarak devre dışıdır. Bunu etkinleştirmek için **IncludeDumpFile** parametresi geçin. 
- Şu anda kullanabileceğiniz **FilterByRole** aşağıdaki rolleri tarafından filtre oturum koleksiyonuna parametre:

 |   |   |   |    |
 | - | - | - | -  |   
 |ACS                   |CacheService                   |IBC                            |OEM|
 |ACSDownloadService    |İşlem                        |InfraServiceController         |OnboardRP|
 |ACSFabric             |CPI                            |KeyVaultAdminResourceProvider  |PXE|
 |ACSFrontEnd           |CRP                            |KeyVaultControlPlane           |QueryServiceCoordinator|
 |ACSMetrics            |DeploymentMachine              |KeyVaultDataPlane              |QueryServiceWorker|
 |ACSMigrationService   |DiskRP                         |KeyVaultInternalControlPlane   |SeedRing|
 |ACSMonitoringService  |Domain                         |KeyVaultInternalDataPlane      |SeedRingServices|
 |ACSSettingsService    |ECE                            |KeyVaultNamingService          |SLB|
 |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL|
 |ACSTableServer        |EventRP                        |MetricsAdminRP                 |SRP   |
 |ACSWac                |ExternalDNS                    |MetricsRP                      |Depolama|
 |ADFS                  |FabricRing                     |MetricsServer                  |StorageController   |
 |ApplicationController |FabricRingServices             |MetricsStoreService            |URP   |
 |ASAppGateway          |FirstTierAggregationService    |MonAdminRP                     |UsageBridge|
 |AzureBridge           |FRP                            |MonRP                          |virtualMachines   |
 |AzureMonitor          |Ağ geçidi                        |NC                             |OLUŞTU|
 |BareMetal             |Ögesi               |NonPrivilegedAppGateway        |WASPUBLIC|
 |BRP                   |HintingServiceV2               |NRP                            |   |
 |CA                    |HRP                            |OboService                     |   |
 |   |   |   |    |

### <a name="additional-considerations"></a>Diğer konular

* Komutu, günlükleri toplamaya hangi rolleri üzerinde göre çalıştırılacak biraz zaman alabilir. Faktörlere günlük toplama ve Azure Stack ortamında düğümleri sayısı için belirtilen süreyi de içerir.
* Oturum koleksiyonu çalıştırmaları gibi oluşturduğunuz yeni klasör denetleyin **OutputSharePath** komutunda belirtilen parametre.
* Her rolün içindeki tek bir ZIP dosyaları kendi günlükleri vardır. Toplanan günlükler boyutuna bağlı olarak, bir rol içinde birden çok zip dosyaları bölme günlüklerinin olabilir. İçinde tek bir klasöre sıkıştırması açılan tüm günlük dosyalarını olmasını istiyorsanız, bu tür bir rol için toplu (örneğin, 7zip) sıkıştırılmış dosyayı açabilirsiniz bir araç kullanın. Rolü için tüm sıkıştırılmış dosyalar seçip **buradan ayıklamak**. Bu, bu rolün tek ve birleştirilmiş bir klasördeki tüm günlük dosyalarını unzips.
* Dosya adında **Get-AzureStackLog_Output.log** da sıkıştırılmış günlük dosyalarını içeren klasörde oluşturulur. Günlük toplama sırasında sorunları gidermek için kullanılabilir komut çıktısını bir günlük dosyasıdır. Bazen günlük dosyası içeren `PS>TerminatingError` beklenen günlük dosyaları sonra eksik olduğu sürece, güvenli bir şekilde, göz ardı edilebilir girişler oturum koleksiyonu çalıştırır.
* Belirli bir kültür araştırmak için birden fazla bileşenden günlükleri gerekli olabilir.
    -   İçinde sistem ve tüm altyapı Vm'leri için olay günlükleri toplanır *VirtualMachines* rol.
    -   İçinde sistem ve tüm konaklar için olay günlükleri toplanır *BareMetal* rol.
    -   Yük devretme kümesi ve Hyper-V olay günlükleri toplanır *depolama* rol.
    -   ACS günlükleri toplanır *depolama* ve *ACS* rolleri.

> [!NOTE]
> Boyutu ve yaş sınırı, günlükleriyle yığılma katılmaz emin olmak için depolama alanınızı etkili kullanımını sağlamak için temel olarak toplanan günlüklerde uygulanır. Ancak, bir sorun özel durumunda tanılama yaparken, bazen bu limitleri nedeniyle bulunmayabilir günlükleri gerekir. Bu nedenle, **kesinlikle önerilir** günlüklerinizi bir dış depolama alanı (Azure depolama hesabında, ek şirket içi depolama cihazı vb.) için yük boşaltma 8 ila 12 saatte bir ve orada bağlı olarak, 1-3 ay tutun, gereksinimleri. Ayrıca, bu depolama konumu şifrelendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Azure Stack sorunlarını giderme](azure-stack-troubleshooting.md)

