---
title: Azure Stream analytics'te kontrol noktası ve yeniden yürütme işi kurtarma kavramları
description: Bu makalede, Azure Stream analytics'te kontrol noktası ve yeniden yürütme işi kurtarma kavramlarını açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 9dcfbd4b5fcc8462c88b16f585424166ecd3d499
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61361904"
---
# <a name="checkpoint-and-replay-concepts-in-azure-stream-analytics-jobs"></a>Azure Stream Analytics işlerini kontrol noktası ve yeniden yürütme kavramları
Bu makalede, bu kurtarma işi sahip iç denetim noktası ve yeniden yürütme kavramları Azure Stream Analytics ve etkisi açıklanır. Her zaman bir Stream Analytics işi çalıştırır, durum bilgilerini dahili olarak korunur. Bu durum bilgilerini bir denetim noktasında düzenli olarak kaydedilir. Bir iş hatası veya yükseltme oluşması durumunda bazı senaryolarda, iş kurtarma için denetim noktası bilgileri kullanılır. Diğer durumlarda, kontrol noktasını kurtarma için kullanılamaz ve bir yeniden yürütme gereklidir.

## <a name="stateful-query-logicin-temporal-elements"></a>Durum bilgisi olan sorgu mantığının zamana bağlı öğeleri
Azure Stream Analytics işi benzersiz yeteneğinin, pencereli toplamlar, zamana bağlı birleştirmeler ve geçici analiz işlevleri gibi durum bilgisi olan işlem gerçekleştirmektir. İş çalıştırıldığında bu işleçlerden her biri durum bilgilerini tutar. Bu sorgu öğeleri için maksimum pencere boyutunu yedi gündür. 

Geçici pencere kavramı çeşitli Stream Analytics sorgu öğelerin görünür:
1. Pencereli toplamlar (grup tarafından atlaması ve windows kayan atlayan)

2. Zamana bağlı birleşimler (birleştirme DATEDIFF ile)

3. Zamana bağlı analitik işlevler (ISFİRST, LAST ve GECİKME süresi sınırı)


## <a name="job-recovery-from-node-failure-including-os-upgrade"></a>İşletim sistemi yükseltme de dahil olmak üzere, düğüm hatasından kurtarma işi
Bir Stream Analytics işi her çalıştırıldığında, dahili olarak onu kullanıma birden çok çalışan düğümü üzerinde çalışacak şekilde ölçeklendirilir. Her alt düğümün durumu yardımcı olan bir hata oluşması durumunda kurtarma sistem, birkaç dakikada denetim noktası oluşturuldu.

Bazen, verilen çalışan düğümü başarısız olabilir veya işletim sistemi yükseltmesi için çalışan düğümüne ortaya çıkabilir. Stream Analytics otomatik olarak kurtarmak için yeni bir iyi durumdaki düğüme alır ve en son kullanılabilir denetim noktasından önceki alt düğümün durumunun geri yüklendiği. Çalışmayı sürdürmek için az miktarda bir yeniden yürütme süresi ne zaman denetim noktası alındığını durumunu geri yüklemek gereklidir. Genellikle, geri yükleme boşluk yalnızca birkaç dakikadır. İş için yeterli sayıda akış birimi seçildiğinde yeniden yürütme hızla tamamlanmalıdır. 

Tam olarak paralel sorguda bir alt düğümde hata oluştuktan sonra Kaçırdığınız süresini orantılıdır:

[Giriş olayı oranı] x [aralık uzunluğu] / [bölümleri işleme sayı]

Düğüm hatası nedeniyle hiç olmadığı kadar önemli bir işleme gecikmesi inceleyin ve işletim sistemi yükseltme, sorgu tam olarak yapmayı düşünün paralel ve daha fazla akış birimi ayırmak için işi ölçeklendirin. Daha fazla bilgi için [verimliliğini artırmak için Azure Stream Analytics işi ölçeklendirme](stream-analytics-scale-jobs.md).

Bu tür bir kurtarma işlemi gerçekleşen zaman geçerli Stream Analytics bir rapor göstermez.

## <a name="job-recovery-from-a-service-upgrade"></a>Hizmet yükseltme işi kurtarma 
Microsoft Stream Analytics işleri Azure hizmetinde çalışan ikili dosyaları bazen yükseltir. Bu saatler, kullanıcıların çalışan işleri daha yeni sürüme yükseltilir ve iş otomatik olarak yeniden başlatır. 

Şu anda kurtarma kontrol noktası biçimi yükseltmeleri arasında korunmaz. Sonuç olarak, akış sorgu durumu tamamen yeniden yürütme teknik kullanılarak geri yüklenmelidir. Stream Analytics işlerinin tam yeniden sağlamak için sorgu penceresinde aynı kaynak verilere yönelik bekletme ilkesi en az ayarlamak önemlidir önce girişe boyutlandırır. Kaynak verilerin yeteri kadar geri tam pencere boyutunu içerecek şekilde tutulabileceği değil olduğundan Bunu yapmazsanız, hizmet yükseltme sırasında yanlış ya da kısmi sonuçları neden olabilir.

Genel olarak, gerekli yeniden yürütme miktarını göre ortalama olay oranı çarpılan penceresinin boyutunu orantılıdır. Örneğin, bir iş ile bir giriş oran, saniyede 1000 olay için bir saatten daha büyük bir pencere boyutu büyük yeniden yürütme boyutuna sahip kabul edilir. Verilerin bir saat kadar bazı uzun bir süre için durum tam oluşturabilir ve çıkış (çıktı) neden olabilecek doğru sonuçlar Gecikmeli başlatmak için yeniden işlenen olması gerekebilir. Hiçbir windows veya diğer geçici işleçler sorgularla ister `JOIN` veya `LAG`, sıfır yeniden yürütme olması gerekir.

## <a name="estimate-replay-catch-up-time"></a>Oldukça yeniden yürütme süresi, tahmin
Bir hizmeti yükseltmesi nedeniyle gecikme uzunluğunu tahmin etmek için bu tekniği izleyebilirsiniz:

1. Giriş olayı beklenen olay hızı anda sorgunuzda büyük pencere boyutunu kapsayacak şekilde Hub'la yeterli veri yükleyin. Olayları zaman damgası olmalıdır bu süre boyunca duvar saati süresi yakın olarak bir canlı girdi akışı olup olmadığını. Örneğin, 3 günlük penceresini sorgunuzda varsa, üç gün için olay Hub'ına olayları gönderme ve olayları göndermeye devam eder. 

2. İş kullanmaya başlamak **artık** başlangıç saati olarak. 

3. Başlangıç saati ve ilk çıkış oluşturulduğunda arasındaki süreyi ölçer. İş hizmeti yükseltmesi sırasında ödemesi gerekir ne kadar gecikme kaba zamandır.

4. İşinizi bölümlemek ve daha fazla düğüm yük dağıldığında şekilde SUs sayısını artırmak gecikme çok uzunsa deneyin. Alternatif olarak, sorgunuzu pencere boyutlarını azaltmayı göz önünde bulundurun ve daha fazla toplama veya diğer durum bilgisi olan (örneğin, Azure SQL veritabanı'nı kullanarak) bir aşağı akış havuzunda Stream Analytics işi tarafından üretilen çıkış işleme gerçekleştirin.

Genel Hizmet kararlılık kaygısı görev açısından kritik iş yükseltme sırasında yinelenen işleri eşleştirilmiş Azure bölgelerinde çalıştırılan göz önünde bulundurun. Daha fazla bilgi için [garantisi Stream Analytics iş güvenilirliği hizmet güncelleştirmeleri sırasında](stream-analytics-job-reliability.md).

## <a name="job-recovery-from-a-user-initiated-stop-and-start"></a>Bir kullanıcı tarafından başlatılan durdurma ve başlatma işi kurtarma
Sorgu sözdizimi akış işi, düzenlemek veya giriş ve çıkışları ayarlamak için işin değişiklikleri yapın ve iş tasarım yükseltmek için durdurulması gerekir. Böyle senaryolarda kurtarma senaryosunda bir kullanıcı Akış işini durdurur ve yeniden başlatır, hizmet yükseltme benzerdir. 

Bir kullanıcı tarafından başlatılan iş yeniden başlatma denetim noktası verileri kullanılamaz. Çıkış gecikmesi böyle bir yeniden başlatma sırasında tahmin etmek için önceki bölümde açıklandığı gibi aynı yordamı kullanın ve gecikme çok uzunsa benzer risk azaltma uygulayın.

## <a name="next-steps"></a>Sonraki adımlar
Güvenilirlik ve ölçeklenebilirlik ile ilgili daha fazla bilgi için şu makalelere bakın:
- [Öğretici: Azure Stream Analytics işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md)
- [Azure Stream Analytics işi verimliliğini artırmak için ölçeklendirme](stream-analytics-scale-jobs.md)
- [Stream Analytics işi güvenilirlik garanti sırasında hizmet güncelleştirmeleri](stream-analytics-job-reliability.md)
