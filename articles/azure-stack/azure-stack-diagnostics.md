---
title: "Azure Stack’te tanılama"
description: "Azure yığınında tanılama günlük dosyaları toplamak nasıl"
services: azure-stack
author: adshar
manager: byronr
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 10/2/2017
ms.author: adshar
ms.openlocfilehash: 9b1fbbf63ddd8bac2c1a76bbcd5daca69e2513f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-diagnostics-tools"></a>Azure yığın tanılama araçları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*
 
Azure yığın birlikte çalışan ve birbiriyle etkileşim bileşenleri büyük bir koleksiyonudur. Tüm bu bileşenlerin kendi benzersiz günlükler oluşturur. Bu, özellikle birden çok etkileşen Azure yığın bileşenlerinden gelen hataları zor bir görev tanılama sorunları hale getirebilirsiniz. 

Bizim tanılama araçları, kolay ve verimli günlüğü koleksiyonu mekanizması sağlamaya yardımcı olur. Aşağıdaki diyagramda gösterildiği toplama araçları Azure yığın işlerinde nasıl oturum:

![Günlük toplama araçları](media/azure-stack-diagnostics/image01.png)
 
 
## <a name="trace-collector"></a>Trace Toplayıcı
 
Trace Toplayıcı varsayılan olarak etkindir. Sürekli olarak arka planda çalışır tüm olay izleme için Windows (ETW) günlüklerini bileşen hizmetlerinden Azure yığında toplar ve bunları ortak bir yerel paylaşımında depolar. 

Trace Toplayıcı hakkında bilinmesi gereken önemli şeyler şunlardır:
 
* Trace Toplayıcı, varsayılan boyutu sınırları ile sürekli olarak çalışır. En büyük boyutu izin verilen her dosya (200 MB) için varsayılan **değil** kesme boyutu. Bir boyut denetimi düzenli aralıklarla oluşur (şu anda 2 dakikada bir) ve geçerli dosya > = 200 MB kaydedilir ve yeni bir dosya oluşturulur. Ayrıca bir 8 GB (yapılandırılabilir) sınırı yoktur olay oturumu oluşturulan toplam dosya boyutu. Bu sınıra ulaşıldığında, yeni bir tane oluşturuldukça eski dosyalar silinir.
* Günlükleri, 5 gün yaş sınırı yoktur. Bu sınır de yapılandırılabilir. 
* Her bileşen bir JSON dosyası aracılığıyla izleme yapılandırma özelliklerini tanımlar. JSON dosyaları depolanmış `C:\TraceCollector\Configuration`. Gerekirse, toplanan günlükleri yaş ve boyutu sınırları değiştirmek için bu dosyalar düzenlenebilir. Bu dosyalardaki değişiklikler yeniden başlatılmasını gerektiren *Microsoft Azure yığın izleme toplayıcı* değişikliklerin etkili olması hizmet.
* Aşağıdaki örnekte bir XRP VM'den FabricRingServices işlemleri için izleme yapılandırma JSON dosyasıdır: 

```
{
    "LogFile": 
    {
        "SessionName": "FabricRingServicesOperationsLogSession",
        "FileName": "\\\\SU1FileServer\\SU1_ManagementLibrary_1\\Diagnostics\\FabricRingServices\\Operations\\AzureStack.Common.Infrastructure.Operations.etl",
        "RollTimeStamp": "00:00:00",
        "MaxDaysOfFiles": "5",
        "MaxSizeInMB": "200",
        "TotalSizeInMB": "5120"
    },
    "EventSources":
    [
        {"Name": "Microsoft-AzureStack-Common-Infrastructure-ResourceManager" },
        {"Name": "Microsoft-OperationManager-EventSource" },
        {"Name": "Microsoft-Operation-EventSource" }
    ]
}
```

* **MaxDaysOfFiles**

    Bu parametre korumak için dosya yaşı denetler. Eski günlük dosyalarını silinir.
* **Parametresinden**

    Bu parametre için tek bir dosya boyutu eşiğini denetler. Boyut üst sınırına ulaşıldığında, yeni bir .etl dosyası oluşturulur.
* **TotalSizeInMB**

    Bu parametre bir olay oturumundan oluşturulan .etl dosyaları toplam boyutunu denetler. Toplam dosya boyutu bu parametresi değerinden büyükse, eski dosyalar silinir.
  
## <a name="log-collection-tool"></a>Günlük koleksiyonu aracı
 
PowerShell komut `Get-AzureStackLog` Azure yığın ortamında tüm bileşenleri günlükleri toplamak için kullanılabilir. Bu kullanıcı tanımlı bir konumda ZIP dosyaları kaydeder. Teknik Destek ekibimiz sorunu gidermenize yardımcı olması için günlüklerinizi gerekiyorsa, bunlar, bu aracı çalıştırmanızı isteyebilir.

> [!CAUTION]
> Bu günlük dosyaları, kişisel bilgileri (PII) içerebilir. Genel olarak tüm günlük dosyalarını sonrası önce bu dikkate alın.
 
Toplanan bazı örnek günlük türleri şunlardır:
*   **Azure yığın dağıtım günlükleri**
*   **Windows olay günlükleri**
*   **Panther günlükleri**

   VM oluşturma sorunları gidermek için:
*   **Küme günlükleri**
*   **Depolama tanılama günlükleri**
*   **ETW günlükleri**

Bu dosyalar izleme toplayıcısı tarafından toplanan ve nerede bir paylaşımda depolanan `Get-AzureStackLog` bunları alır.
 
**Get-AzureStackLog bir Azure yığın Geliştirme Seti (ASDK) sistemde çalıştırmak için**
1.  Ana bilgisayarda AzureStack\AzureStackAdmin oturum açın.
2.  Bir yönetici olarak bir PowerShell penceresi açın.
3.  `Get-AzureStackLog` öğesini çalıştırın.  

    **Örnekler**

    - Tüm rolleri için tüm günlükleri toplama:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs`

    - VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal`

    - Tarihi geçmiş 8 saat için günlük dosyalarını filtreleme ile VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)`

    - 8 saat önce 2 saat önce arasındaki zaman aralığı için günlük dosyaları için filtreleme tarihle VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:

      `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)`

**Get-AzureStackLog bir Azure yığın üzerinde çalıştırmak için sistem tümleşik:**

Tümleşik bir sistemde oturum koleksiyonu aracını çalıştırmak için erişim için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) olması gerekir. Tümleşik bir sistemde günlükleri toplamak için CESARETLENDİRİCİ kullanarak çalıştırabilirsiniz bir örnek komut dosyası şöyledir:

```
$ip = "<IP OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDREESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- CESARETLENDİRİCİ günlükleri toplamak, belirtmeniz `OutputPath` HLH makine üzerinde bir konum parametresi. Ayrıca konumu şifrelendiğinden emin olun.
- Parametreleri `OutputSharePath` ve `OutputShareCredential` isteğe bağlıdır ve harici bir paylaşılan klasöre günlükleri karşıya yükleme sırasında kullanılır. Şu parametreleri kullan *ayrıca* için `OutputPath`. Varsa `OutputPath` belirtilmezse, günlük toplama Aracı'nı CESARETLENDİRİCİ VM sistem sürücüsünde depolama için kullanır. Bu betik sürücü alanı sınırlı olduğu için başarısız olmasına neden olabilir.
- Önceki örnekte gösterildiği gibi `FromDate` ve `ToDate` parametreleri, belirli bir süre için günlükleri toplamak için kullanılabilir. Bu tümleşik bir sistemde bir güncelleştirme paketini uygulamadan sonra günlükleri toplamayı gibi senaryolar için kullanışlı gelebilir.

**ASDK ve tümleşik sistemleri için parametre dikkate alınacak noktalar:**

- Varsa `FromDate` ve `ToDate` parametreleri belirtilmemişse, günlükleri, son dört saat için varsayılan olarak toplanır.
- Kullanabileceğiniz `TimeOutInMinutes` günlük toplama için zaman aşımını ayarlamanız parametresi. Varsayılan olarak, 150 (2.5 saat) ayarlanır.

- Şu anda kullanabileceğiniz `FilterByRole` aşağıdaki rolleri tarafından filtre oturum koleksiyonuna parametre:

   |   |   |   |
   | - | - | - |
   | `ACSMigrationService`     | `ACSMonitoringService`   | `ACSSettingsService` |
   | `ACS`                     | `ACSFabric`              | `ACSFrontEnd`        |
   | `ACSTableMaster`          | `ACSTableServer`         | `ACSWac`             |
   | `ADFS`                    | `ASAppGateway`           | `BareMetal`          |
   | `BRP`                     | `CA`                     | `CPI`                |
   | `CRP`                     | `DeploymentMachine`      | `DHCP`               |
   |`Domain`                   | `ECE`                    | `ECESeedRing`        |        
   | `FabricRing`              | `FabricRingServices`     | `FRP`                |
   |` Gateway`                 | `HealthMonitoring`       | `HRP`                |               
   | `IBC`                     | `InfraServiceController` | `KeyVaultAdminResourceProvider`|
   | `KeyVaultControlPlane`    | `KeyVaultDataPlane`      | `NC`                 |            
   | `NonPrivilegedAppGateway` | `NRP`                    | `SeedRing`           |
   | `SeedRingServices`        | `SLB`                    | `SQL`                |     
   | `SRP`                     | `Storage`                | `StorageController`  |
   | `URP`                     | `UsageBridge`            | `VirtualMachines`    |  
   | `WAS`                     | `WASPUBLIC`              | `WDS`                |


Dikkat edilecek bazı ek noktalar:

* Komut günlükleri toplamaya hangi rollere göre çalıştırmak için biraz zaman alır. Faktörlere de günlük toplama ve Azure yığın ortamında düğümleri sayısı için belirtilen süre içerir.
* Günlük toplama tamamlandıktan sonra oluşturulan yeni klasörü denetleyin `-OutputPath` komutunda belirtilen parametre.
* Her rol günlüklerinin içindeki tek tek posta dosyaları sahiptir. Toplanan günlükleri boyutuna bağlı olarak, bir rol içindeki birden çok ZIP dosyaları bölme günlüklerinin olabilir. Tek bir klasör içinde sıkıştırması tüm günlük dosyalarını sahip olmak istiyorsanız, bu tür bir rol için (örneğin, 7zip) toplu genişletebilirsiniz bir araç kullanın. Rolü için daraltılmış tüm dosyaları seçin ve Seç **burada ayıklamak**. Bu rol tek bir birleştirilmiş klasördeki tüm günlük dosyalarını unzips.
* Dosya adında `Get-AzureStackLog_Output.log` de daraltılmış günlük dosyalarını içeren klasörde oluşturulur. Günlük toplama sırasında sorunları gidermek için kullanılabilir komut çıktısı Günlüğü dosyasıdır.
* Belirli bir arızası araştırmak için birden çok bileşenden günlükleri gerekli olabilir.
    -   Sistem ve tüm altyapı VM'ler için olay günlüklerini de toplanır *VirtualMachines* rol.
    -   Sistem ve tüm ana bilgisayarlar için olay günlüklerini de toplanır *BareMetal* rol.
    -   Yük devretme kümesi ve Hyper-V olay günlüklerini toplanır *depolama* rol.
    -   ACS günlükleri toplanır *depolama* ve *ACS* rolleri.

> [!NOTE]
> Boyutu ve yaş sınırları günlükleriyle taşan açılmıyor emin olmak için depolama alanınızı etkili kullanımını sağlamak için temel olarak toplanan günlüklerini zorunlu tutulmaz. Ancak, bir sorunu tanılamada bazen bu sınırları nedeniyle artık mevcut olmayabilir günlükleri gerekir. Bu nedenle, olan **tavsiye** günlüklerinizi bir harici depolama alanı (Azure depolama hesabı, ek şirket içi depolama aygıtı vb.) için yük boşaltma 8 ila 12 saatte bir ve bunları orada bağlı olarak, 1-3 ay tutun, gereksinimleri. Ayrıca, bu depolama konumu şifrelendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Azure yığın sorunlarını giderme](azure-stack-troubleshooting.md)
