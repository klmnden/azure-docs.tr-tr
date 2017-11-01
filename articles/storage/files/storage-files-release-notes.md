---
title: "Azure Dosya Eşitleme aracısı sürüm notları | Microsoft Docs"
description: "Azure Dosya Eşitleme yayın notları"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 1db3fef17e24022bff59665558f4a354f8c37d1d
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="azure-file-sync-agent-release-notes"></a>Azure Dosya Eşitleme aracısı sürüm notları
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları yer almaktadır.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aşağıdaki sürümleri destekler:

| Aracı sürüm numarası | Sürüm tarihi | Destek sonu |
|----------------------|--------------|------------------|
| 1.1.0.0 | 26.09.2017 | Geçerli sürüm |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-1100"></a>Aracı sürümü 1.1.0.0
Aşağıdaki sürüm notları 9 Eylül 2017 tarihinde yayımlanmış olan aracı sürümü 1.1.0.0 için geçerlidir. Bu sürüm, Azure Dosya Eşitleme uygulamasının ilk Önizleme sürümüdür!

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server'a yüklemek ve yapılandırmak için bkz. [Azure Dosya Eşitleme (önizleme) dağıtımı için hazırlanma](storage-sync-files-planning.md) ve [Azure Dosya Eşitleme (önizleme) aracısını dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin (MSI) yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Yalnızca Windows Server 2016 ve 2012 R2 üzerinde desteklenir.
- Aracı için en az 2 GB fiziksel bellek gerekir.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [Azure Dosya Eşitleme (önizleme) ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisini (veya diğer) dosya filtrelerini kullanmayın: Dosya filtreleri dosyaları engelleyerek sonsuz eşitleme hatalarına neden olabilir.
- Kayıtlı Sunucu çoğaltma işlemleri (VM kopyalama dahil) beklenmeyen sonuçlara neden olabilir (özellikle eşitleme çalışmayabilir).
- Yinelenen Verileri Kaldırma ve bulut katmanlaması aynı birimde desteklenmez.
 
### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistemin geri kalanı normal şekilde çalışmaya devam eder:
- 2048 karakterden uzun yollar
- Bir güvenlik tanımlayıcısının 2 K üzerindeki DACL bölümü (bu yalnızca tek bir öğede 40'tan fazla Erişim Denetimi Girişi varsa sorun oluşturur)
- Güvenlik tanımlayıcısının SACL bölümü (denetim için kullanılır)
- Genişletilmiş öznitelikler
- Alternatif veri akışları
- Yeniden ayrıştırma noktaları
- Sabit bağlantılar
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz
- Hizmetimizin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar 
    
    > [!Note]  
    > Azure Dosya Eşitleme taşıma durumundaki verileri her zaman şifreler ve Azure'da bekleyen veriler de şifrelenebilir.
 
### <a name="server-endpoints"></a>Sunucu Uç Noktaları
- Sunucu uç noktası yalnızca NTFS birimlerde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sunucu uç noktası sistem biriminde olamaz (örneğin C:\Klasörüm bir bağlama noktası değilse C:\Klasörüm yolu kabul edilemez).
- Yük Devretme Kümelemesi yalnızca Kümelenmiş Diskler için desteklenir, Küme Paylaşılan Birimleri (CSV) için desteklenmez.
- Sunucu uç noktası iç içe yerleştirilmiş olamaz ancak aynı birimde birbirine paralel şekilde bulunabilir.
- Bir sunucudan aynı anda çok sayıda dizin (10.000'den fazla) silinmesi durumunda eşitleme hataları oluşabilir. Dizinleri 10.000'den az sayıda partiler halinde silin ve bir sonraki partiyi silmeden önce silme işlemlerinin başarıyla eşitlendiğinden emin olun.
- Birimin kök dizininde desteklenmez.
- Sunucu uç noktasında işletim sistemi veya uygulama disk belleği dosyası depolamayın.
 
### <a name="cloud-tiering"></a>Bulut katmanlaması
- Dosyaların düzgün geri çekilmesini sağlamak için sistem yeni veya değiştirilen dosyaları 32 saate kadar otomatik olarak katmanlayamaz. Buna yeni bir Sunucu Uç Noktası yapılandırıldıktan sonra gerçekleştirilen ilk katmanlama dahildir. İstediğiniz zaman katmanlama gerçekleştirebileceğiniz PowerShell cmdlet'i sayesinde arka plan işlemini beklemeden katmanlamayı daha verimli bir şekilde değerlendirebilirsiniz.
- Katmanlanmış bir dosyanın Robocopy kullanılarak başka bir konuma kopyalanması halinde oluşturulan dosya katmanlanmış olmaz ancak Robocopy, çevrimdışı özniteliğini kopyalama işlemlerine yanlışlıkla dahil ettiğinden bu öznitelik ayarlanmış olabilir.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.