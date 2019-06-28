---
title: Azure Stream Analytics işinizi Temizle
description: Bu makalede, Azure Stream Analytics işlerini silmek için farklı yöntemler gösterilmektedir.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 6/21/2019
ms.custom: seodec18
ms.openlocfilehash: cb81c73f7946a10bae0470a55dcf1c0d55c2b847
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330043"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Durdurma veya Azure Stream Analytics işinizi Sil

Azure Stream Analytics işleri kolayca durduruldu veya Azure portalı, Azure PowerShell, .net veya REST API'si için Azure SDK'sı aracılığıyla silindi. Bir Stream Analytics işi, silindikten sonra kurtarılamaz.

>[!NOTE] 
>Stream Analytics işinizi durdurduğunuzda, verileri olay hub'ları veya Azure SQL veritabanı gibi giriş ve çıkış depolama alanında yalnızca devam ettirir. Azure'dan verilerini kaldırmak için gerekliyse, Stream Analytics işinizin giriş ve çıkış kaynakları temizleme işlemini izleyin emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portalında bir işini durdurma

Bir işi durdurduğunuzda, deprovisionned kaynaklarıdır ve olayları işlemeyi durdurur. Bu projeyle ilgili ücretleri de durdurulur. Ancak, tüm yapılandırma tutulur ve daha sonra işi yeniden 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Çalışan Stream Analytics işinizi bulun ve seçin.

3. Stream Analytics işi sayfasında **Durdur** işi durdurulamadı. 

   ![Azure Stream Analytics işini durdurma](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portalında iş Sil

>[!WARNING] 
>Bir Stream Analytics işi, silindikten sonra kurtarılamaz.

1. Azure Portal’da oturum açın. 

2. Var olan Stream Analytics işinizi bulun ve seçin.

3. Stream Analytics işi sayfasında **Sil** işi silmek için. 

   ![Azure Stream Analytics işini sil](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Durdurma veya PowerShell kullanarak iş Sil

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell kullanarak bir işi durdurmak için kullanın [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet'i. PowerShell kullanarak işi silmek için kullanın [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet'i.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Durdurma veya .NET için Azure SDK'sını kullanarak iş Sil

.NET için Azure SDK'sını kullanarak bir işi durdurmak için kullanın [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) yöntemi. .NET için Azure SDK'sını kullanarak bir işi silmek için [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) yöntemi.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Durdurma veya REST API'yi kullanarak iş Sil

REST API kullanarak bir işi durdurmak için başvurmak [Durdur](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) yöntemi. REST API kullanarak işi silmek için başvurmak [Sil](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) yöntemi.
