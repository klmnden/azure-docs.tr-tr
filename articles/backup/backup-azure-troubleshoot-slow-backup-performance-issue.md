---
title: Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
description: Neden Azure Backup performans sorunlarını tanılamanıza yardımcı olmak için sorun giderme kılavuzu verilmiştir
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
ms.openlocfilehash: 592a46077bb9e3469f3a42a95173af1b6db93510
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704929"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
Bu makalede, Azure Backup kullanırken, dosya ve klasörlerin yavaş yedekleme performansı nedenini tanılamanıza yardımcı olmak için sorun giderme kılavuzu verilmiştir. Dosyalarını yedeklemek için Azure Backup Aracısı'nı kullandığınızda, yedekleme işlemi beklenenden daha uzun sürebilir. Bu gecikme, bir veya daha fazlasını tarafından kaynaklanabilir:

* [Yedeklenmekte olan bir bilgisayarda performans sorunları vardır.](#cause1)
* [Başka bir işlem veya virüsten koruma yazılımı Azure yedekleme işlemine engelliyor.](#cause2)
* [Backup Aracısı, Azure sanal makinesi (VM) üzerinde çalışıyor.](#cause3)  
* [Çok sayıda dosya (milyon) yedekleme yapıyorsanız.](#cause4)

İndirme ve yükleme sorunlarını giderme başlamadan önce öneririz [en son Azure Backup aracısını](https://aka.ms/azurebackup_agent). Çeşitli sorunları gidermek, özellikleri ekleyin ve performansı artırmak için Backup Aracısı sık güncelleştirmeler vermiyoruz.

Ayrıca gözden geçirmenizi öneririz [Azure Backup hizmeti hakkında SSS](backup-azure-backup-faq.md) değil karşılaştığınız herhangi bir ortak yapılandırma sorunlarını emin olmak için.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Neden: Bilgisayarda performans sorunları
Yedeklenmekte olan bir bilgisayarda performans sorunlarını gecikmelere neden olabilir. Örneğin, bilgisayarın özelliği okuma veya yazma disk veya ağ üzerinden veri göndermek için kullanılabilir bant genişliği performans sorunlarını neden olabilir.

Windows adlı yerleşik bir aracı sağlayan [Performans İzleyicisi](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Bu performans sorunlarını algılamak için Perfmon).

Bazı performans sayaçları ve en iyi yedekleme için performans sorunlarını tanılamada yararlı olabilir aralıkları aşağıda verilmiştir.

| Sayaç | Durum |
| --- | --- |
| Mantıksal Disk (Fiziksel Disk)--% boşta |• %100 %50 boşta boşta Sağlıklı =</br>• %49 %20 boşta boşta = uyarı veya İzleyici</br>• %19 %0 boşta boşta = kritik veya tanesi belirtimi |
| Mantıksal Disk (Fiziksel Disk)--% ortalama Disk sn okuma veya yazma |• 0,001 ms ile 0.015 ms Sağlıklı =</br>• 0.015 ms 0.025 ms için uyarı veya İzleyici =</br>• 0.026 ms veya daha uzun süre = kritik veya tanesi belirtimi |
| Mantıksal Disk (Fiziksel Disk)--geçerli Disk Sırası Uzunluğu (tüm örnekler için) |6 dakikadan fazla süre için 80 istek |
| Bellek--olmayan havuzda bayt |• Küçüktür %60 tüketilen havuzunun Sağlıklı =<br>• %61 80 tüketilen havuzunun uyarı veya İzleyici =</br>• % 80 havuzu tüketilmiş büyüktür = kritik veya tanesi belirtimi |
| Disk belleği havuzu bayt bellek-- |• Küçüktür %60 tüketilen havuzunun Sağlıklı =</br>• %61 80 tüketilen havuzunun uyarı veya İzleyici =</br>• % 80 havuzu tüketilmiş büyüktür = kritik veya tanesi belirtimi |
| Bellek--kullanılabilir megabayt |• 50 boş bellek yüzdesi veya daha Sağlıklı =</br>• 25 kullanılabilir boş bellek yüzdesi İzleyici =</br>• 10 kullanılabilir boş bellek yüzdesi uyarı =</br>• 100 MB veya 5 boş bellek yüzdesi az kritik veya tanesi Spec = |
| Processor--\%Processor Time (all instances) |• Less than 60% consumed = Healthy</br>• 61% to 90% consumed = Monitor or Caution</br>• 91% to 100% consumed = Critical |

> [!NOTE]
> If you determine that the infrastructure is the culprit, we recommend that you defragment the disks regularly for better performance.
>
>

<a id="cause2"></a>

## Cause: Another process or antivirus software interfering with Azure Backup
We've seen several instances where other processes in the Windows system have negatively affected performance of the Azure Backup agent process. For example, if you use both the Azure Backup agent and another program to back up data, or if antivirus software is running and has a lock on files to be backed up, the multiple locks on files might cause contention. In this situation, the backup might fail, or the job might take longer than expected.

The best recommendation in this scenario is to turn off the other backup program to see whether the backup time for the Azure Backup agent changes. Usually, making sure that multiple backup jobs are not running at the same time is sufficient to prevent them from affecting each other.

For antivirus programs, we recommend that you exclude the following files and locations:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe as a process
* C:\Program Files\Microsoft Azure Recovery Services Agent\ folders
* Scratch location (if you're not using the standard location)

<a id="cause3"></a>

## Cause: Backup agent running on an Azure virtual machine
If you're running the Backup agent on a VM, performance will be slower than when you run it on a physical machine. This is expected due to IOPS limitations.  However, you can optimize the performance by switching the data drives that are being backed up to Azure Premium Storage. We're working on fixing this issue, and the fix will be available in a future release.

<a id="cause4"></a>

## Cause: Backing up a large number (millions) of files
Moving a large volume of data will take longer than moving a smaller volume of data. In some cases, backup time is related to not only the size of the data, but also the number of files or folders. This is especially true when millions of small files (a few bytes to a few kilobytes) are being backed up.

This behavior occurs because while you're backing up the data and moving it to Azure, Azure is simultaneously cataloging your files. In some rare scenarios, the catalog operation might take longer than expected.

The following indicators can help you understand the bottleneck and accordingly work on the next steps:

* **UI is showing progress for the data transfer**. The data is still being transferred. The network bandwidth or the size of data might be causing delays.
* **UI is not showing progress for the data transfer**. Open the logs located at C:\Program Files\Microsoft Azure Recovery Services Agent\Temp, and then check for the FileProvider::EndData entry in the logs. This entry signifies that the data transfer finished and the catalog operation is happening. Don't cancel the backup jobs. Instead, wait a little longer for the catalog operation to finish. If the problem persists, contact [Azure support](https://portal.azure.com/#create/Microsoft.Support).İşlemci--\`işlemci zamanı (tüm örnekler için)es and folders in Azure Backup
description: Provides troubleshooting guidance to help you diagnose the cause of Azure Backup performance issues
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
---
# Troubleshoot slow backup of files and folders in Azure Backup
This article provides troubleshooting guidance to help you diagnose the cause of slow backup performance for files and folders when you're using Azure Backup. When you use the Azure Backup agent to back up files, the backup process might take longer than expected. This delay might be caused by one or more of the following:

* [There are performance bottlenecks on the computer that’s being backed up.](#cause1)
* [Another process or antivirus software is interfering with the Azure Backup process.](#cause2)
* [The Backup agent is running on an Azure virtual machine (VM).](#cause3)  
* [You're backing up a large number (millions) of files.](#cause4)

Before you start troubleshooting issues, we recommend that you download and install the [latest Azure Backup agent](https://aka.ms/azurebackup_agent). We make frequent
updates to the Backup agent to fix various issues, add features, and improve performance.

We also strongly recommend that you review the [Azure Backup service FAQ](backup-azure-backup-faq.md) to make sure you're not experiencing any of the common configuration issues.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## Cause: Performance bottlenecks on the computer
Bottlenecks on the computer that's being backed up can cause delays. For example, the computer's ability to read or write to disk, or available bandwidth to send data over the network, can cause bottlenecks.

Windows provides a built-in tool that's called [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) to detect these bottlenecks.

Here are some performance counters and ranges that can be helpful in diagnosing bottlenecks for optimal backups.

| Counter | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |• 100% idle to 50% idle = Healthy</br>• 49% idle to 20% idle = Warning or Monitor</br>• 19% idle to 0% idle = Critical or Out of Spec |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |• 0.001 ms to 0.015 ms  = Healthy</br>• 0.015 ms to 0.025 ms = Warning or Monitor</br>• 0.026 ms or longer = Critical or Out of Spec |
| Logical Disk(Physical Disk)--Current Disk Queue Length (for all instances) |80 requests for more than 6 minutes |
| Memory--Pool Non Paged Bytes |• Less than 60% of pool consumed = Healthy<br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Pool Paged Bytes |• Less than 60% of pool consumed = Healthy</br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Available Megabytes |• 50% of free memory available or more = Healthy</br>• 25% of free memory available = Monitor</br>• 10% of free memory available = Warning</br>• Less than 100 MB or 5% of free memory available = Critical or Out of Spec |
| Processor--\%Processor Time (all instances) |• % 60'den az kullanılan Sağlıklı =</br>• 61 tüketilen % 90 = İzleyici veya uyarı</br>• %91 100 arasında tüketilen kritik = |

> [!NOTE]
> Altyapı sorunlu olduğunu belirlerseniz, daha iyi performans için düzenli olarak disk birleştirme öneririz.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Neden: Başka bir işlem veya virüsten koruma yazılımı Azure Backup ile engelliyor
Windows sisteminde diğer işlemleri olumsuz yönde etkilenen Azure Backup aracısı işleminin performansını burada birkaç örneğe gördük. Örneğin, verileri yedeklemek için Azure Backup aracısını hem de başka bir program kullanıyorsanız veya virüsten koruma yazılımı çalıştıran ve yedeklenecek dosyaları üzerinde bir kilit sahiptir, üzerinde birden çok kilitler dosyaları Çekişme neden olabilir. Bu durumda, yedekleme başarısız veya iş beklenenden daha uzun sürebilir.

Bu senaryoda en iyi kullanılması, Azure Yedekleme aracısı yedekleme zamanını değişiklikler olup olmadığını görmek için diğer yedekleme programınızı kapatmak için önerilir. Genellikle, birden çok yedekleme işleri aynı anda çalışmayan sağlamaktan birbirine etkilemesini önlemek yeterli olur.

Virüsten koruma programları için konumları ve aşağıdaki dosyaları dışarıda öneririz:

* Bir işlem olarak C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe
* C:\Program Files\Microsoft Azure Recovery Services Agent\ klasörleri
* Karalama konumu (standart konumu değil kullanıyorsanız)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Neden: Azure sanal makinesinde çalışan yedekleme aracısı
Backup Aracısı VM üzerinde çalıştırıyorsanız, performansı, fiziksel bir makinede çalıştırıldığında yavaş olur. IOPS sınırlamaları nedeniyle bu bekleniyor.  Ancak, Azure Premium Depolama'ya yedeklenen veri sürücülerine geçiş yaparak performansını iyileştirebilirsiniz. Bu sorunu düzeltmeye çalışıyoruz ve düzeltme gelecekteki bir sürümde sağlanacaktır.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Neden: Yedekleme dosyaları çok sayıda (milyon)
Büyük miktarda veri taşıma daha küçük bir veri hacmi taşımaktan daha uzun sürer. Bazı durumlarda yalnızca boyutunu verilerin, aynı zamanda dosyaları veya klasörleri için yedekleme zamanı ilişkilidir. Bu, özellikle küçük dosyaları (birkaç kilobayt birkaç bayt) milyonlarca yedeklenen olduğunda geçerlidir.

Bu davranış, veri yedekleme ve Azure'a taşıma, ancak Azure dosyaları aynı anda kataloglama kaynaklanır. Bazı nadir senaryolarda katalog işlemi beklenenden daha uzun sürebilir.

Aşağıdaki göstergelerden bir performans sorunu anlamanıza ve sonraki adımlara göre uygun şekilde çalışması yardımcı olabilir:

* **UI veri aktarımı için ilerlemeyi gösteren**. Veri hala aktarıldığı. Ağ bant genişliğini veya veri boyutu gecikmelere neden.
* **UI göstermiyorsa ilerleme veri aktarımı için**. C:\Program Files\Microsoft Azure Recovery Services Agent\Temp bulunan günlüklerini açın ve ardından günlüklerinde FileProvider::EndData giriş olup olmadığını denetleyin. Bu giriş, veri aktarımı tamamlandı ve Katalog işleminin gerçekleştiği gösterir. Yedekleme işleri iptal etme. Bunun yerine, biraz daha uzun Kataloğu işlemin tamamlanmasını bekleyin. Sorun devam ederse, iletişim [Azure Destek](https://portal.azure.com/#create/Microsoft.Support).
