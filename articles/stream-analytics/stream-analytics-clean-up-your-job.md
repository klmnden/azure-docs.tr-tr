---
title: Azure Stream Analytics işinizi Temizle
description: Bu makalede, Azure Stream Analytics işleri silme için bir kılavuzdur.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 580d05909ff3c94c982be5353b3b5e86a78fc43f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969349"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Azure Stream Analytics işinizi Temizle

Azure Stream Analytics işleri, Azure portalı, Azure PowerShell, .net veya REST API'si için Azure SDK üzerinden kolayca silinebilir.

>[!NOTE] 
>Stream Analytics işinizi durdurduğunuzda, verileri olay hub'ları veya Azure SQL veritabanı gibi giriş ve çıkış depolama alanında yalnızca devam ettirir. Azure'dan verilerini kaldırmak için gerekliyse, Stream Analytics işinizin giriş ve çıkış kaynakları temizleme işlemini izleyin emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portalında bir işini durdurma

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Çalışan Stream Analytics işinizi bulun ve seçin.

3. Stream Analytics işi sayfasında **Durdur** işi durdurulamadı. 

   ![İşi Durdur](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portalında iş Sil

1. Azure Portal’da oturum açın. 

2. Var olan Stream Analytics işinizi bulun ve seçin.

3. Stream Analytics işi sayfasında **Sil** işi silmek için. 

   ![İşi Sil](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Durdurma veya PowerShell kullanarak iş Sil

PowerShell kullanarak bir işi durdurmak için kullanın [Stop-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0) cmdlet'i. PowerShell kullanarak işi silmek için kullanın [Remove-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0) cmdlet'i.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Durdurma veya .NET için Azure SDK'sını kullanarak iş Sil

.NET için Azure SDK'sını kullanarak bir işi durdurmak için kullanın [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) yöntemi. .NET için Azure SDK'sını kullanarak bir işi silmek için [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) yöntemi.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Durdurma veya REST API'yi kullanarak iş Sil

REST API kullanarak bir işi durdurmak için başvurmak [Durdur](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) yöntemi. REST API kullanarak işi silmek için başvurmak [Sil](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) yöntemi.