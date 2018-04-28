---
title: Azure akış analizi için girişleri anlama
description: Bu makale bir Azure Stream Analytics işinde başvuru veri girişi akış girişine karşılaştırma girişleri kavramı açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 926821e2ba9912ae0140f11c9fe9a2d504609a1e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Azure akış analizi için girişleri anlama

Azure Stream Analytics işleri için bir veya daha fazla veri girişleri bağlayın. Her giriş, varolan bir veri kaynağına bağlantı tanımlar. Akış analizi veri gelen olay kaynakları Event Hubs, IOT Hub ve Blob Depolama da dahil olmak üzere birkaç tür kabul eder. Girdiler, her iş için yazma akış SQL sorgusunda adıyla başvurulur. Sorgu, veri blend veya başvuru verileri için bir arama ile veri akışı karşılaştırmak için birden çok girişi birleştirme ve sonuçları çıktıları geçirin. 

Akış analizi kaynaklarını üç tür birinci sınıf tümleştirme girdi olarak sahiptir:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, aynı abonelikte Azure Stream Analytics işi olarak veya farklı bir abonelik dinamik.

Kullanabileceğiniz [Azure portal](stream-analytics-quick-create-portal.md#configure-input-to-the-job), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/New-AzureRmStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), ve [Visual Studio](stream-analytics-tools-for-visual-studio.md)oluşturmak, düzenlemek ve akış analizi işi girişleri test etmek için.

## <a name="stream-and-reference-inputs"></a>Akış ve başvuru girişleri
Verileri bir veri kaynağına itildiği gibi bu akış analizi işi tarafından tüketilen ve gerçek zamanlı olarak işlenen. Girişleri iki türlerine bölünen: veri akışı girişleri ve başvuru verileri girdi.

### <a name="data-stream-input"></a>Giriş veri akışı
Veri akışı bir sınırsız olayları zamanla dizisidir. Akış analizi işleri, en az bir veri akış girişi eklemeniz gerekir. Olay hub'ları, IOT Hub ve Blob Depolama veri akışı giriş kaynağı olarak desteklenir. Olay hub'ları, birden çok cihazları ve Hizmetleri olay akışları toplayacak şekilde kullanılır. Bu akışlar, sosyal medya Etkinlik Akışları, hisse senedi ticareti bilgi veya algılayıcı verileri içerebilir. IOT hub'ları, nesnelerin interneti (IOT) senaryolarında bağlı aygıtlardan verileri toplamak için getirilmiştir.  BLOB Depolama günlük dosyaları gibi bir akış olarak toplu veri alma için bir giriş kaynağı kullanılabilir.  

Veri girişleri akış hakkında daha fazla bilgi için bkz: [Stream Analytics giriş olarak veri akışı](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Başvuru veri girişi
Akış analizi, aynı zamanda giriş olarak bilinen destekler *başvuru verileri*. Başvuru verileri olan ya da tamamen statik veya yavaş değişir. Genellikle, bağıntı ve aramalar gerçekleştirmek için kullanılır. Örneğin, statik değerleri aramak için bir SQL birleştirme gerçekleştireceği kadar veri akış girişine başvuru verilerini de verileri birleştirebilirsiniz. Azure Blob Depolama şu anda yalnızca desteklenen giriş başvuru verileri kaynağıdır. Başvuru veri kaynağı BLOB'ları, 100 MB boyutunda sınırlıdır.

Başvuru veri girişleri hakkında daha fazla bilgi için bkz: [kullanarak başvuru verileri için Stream Analytics arama](stream-analytics-use-reference-data.md)

Bu makalede bir adımdır [Stream Analytics öğrenme yolu](/documentation/learning-paths/stream-analytics/).

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)