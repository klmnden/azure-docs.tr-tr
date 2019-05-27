---
title: Azure Stream Analytics işinizi Temizle
description: Bu makalede, Azure Stream Analytics işlerini silmek için farklı yöntemler gösterilmektedir.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: e43e1034abe4bbe3d31a46ab3b98b0efe612b852
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159440"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Azure Stream Analytics işinizi Temizle

Azure Stream Analytics işleri, Azure portalı, Azure PowerShell, .net veya REST API'si için Azure SDK üzerinden kolayca silinebilir. Bir Stream Analytics işi, silindikten sonra kurtarılamaz.

>[!NOTE] 
>Stream Analytics işinizi durdurduğunuzda, verileri olay hub'ları veya Azure SQL veritabanı gibi giriş ve çıkış depolama alanında yalnızca devam ettirir. Azure'dan verilerini kaldırmak için gerekliyse, Stream Analytics işinizin giriş ve çıkış kaynakları temizleme işlemini izleyin emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portalında bir işini durdurma

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Çalışan Stream Analytics işinizi bulun ve seçin.

3. Stream Analytics işi sayfasında **Durdur** işi durdurulamadı. 

   ![Azure Stream Analytics işini durdurma](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portalında iş Sil

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
