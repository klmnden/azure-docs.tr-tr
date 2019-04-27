---
title: Azure IOT Central verilerinizi Power BI panosunda Görselleştirme | Microsoft Docs
description: Power BI çözümü için Azure IOT Central, IOT Central verilerini Görselleştirme ve çözümleme için kullanın.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/15/2019
ms.topic: conceptual
ms.openlocfilehash: 322be1e13662d92a3cb0a805a9ccaacd05928f7d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60886817"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Power BI panosunda, Azure IOT Central verilerini Görselleştirme ve çözümleme

*Bu konu, Yöneticiler için geçerlidir.*

![Power BI çözüm işlem hattı](media/howto-connect-powerbi/iot-continuous-data-export.png)

Power BI çözümü için Azure IOT Central, IOT cihazlarınızı performansını izlemek için güçlü bir Power BI panosu oluşturmak için kullanın. Power BI Panonuzda şunları yapabilirsiniz:
- Cihazlarınızı zamanla gönderdiğiniz veri miktarını izleme
- Telemetri, durumları ve etkinlikler arasında veri hacmi karşılaştırın
- Ölçümler çok sayıda raporlama cihazları belirleyin
- Cihaz ölçümleri'nin geçmiş eğilimleri gözlemleyin
- Çok sayıda önemli olaylar göndermek sorunlu cihazları belirleyin

Bu çözüm, Azure Blob Depolama hesabınızda veri alan bir işlem hattı ayarlar [verileri sürekli dışarı aktarma](howto-export-data.md). Bu veriler, Azure işlevleri, Azure Data Factory ve Azure SQL veritabanı için işlem ve veri dönüştürmek için aracılığıyla akar. Çıktı görselleştirilmiş ve çözümlenen bir Power BI raporundaki PBIX dosyası olarak indirebilirsiniz. Her bileşen, gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Bu nedenle tüm bu kaynakları Azure aboneliğinizde oluşturulur.

## <a name="get-the-power-bi-solution-for-azure-iot-centralhttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Alma [Power BI çözümü için Azure IOT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) Microsoft appsource'tan.

## <a name="prerequisites"></a>Önkoşullar
Çözümünü ayarlama aşağıdakileri gerektirir:
- Bir Azure aboneliğine erişim
- Verileriniz dışarı [verileri sürekli dışarı aktarma](howto-export-data.md) IOT Central uygulamanızdan. Ölçümler, cihazları ve cihaz şablonu akışları en iyi Power BI panosu için etkinleştirmeniz önerilir.
- Power BI Desktop (en son sürüm)
- Power BI Pro (panoyu başkalarıyla paylaşmak istiyorsanız)

## <a name="reports"></a>Reports

İki raporlar otomatik olarak oluşturulur. 

İlk rapor, cihazların raporladığı ölçümleri bir geçmiş görünümünü gösterir ve ölçümleri ve ölçümler için en yüksek sayıda göndermiş cihazlar farklı türde keser.

![Power BI rapor sayfası 1](media/howto-connect-powerbi/template-page1-hasdata.PNG)

İkinci raporun daha ayrıntılı olay türlerine geçiyor ve hataları ve Uyarıları bildirilen bir geçmiş görünümünü gösterir. Ayrıca, hangi cihazların, özellikle hata olayları ve uyarı olayları yanı sıra tüm en yüksek olay sayısı raporladığınız gösterir.

![Power BI rapor sayfası 2](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="architecture"></a>Mimari
Azure portalında oluşturulan tüm kaynakları erişilebilir. Her şeyi altında bir kaynak grubu olması gerekir.

![Kaynak grubu, Azure Portal görünümü](media/howto-connect-powerbi/azure-deployment.PNG)

Her bir kaynağın ayrıntılarını ve nasıl kullanıldıkları, aşağıda tanımlanmıştır.

### <a name="azure-functions"></a>Azure İşlevleri
Azure işlev uygulaması, her seferinde yeni bir dosya, BLOB depolamaya yazılır tetiklenen. İşlevler, alanların her ölçümleri, cihazları ve cihaz şablonları dosya içinden ayıklayın ve Azure Data Factory tarafından kullanılmak üzere birkaç Ara SQL tablolarının doldurur.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, bağlı hizmet olarak SQL veritabanına bağlanır. Bu verileri işlemek ve analiz tablolarda depolama saklı yordam etkinlikleri çalıştırır.

### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Bu tablolar varsayılan raporları doldurmak için otomatik olarak oluşturulur. Power bı'da bu şemaları keşfedin ve bu verileri kendi Görselleştirmelerini de oluşturabilirler.

| Tablo adı |
|------------|
|[analytics]. [Ölçümleri]|
|[analytics]. [İletileri]|
|[aşama]. [Ölçümleri]|
|[analytics]. [Özellikleri]|
|[analytics]. [PropertyDefinitions]|
|[analytics]. [MeasurementDefinitions]|
|[analytics]. [Cihazları]|
|[analytics]. [DeviceTemplates]|
|[dbo]. [date]|
|[dbo]. [Defteriniz]|

## <a name="estimated-costs"></a>Tahmini maliyetler

İlgili Azure maliyetleri (Azure işlevi, Data Factory, Azure SQL) tahmini aşağıdadır. Tüm fiyatlar ABD Doları cinsindendir. Her zaman en son fiyatları gerçek fiyatları almak için tek tek hizmetlerin görünmelidir böylece fiyatlar bölgeye göre değişir aklınızda bulundurun.
Aşağıdaki varsayılan değerler (şeyler olarak ayarladıktan sonra aşağıdakilerden herhangi birini değiştirebilirsiniz) şablonunda sizin için ayarlanır:

- Azure işlevleri: App Service planı S1, $74.40/ ay
- Azure SQL S1, ~$30/month

İle çeşitli fiyatlandırma seçenekleri hakkında bilgilenmeli ve ihtiyaçlarınıza uyacak şekilde şeyler ince ayar yapmanızı öneririz.

## <a name="resources"></a>Kaynaklar

AppSource almak için ziyaret [Power BI çözümü için Azure IOT Central](https://aka.ms/iotcentralpowerbisolutiontemplate).

## <a name="next-steps"></a>Sonraki adımlar

Verilerinizi Power bı'da görselleştirin gerçekleştirmeyi öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazları yönetme](howto-manage-devices.md)