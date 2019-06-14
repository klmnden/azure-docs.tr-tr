---
title: Azure Data Lake Analytics kodunuzu test etme
description: Test çalışmaları için Azure Data Lake Analytics U-SQL ve genişletilmiş C# kodu için eklemeyi öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 4532e0c6e8095c9d64897410e0492e2135d8a478
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630108"
---
# <a name="test-your-azure-data-lake-analytics-code"></a>Azure Data Lake Analytics kodunuzu test etme

Azure Data Lake U-SQL dili, bildirim temelli SQL kesinlik temelli C# verileri dilediğiniz ölçekte işlemek için ile birleştiren sağlar. Bu belgede, U-SQL ve genişletilmiş C# UDO'su (kullanıcı tanımlı işleç) kodu için test çalışmaları oluşturma hakkında bilgi edinin.

## <a name="test-u-sql-scripts"></a>U-SQL betikleri test

U-SQL betiği derlenir ve yerel makinenizde veya bulutta makine üzerinde çalıştırmak yürütülebilir kodu için iyileştirilmiştir. Derleme ve en iyi duruma getirme işlemi tamamını U-SQL betiği bir bütün olarak değerlendirir. Geleneksel yapamayacağınız "birim testi" her deyim için. Ancak, SDK'sı U-SQL kullanarak test edin ve yerel çalıştırma SDK'si, bunu komut dosyası düzeyi testleri.

### <a name="create-test-cases-for-u-sql-script"></a>U-SQL betiği için test çalışmaları oluşturma

Visual Studio için Azure Data Lake araçları, U-SQL betiği test durumları oluşturmak sağlar.

1.  Çözüm Gezgini'nde bir U-SQL betiği sağ tıklayın ve ardından **birim testi oluşturma**.
2.  Yeni bir test projesi oluşturun veya var olan bir test projesine test çalışması ekleyin.

    ![--Visual Studio için Data Lake araçları, U-SQL test projesi oluşturma](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![--Visual Studio için Data Lake araçları, U-SQL test projesi yapılandırması oluşturma](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-the-test-data-source"></a>Test verisi kaynağı yönetme

U-SQL betikleri test ettiğinizde, giriş dosyaları test etmek. Test verilerini yapılandırarak yönetebileceğiniz **veri kaynağı Test** U-SQL de proje özellikleri. 

Çağırdığınızda `Initialize()` arabirimi SDK, yerel geçici verileri kök klasörü U-SQL test, test projesi çalışma dizini altında oluşturulur ve tüm dosyaları ve alt klasörleri (ve alt klasör altındaki dosyalar) test veri kaynak klasördeki kopyalanır U-SQL betiği test çalışmaları çalıştırmadan önce geçici bir yerel verileri kök klasörü. Test veri klasörü yolu noktalı virgül ile ayırarak, daha fazla test veri kaynak klasör ekleyebilirsiniz.

![Visual Studio için Data Lake araçları--proje test veri kaynağını yapılandırma](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-the-database-environment-for-testing"></a>Veritabanı ortam test için yönetme

U-SQL betiklerinizi kullanın ya da sorgu ile U-SQL veritabanı (örneğin, saklı yordamlar çağırırken) nesneleri, U-SQL test çalışmaları çalıştırmadan önce veritabanının ortamını başlatmanız gerekir. `Initialize()` U-SQL test SDK arabiriminde test projesinin çalışma dizinindeki yerel geçici veri kök klasörünün U-SQL projesi tarafından başvurulan tüm veritabanlarını dağıtmanıza yardımcı olur. 

Daha fazla bilgi edinin [bir U-SQL projesi için U-SQL veritabanı proje başvurularını yönetme](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Test sonuçlarını doğrulayın

`Run()` Arabirimi, iş yürütme sonucu döndürür. 0 başarılı ve başarısız 1 anlamına gelir. C# onay işlevleri, çıkışları doğrulamak için de kullanabilirsiniz. 

### <a name="run-test-cases-in-visual-studio"></a>Visual Studio'da Test çalışmaları

U-SQL betiği test projesi, bir C# birim testi çerçevesi üzerine oluşturulmuştur. Projeyi derledikten sonra tüm test çalışmalarını çalıştırabilirsiniz **Test Gezgini > çalma listesi**. Alternatif olarak, .cs dosyaya sağ tıklayın ve ardından **çalıştırmak testlerini**.

## <a name="test-c-udos"></a>C# Udo'lar test

### <a name="create-test-cases-for-c-udos"></a>C# Udo'lar için test çalışmaları oluşturma

Bir C# birim testi çerçevesi kullanarak C# Udo'lar (kullanıcı tanımlı işleçler) test etmek için kullanabilirsiniz. Karşılık gelen hazırlama gerek Udo'lar test ederken **IRowset** nesneler giriş olarak.

IRowset nesnesi oluşturmanın iki yolu vardır:

- IRowset oluşturmak için bir dosyadan veri yükleme:

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

- Veri toplama verilerden IRowset oluşturmak için kullanın:

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

UDO işlevleri çağırdıktan sonra C# onay işlevlerini kullanarak ve şema satır kümesi değeri doğrulama sonuçlarını doğrulayabilirsiniz. Örnek kod ile U-SQL C# UDO'su birim test örnek projesinde kullanabileceğiniz **Dosya > Yeni > Proje** Visual Studio'da.

### <a name="run-test-cases-in-visual-studio"></a>Visual Studio'da Test çalışmaları

Ancak test projesini derledikten sonra tüm test çalışmalarını çalıştırabilirsiniz **Test Gezgini > çalma listesi**, veya .cs dosyasını sağ tıklatın ve seçin **çalıştırmak testlerini**.

## <a name="run-test-cases-in-azure-devops"></a>Azure DevOps sınama çalışmalarını çalıştırma

Her ikisi de **U-SQL betiği test projeleri** ve **C# UDO'su test projeleri** C# birim testi projelerini devralır. [Visual Studio test görevi](https://docs.microsoft.com/azure/devops/pipelines/test/getting-started-with-continuous-testing?view=vsts) Azure DevOps bu test çalışmalarını çalıştırabilirsiniz. 

### <a name="run-u-sql-test-cases-in-azure-devops"></a>Azure DevOps U-SQL sınama çalışmalarını çalıştırma

U-SQL test için yüklediğiniz emin `CPPSDK` derleme makinesi ve sonra geçişi `CPPSDK` USqlScriptTestRunner yoluna (cppSdkFolderFullPath: \@"").

**CPPSDK nedir?**

CPPSDK Microsoft Visual C++ 14 ve Windows SDK'sı 10.0.10240.0 içeren bir pakettir. Bu gerekli olan ortamınızdır U-SQL çalışma zamanı tarafından. Azure Data Lake araçları Visual Studio yükleme klasörü altında bu paketi alabilirsiniz:

- Altında olan Visual Studio 2015 için `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Altında olan Visual Studio 2017 için `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**Azure DevOps yapı aracısı CPPSDK hazırlama**

Azure DevOps CPPSDK bağımlılığı hazırlamak için en yaygın yolu aşağıdaki gibidir:

1.  CPPSDK kitaplıkları içeren klasör zip.
2.  Kaynak Denetim sisteminize .zip dosyasını inceleyin. (.Zip dosyası, böylece bazı dosyalar ".gitignore" tarafından göz ardı olmayan CPPSDK klasörü altındaki tüm kitaplıkları iade sağlar.)   
3.  Derleme işlem hattı, .zip dosyasını çıkartın.
4.  Noktası `USqlScriptTestRunner` derleme makinesi üzerindeki sıkıştırması bu klasöre.

### <a name="run-c-udo-test-cases-in-azure-devops"></a>Azure DevOps C# UDO'su sınama çalışmalarını çalıştırma

C# UDO'su test için Udo'lar için gerekli olan aşağıdaki derlemelere başvuru emin olun. Bloblarda başvuruyorsa [Nuget paketini Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), kendi derleme işlem hattı, bir NuGet Restore görevi eklediğinizden emin olun.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics için CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
- [U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın](data-lake-analytics-data-lake-tools-develop-usql-database.md)

