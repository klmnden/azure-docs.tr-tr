---
title: Azure Stack’te tanılama
description: Azure yığınında tanılama günlük dosyaları toplamak nasıl
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 04/27/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: 28e1939d3c9cb5a9b9080e60230ad5600ad8a6a3
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-stack-diagnostics-tools"></a>Azure yığın tanılama araçları

Azure yığın birlikte çalışan ve birbiriyle etkileşim bileşenleri büyük bir koleksiyonudur. Tüm bu bileşenlerin kendi benzersiz günlükler oluşturur. Bu, özellikle Azure yığın bileşenleri etkileşim birden çok from gelen hataları zor bir görev tanılama sorunları hale getirebilirsiniz. 

Bizim tanılama araçları, kolay ve verimli günlüğü koleksiyonu mekanizması sağlamaya yardımcı olur. Aşağıdaki diyagramda gösterildiği toplama araçları Azure yığın işlerinde nasıl oturum:

![Azure yığın tanılama araçları](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Trace Toplayıcı
 
Trace Toplayıcı varsayılan olarak etkindir ve Azure yığın bileşen hizmetlerinden tüm olay izleme için Windows (ETW) günlükleri toplamak için arka planda sürekli olarak çalışır. ETW günlükleri beş gün yaş sınırına sahip ortak bir yerel paylaşıma depolanır. Bu sınıra ulaşıldığında, yeni bir tane oluşturuldukça eski dosyalar silinir. Her dosya için varsayılan boyut üst sınırını 200 MB'tır. Bir boyut denetimi 2 dakikada bir gerçekleşir ve geçerli dosya > = 200 MB kaydedilir ve yeni bir dosya oluşturulur. Ayrıca bir 8 GB sınırı yoktur olay oturumu oluşturulan toplam dosya boyutu. 

## <a name="log-collection-tool"></a>Günlük koleksiyonu aracı
 
PowerShell cmdlet **Get-AzureStackLog** Azure yığın ortamında tüm bileşenleri günlükleri toplamak için kullanılabilir. Bu kullanıcı tanımlı bir konumda ZIP dosyaları kaydeder. Azure yığın teknik destek ekibinin sorunu gidermenize yardımcı olması için günlüklerinizi gerekiyorsa, bunlar, bu aracı çalıştırmanızı isteyebilir.

> [!CAUTION]
> Bu günlük dosyaları, kişisel bilgileri (PII) içerebilir. Genel olarak tüm günlük dosyalarını sonrası önce bu dikkate alın.
 
Toplanan bazı örnek günlük türleri şunlardır:
*   **Azure yığın dağıtım günlükleri**
*   **Windows olay günlükleri**
*   **Panther günlükleri**
*   **Küme günlükleri**
*   **Depolama tanılama günlükleri**
*   **ETW günlükleri**

Bu dosyalar toplanır ve bir paylaşımına izleme toplayıcısı tarafından kaydedilir. **Get-AzureStackLog** PowerShell cmdlet'ini ardından bunları gerekli olduğunda toplamak için kullanılabilir.
 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Get-AzureStackLog bir Azure yığın Geliştirme Seti (ASDK) sistemde çalıştırmak için
1. Olarak oturum **AzureStack\CloudAdmin** ana bilgisayarda.
2. Bir yönetici olarak bir PowerShell penceresi açın.
3. Çalıştırma **Get-AzureStackLog** PowerShell cmdlet'i.

**Örnekler:**

  Tüm rolleri için tüm günlükleri toplama:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  Tarihi geçmiş 8 saat için günlük dosyalarını filtreleme ile VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  8 saat önce 2 saat önce arasındaki zaman aralığı için günlük dosyaları için filtreleme tarihle VirtualMachines ve BareMetal rollerinden günlüklerini toplayın:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems-version-1804-and-later"></a>Get-AzureStackLog Azure yığında çalıştırmak için 1804 ve üzeri sistemleri sürüm tümleşik

Tümleşik bir sistemde oturum koleksiyonu aracını çalıştırmak için erişim için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) olması gerekir. Tümleşik bir sistemde günlükleri toplamak için CESARETLENDİRİCİ kullanarak çalıştırabilirsiniz bir örnek komut dosyası şöyledir:

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

- Parametreleri **OutputSharePath** ve **OutputShareCredential** günlükleri harici bir paylaşılan klasöre yüklemek için kullanılır.
- Önceki örnekte gösterildiği gibi **FromDate** ve **ToDate** parametreleri, belirli bir süre için günlükleri toplamak için kullanılabilir. Bu tümleşik bir sistemde bir güncelleştirme paketini uygulamadan sonra günlükleri toplamayı gibi senaryolar için kullanışlı gelebilir.


### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems-version-1803-and-earlier"></a>Get-AzureStackLog Azure yığında çalıştırmak için sistemleri sürüm 1803 ve önceki tümleşik

Tümleşik bir sistemde oturum koleksiyonu aracını çalıştırmak için erişim için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) olması gerekir. Tümleşik bir sistemde günlükleri toplamak için CESARETLENDİRİCİ kullanarak çalıştırabilirsiniz bir örnek komut dosyası şöyledir:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDRESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- CESARETLENDİRİCİ günlükleri toplamak, belirtmeniz **OutputPath** donanım yaşam döngüsü ana bilgisayar (HLH) makine üzerinde bir konum parametresi. Ayrıca konumu şifrelendiğinden emin olun.
- Parametreleri **OutputSharePath** ve **OutputShareCredential** isteğe bağlıdır ve harici bir paylaşılan klasöre günlükleri karşıya yükleme sırasında kullanılır. Şu parametreleri kullan *ayrıca* için **OutputPath**. Varsa **OutputPath** belirtilmezse, günlük toplama Aracı'nı CESARETLENDİRİCİ VM sistem sürücüsünde depolama için kullanır. Bu betik sürücü alanı sınırlı olduğu için başarısız olmasına neden olabilir.
- Önceki örnekte gösterildiği gibi **FromDate** ve **ToDate** parametreleri, belirli bir süre için günlükleri toplamak için kullanılabilir. Bu tümleşik bir sistemde bir güncelleştirme paketini uygulamadan sonra günlükleri toplamayı gibi senaryolar için kullanışlı gelebilir.


### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>ASDK ve tümleşik sistemleri için parametre konuları

- Varsa **FromDate** ve **ToDate** parametreleri belirtilmemişse, günlükleri, son dört saat için varsayılan olarak toplanır.
- Kullanabileceğiniz **TimeOutInMinutes** günlük toplama için zaman aşımını ayarlamanız parametresi. Varsayılan olarak, 150 (2.5 saat) ayarlanır.

- Şu anda kullanabileceğiniz **FilterByRole** aşağıdaki rolleri tarafından filtre oturum koleksiyonuna parametre:

   |   |   |   |
   | - | - | - |
   | ACS                    | DeploymentMachine                | NC                         |
   | ACSBlob                | DiskRP                           | Ağ                    |
   | ACSFabric              | Etki alanı                           | NonPrivilegedAppGateway    |
   | ACSFrontEnd            | ECE                              | NRP                        |
   | ACSMetrics             | ExternalDNS                      | OEM                        |
   | ACSMigrationService    | Yapı                           | PXE                        |
   | ACSMonitoringService   | FabricRing                       | SeedRing                   | 
   | ACSSettingsService     | FabricRingServices               | SeedRingServices           |
   | ACSTableMaster         | FRP                              | SLB                        |   
   | ACSTableServer         | Galeri                          | SlbVips                    |
   | ACSWac                 | Ağ Geçidi                          | SQL                        |   
   | ADFS                   | Ögesi                 | SRP                        |
   | ASAppGateway           | HRP                              | Depolama                    |   
   | NCAzureBridge          | IBC                              | StorageAccounts            |    
   | AzurePackConnector     | Identityprovider                 | StorageController          |  
   | AzureStackBitlocker    | IDN'ler                             | Kiracı                     |
   | BareMetal              | InfraServiceController           | TraceCollector             |
   | BRP                    | Altyapı                   | URP                        |
   | CA                     | KeyVaultAdminResourceProvider    | UsageBridge                |
   | Bulut                  | KeyVaultControlPlane             | virtualMachines            |
   | Küme                | KeyVaultDataPlane                | EDİLDİ                        |
   | İşlem                | KeyVaultInternalControlPlane     | WASBootstrap               |
   | CPI                    | KeyVaultInternalDataPlane        | WASPUBLIC                  |
   | CRP                    | KeyVaultNamingService            |                            |
   | DatacenterIntegration  | MonitoringAgent                  |                            |
   |                        |                                  |                            |

### <a name="bkmk_gui"></a>Bir grafik kullanıcı arabirimini kullanarak günlüklerini toplayın
Azure yığın günlükleri almak Get-AzureStackLog cmdlet'i için gerekli parametreleri sağlayarak yerine, ana Azure yığın araçları GitHub araçları deposunu bulunan kullanılabilir açık kaynak Azure yığın araçları da kullanabilirsiniz http://aka.ms/AzureStackTools.

**ERCS_AzureStackLogs.ps1** PowerShell Betiği GitHub araçları deposunda depolanır ve düzenli olarak güncelleştirilir. Kullanılabilir en son sürüme sahip, doğrudan indirmelisiniz emin olmak için http://aka.ms/ERCS. Yönetici bir PowerShell oturumundan başlatıldı, komut dosyası ayrıcalıklı uç noktasına bağlanır ve Get-AzureStackLog sağlanan parametrelerle çalıştırır. Hiçbir parametre kullanılmazsa, komut dosyasını bir grafik kullanıcı arabirimi aracılığıyla parametreler için sormadan için varsayılan olarak ayarlanır.

ERCS_AzureStackLogs.ps1 PowerShell komut dosyası hakkında daha fazla bilgi için izleyebilir [kısa bir video](https://www.youtube.com/watch?v=Utt7pLsXEBc) veya betiğin görüntüleyin [Benioku dosyasını](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md) Azure yığın araçları GitHub deposunda bulunan. 

### <a name="additional-considerations"></a>Diğer konular

* Komut günlükleri toplamaya hangi rollere göre çalıştırmak için biraz zaman alır. Faktörlere de günlük toplama ve Azure yığın ortamında düğümleri sayısı için belirtilen süre içerir.
* Koleksiyon çalışmalarını günlük olarak oluşturulan yeni klasörü denetleyin **OutputSharePath** komutunda belirtilen parametre.
* Her rol günlüklerinin içindeki tek tek posta dosyaları sahiptir. Toplanan günlükleri boyutuna bağlı olarak, bir rol içindeki birden çok ZIP dosyaları bölme günlüklerinin olabilir. Tek bir klasör içinde sıkıştırması tüm günlük dosyalarını sahip olmak istiyorsanız, bu tür bir rol için (örneğin, 7zip) toplu genişletebilirsiniz bir araç kullanın. Rolü için daraltılmış tüm dosyaları seçin ve Seç **burada ayıklamak**. Bu rol tek bir birleştirilmiş klasördeki tüm günlük dosyalarını unzips.
* Dosya adında **Get-AzureStackLog_Output.log** de daraltılmış günlük dosyalarını içeren klasörde oluşturulur. Günlük toplama sırasında sorunları gidermek için kullanılabilir komut çıktısı Günlüğü dosyasıdır.
* Belirli bir arızası araştırmak için birden çok bileşenden günlükleri gerekli olabilir.
    -   Sistem ve tüm altyapı VM'ler için olay günlüklerini de toplanır *VirtualMachines* rol.
    -   Sistem ve tüm ana bilgisayarlar için olay günlüklerini de toplanır *BareMetal* rol.
    -   Yük devretme kümesi ve Hyper-V olay günlüklerini toplanır *depolama* rol.
    -   ACS günlükleri toplanır *depolama* ve *ACS* rolleri.

> [!NOTE]
> Boyutu ve yaş sınırları günlükleriyle taşan açılmıyor emin olmak için depolama alanınızı etkili kullanımını sağlamak için temel olarak toplanan günlüklerini zorunlu tutulmaz. Ancak, bir sorunu tanılamada bazen bu sınırları nedeniyle artık mevcut olmayabilir günlükleri gerekir. Bu nedenle, olan **tavsiye** günlüklerinizi bir harici depolama alanı (Azure depolama hesabı, ek şirket içi depolama aygıtı vb.) için yük boşaltma 8 ila 12 saatte bir ve bunları orada bağlı olarak, 1-3 ay tutun, gereksinimleri. Ayrıca, bu depolama konumu şifrelendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Azure yığın sorunlarını giderme](azure-stack-troubleshooting.md)

