---
title: Azure dosya eşitleme'yi izleyin | Microsoft Docs
description: Azure dosya eşitleme izlemeyi öğrenin.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: a14b0f2b01a0566a47cbcb02ee4315adcba9a90f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200811"
---
# <a name="monitor-azure-file-sync"></a>Azure Dosya Eşitleme’yi izleme

Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

Bu makalede, Azure portalı ve Windows Server'ı kullanarak, Azure dosya eşitleme dağıtımının nasıl izleneceği açıklanır.

Aşağıdaki izleme seçeneklerini şu anda kullanılabilir:

## <a name="azure-portal"></a>Azure portal

Azure portalında ölçümleri kayıtlı sunucu sistem durumu ve sunucu uç noktası durumu (eşitleme sistem durumu) görüntüleyebilirsiniz.

### <a name="storage-sync-service"></a>Depolama Eşitleme Hizmeti

Kayıtlı sunucu sistem durumu, sunucu uç noktası durumu ve ölçümleri görüntülemek için Azure portalında depolama eşitleme hizmeti gidin. Kayıtlı sunucu sağlığı kayıtlı sunucuları dikey pencerede görülebilir. Eşitleme grupları dikey penceresinde sunucu uç noktası durumu görülebilir.

Kayıtlı sunucu durumu
- Kayıtlı sunucu durumu çevrimiçi ise, sunucunun başarıyla hizmetiyle iletişim kuruyor.
- Kayıtlı sunucu durumu görünür çevrimdışı ise, depolama eşitleme İzleyicisi (AzureStorageSyncMonitor.exe) işlem sunucusunda çalışan doğrulayın. Sunucu güvenlik duvarı veya proxy arkasında ise başına proxy ve güvenlik duvarı yapılandırma [belgeleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy).

Sunucu uç noktası durumu
- Sunucu uç noktası durumu Portalı'nda sunucunun (ID 9102 ve 9302) Telemetri olay günlüğüne kaydedilir eşitleme olayları temel alır. Eşitleme oturumu (örneğin, hata iptal edildi) geçici bir hata nedeniyle başarısız olursa, geçerli eşitleme oturumu (olay kimliği 9302 dosyaları uygulanmış olup olmadığını belirlemek için kullanılır) ilerleme kaydediyor mu sürece eşitleme hala portalda sağlıklı gösterebilir. Daha fazla bilgi için aşağıdaki belgelere bakın: [Eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [eşitleme ilerleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Portal bir eşitleme hatası değil yapmayı ilerleme eşitlenecek son gösterir, denetleyin [sorun giderme belgelerinin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) Kılavuzu.

Ölçümler
- Aşağıdaki ölçümler depolama eşitleme hizmeti Portalı'nda görüntülenebilir:

  | Ölçüm adı | Açıklama | Portal blade(s) | 
  |-|-|-|
  | Eşitlenen bayt | (Karşıya yükleme ve indirme) aktarılan verilerin boyutu | Eşitleme grubu, sunucu uç noktası |
  | Bulut katmanlaması geri çağırma | Geri veri boyutu | Kayıtlı sunucular |
  | Dosyalar eşitlenmiyor | Eşitleme başarısız olan dosya sayısı | Sunucu uç noktası |
  | Eşitlenmiş dosyaları | Dosya sayısı (karşıya yükleme ve indirme) aktarılan | Eşitleme grubu, sunucu uç noktası |
  | Sunucu sinyali | Sunucudan alınan sinyal sayısı | Kayıtlı sunucular |

- Daha fazla bilgi için bkz. [Azure İzleyici](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor) bölümü. 

  > [!Note]  
  > Depolama eşitleme hizmeti Portalı'nda grafikleri, 24 saatlik bir zaman aralığı vardır. Farklı tarih aralıkları veya boyutlar görüntülemek için Azure İzleyici'yi kullanın.


### <a name="azure-monitor"></a>Azure İzleyici

Kullanım [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) İzleyici eşitleme, bulut katmanı ve sunucu bağlantısı. Azure dosya eşitleme için ölçümleri, varsayılan olarak etkindir ve Azure İzleyici her 15 dakikada gönderilir.

Azure İzleyici'de Azure dosya eşitleme ölçümleri görüntülemek için depolama Eşitleme Hizmetleri kaynak türü seçin.

Aşağıdaki ölçümler Azure dosya eşitleme için Azure İzleyici'de kullanılabilir:

| Ölçüm adı | Açıklama |
|-|-|
| Eşitlenen bayt | (Karşıya yükleme ve indirme) aktarılan verilerin boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Bulut katmanlaması geri çağırma | Geri verilerin boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu Adı |
| Dosyalar eşitlenmiyor | Eşitleme başarısız olan dosya sayısı.<br><br>Birim: Sayı<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Eşitlenmiş dosyaları | Dosya sayısı (karşıya yükleme ve indirme) aktardı.<br><br>Birim: Sayı<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Sunucu sinyali | Sunucudan alınan sinyal sayısı.<br><br>Birim: Sayı<br>Toplama türü: Maksimum<br>Geçerli boyut: Sunucu Adı |
| Eşitleme oturumu sonucu | Eşitleme oturumu sonucu (1 = başarılı eşitleme oturumu; 0 başarısız eşitleme oturumu =)<br><br>Birim: Sayı<br>Toplama türleri: Maksimum<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |

## <a name="windows-server"></a>Windows Server

Windows Server'da bulut katmanlama, kayıtlı sunucu görüntüleyebilir ve sistem durumu eşitleyin.

### <a name="event-logs"></a>Olay günlükleri

Telemetri olay günlüğüne sunucuda kayıtlı sunucu, eşitleme ve bulut katmanlaması sistem durumu izlemek için kullanın. Telemetri olay günlüğü, uygulamalar ve Olay Görüntüleyicisi'nde Services\Microsoft\FileSync\Agent altında bulunur.

Sync Health
- Eşitleme oturumu tamamlandıktan sonra olay kimliği 9102 günlüğe kaydedilir. Bu olay Eşitleme oturumları tamamlanmasıyla, belirlemek için kullanılması gereken (HResult = 0) ve öğe başına olup olmadığını Eşitleme hataları. Daha fazla bilgi için aşağıdaki belgelere bakın: [Eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [öğesi başına hataları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > Bazen Eşitleme oturumları genel başarısız veya sıfır olmayan PerItemErrorCount sahip ancak hala ilerleme, başarılı bir şekilde eşitleniyor bazı dosyalar ile yapın. Bu, oturumun ne kadar başarılı olup size uygulanan alanları (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount ve AppliedSizeBytes) görülebilir. Bir satır artan bir uygulanan sayımı başarısız olan ancak birden çok eşitleme oturumu görürseniz, bir destek bileti açmadan önce denemek için eşitleme zamanı vermeniz gerekir.

- Bir active eşitleme oturumu varsa olay kimliği 9302 5-10 dakikada bir günlüğe kaydedilir. Bu olay, geçerli eşitleme oturumu ilerleme kaydediyor mu belirlemek için kullanılmalıdır (AppliedItemCount > 0). Eşitleme İlerleme kaydediyor değil, eşitleme oturumu sonunda başarısız olması ve bir olay kimliği 9102 hata günlüğe kaydedilir. Daha fazla bilgi için aşağıdaki belgelere bakın: [Eşitleme İlerleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)

Kayıtlı sunucu durumu
- Olay Kimliği 9301 oluşturun ne zaman bir sunucu işler hizmetini sorgular her 30 saniyede bir günlüğe kaydedilir. GetNextJob durumuyla tamamlanırsa = 0, sunucu hizmeti ile iletişim kuramıyor. GetNextJob hatayla tamamlanırsa denetleyin [sorun giderme belgelerinin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) Kılavuzu.

Bulut Katmanlaması sistem durumu
- Bir sunucuda katmanlama etkinliğini izlemek için olay kimliği 9003 9016 ve 9029 Telemetri olay günlüğüne (uygulamalar ve Olay Görüntüleyicisi'nde Services\Microsoft\FileSync\Agent altında bulunur) kullanın.

  - Olay Kimliği 9003 sunucu uç noktası için hata dağıtımını sağlar. Örneğin, toplam hata sayısı, hata kodu, vb. Unutmayın, hata kodu bir olay kaydedilir.
  - Olay Kimliği 9016 hayalet sonuçları için bir birim sağlar. Örneğin, boş alan yüzdesi olan dosya sayısı, oturumda ghost vb. için başarısız oldu. dosya sayısı hayalet kopyası.
  - Olay Kimliği 9029 hayalet oturum bilgilerini sunucu uç noktası sağlar. Örneğin, dosyaların sayısı, oturumda çalıştı dosya sayısı oturumda, dosyaların sayısı, katmanlı zaten katmanlı, vs.
  
- Bir sunucuya geri çağırma etkinliği izlemek için olay kimliği 9005 9006 ve 9009 9059 Telemetri olay günlüğüne (uygulamalar ve Olay Görüntüleyicisi'nde Services\Microsoft\FileSync\Agent altında bulunur) kullanın.

  - Olay Kimliği 9005 sunucu uç noktası için geri çağırma güvenilirlik sağlar. Örneğin, erişilen, benzersiz dosyaları toplam benzersiz başarısız erişim, vb. ile toplam sayısı.
  - Sunucu uç noktası için geri çağırma hatası dağıtım öğesini belirten Olay No. 9006 sağlar. Örneğin, toplam başarısız istekler, hata kodu, vb. Unutmayın, hata kodu bir olay kaydedilir.
  - Olay Kimliği 9009 sunucu uç noktası için geri çağırma oturum bilgilerini sağlar. Örneğin, DurationSeconds, CountFilesRecallSucceeded CountFilesRecallFailed, vb.
  - Olay Kimliği 9059 uygulama geri çekme dağıtım için sunucu uç noktası sağlar. Örneğin, ShareId, uygulama adı ve TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Performans sayaçları

Azure dosya eşitleme performans sayaçları sunucuda eşitleme etkinliğini izlemek için kullanın.

Azure dosya eşitleme performansını görüntülemek için sunucu üzerinde sayaçları, Performans İzleyicisi'ni (Perfmon.exe) başlatın ve sayaçlar AFS bayt aktarma ve eşitleme işlemlerine AFS nesneleri altında bulunabilir.

Azure dosya eşitleme aşağıdaki performans sayaçlarını Performans İzleyicisi'nde kullanılabilir:

| Performans Object\Counter adı | Açıklama |
|-|-|
| AFS bayt Transferred\Downloaded bayt/sn | Saniye başına indirilen bayt sayısı. |
| AFS bayt Transferred\Uploaded bayt/sn | Saniye başına karşıya yüklenen bayt sayısı. |
| AFS bayt Transferred\Total bayt/sn | Toplam bayt / saniye (karşıya yükleme ve indirme). |
| AFS eşitleme Operations\Downloaded eşitleme dosyaları/sn | Saniye başına indirilen dosyaların sayısı. |
| AFS eşitleme Operations\Uploaded eşitleme dosyaları/sn | Saniye başına karşıya yüklenen dosya sayısı. |
| AFS eşitleme Operations\Total eşitleme dosya işlemi/sn | Eşitlenmiş dosyaları (karşıya yükleme ve indirme) toplam sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Güvenlik Duvarı ve Ara sunucu ayarlarını göz önünde bulundurun.](storage-sync-files-firewall-and-proxy.md)
- [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md)
- [Azure dosyaları hakkında sık sorulan sorular](storage-files-faq.md)
