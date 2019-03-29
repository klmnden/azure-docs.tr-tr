---
title: Azure Application Insights işlem tanılamaları | Microsoft Docs
description: Application Insights uçtan uca işlem tanılamaları
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/19/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: c6c44525018e2115f1df8ed2d3f15432b95490c6
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619841"
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Birleşik bileşenler arası işlem tanılamaları

Birleşik tanılama koreluje s sunucu tarafı telemetri tek bir görünümde tüm Application Insights'ın izlenen bileşenler genelinde karşılaşırsınız. Ayrı bir izleme anahtarı ile birden çok kaynaklarınız varsa önemi yoktur. Application Insights temel ilişki algılar ve uygulama bileşeni, bağımlılık veya bir işlem yavaşlama veya hata özel durum kolayca tanılamanıza olanak tanır.

## <a name="what-is-a-component"></a>Bir bileşen nedir?

Bağımsız bir şekilde dağıtılabilen dağıtılmış mikro uygulamanızın parçalarını bileşenlerdir. Geliştiriciler ve işlem ekipleri bu uygulama bileşenleri tarafından oluşturulan telemetri erişimi veya kod düzeyinde görünürlüğe sahip.

* Bileşenleri "gözlemlenen" dış bağımlılıkları SQL gibi farklı takım/kuruluşunuz olmayabilir EventHub vb. erişim (kod veya telemetri).
* Bileşenleri rol/sunucu/kapsayıcı örnekleri herhangi bir sayıda çalışır.
* Bileşenleri ayrı bir Application Insights izleme anahtarı (abonelikler farklı olsa bile) olması ya da farklı roller için tek bir Application Insights izleme anahtarı raporlama gerçekleştirebilirsiniz. Yeni deneyimi nasıl bunlar ayarlanan bakılmaksızın tüm bileşenler genelinde ayrıntıları gösterir.

> [!NOTE]
> * **İlgili öğe bağlantıları eksik?** Tüm ilişkili telemetri bulunan [üst](#cross-component-transaction-chart) ve [alt](#all-telemetry-with-this-operation-id) sol tarafındaki bölümlerini. 

## <a name="transaction-diagnostics-experience"></a>İşlem tanılama deneyimi
Bu görünüm dört bölümden oluşur: sonuçları listesi, bileşenler arası işlem grafik, bu işlem ve Ayrıntılar bölmesinde soldaki herhangi bir telemetri seçili öğe için ilgili tüm telemetri zaman sıralı listesi.

![Anahtar bölümleri](media/transaction-diagnostics/4partsCrossComponent.png)

## <a name="cross-component-transaction-chart"></a>Bileşenler arası işlem grafiği

Bu grafiği yatay bir çubuk olan bir zaman çizelgesi istekleri ve bağımlılıkları süresince bileşenlerinde sağlar. Toplanan herhangi bir özel durum da zaman çizelgesinde işaretlenir.

* Bu grafik en üst satıra giriş noktasını temsil eder. Birinci Bileşen gelen istek bu işlem sırasında çağrılır. İşlemin tamamlanması için geçen toplam süre süresidir.
* Dış bağımlılıklara yapılan çağrıları bağımlılık türünü temsil eden bir simge ile basit daraltılabilir olmayan satırlar var.
* Diğer bileşenleri çağrıları daraltılabilir satırları aynıdır. Her satır, bileşen çağrılan belirli bir işlemi karşılık gelir.
* Varsayılan, isteği, bağımlılık veya özel durum, seçtiğiniz sağ tarafta görüntülenir.
* Görmek için herhangi bir satırı seçin, [ayrıntıları sağda](#details-of-the-selected-telemetry). 

> [!NOTE]
> Diğer bileşenleri çağrısına sahip iki satır: bir satır arayana bileşenin giden çağrısından (bağımlılık) temsil eder ve iki satırdan adlı bileşenin gelen isteğiyle karşılık gelir. Önde gelen simge ve süresi çubukları ayrı stilini bunları ayırt yardımcı olur.

## <a name="all-telemetry-with-this-operation-id"></a>Bu işlem kimliğine sahip tüm telemetri

Bu bölümde, bu işlem için ilgili telemetri zaman dizisindeki düz liste görünümü gösterilir. Ayrıca özel olaylar ve işlem grafiği görüntülenmiyor izlemeleri gösterir. Belirli bir bileşeni/çağrısı tarafından oluşturulan telemetri için bu listeyi filtreleyebilirsiniz. Karşılık gelen görmek için bu listede tüm telemetri öğesinin seçebileceğiniz [ayrıntıları sağda](#details-of-the-selected-telemetry).

![Zaman serisi tüm telemetri](media/transaction-diagnostics/allTelemetryDrawerOpened.png)

## <a name="details-of-the-selected-telemetry"></a>Seçili telemetri ayrıntıları

Daraltılabilir bu bölme, işlem grafik veya listesinden herhangi bir seçili öğe ayrıntılarını gösterir. "Show all" tüm toplanan standart özniteliklerini listeler. Herhangi bir özel özniteliği ayrı olarak standart kümesi listelenmiştir. "..." Yığın İzleme penceresini izleme kopyalamak için bir seçenek aşağıda üzerinde tıklayın. "Açık profiler izlemeleri" veya "Aç hata ayıklama anlık görüntüsü" kod düzeyi tanılama ilgili ayrıntı bölmelerinde gösterir.

![Özel durum ayrıntısı](media/transaction-diagnostics/exceptiondetail.png)

## <a name="search-results"></a>Arama sonuçları

Daraltılabilir bu bölme filtre ölçütlerini karşılayan diğer sonuçları gösterilmektedir. Herhangi bir sonuç ilgili ayrıntılarını yukarıda listelenen 3 bölümden güncelleştirmek için tıklayın. Örnekleme herhangi biri etkin olsa bile, tüm bileşenlerden kullanılabilir ayrıntılarını sağlamak büyük olasılıkla örnekleri bulmak deneyin. Bunlar, "önerilen" örnek olarak gösterilir.

![Arama sonuçları](media/transaction-diagnostics/searchResults.png)

## <a name="profiler-and-snapshot-debugger"></a>Profiler ve anlık görüntü hata ayıklayıcısı

[Application Insights profiler](../../azure-monitor/app/profiler.md) veya [anlık görüntü hata ayıklayıcısı](snapshot-debugger.md) Yardım ile kod düzeyi tanılama performans ve başarısızlık sorunların. Bu deneyim ile anlık görüntülerden herhangi bir bileşeni tek bir'a tıklayın veya profil oluşturucu izlemeleri görebilirsiniz.

Profiler çalışmaya alınamadı, lütfen başvurun **serviceprofilerhelp\@microsoft.com**

Snapshot Debugger çalışma alınamadı, lütfen başvurun **snapshothelp\@microsoft.com**

![Profiler tümleştirme](media/transaction-diagnostics/profilerTraces.png)

## <a name="faq"></a>SSS

*Grafik üzerinde tek bir bileşen görüyorum ve diğerleri yalnızca dış bağımlılıkları olmadan, bu bileşenler içerisinden ne ayrıntılı olarak gösterildiğini.*

Olası nedenler:

* Diğer bileşenlerle, Application Insights ile izlenen?
* Bunlar, en son kararlı Application Insights SDK'sını kullanıyor musunuz?
* Bu bileşenler ayrı bir Application Insights kaynaklarını ise, telemetri için gerekli erişim gerekiyor?

Erişiminiz ve bileşenlerinin en son Application Insights SDK'ları ile işaretlenmiş, en sağdaki geri bildirim kanalı aracılığıyla bize bildirin.

*Bağımlılıklar için yinelenen satırları görüyorum. Bu beklenen bir durumdur?*

Şu anda, biz yerine gelen istek ayrı giden bağımlılık çağrısının gösteriliyor. Genellikle, iki çağrıları yalnızca gidiş dönüş ağ nedeniyle farklı olan süre değeri ile aynı görünür. Önde gelen simge ve süresi çubukları ayrı stilini bunları ayırt yardımcı olur. Bu verilerini sunumu karmaşıktır? Görüşlerinizi bize bildirin!

*Peki saat farklı bileşen örneklerinde eğriltir?*

Zaman çizelgelerini saat farklarından işlem grafikteki için ayarlanır. Ayrıntılar bölmesinde veya Analiz kullanarak tam zaman damgaları görebilirsiniz.

*Yeni deneyimi ilgili öğeleri sorguların çoğu neden eksik?*

Bu tasarım gereğidir. İlgili olan tüm öğeler, tüm bileşenleri genelindeki zaten sol tarafındaki (üst ve alt bölümleri) kullanılabilir. Sol tarafta kapsamıyordur ilgili öğeleri iki yeni bir deneyimi vardır: beş dakikadan önce ve sonra bu olay ve kullanıcı zaman çizelgesini tüm telemetri.
