---
title: "Azure Zaman Serisi Görüşleri’ne Genel Bakış | Microsoft Docs"
description: "Zaman serisi verilerinin analizi ve IoT çözümleri için yeni bir hizmet olan Azure Zaman Serisi Görüşleri’ne giriş"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 1814459e47280af62450a4093140ab6ab9b765fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri nedir?

Azure Zaman Serisi Görüşleri depolama, analiz ve görselleştirme bileşenleri olan, milyarlarca olayı eşzamanlı olarak girmeyi, depolamayı, incelemeyi ve analiz etmeyi kolaylaştıran ve yönetilen bir bulut hizmetidir. Zaman Serisi Görüşleri verilerinize ilişkin genel bir görünüm sunar, gizli eğilimleri ve anormallikleri keşfetmenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirmenize yardımcı olarak IoT Çözümlerinizi hızla doğrulamanıza ve cihazların kapalı kalma süresinden kaynaklanan maliyetleri önlemenize olanak sağlar. Zaman Serisi Görüşleri olay aracılarından (örn. IoT Hubs veya Event Hubs) zaman serisi verilerini alır, verilerin dizinini oluşturur ve yapılandırılabilir bir saklama ilkesine göre kullanım dışı bırakır. Kullanıcılar sezgisel bir UX veya REST Sorgu API’leri aracılığıyla verileri kullanır.

![Zaman Serisi Görüşleri’ne Genel Bakış](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Birincil senaryolar

* IoT çözümlerini dakikalar içinde izleyin ve doğrulayın.
* IoT verilerini ölçekleyerek görselleştirin ve analiz edin.
* Kök neden analizini ve anormallik algılamayı hızlandırın.
* Çok sayıda cihazın, tesisin ve verinin genel görünümünü oluşturun.

## <a name="capabilities-and-benefits"></a>İşlevler ve avantajlar

* **Başlamak çok kolay**: Azure Zaman Serisi Görüşleri verilerin önceden hazırlanmasını gerektirmez ve inanılmaz hızlı çalışır. Azure IoT Hub'ınızdaki veya Event Hub'ındaki milyarlarca olaya dakikalar içinde bağlanın. Bağlandıktan sonra, IoT çözümlerinizi hızla doğrulamak için sensör verilerini saniyeler içinde görselleştirin ve verilerinizle etkileşim kurmaya hemen başlayın. Zaman Serisi Görüşleri kolay kullanılır; tek bir kod satırı bile yazmadan verilerinizle etkileşimli çalışabilirsiniz.  Yeni dil öğrenmek gerekmez; Zaman Serisi Görüşleri ileri düzey kullanıcılara ayrıntılı, serbest metin kullanılan bir sorgu yüzeyi ve herkese üzerine gelip tıklamayla ulaşılan açıklamalar sağlar.

* **Neredeyse Gerçek zamanlı görüşler**: Zaman Serisi Görüşleri her gün, bir dakikalık bir gecikme süresiyle yüz milyonlarca sensör olayını alabildiğinden, değişikliklere hızla yanıt verebilirsiniz. Zaman Serisi Görüşleri, eğilimleri ve anormallikleri hızla saptamanıza, karmaşık kök-neden analizleri yürütmenize ve masraflı sistem kapatma sürelerini önlemenize yardımcı olarak sensör verileriniz üzerinde derin görüşler kazanmanıza katkıda bulunur. Gerçek zamanlı verilerle geçmiş verileri arasında çapraz bağıntıya olanak tanıdığından, Zaman Serisi Görüşleri verilerindeki gizli eğilimleri ortaya çıkarmanıza yardımcı olur.

* **Özel çözümler oluşturma**: Azure Zaman Serisi Öngörüleri verilerini mevcut uygulamalarınıza ekleyin ya da Zaman Serisi Öngörüleri REST API’leriyle yeni özel çözümler oluşturun. Diğer kişilerin de keşiflerinizi incelemesini sağlamak için paylaşabileceğiniz kişiselleştirilmiş görünümler oluşturun ve bunları paylaşın.

* **Ölçeklenebilirlik**: Zaman Serisi Görüşleri, ölçekli olarak IoT'yi destekleyecek şekilde tasarlanmıştır. Önizlemede, günde 1 milyon ile 100 milyon arası olay alabilir ve varsayılan saklama süresi 31 gündür. Muazzam miktarlardaki geçmiş verilerine ek olarak canlı veri akışlarını da neredeyse gerçek zamanlı olarak görselleştirebilir ve analiz edebilirsiniz. Sürekli artan kurumsal ölçekle başa çıkmak için ilerletme, giriş ve saklama hızları artırılacaktır.

## <a name="time-series-insights-glossary"></a>Zaman Serisi Öngörüleri sözlüğü

* **Ortam**: Ortam, giriş ve depolama kapasitesi olan bir Azure kaynağıdır.  Müşteriler, Azure Portal aracılığıyla ortamları kendilerine gereken kapasiteyle hazırlar.
* **Olay Kaynağı**: Olay Kaynağı, Azure Event Hubs gibi bir olay aracısından türetilir.  Zaman Serisi Görüşleri doğrudan Olay Kaynaklarına bağlanarak, hiç kod yazmadan veri akışını alır. Şu anda Zaman Serisi Görüşleri, Azure Event Hubs'ı ve Azure IoT Hub'larını destekler.
* **Başvuru verileri**: Zaman Serisi Görüşleri kullanıcılara zaman serisi verilerini başvuru verileriyle birleştirme olanağı sağlar.  Başvuru verileri cihazlar hakkındaki meta verileri veya görece seyrek değişen diğer statik verileri içerebilir. Zaman Serisi Görüşleri başvuru verilerini veri akışlarıyla birleştirerek, kullanıcıların bu verileri neredeyse gerçek zamanlı olarak görselleştirmesine ve analiz etmesine olanak tanır.
