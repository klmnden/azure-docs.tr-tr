---
title: Azure dosya eşitleme'yi izleyin | Microsoft Docs
description: Azure dosya eşitleme izlemeyi öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/28/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 86c4bf328430bbc623d8e493eec5db520d50ef82
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485989"
---
# <a name="monitor-azure-file-sync"></a>Azure Dosya Eşitleme’yi izleme

Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

Bu makalede, Azure İzleyici, depolama eşitleme hizmeti ve Windows Server'ı kullanarak Azure dosya eşitleme dağıtımınızı izleme açıklar.

Aşağıdaki izleme seçeneklerini şu anda kullanılabilir.

## <a name="azure-monitor"></a>Azure İzleyici

Kullanım [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) ölçümleri görüntüleyin ve Eşitleme uyarılarını yapılandırmak için bulut katmanlama ve sunucu bağlantısı.  

### <a name="metrics"></a>Ölçümler

Azure dosya eşitleme için ölçümleri, varsayılan olarak etkindir ve Azure İzleyici her 15 dakikada gönderilir.

Azure İzleyici'de Azure dosya eşitleme ölçümleri görüntülemek için seçin **depolama Eşitleme Hizmetleri** kaynak türü.

Aşağıdaki ölçümler Azure dosya eşitleme için Azure İzleyici'de kullanılabilir:

| Ölçüm adı | Açıklama |
|-|-|
| Eşitlenen bayt | (Karşıya yükleme ve indirme) aktarılan verilerin boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Bulut katmanlaması geri çağırma | Geri verilerin boyutu.<br><br>**Not**: Bu ölçüm gelecekte kaldırılacaktır. Bulut katmanlaması geri çağırma boyut ölçümü geri verilerin boyutunu izlemek için kullanın.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu adı |
| Bulut katmanlaması geri çağırma boyutu | Geri verilerin boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu adı, eşitleme grubu adı |
| Uygulama tarafından katmanlama geri çağırma boyutu bulut | Uygulama tarafından geri verilerin boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Uygulama adı, sunucu adı, eşitleme grubu adı |
| Bulut katmanlaması geri çağırma aktarım hızı | Verileri geri çağırma Aktarım boyutu.<br><br>Birim: Bayt<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu adı, eşitleme grubu adı |
| Dosyalar eşitleniyor değil | Eşitleme başarısız olan dosya sayısı.<br><br>Birim: Count<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Eşitlenmiş dosyaları | Dosya sayısı (karşıya yükleme ve indirme) aktardı.<br><br>Birim: Sayı<br>Toplama türü: Toplam<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı.<br><br>Birim: Count<br>Toplama türü: Maksimum<br>Geçerli boyut: Sunucu adı |
| Eşitleme oturumu sonucu | Eşitleme oturumu sonucu (1 = başarılı eşitleme oturumu; 0 başarısız eşitleme oturumu =)<br><br>Birim: Sayı<br>Toplama türleri: Maksimum<br>Geçerli boyut: Sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |

### <a name="alerts"></a>Uyarılar

Azure İzleyici'de uyarıları yapılandırmak için depolama eşitleme hizmeti seçin ve ardından [Azure dosya eşitleme ölçüm](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#metrics) uyarı için kullanılacak.  

Aşağıdaki tabloda bazı örnek senaryoları izlemek için ve uyarı için kullanılacak uygun ölçüm listeler:

| Senaryo | Ölçüm uyarısı için kullanılacak |
|-|-|
| Sunucu uç noktası durumu Portalı'nda hata = | Eşitleme oturumu sonucu |
| Dosyaları bir sunucuya eşitleme ya da bulut uç noktası başarısız oluyor | Dosyalar eşitleniyor değil |
| Kayıtlı sunucu depolama eşitleme hizmeti ile iletişim kurmak başarısız oluyor | Sunucu çevrimiçi durumu |
| Bulut katmanlaması geri çağırma boyutunu bir gün içinde 500GiB aştı  | Bulut katmanlaması geri çağırma boyutu |

Azure İzleyici'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için [Microsoft azure'da uyarılara genel bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="storage-sync-service"></a>Depolama eşitleme hizmeti

Kayıtlı sunucu sistem durumu, sunucu uç noktası durumu ve ölçümleri görüntülemek için Azure portalında depolama eşitleme hizmeti gidin. Kayıtlı sunucu durumunu görüntüleyebileceğiniz **kayıtlı sunucuları** dikey penceresinde ve sunucu uç noktası durumu **eşitleme grupları** dikey.

### <a name="registered-server-health"></a>Kayıtlı sunucu durumu

- Varsa **kayıtlı sunucu** durumu **çevrimiçi**, sunucunun başarıyla hizmetiyle iletişim kurulurken.
- Varsa **kayıtlı sunucu** durumu **çevrimdışı görünür**, sunucudaki depolama eşitleme İzleyicisi (AzureStorageSyncMonitor.exe) işleminin çalıştığını doğrulayın. Sunucu güvenlik duvarı veya proxy arkasında olup olmadığını [bu makalede](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) proxy ve Güvenlik Duvarı'nı yapılandırmak için.

### <a name="server-endpoint-health"></a>Sunucu uç noktası durumu

- Sunucu uç noktası durumu Portalı'nda sunucunun (ID 9102 ve 9302) Telemetri olay günlüğüne kaydedilir eşitleme olayları temel alır. Hata iptal gibi bir eşitleme oturumunu geçici bir hata nedeniyle başarısız olursa, geçerli eşitleme oturumu ilerleme kaydediyor mu sürece eşitleme hala portalda sağlıklı görünebilir. Olay Kimliği 9302 dosyaları uygulanmış olup olmadığını belirlemek için kullanılır. Daha fazla bilgi için [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [eşitleme ilerleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Eşitleme İlerleme kaydediyor değil çünkü portalı bir eşitleme hatası gösterirse bkz [sorun giderme belgelerinin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) Kılavuzu.

### <a name="metric-charts"></a>Ölçüm grafikleri

- Depolama eşitleme hizmeti Portalı'nda aşağıdaki ölçüm grafikleri görünür:

  | Ölçüm adı | Açıklama | Dikey adı |
  |-|-|-|
  | Eşitlenen bayt | (Karşıya yükleme ve indirme) aktarılan verilerin boyutu | Eşitleme grubu, sunucu uç noktası |
  | Bulut katmanlaması geri çağırma | Geri veri boyutu | Kayıtlı sunucular |
  | Dosyalar eşitleniyor değil | Eşitleme başarısız olan dosya sayısı | Sunucu uç noktası |
  | Eşitlenmiş dosyaları | Dosya sayısı (karşıya yükleme ve indirme) aktarılan | Eşitleme grubu, sunucu uç noktası |
  | Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı | Kayıtlı sunucular |

- Daha fazla bilgi için bkz. [Azure İzleyici](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > Depolama eşitleme hizmeti Portalı'nda grafikleri, 24 saatlik bir zaman aralığı vardır. Farklı tarih aralıkları veya boyutlar görüntülemek için Azure İzleyici'yi kullanın.

## <a name="windows-server"></a>Windows Server

Windows Server'da bulut katmanlama, kayıtlı sunucu görüntüleyebilir ve sistem durumu eşitleyin.

### <a name="event-logs"></a>Olay günlükleri

Telemetri olay günlüğüne sunucuda kayıtlı sunucu, eşitleme ve bulut katmanlaması sistem durumu izlemek için kullanın. Telemetri olay günlüğü altında Olay Görüntüleyicisi'nde bulunan *uygulamalar ve Services\Microsoft\FileSync\Agent*.

Eşitleme durumu:

- Eşitleme oturumu sona erdikten sonra olay kimliği 9102 günlüğe kaydedilir. Eşitleme oturumları başarılı olup olmadığını belirlemek için bu olayı kullanın (**HResult = 0**) ve öğe başına olup olmadığını Eşitleme hataları. Daha fazla bilgi için [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [öğesi başına hataları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) belgeleri.

  > [!Note]  
  > Bazen Eşitleme oturumları genel başarısız veya sıfır olmayan PerItemErrorCount vardır. Ancak yine de ilerleme yapmaları ve bazı dosyalar başarıyla eşitleyin. Uygulanan alanları AppliedFileCount, AppliedDirCount AppliedTombstoneCount ve AppliedSizeBytes gibi görebilirsiniz. Bu alanlar oturumun ne kadar başarılı söyleyin. Bir satırda başarısız birden çok eşitleme oturumu bakın ve artan bir uygulanan sayımı sahiptirler, bir destek bileti açmadan önce yeniden denemek için zaman verin.

- Bir active eşitleme oturumu varsa olay kimliği 9302 5-10 dakikada bir günlüğe kaydedilir. Geçerli eşitleme oturumu ilerleme kaydediyor mu belirlemek için bu olayı kullanın (**AppliedItemCount > 0**). Eşitleme İlerleme kaydediyor değil, eşitleme oturumu sonunda başarısız olması ve bir olay kimliği 9102 hata günlüğe kaydedilir. Daha fazla bilgi için [eşitleme ilerleme belgeleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Kayıtlı sunucu sistem durumu:

- Olay Kimliği 9301 oluşturun ne zaman bir sunucu işler hizmetini sorgular her 30 saniyede bir günlüğe kaydedilir. GetNextJob ile sonuçlansa **durumu = 0**, sunucu hizmeti ile iletişim kuramıyor. GetNextJob bir hata ile sonuçlansa denetleyin [sorun giderme belgelerinin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) Kılavuzu.

Katmanlama sağlık bulut:

- Bir sunucuda katmanlama etkinliğini izlemek için olay kimliği 9003 9016 ve 9029 Telemetri olay günlüğündeki Olay Görüntüleyicisi'ni altında bulunan, kullanmak *uygulamalar ve Services\Microsoft\FileSync\Agent*.

  - Olay Kimliği 9003 sunucu uç noktası için hata dağıtımını sağlar. Örneğin: Toplam hata sayısı ve hata kodu. Hata kodu bir olay günlüğe kaydedilir.
  - Olay Kimliği 9016 hayalet sonuçları için bir birim sağlar. Örneğin: Boş alan yüzdesi, oturumda hayalet kopyası dosyalarının sayısıdır ve dosya sayısını hayalet başarısız oldu.
  - Olay Kimliği 9029 hayalet oturum bilgilerini sunucu uç noktası sağlar. Örneğin: Dosya sayısı oturumda çalıştı dosyaların sayısı, oturumda katmanlı ve dosyaların sayısı, zaten katmanlı.
  
- Bir sunucuya geri çağırma etkinliği izlemek için olay kimliği 9005 9006 ve 9009 9059 Telemetri olay günlüğündeki Olay Görüntüleyicisi'ni altında bulunan, kullanmak *uygulamalar ve Services\Microsoft\FileSync\Agent*.

  - Olay Kimliği 9005 sunucu uç noktası için geri çağırma güvenilirlik sağlar. Örneğin: Toplam benzersiz dosya erişilen ve toplam benzersiz dosyalarıyla başarısız erişim.
  - Sunucu uç noktası için geri çağırma hatası dağıtım öğesini belirten Olay No. 9006 sağlar. Örneğin: Toplam başarısız istek ve hata kodu. Hata kodu bir olay günlüğe kaydedilir.
  - Olay Kimliği 9009 sunucu uç noktası için geri çağırma oturum bilgilerini sağlar. Örneğin: DurationSeconds CountFilesRecallSucceeded ve CountFilesRecallFailed.
  - Olay Kimliği 9059 uygulama geri çekme dağıtım için sunucu uç noktası sağlar. Örneğin: ShareId, uygulama adı ve TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Performans sayaçları

Azure dosya eşitleme performans sayaçları sunucuda eşitleme etkinliğini izlemek için kullanın.

Azure dosya eşitleme performans sayaçları sunucuda görüntülemek için Performans İzleyicisi'ni (Perfmon.exe) açın. Altına sayaçları bulabilirsiniz **AFS aktarılan bayt** ve **AFS eşitleme işlemleri** nesneleri.

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
