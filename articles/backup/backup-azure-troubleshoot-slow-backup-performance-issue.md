---
title: Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
description: Neden Azure Backup performans sorunlarını tanılamanıza yardımcı olmak için sorun giderme kılavuzu verilmiştir
services: backup
author: genlin
manager: cshepard
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: f24a60ab9bdcf1231085de4edeeb89ce1edf4e80
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337638"
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
| İşlemci--\%işlemci zamanı (tüm örnekler için) |• % 60'den az kullanılan Sağlıklı =</br>• 61 tüketilen % 90 = İzleyici veya uyarı</br>• %91 100 arasında tüketilen kritik = |

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
