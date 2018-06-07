---
title: Azure Stream Analytics işiniz Temizle
description: Bu makalede, Azure akış analizi işi silmek nasıl bir kılavuzdur.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 4773f542f12ae1773e881106bc8948c663bfd1e3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661083"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Azure Stream Analytics işiniz Temizle

Azure akış analizi işleri, Azure portal, Azure PowerShell, .net veya REST API için Azure SDK aracılığıyla kolayca silinebilir.

>[!NOTE] 
>Stream Analytics işiniz durdurduğunuzda, verileri yalnızca giriş ve çıkış depolama biriminde bulunan, olay hub'ları veya Azure SQL veritabanı gibi devam eder. Azure'dan verileri kaldırmak için gerekliyse, akış analizi işinin giriş ve çıkış kaynaklar için kaldırma işlemi izleyin emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portalında işini durdurma

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Çalışan bir akış analizi işi bulun ve seçin.

3. Akış analizi işi sayfasında seçin **durdurmak** işi durdurulamıyor. 

   ![İşi Durdur](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portalında bir işi Sil

1. Azure Portal’da oturum açın. 

2. Var olan Stream Analytics işi bulun ve seçin.

3. Akış analizi işi sayfasında seçin **silmek** işini silmek için. 

   ![İşi Sil](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Durdurma veya PowerShell kullanarak işi Sil

PowerShell kullanarak bir işi durdurmak için kullanma [Stop-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0) cmdlet'i. PowerShell kullanarak işi silmek için kullanın [Kaldır AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0) cmdlet'i.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Durdurun ya da .NET için Azure SDK'sını kullanarak iş Sil

.NET için Azure SDK'yı kullanarak bir işi durdurmak için kullanma [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) yöntemi. .NET için Azure SDK'sını kullanarak işi silmek için [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) yöntemi.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Durdurma veya REST API'sini kullanarak iş Sil

REST API kullanarak bir işi durdurmak için başvurmak [durdurmak](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#stop) yöntemi. REST API kullanarak işi silmek için başvurmak [silmek](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#delete) yöntemi.