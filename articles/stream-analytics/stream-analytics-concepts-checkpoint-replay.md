---
title: Azure Stream Analytics denetim noktası ve yeniden yürütme iş kurtarma kavramları
description: Bu makalede Azure akış analizi denetim noktası ve yeniden yürütme iş kurtarma kavramları açıklanmaktadır.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: 1a7cb6c5d9c3383b127ce38ae21bb2dc811e1f2e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31529488"
---
# <a name="checkpoint-and-replay-concepts-in-azure-stream-analytics-jobs"></a>Azure akış analizi işi denetim noktası ve yeniden yürütme kavramları
Bu makalede bu işi kurtarma sahip iç denetim noktası ve yeniden yürütme kavramlarını Azure akış analizi ve etkisini açıklar. Her zaman bir akış analizi işi çalışır, durum bilgilerini dahili olarak korunur. Bu durum bilgilerini bir denetim noktasını düzenli olarak kaydedilir. Bir iş hatası veya yükseltme oluşursa, bazı senaryolarda, iş kurtarma için kontrol noktası bilgileri kullanılır. Diğer durumlarda, kurtarma için kontrol noktası kullanılamaz ve bir yeniden yürütme gereklidir.

## <a name="stateful-query-logic-in-temporal-elements"></a>Zamana bağlı öğelerindeki durum bilgisi sorgusu mantığı
Azure Stream Analytics işi benzersiz yeteneğini pencerelenmiş toplamlar, zamana bağlı birleştirmeler ve zamana bağlı analitik işlevler gibi durum bilgisi olan işlem gerçekleştirmek için biridir. İş çalıştırıldığında her Bu operatörler durum bilgilerini tutar. Bu sorgu öğeler için en fazla pencere boyutu yedi gündür. 

Geçici pencere kavramı birkaç Stream Analytics sorgu öğeleri görünür:
1. Pencerelenmiş toplamlar (grubu tarafından atlamalı ve windows kayan dönen)

2. Zamana bağlı birleşimler (birleşim DATEDIFF ile)

3. Zamana bağlı analitik işlevler (ISFİRST, son ve GECİKME sınırı süre ile)


## <a name="job-recovery-from-node-failure-including-os-upgrade"></a>İşletim sistemi yükseltme de dahil olmak üzere düğüm hatasından iş kurtarma
Akış analizi işi her çalıştığında, dahili olarak, giden birden çok alt düğümleri arasında çalışmaya ölçeklendirilir. Her alt düğümün her beş dakikada bir hata oluşursa kurtarmak sistem yardımcı olan belirttiğinizde durumudur.

Bazen, verilen çalışan düğüme başarısız olabilir veya bir işletim sistemi yükseltmesi için çalışan düğümüne oluşabilir. Otomatik olarak kurtarmak için yeni bir sağlıklı düğüm akış analizi edinir ve önceki alt düğümün durumu en son kullanılabilir denetim noktasından geri yüklenir. İş devam etmek için yeniden yürütme az miktarda zaman denetim noktası alındığını saati durumunu geri yüklemek gereklidir. Genellikle, geri yükleme boşluk yalnızca birkaç dakika olur. İş için yeterli Streaming Units seçildiğinde yeniden yürütme hızla tamamlanmalıdır. 

Tam olarak paralel sorguda bir alt düğüm hatasından sonra Yakala süresini orantılıdır:

[Giriş olay hızı] x [aralık uzunluğu] / [sayısı bölümleri işleme]

Şimdiye kadar önemli bir işleme gecikmesi düğüm hatası nedeniyle inceleyin ve işletim sistemi yükseltme, sorgu tam olarak yapmayı düşünün paralel ve daha fazla akış birimi ayırmak için iş ölçeklendirin. Daha fazla bilgi için bkz: [verimliliğini artırmak için bir Azure akış analizi işi ölçeklendirme](stream-analytics-scale-jobs.md).

Bu tür bir kurtarma işlemi yer alırken geçerli akış analizi raporu göstermez.

## <a name="job-recovery-from-a-service-upgrade"></a>Hizmet yükseltme iş kurtarma 
Microsoft, bazen akış analizi işleri Azure hizmetinde çalıştırmak ikili dosyalarını yükseltir. Şu zamanlarda kullanıcıların çalışan işleri daha yeni sürümüne yükseltilir ve iş otomatik olarak yeniden başlatılır. 

Şu anda kurtarma kontrol noktası biçimi yükseltmeler arasında korunmaz. Sonuç olarak, akış sorgu durumunu tamamen yeniden yürütme teknik kullanılarak geri yüklenmelidir. Akış analizi işleri tam oynamanıza izin vermek üzere sorgu penceresi aynı gelen en az kaynak verileri için Bekletme İlkesi ayarlamak önemlidir önce giriş boyutlandırır. Veri kaynağını yetecek kadar geri tam pencere boyutunu içerecek şekilde korunmayabilir değil bu yana Bunu yapmazsanız, hizmet yükseltme sırasında yanlış ya da kısmi sonuçlarında neden olabilir.

Genel olarak, gerekli yeniden yürütme miktarı tarafından ortalama olay hızı çarpılan pencere boyutunu orantılıdır. Örneğin, bir giriş oran, saniyede 1000 olayların ile bir iş için bir saatten daha büyük bir pencere boyutu büyük yeniden yürütme boyutuna sahip kabul edilir. Büyük yeniden yürütme boyutuyla sorgular için bazı uzun bir süre için Gecikmeli output (çıktı) gözlemleyebilirsiniz. 

## <a name="estimate-replay-catch-up-time"></a>Yakalama yeniden yürütme süresi, tahmin
Bir hizmet yükseltmesi nedeniyle bekleme süresini tahmin etmek için bu tekniği izleyebilirsiniz:

1. Giriş olay Sorgunuzdaki beklenen olay hızı en büyük pencere boyutunu kapsayacak şekilde Hub yeterli verilerle yükleyin. Olaylar zaman damgası olmalıdır bu süre boyunca duvar saati süresi yakın olarak bir canlı Giriş akışı olup olmadığını. Örneğin, sorgunuzda 3 günlük penceresini varsa, üç gün boyunca olay Hub'ına olayları göndermek ve olayları göndermeye devam eder. 

2. İş kullanmaya başlamak **şimdi** başlangıç saati olarak. 

3. Başlangıç saati ve ilk çıkış oluşturulduğunda arasındaki süreyi ölçer. İş hizmeti yükseltmesi sırasında tabi ne kadar gecikme kaba saattir.

4. Gecikme çok uzunsa, işinizi bölümlemek ve daha fazla düğüm yükü yayılır şekilde SUs sayısını artırmak deneyin. Alternatif olarak, pencere boyutları Sorgunuzdaki azaltmayı göz önünde bulundurun ve daha fazla toplama veya aşağı akış havuz (örneğin, Azure SQL veritabanı kullanarak) Stream Analytics işinde tarafından üretilen çıkış işleme diğer durum bilgisi olan gerçekleştirin.

Genel Hizmet kararlılık kaygısı görev kritik işleri, yükseltme sırasında yinelenen işleri eşleştirilmiş Azure bölgelerinde çalıştırmayı deneyin. Daha fazla bilgi için bkz: [garantisi Stream Analytics işi güvenilirlik hizmet güncelleştirmeleri sırasında](stream-analytics-job-reliability.md).

## <a name="job-recovery-from-a-user-initiated-stop-and-start"></a>Bir kullanıcı tarafından başlatılan durdurma ve başlatma işi kurtarma
Sorgu sözdizimi iş akışında, düzenlemek veya girişleri ve çıkışları ayarlamak için iş değişiklikleri yapın ve iş tasarım yükseltmek için durdurulması gerekir. Bu senaryolarda, bir kullanıcı Akış işini durdurur ve yeniden başladığında kurtarma senaryosunda hizmet yükseltmesi için benzer. 

Bir kullanıcı tarafından başlatılan iş yeniden başlatma denetim noktası verileri kullanılamaz. Bu tür bir yeniden başlatma sırasında çıkış gecikme tahmin etmek için önceki bölümde açıklandığı gibi aynı yordamı kullanın ve gecikme çok uzunsa, benzer azaltma uygulayın.

## <a name="next-steps"></a>Sonraki adımlar
Güvenilirlik ve ölçeklendirilebilirlik hakkında daha fazla bilgi için bu makalelere bakın:
- [Öğretici: Azure akış analizi işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md)
- [Bir Azure akış analizi işi verimliliğini artırmak için ölçeklendirme](stream-analytics-scale-jobs.md)
- [Akış analizi işi güvenilirlik hizmet güncelleştirmeleri sırasında garanti](stream-analytics-job-reliability.md)
