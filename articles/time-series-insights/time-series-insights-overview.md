---
title: "Azure Zaman Serisi Görüşleri’ne Genel Bakış | Microsoft Docs"
description: "Zaman serisi verilerinin analizi ve IoT çözümleri için yeni bir hizmet olan Azure Zaman Serisi Görüşleri’ne giriş"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/20/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: cc35c00bec18ee1d9ed52b29b68ddacc1136dc70
ms.lasthandoff: 04/25/2017

---

# <a name="overview-of-azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri’ne Genel Bakış

Azure Zaman Serisi Görüşleri depolama, analiz ve görselleştirme bileşenleri olan, milyarlarca olayı eşzamanlı olarak girmeyi, depolamayı, incelemeyi ve analiz etmeyi inanılmaz kolaylaştıran ve tümüyle yönetilen bir bulut hizmetidir. Zaman Serisi Görüşleri verilerinize ilişkin genel bir görünüm sunar, gizli eğilimleri ve anormallikleri keşfetmenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirmenize yardımcı olarak IoT Çözümlerinizi hızla doğrulamanıza ve görev açısından kritik cihazlarda, kapalı kalma süresinden kaynaklanan maliyetleri önlemenize olanak sağlar. Zaman Serisi Görüşleri olay aracılarından (örn. IoT Hubs veya Event Hubs) zaman serisi verilerini alır, verilerin dizinini oluşturur ve yapılandırılabilir bir saklama ilkesine göre kullanım dışı bırakır. Kullanıcılar kendi iş senaryoları için sezgisel bir UX veya REST Sorgu API’leri aracılığıyla verileri kullanır.

![Zaman Serisi Görüşleri’ne Genel Bakış](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Birincil senaryolar

* IoT çözümlerini dakikalar içinde doğrulama ve izleme
* Her ölçekte IoT verilerini sezgisel olarak görselleştirme ve analiz etme
* Kök neden analizini ve anormallik algılamayı hızlandırma
* Çok sayıda cihazın, tesisin ve verinin genel görünümünü oluşturma

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

* **Başlangıç yapması kolay**: Azure Zaman Serisi Görüşleri önceden veri hazırlığı gerektirmez ve inanılmaz hızlı çalışır; dolayısıyla Azure IoT Hub’ınızda veya Olay Hub’ınızda dakikalar içinde milyarlarca olaya bağlanabilirsiniz. Bağlandıktan sonra, IoT çözümünüzü hızla doğrulamak için sensör verilerinizi saniyeler içinde görselleştirebilir ve bunlarla etkileşimli çalışabilirsiniz. Zaman Serisi Görüşleri inanılmaz sezgiseldir ve kolay kullanılır; tek bir kod satırı bile yazmak zorunda kalmadan verilerinizle etkileşimli çalışabilirsiniz.  Bunlara ek olarak, yeni dil öğrenmek gerekmez çünkü Zaman Serisi Görüşleri ileri düzey kullanıcılara ayrıntılı, serbest metin kullanılan bir sorgu yüzeyi sağladığı gibi, yeni başlayanlara da üzerine gelip tıklamayla ulaşılan açıklamalar sağlar.

* **Saniyeler içinde neredeyse Gerçek zamanlı görüşler**: Hepsine aynı yerden ulaşabildiğiniz Zaman Serisi Görüşleri depolama, analiz ve görselleştirme bileşenleriyle, zaman serisi verilerinizden daha fazla değer elde edin. Zaman Serisi Görüşleri her gün, bir dakikalık bir gecikmeyle yüz milyonlarca sensör olayını alabildiğinden, değişikliklere hızla yanıt verebilirsiniz. Zaman Serisi Görüşleri, eğilimleri ve anormallikleri hızla saptamanıza yardımcı olarak sensör verileriniz üzerinde daha derin görüşler sağlar, karmaşık kök-neden analizlerini kolayca yürütmenize olanak tanır ve masraflı sistem kapatma sürelerini önler. Gerçek zamanlı verilerle geçmiş verileri arasında çapraz bağıntıya olanak tanıdığından, Zaman Serisi Görüşleri kullanıcıların verilerindeki gizli eğilimleri ortaya çıkarmasını sağlar.

* **Özel çözümler oluşturma**: Azure Zaman Serisi Öngörüleri verilerini mevcut uygulamalarınıza ekleyin ya da Zaman Serisi Öngörüleri REST API’leriyle yeni özel çözümler oluşturun.  Artı olarak, kişiselleştirilmiş görünümleri oluşturmak ve paylaşmak çok kolay olduğundan diğer kişiler sizin yaptığınız keşifleri basit bir yöntemle inceleyebilir.

* **Ölçeklenebilirlik**: Zaman Serisi Görüşleri, IoT ölçeğini destekleyecek şekilde tasarlanmıştır. Önizlemede, günde 1 milyon ile 100 milyon arası olay alabilir ve varsayılan saklama süresi 31 gündür. Zaman Serisi Görüşleri kullanıcıların muazzam miktarlardaki geçmiş verilerine ek olarak canlı veri akışlarını da neredeyse gerçek zamanlı olarak görselleştirmesine ve analiz etmesine olanak tanır. Sürekli artan kurumsal ölçekle başa çıkmak için ilerletme, giriş ve saklama hızları artırılacaktır.

## <a name="taxonomy-of-time-series-insights"></a>Zaman Serisi Görüşleri’nin Taksonomisi

* **Ortam**: Ortam, giriş ve depolama kapasitesi olan bir Azure kaynağıdır.  Müşteriler, Azure Portal aracılığıyla ortamları kendilerine gereken kapasiteyle hazırlar.
* **Olay Kaynağı**: Olay Kaynağı, Azure Event Hubs gibi bir olay aracısından türetilir.  Zaman Serisi Görüşleri doğrudan Olay Kaynaklarına bağlanarak, kullanıcının tek bir kod satırı bile yazmasına gerek kalmadan veri akışını alır. Şu anda, Zaman Serisi Görüşleri Azure Event Hubs ve Azure IoT Hubs desteği sağlamaktadır ve gelecekte daha fazla Olay Kaynağı eklenecektir.
* **Başvuru verileri**: Zaman Serisi Görüşleri kullanıcılara zaman serisi verilerini başvuru verileriyle birleştirme olanağı sağlar.  Başvuru verileri cihazlar hakkındaki meta verileri veya görece seyrek değişen diğer statik verileri içerebilir. Zaman Serisi Görüşleri başvuru verilerini veri akışlarıyla birleştirerek, kullanıcıların bu verileri neredeyse gerçek zamanlı olarak görselleştirmesine ve analiz etmesine olanak tanır.

