---
title: Azure Data Lake Analytics kodunuzu test etme | Microsoft Docs
description: Test çalışmaları için Azure Data Lake Analytics U-SQL ve genişletilmiş C# kodu için eklemeyi öğrenin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: bfb314348caf1d5bf83c940c0bce79e87d6d2593
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889673"
---
# <a name="how-to-test-your-azure-data-lake-analytics-code"></a>Azure Data Lake Analytics kodunuzu test etme

Azure Data Lake U-SQL sorgu dili gibi kesinlik temelli C# verileri dilediğiniz ölçekte işlemek için ile birleştiren bir dil olarak sağlar. Bu belgede, U-SQL ve genişletilmiş C# UDO'su (kullanıcı tanımlı işleç) kodu için test çalışmaları oluşturma konusunda bilgi edinin.

## <a name="test-u-sql-scripts"></a>U-SQL betikleri test

U-SQL betiği derlenmiş ve yürütülebilir kod için en iyi duruma getirilmiş ve Bulut veya yerel makinenize makineler arasında çalıştırın. Derleme ve en iyi duruma getirme işlemi tamamını U-SQL betiği yürütmek için bir bütün olarak değerlendirir. Geleneksel yapamayacağınız "birim testi" her deyim için. Ancak, U-SQL test SDK ve yerel çalıştırma SDK'si kullanarak bunu yapabilirsiniz betik seviyeli testler.

### <a name="create-test-cases-for-u-sql-script"></a>U-SQL betiği için test çalışmaları oluşturma

Visual Studio için Azure Data Lake araçları, U-SQL betiği test çalışmaları oluşturmanıza yardımcı olması için iyi bir deneyim sağlar.

1.  Çözüm Gezgini'nde bir U-SQL betiği sağ tıklatın ve seçin **birim testi oluşturma**.
2.  Yeni bir test projesi oluşturun veya var olan bir test projesi için test çalışması eklemek için yapılandırın.

    ![Visual Studio için Data Lake araçları, u-sql test projesi oluşturma](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![Visual Studio için Data Lake araçları, u-sql test projesi yapılandırması oluşturma](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-test-data-source"></a>Test verisi kaynağı yönetme

U-SQL betikleri, test, test giriş dosyası gereklidir. Bunlar yönetebileceğiniz test verileri yapılandırarak **veri kaynağı Test** U-SQL projesi özellik. Çağırdığınızda `Initialize()` arabirimi U-SQL SDK'sı, geçici bir yerel veri kök klasöre testinde, test projesinin çalışma dizini altında oluşturulur ve tüm dosyaları ve alt klasörleri (ve alt klasör altındaki dosyalar) test veri kaynak klasördeki geçici kopyalanır U-SQL betiği test çalışmaları çalıştırmadan önce yerel verileri kök klasörü. Test verileri klasörü yolu noktalı virgül ile kopyalamıştır tarafından daha fazla test veri kaynak klasör ekleyebilirsiniz.

![Visual Studio için Data Lake araçları proje test veri kaynağını yapılandırma](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-database-environment-for-test"></a>Veritabanı ortam test için yönetme

U-SQL betiklerinizi kullanın veya saklı yordam çağırma ile U-SQL veritabanı nesneleri, örneğin, sorgu, U-SQL test çalışmaları çalıştırmadan önce veritabanının ortamını başlatmanız gerekir. `Initialize()` U-SQL test SDK arabiriminde test projesinin çalışma dizinindeki yerel geçici veri kök klasörünün U-SQL projesi tarafından başvurulan tüm veritabanlarını dağıtmanıza yardımcı olur. 

Daha fazla bilgi edinin [U-SQL veritabanını nasıl yönetebileceğinizi projeleri için U-SQL projesi başvuruları](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Test sonuçlarını doğrulayın

`Run()` 1 anlamına gelir başarısız arabirimi, iş yürütme sonucu döndürür ve 0, başarılı anlamına gelir. C# onay işlevleri, çıkışları doğrulamak için de kullanabilirsiniz. 

### <a name="execute-test-cases-in-visual-studio"></a>Visual Studio'da Test çalışmalarını yürütürken

U-SQL betiği test projesi, C# birim testi çerçevesi üzerine oluşturulmuştur. Yapıdan sonra proje, tüm test çalışmalarını çalıştırabilirsiniz **Test Gezgini > çalma listesi**, veya .cs dosyasını sağ tıklatın ve seçin **çalıştırmak testlerini**.

## <a name="test-c-udos"></a>C# Udo'lar test

### <a name="create-test-cases-for-c-udos"></a>C# Udo'lar için test çalışmaları oluşturma

C# UDOs(User-Defined Operator) test etmek için C# birim testi çerçevesini kullanabilirsiniz. Karşılık gelen hazırlama gerek Udo'lar test ederken **IRowset** girdi olarak nesnesi.

IRowset oluşturmanın iki yolu vardır:

1.  IRowset oluşturmak için bir dosyadan veri yükleme

    ```csharp
    //Schema: "a:int, b:int"
    USqlColumn<int> col1 = new USqlColumn<int>("a");
    USqlColumn<int> col2 = new USqlColumn<int>("b");
    List<IColumn> columns = new List<IColumn> { col1, col2 };
    USqlSchema schema = new USqlSchema(columns);

    //Generate one row with default values
    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Get data from file
    IRowset rowset = UnitTestHelper.GetRowsetFromFile(@"processor.txt", schema, output.AsReadOnly(), discardAdditionalColumns: true, rowDelimiter: null, columnSeparator: '\t');
    ```

2.  Veri toplama IRowset oluşturmak için verileri kullan

    ```csharp
    //Schema: "a:int, b:int"
    USqlSchema schema = new USqlSchema(
        new USqlColumn<int>("a"),
        new USqlColumn<int>("b")
    );

    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Generate Rowset with specified values
    List<object[]> values = new List<object[]>{
        new object[2] { 2, 3 },
        new object[2] { 10, 20 }
    };

    IEnumerable<IRow> rows = UnitTestHelper.CreateRowsFromValues(schema, values);
    IRowset rowset = UnitTestHelper.GetRowsetFromCollection(rows, output.AsReadOnly());
    ```

### <a name="verify-test-results"></a>Test sonuçlarını doğrulayın

UDO işlevleri çağrıldıktan sonra şema ve onay C# işlevlerini kullanma satır değeri doğrulama sonucu doğrulayabilirsiniz. Örnek kod, U-SQL C# projesinde UDO birim testi örneği olabilir **Dosya > Yeni > Proje** Visual Studio'da.

### <a name="execute-test-cases-in-visual-studio"></a>Visual Studio'da Test çalışmalarını yürütürken

Derleme sonrası testin projesinin tüm test çalışmalarını ancak çalıştırabileceğiniz **Test Gezgini > çalma listesi**, veya .cs dosyasını sağ tıklatın ve seçin **çalıştırmak testlerini**.

## <a name="run-test-cases-in-visual-studio-team-service"></a>Visual Studio Team Service sınama çalışmalarını çalıştırma

Her ikisi de **U-SQL betiği test projesi** ve **C# UDO'su test projesi** C# birim testi projesi devralır. [Visual Studio Test görevi](https://docs.microsoft.com/vsts/pipelines/test/getting-started-with-continuous-testing?view=vsts) Visual Studio Team Service bu test çalışmalarını çalıştırabilirsiniz. 

### <a name="run-u-sql-test-cases-in-visual-studio-team-service"></a>Visual Studio Team Service U-SQL sınama çalışmalarını çalıştırma

U-SQL test için yüklediğiniz emin `CPPSDK` derleme makinesi ve pass `CPPSDK` USqlScriptTestRunner yoluna (cppSdkFolderFullPath: @"").

**CPPSDK nedir?**

Microsoft Visual C++ 14 ve U-SQL çalışma zamanı tarafından gerekli ortam Windows SDK 10.0.10240.0, bir paketi içerir CPPSDK olur. Bu paketin Azure Data Lake Araçları altında Visual Studio yükleme klasörü alabilirsiniz:

- Altında olan Visual Studio 2015 için `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Altında olan Visual Studio 2017 için `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**Visual Studio Team Service yapı aracısı CPPSDK hazırlamayı öğrenin**

Visual Studio Team Service bu CPPSDK bağımlılıkta hazırlama ortak yoludur:

1.  Zip klasörünü CPPSDK kitaplıkları içerir.
2.  Kaynak denetimi sisteminizden zip dosyasını inceleyin. (Zip dosyası CPPSDK klasörü altındaki tüm kitaplıkları iade ya da bazı dosyalar ".gitignore" tarafından göz ardı edilir emin yapabilirsiniz.)
3.  Derleme işlem hattı zip dosyasında sıkıştırmasını açın.
4.  Noktası `USqlScriptTestRunner` derleme makinesi üzerindeki sıkıştırması bu klasöre.

### <a name="run-c-udo-test-cases-in-visual-studio-team-service"></a>Visual Studio Team Service C# UDO'su sınama çalışmalarını çalıştırma

C# UDO'su test için aşağıda Udo'lar için gerekli bütünleştirilmiş kodları başvuru emin olun. Bloblarda başvuruyorsa [Nuget paketini Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), kendi derleme işlem hattı, bir NuGet Restore görevi eklediğinizden emin olun.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Data Lake Analytics için CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
- [U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın](data-lake-analytics-data-lake-tools-develop-usql-database.md)

