---
title: Azure Data Lake Analytics kodu yerel olarak hata ayıklama | Microsoft Docs
description: Yerel iş istasyonunuzda U-SQL işlerinde hata ayıklama için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: 181512c12c1e72e6aa8205aabd5ea22816d4c5df
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889678"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Azure Data Lake Analytics kodu yerel olarak hata ayıklama

Azure Data Lake hizmetinde gibi çalıştırın ve Azure Data Lake Analytics kodunuzu, iş istasyonunda, hata ayıklamak için Visual Studio için Azure Data Lake Araçları'nı kullanabilirsiniz.

Bilgi [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

Göndererek ve Azure Data Lake Analytics hizmetine kaydetme C# derlemeleri üzerinde hata ayıklama yapabilirsiniz. Hem dosyanın arkasındaki kodda hem de başvuruda bulunulan bir C# projesinde kesme noktaları ayarlayabilirsiniz.

### <a name="to-debug-local-code-in-code-behind-file"></a>Dosyanın arkasındaki kodda yerel kod hatalarını ayıklamak için

1. Dosyanın arkasındaki kodda kesme noktaları ayarlayın.
2. Betik üzerinde yerel olarak hata ayıklamak için F5 tuşuna basın.

> [!NOTE]
   > Aşağıdaki yordam yalnızca Visual Studio 2015'te çalışır. Daha eski Visual Studio sürümlerinde, pdb dosyalarını kendiniz eklemeniz gerekebilir.  
   >
   >

### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Başvuruda bulunulan bir C# projesinde yerel kod hatalarını ayıklamak için

1. Bir C# Derleme projesi oluşturun ve projeyi çıktı dll'sini üretmek üzere oluşturun.
2. U-SQL deyimi kullanarak dll'yi kaydetme:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. C# kodunda kesme noktalarını ayarlayın.
4. C# DLL'sine yerel olarak başvuran ile hataları ayıklamak için F5 tuşuna basın.


## <a name="next-steps"></a>Sonraki adımlar

- Daha karmaşık bir sorgu görmek için bkz: [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- İş ayrıntılarını görüntülemek için bkz: [kullanımı iş tarayıcı ve Azure Data Lake Analytics işleri için iş görünümünü](data-lake-analytics-data-lake-tools-view-jobs.md).
- Köşe yürütme görünümünü kullanma hakkında bilgi için bkz: [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
