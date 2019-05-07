---
title: Azure dosyaları performans sorunlarını giderme kılavuzu
description: Azure premium dosya paylaşımları (Önizleme) ve ilişkili geçici çözümler ile performans sorunlarını bilinir.
services: storage
author: gunjanj
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 5ae0bb736a7cc0bbc38df5905abc5d8a71f60eb9
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190045"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Azure dosyaları performans sorunlarını giderme

Premium Azure dosya paylaşımları (Önizleme) ilgili bazı yaygın sorunlar bu makalede listelenmektedir. Bu sorunları karşılaştığında olası nedenler ve geçici çözümler sağlar.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Yüksek gecikme süresi, aktarım hızının düşük olmasını ve genel performans sorunları

### <a name="cause-1-share-experiencing-throttling"></a>1. neden: Yaşayan azaltma paylaşın

Bir paylaşımı varsayılan kota (en fazla 300 veri bloğu için bir saat için olası) ile 100 temel IOPS sağlar 100 GiB ' dir. Sağlama ve IOPS ilişkisini hakkında daha fazla bilgi için bkz. [sağlanan paylaşımlar](storage-files-planning.md#provisioned-shares) Planlama Kılavuzu'nun bölümü.

Paylaşımınızın kısıtlanan, onaylamak için portalda Azure ölçümleri yararlanabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** bulun **ölçümleri**.

1. **Ölçümler**’i seçin.

1. Kaynak depolama hesabınızı seçin.

1. Seçin **dosya** ölçüm ad alanı olarak.

1. Seçin **işlemleri** ölçüm olarak.

1. İçin bir filtre ekleyin **ResponseType** ve tüm isteklere yanıt kodunu olup olmadığını görmek için onay **SuccessWithThrottling**.

![Premium fileshares ölçümleri seçenekleri](media/storage-troubleshooting-premium-fileshares/metrics.png)

### <a name="solution"></a>Çözüm

- Artış, paylaşımında daha yüksek bir kota belirterek sağlanan kapasiteyi paylaşın.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>2. neden: Meta veri/ad alanı ağır iş yükü

İsteklerinizin çoğunu ise meta veri merkezli (createfile/openfile/closefile/QueryInfo/querydirectory gibi) sonra gecikme süresini okuma/yazma işlemleri için karşılaştırıldığında daha zayıf olur.

İsteklerinizin çoğunu meta veri merkezli olup olmadığını onaylamak için yukarıdaki adımların aynısını kullanabilirsiniz. Filtre için ekleme yerine dışında **ResponseType**, için bir filtre ekleyin **API adı**.

![Ölçümlerinizin API adı için filtre](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Geçici çözüm

- Uygulama meta veri işlemi sayısını azaltmak için değiştirilip değiştirilmediğini denetleyin.

### <a name="cause-3-single-threaded-application"></a>3. neden: Tek iş parçacıklı uygulama

Müşteri tarafından kullanılmakta olan uygulamasının tek iş parçacıklı ise, bu önemli ölçüde daha düşük IOPS/işleme, sağlanan paylaşım boyutuna göre mümkün olan en yüksek değerinden neden olabilir.

### <a name="solution"></a>Çözüm

- Uygulama paralellik iş parçacığı sayısını artırma.
- Uygulamalara paralellik mümkün olduğu geçin. Örneğin, kopyalama işlemleri için müşterilerin AzCopy veya RoboCopy Windows istemcilerinden kullanabilirsiniz veya **paralel** Linux istemcilerinde komutu.

## <a name="very-high-latency-for-requests"></a>İstekler için çok yüksek gecikme süresi

### <a name="cause"></a>Nedeni

İstemci VM, premium dosya paylaşımı farklı bir bölgede bulunamıyor.

### <a name="solution"></a>Çözüm

- Premium dosya paylaşımı ile aynı bölgede bulunan bir VM'den uygulamayı çalıştırın.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>İstemci ağ tarafından desteklenen en yüksek aktarım hızı elde etmek oluşturulamıyor

Olası bir nedeni, yetersiz olduğunu fo SMB çok kanal desteği. Şu anda, Azure dosya paylaşımları, yalnızca VM istemciden sunucuya yalnızca bir bağlantı olduğundan tek bir kanal destekler. Bir çekirdek tarafından ulaşılabilir bir VM'den en yüksek aktarım bağlı şekilde tek Bu bağlantı tek bir çekirdek VM, istemci üzerinde pegged.

### <a name="workaround"></a>Geçici çözüm

- Bir VM ile daha büyük bir çekirdek edinme, aktarım hızını artırmanıza yardımcı olabilir.
- İstemci uygulamasını çalıştıran birden fazla VM'den aktarım hızı artar.
- Mümkün olduğunda REST API'lerini kullanın.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Aktarım hızı Linux istemcilerde Windows istemcilerine karşılaştırıldığında önemli ölçüde daha küçük.

### <a name="cause"></a>Nedeni

Bu, Linux üzerinde SMB istemci uygulama ile bilinen bir sorundur.

### <a name="workaround"></a>Geçici çözüm

- Yükü birden çok VM arasında yayılabilir.
- Aynı VM'de ile birden çok bağlama noktaları kullanan **nosharesock** seçeneği ve yayılan yük boyunca bu bağlama noktaları.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Kapsamlı açma/kapatma işlemleri ile ilgili meta verileri ağır iş yükleri için yüksek gecikme süresi.

### <a name="cause"></a>Nedeni

Dizin kiralama desteği eksikliği.

### <a name="workaround"></a>Geçici çözüm

- Mümkünse, kısa bir süre içinde aynı dizine aşırı açma/kapatma tutamacı kaçının.
- Linux VM'ler için dizin girişi önbellek zaman aşımı belirterek artırmak **actimeo =<sec>**  bağlama seçeneği olarak. Üç veya beş gibi daha büyük bir değer yardımcı olabilecek şekilde varsayılan olarak, bir saniye olan.
- Linux VM'ler için çekirdek 4.20 veya üzeri sürüme yükseltin.

## <a name="low-iops-on-centosrhel"></a>CentOS/rhel düşük IOPS

### <a name="cause"></a>Nedeni

Birden fazla g/ç derinliği CentOS/RHEL üzerinde desteklenmiyor.

### <a name="workaround"></a>Geçici çözüm

- CentOS 8 yükseltme / RHEL 8.
- Ubuntu için değiştirin.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>IOPS için dalgalanmalara/saw-tooth deseni

### <a name="cause"></a>Nedeni

İstemci uygulama, temel IOPS sürekli olarak aşıyor. Şu anda hiçbir hizmet tarafı yoktur istek yükünü düzgünleştirme, bu nedenle istemci IOPS, temel aşarsa, kısıtlanan hizmet tarafından. Bu kısıtlama dalgalanmalara/saw-tooth IOPS desen karşılaşan istemci neden olabilir. Bu durumda, istemci tarafından gerçekleştirilen ortalama IOPS, temel IOPS daha düşük olabilir.

### <a name="workaround"></a>Geçici çözüm

- Paylaşım yok kısıtlanmazsınız böylece istemci uygulamadan istek yükünü azaltın.
- Böylece paylaşımı olmayan kısıtlanmazsınız paylaşımının kotasını artırın.

## <a name="excessive-directoryopendirectoryclose-calls"></a>DirectoryOpen/DirectoryClose çağrıları

### <a name="cause"></a>Nedeni

Varsa DirectoryOpen/DirectoryClose çağrılarının sayısı arasında ilk API çağrıları ve birçok çağrı, Azure VM istemcide yüklü virüsten koruma ile ilgili bir sorun olabilir, yapmak için istemci beklemiyoruz.

### <a name="workaround"></a>Geçici çözüm

- Bu sorun için bir düzeltme kullanılabilir [için Nisan Platform güncelleştirmesi Windows](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform).

## <a name="file-creation-is-slower-than-expected"></a>Dosya oluşturma, beklenenden daha yavaştır

### <a name="cause"></a>Nedeni

Çok sayıda dosya oluşturmayı kullanan iş yükleri, premium dosya paylaşımlarının performansını ve standart dosya paylaşımları arasında önemli bir fark görmez.

### <a name="workaround"></a>Geçici çözüm

- Yok.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Windows 8.1 veya Server 2012 R2'in yavaş performans

### <a name="cause"></a>Nedeni

Daha yüksek g/ç kullanımlı iş yükleri için Azure dosyaları erişim gecikme süresi bekleniyor.

### <a name="workaround"></a>Geçici çözüm

- Kullanılabilir yükleme [düzeltme](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).