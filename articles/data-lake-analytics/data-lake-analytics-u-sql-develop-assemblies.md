---
title: "Azure Data Lake Analytics işleri için U-SQL derlemeleri geliştirme | Microsoft Docs"
description: "Kullanılan ve Data Lake Analytics işleri yeniden derlemeleri geliştirmeyi öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Azure Data Lake Analytics işleri için U-SQL derlemeleri geliştirin
Arka plan kodu derlemeleri kullanılan ve Data Lake Analytics işleri yeniden dönüştürmek öğrenin. 

U-SQL C#, VB.Net veya F # gibi .net dillerinde kendi özel kod ekleme kolay hale getirir. Hatta diğer dilleri desteklemek için kendi çalışma zamanı dağıtabilirsiniz.

Özel kod kullanmak için en kolay yolu, Visual Studio'nun arka plan kodu özellikleri için Data Lake araçları kullanmaktır. Daha fazla bilgi için bkz: [öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md). Arka plan kodu kullanmanın birkaç dezavantajları vardır:

- Kaynak kodu her komut dosyası gönderi için karşıya.
- arka plan kodu diğer işlemlerle paylaşılamaz.

Bu dezavantajları çözmek için arka plan kodu derlemelerine kapatma ve Data Lake Analytics Kataloğu'na derlemelerini kaydedin.

## <a name="prerequisites"></a>Ön koşullar
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 4 veya Visual Studio 2012 Visual C++ yüklü güncelleştirme
* .NET sürüm 2.5 veya üzeri için Microsoft Azure SDK.  Web Platformu yükleyicisi ya da Visual Studio Yükleyicisi'ni kullanarak yükleme
* Bir Data Lake Analytics hesabı.  Bkz. [Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).
* Git üzerinden [Azure Data Lake Analytics U-SQL Studio ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) Öğreticisi.
* Azure'a bağlayın.
* Veri kaynağını karşıya yüklemek için bkz: [Azure Data Lake Analytics U-SQL Studio ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>U-SQL derlemelerde geliştirin

**Oluşturmak ve U-SQL işi göndermek için**

1. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın.
2. Genişletme **yüklü**, **şablonları**, **Azure Data Lake**, **U-SQL(ADLA)**seçin **sınıf kitaplığı (için U-SQL Uygulama)** şablonu ve ardından **Tamam**.
3. Kodunuzu Class1.cs içinde yazın.  Kod örneği verilmiştir.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Tıklatın **yapı** menüsüne ve ardından **yapı çözümü** dll oluşturmak için.

## <a name="register-assemblies"></a>Derlemeleri kaydetme

Bkz: [kullanım Data Lake Analytics(U-SQL) katalog](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-the-assemblies"></a>Derlemeleri kullanma

Bkz: [Azure Data Lake araçları Visual Studio kodunu kullanın](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Ayrıca bkz.
* [PowerShell kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Portalı'nı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanın](data-lake-analytics-data-lake-tools-get-started.md)
* [Kullanım Data Lake Analytics(U-SQL) Kataloğu](data-lake-analytics-use-u-sql-catalog.md)
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)