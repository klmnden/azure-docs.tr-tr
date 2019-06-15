---
title: Azure Data Lake Analytics kodu yerel olarak hata ayıklama
description: Yerel iş istasyonunuzda U-SQL işlerinde hata ayıklama için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 0827311218202de447e5cf27356e00c4da020e94
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61473000"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Azure Data Lake Analytics kodu yerel olarak hata ayıklama

Azure Data Lake Analytics hizmeti gibi çalıştırmak ve yerel iş istasyonunda, Azure Data Lake Analytics kodda hata ayıklamak için Visual Studio için Azure Data Lake Araçları'nı kullanabilirsiniz.

Bilgi edinmek için nasıl [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

Göndererek ve bunları Azure Data Lake Analytics hizmetine kaydetme C# derlemeleri üzerinde hata ayıklama yapabilirsiniz. Arka plan kod dosyasında ve başvurulan bir C# projesinde kesme noktaları ayarlayabilirsiniz.

### <a name="debug-local-code-in-a-code-behind-file"></a>Bir arka plan kod dosyasında yerel kod hatalarını ayıklama

1. Arka plan kod dosyasında kesme noktalarını ayarlayın.
2. Seçin **F5** betik üzerinde yerel olarak hata ayıklamak için.

> [!NOTE]
   > Aşağıdaki yordam yalnızca Visual Studio 2015'te çalışır. Eski Visual Studio sürümlerinde, el ile eklemeniz gerekebilir **PDB** dosyaları.  
   >
   >

### <a name="debug-local-code-in-a-referenced-c-project"></a>Başvurulan bir C# projesinde yerel kod hatalarını ayıklama

1. Bir C# derleme projesi oluşturun ve çıktı üretmek için derleyin **DLL** dosya.
2. Kayıt **DLL** bir U-SQL deyimini kullanarak dosya:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. C# kodunda kesme noktalarını ayarlayın.
4. Seçin **F5** C# başvurarak hataları ayıklamak için **DLL** dosyasını yerel.


## <a name="next-steps"></a>Sonraki adımlar

- Daha karmaşık bir sorgu örneği için bkz: [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- İş ayrıntılarını görüntülemek için bkz: [kullanımı iş tarayıcı ve Azure Data Lake Analytics işleri için iş görünümünü](data-lake-analytics-data-lake-tools-view-jobs.md).
- Köşe yürütme görünümünü kullanma hakkında bilgi için bkz: [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
