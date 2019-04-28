---
title: IOT Çözüm Hızlandırıcıları başvuru mimarisi - Azure | Microsoft Docs
description: Azure IOT Çözüm Hızlandırıcıları başvuru mimarisi hakkında bilgi edinin. Mevcut Çözüm Hızlandırıcıları bu başvuru mimarisinde yararlanın. Başvuru mimarisi, kendi özel IOT çözümlerinin oluştururken de kullanabilirsiniz.
author: dominicbetts
ms.author: dobett
ms.date: 12/04/2018
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: philmea
ms.openlocfilehash: ba5eb50dcf800c186124db348ac584ff6f55cebb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450315"
---
# <a name="introduction-to-the-azure-iot-reference-architecture"></a>Azure IOT başvuru mimarisi için giriş

Bu makalede tanıtılır [Azure IOT başvuru mimarisi](https://aka.ms/iotrefarchitecture) ve örnekleri nasıl verir [Azure IOT Çözüm Hızlandırıcıları](about-iot-accelerators.md) kendi önerilerini izleyin.

Açık kaynak [Uzaktan izleme](iot-accelerators-remote-monitoring-sample-walkthrough.md) ve [Connected Factory](iot-accelerators-connected-factory-sample-walkthrough.md) Çözüm Hızlandırıcıları başvuru mimarisi önerilerin çoğu izleyin. Çözüm Hızlandırıcıları, kendi IOT çözümünüzü için bir başlangıç noktası olarak veya learning araçları kullanabilirsiniz.

## <a name="overview"></a>Genel Bakış

Başvuru mimarisi, bir IOT çözüm teknoloji prensiplerinin, oluşumunu Azure IOT Hizmetleri ve cihazlar gibi öğeleri açıklar. Mimari Ayrıca uygulama teknolojileri hakkında öneriler sağlar.

Yüksek düzeyde, bir IOT çözümü, yapılan olarak görüntüleyebilirsiniz:

* Oluşturan ve ölçümler ve olaylar gibi telemetri gönderme şeyler. Uzaktan izleme çözüm hızlandırıcısının kamyon veya asansörler gibi telemetri gönderme şeyleri cihazlardır.
* Nesnelerinizden gelen gönderilen telemetriden oluşturulan bir kavrayış düzeyi. Uzaktan izleme çözüm hızlandırıcısının bir öngörü bir kural oluşturur. Örneğin, sıcaklık bir altyapısındaki bir eşiğe ulaştığında bir kural belirleyebilirsiniz.
* Eylemler, bir işletmeyi ya da işlem geliştirilmesine yardımcı olun ınsights bağlı. Uzaktan izleme çözüm Hızlandırıcısını bir e-posta eylem bir altyapıyla operatörün ilgili olası bir sorunu bildirebilir.

[Azure IOT başvuru mimarisi](https://aka.ms/iotrefarchitecture) living belge güncelleştirmeleri teknoloji geliştikçe olduğu.

## <a name="core-subsystems"></a>Çekirdek alt sistemler

Başvuru mimarisi Aşağıdaki diyagramda da görüldüğü temel alt sistemlerin tanımlar:

![Çekirdek alt sistemler](media/iot-accelerators-architecture-overview/CoreSubsystems.png)

Aşağıdaki bölümlerde, Uzaktan izleme çözüm Hızlandırıcısını bileşenleri için temel alt sistemlerin nasıl eşleştiği açıklanmaktadır.

### <a name="iot-devices"></a>IoT cihazları

Bir IOT çözümü, neredeyse her türlü cihaz ve bulut ağ geçidi arasında güvenli, verimli ve güçlü iletişimini etkinleştirmeniz gerekir. Karmaşık Fabrika üretim hatlarının yüzlerce bileşenleri ve sensörlerden basit sıcaklık algılayıcıları aralığının iş varlıkları cihazlardır.

Bir alan ağ geçidi veya edge cihazı, bir özel cihaz Gereci y: davranıp genel amaçlı yazılım mi

* Protokol dönüştürme işlemek için iletişim etkinleştiricisi.
* Yerel cihaz denetim sistemi ve cihaz telemetri işleme hub. Bir edge cihazının telemetri denetim cihazları yerel olarak, bulutla iletişim kurmadan işleyebilir. Bir edge cihazının de filtre uygulayabilirsiniz veya buluta telemetri miktarını azaltmak için toplam cihaz telemetrisi aktarılır.

Uzaktan izleme çözümünde, cihazlar IOT hub'a bağlanmak ve işleme için telemetri göndermek. Uzaktan izleme çözümü, işleri veya otomatik cihaz yönetimi kullanarak cihazları yönetme işleçleri de imkan tanır. Cihazlarınızı uygulamak için Azure IOT cihaz SDK'larını kullanabilirsiniz.

Uzaktan izleme çözümü dağıtabilir ve IOT Edge cihazları yönetebilirsiniz. Uç cihazlarında işleme Bulut ve cihaz olaylarını yanıtlarını hızlandırmak için gönderilen telemetri hacmini azaltmaya yardımcı olur.

### <a name="cloud-gateway"></a>Bulut ağ geçidi

Bir bulut ağ geçidi, cihazları ve uç cihazları iletişimi sağlar. Bu cihazlar, uzak sitelerde olabilir.

Ağ geçidi bağlantı yönetimi, koruması iletişim yolunun, kimlik doğrulama ve yetkilendirme dahil olmak üzere, cihaz iletişimi yönetir. Ağ geçidi ayrıca bağlantı ve aktarım hızı kotaları zorunlu kılar ve faturalama, tanılama ve izleme diğer görevler için kullanılan telemetri toplar.

Uzaktan izleme çözüm, telemetri gönderecek şekilde cihazlar için güvenli bir uç noktası sağlamak için IOT hub'ı dağıtır. IOT hub ayrıca:

* Çözüme bağlanmasına izin cihazları yönetmek için bir cihaz kimlik deposu içerir.
* Cihazlara komut gönderme için çözüm sağlar. Örneğin, bir Vana baskısı yayımlamayı açmak için.
* Cihaz Yönetimi toplu olarak etkinleştirir. Örneğin, bir dizi cihazda bellenimi yükseltmek için.

### <a name="stream-processing"></a>Akış işleme

Çözüm, telemetri alır gibi telemetri işleme akışını nasıl değişebilir anlamak önemlidir. Senaryoya bağlı olarak farklı aşamalarının veri kayıtlarının akış:

* Depolama, bellek içi önbellek, geçici sıralar ve kalıcı arşivler gibi.

* Analiz çalıştırmak için bir dizi koşula aracılığıyla telemetri girişi ve farklı bir çıkış veri kayıtları oluşturabilir. Örneğin, Avro kodlanmış giriş telemetri JSON kodlanmış çıkış telemetri döndürebilir.

* Özgün giriş telemetri ve analiz çıkış kayıtlar genellikle depolanan ve görüntü için kullanılabilir. Ayrıca telemetri giriş ve çıkış kayıtları e-postaları, olay tickets ve cihaz komutları gibi eylemleri tetikleyebilir.

Yönlendirme, bir veya daha fazla depolama uç noktaları, analiz işlemleri ve eylemleri için telemetri gönderme. Bir çözüm farklı siparişler aşamaları birleştirebilir ve eş zamanlı paralel görevleri tamamlandıkça işleme.

Uzaktan izleme çözüm [Azure Stream Analytics](/azure/stream-analytics/) akış işlemek için. Kural altyapısı çözümdeki, uyarıları ve eylemleri üretmek için Stream Analytics sorgularını kullanır. Örneğin, çözüm bir kamyon ait depolama bölme ortalama sıcaklık üzerinde beş dakika 36 derecenin düştüğünde tanımlamak için bir sorgu kullanabilirsiniz.

### <a name="storage"></a>Depolama

IOT çözümleri, çok miktarda zaman serisi verilerini çoğunlukla olduğu veri oluşturabilir. Bu veriler nerede bu Görselleştirme ve raporlama için kullanılabilir depolanmış olması gerekir. Bir çözüm de daha sonra analiz veya ek işleme için veri erişim gerekebilir. Sıcak ve soğuk veri depolarına bölme veri yaygındır. Sıcak veri deposu, düşük gecikme süreli erişim için en son verileri tutar. Soğuk veri deposunun genellikle geçmiş verileri depolar.

Uzaktan izleme çözüm [Azure Time Series Insights](/azure/time-series-insights/) , soğuk veri deposu olarak Cosmos DB ve normal bir veri deposu olarak.

### <a name="ui-and-reporting-tools"></a>Kullanıcı Arabirimi ve Raporlama araçları

Çözüm kullanıcı Arabirimi sağlar:

* Erişim ve cihaz veri ve analiz sonuçlarını görselleştirme.
* Cihaz kayıt defteri aracılığıyla bulma.
* Cihazların komuta ve denetimi.
* Cihaz sağlama iş akışları.
* Bildirimleri ve Uyarıları.
* Çok sayıda cihazı kaynaklanan verileri görüntülemek için Canlı, etkileşimli panolar ile tümleştirme.  
* Coğrafi konum ve coğrafi kullanan Hizmetleri.

Uzaktan izleme çözümünün bir web bu işlevselliği sunmak için kullanıcı Arabirimi içerir. Web kullanıcı arabirimini içerir:

* Cihazları konumunu göstermek için etkileşimli bir harita.
* Time Series Insights Gezgini, sorgu ve telemetriyi analiz etmek için erişim.

### <a name="business-integration"></a>İş tümleştirmesi

IOT çözüm CRM, ERP ve satır iş kolu uygulamaları gibi iş sistemlerle tümleştirme iş tümleştirmesi katmanı işler. Hizmet faturalama, müşteri desteği, örnekler ve yedek parça sağlayın.

Uzaktan izleme çözümü, bir koşul oluştuğunda e-postalar gönderme kurallarını kullanır. Örneğin, bir kamyon sıcaklık 36 derecenin düştüğünde çözüm operatörün bildirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure IOT başvuru mimarisi hakkında bilgi edindiniz ve nasıl Uzaktan izleme çözüm Hızlandırıcısını önerilerini, aşağıdaki bazı örnekler gördünüz. Okunacak sonraki adımdır [Microsoft Azure IOT başvuru mimarisi](https://aka.ms/iotrefarchitecture).