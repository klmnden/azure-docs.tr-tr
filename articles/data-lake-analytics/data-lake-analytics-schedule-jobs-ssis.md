---
title: SSIS kullanarak Azure Data Lake Analytics U-SQL işleri
description: U-SQL işleri zamanlamak için SQL Server Integration Services'ı kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/17/2018
ms.openlocfilehash: 6894486118f69e682353142be04821e1d28440e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814667"
---
# <a name="schedule-u-sql-jobs-using-sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS) kullanarak U-SQL işleri

Bu belgede, düzenlemek ve SQL Server Integration Service (SSIS) kullanarak U-SQL işleri oluşturma konusunda bilgi edinin. 

## <a name="prerequisites"></a>Önkoşullar

[Tümleştirme hizmetleri için Azure Feature Pack](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud) sağlar [Azure Data Lake Analytics görev](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017) ve [Azure Data Lake Analytics Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017) yardımcı Azure Data Lake'e bağlanma Analiz Hizmeti. Bu görev kullanacak şekilde yüklediğinizden emin olun:

- [Visual Studio için SQL Server veri Araçları (SSDT) yükleme ve indirme](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017)
- [Integration Services (SSIS) için Azure Feature Pack yükleme](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017)

## <a name="azure-data-lake-analytics-task"></a>Azure Data Lake Analytics görevi

Azure Data Lake Analytics görev, kullanıcıların Azure Data Lake Analytics hesabı için U-SQL işlerini gönderme sağlar. 

[Azure Data Lake Analytics görev yapılandırma konusunda bilgi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017).

![Azure Data Lake Analytics SSIS görevde](./media/data-lake-analytics-schedule-jobs-ssis/data-lake-analytics-azure-data-lake-analytics-task-in-ssis.png)

SSIS yerleşik işlevleri kullanarak U-SQL betiği farklı yerden alabilirsiniz ve farklı kullanıcı durumları için U-SQL betiklerini nasıl yapılandırabileceğinizi senaryoları aşağıdaki görevleri gösterir.

## <a name="scenario-1-use-inline-script-call-tvfs-and-stored-procs"></a>Senaryo 1-kullanımı satır içi betik çağrısı tvf ve saklı yakalar

Azure Data Lake Analytics görev Düzenleyicisi'nde, yapılandırma **SourceType** olarak **DirectInput**, U-SQL deyimleri içine yerleştirin **USQLStatement**.

Kolay bakım ve yalnızca kısa bir U-SQL betiği satır içi komut dosyaları, put, kod yönetimi için örneğin, var olan tablo değerli işlevler ve saklı yordamlar, U-SQL veritabanlarınızda çağırabilirsiniz. 

![SSIS görevde satır içi U-SQL betiğini Düzenle](./media/data-lake-analytics-schedule-jobs-ssis/edit-inline-usql-script-in-ssis.png)

İlgili makale: [Parametresine geçirdiğiniz nasıl saklı yordamları](#scenario-6-pass-parameters-to-u-sql-script)

## <a name="scenario-2-use-u-sql-files-in-azure-data-lake-store"></a>Senaryo 2 kullanımı U-SQL dosyaları Azure Data Lake Store

Kullanarak Azure Data Lake Store U-SQL dosyaları kullanabilirsiniz **Azure Data Lake Store dosya sistemi görevi** Azure Feature Pack. Bu yaklaşım, bulut üzerinde depolanan betikleri kullanmanıza olanak sağlar.

Azure Data Lake Store dosya sistemi görevi ve Azure Data Lake Analytics görev arasındaki bağlantıyı ayarlamak için aşağıdaki adımları izleyin.

### <a name="set-task-control-flow"></a>Kümesi görev denetim akışı

SSIS paketi Tasarım görünümünde bir **Azure Data Lake Store dosya sistemi görevi**, **Foreach döngüsü kapsayıcı** ve bir **Azure Data Lake Analytics görev** Foreach döngüsü içinde Kapsayıcı. ADLS hesabınızdaki U-SQL dosyaları geçici bir dizine indirmek için Azure Data Lake Store dosya sistemi görevi yardımcı olur. Azure Data Lake Analytics hesabı bir U-SQL işi olarak geçici klasörü altında her bir U-SQL dosya göndermek için Foreach döngüsü kapsayıcısını ve Azure Data Lake Analytics görev yardımcı olur.

![Azure Data Lake Store U-SQL dosyalarını kullanma](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-data-lake-store.png)

### <a name="configure-azure-data-lake-store-file-system-task"></a>Azure Data Lake Store dosya sistemi görevi yapılandırma

1. Ayarlama **işlemi** için **CopyFromADLS**.
2. Ayarlanan **AzureDataLakeConnection**, daha fazla bilgi edinin [Azure Data Lake Store bağlantısı Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017).
3. Ayarlama **AzureDataLakeDirectory**. U-SQL betiklerinizi depolama klasöre gelin. Azure Data Lake Store hesabının kök klasörüyle ilgili göreli yol kullanın.
4. Ayarlama **hedef** indirilen U-SQL betikleri önbelleğe alan bir klasöre. Bu klasör yolu için U-SQL işi gönderme Foreach döngüsü kapsayıcısında kullanılır. 

![Azure Data Lake Store dosya sistemi görevi yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-azure-data-lake-store-file-system-task.png)

[Azure Data Lake Store dosya sistemi görevi hakkında daha fazla bilgi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-store-file-system-task?view=sql-server-2017).

### <a name="configure-foreach-loop-container"></a>Foreach döngüsü kapsayıcısını yapılandırma

1. İçinde **koleksiyon** sayfasında **Numaralandırıcı** için **Foreach dosya Numaralandırıcı**.

2. Ayarlayın **klasör** altında **Numaralandırıcı yapılandırma** grup geçici klasörüne indirilen U-SQL betikleri içerir.

3. Ayarlama **dosyaları** altında **Numaralandırıcı yapılandırma** için `*.usql` döngü kapsayıcı ile biten dosyaları yalnızca yakalar, böylece `.usql`.

    ![Foreach döngüsü kapsayıcısını yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-collection.png)

4. İçinde **değişken eşlemeleri** sayfasında, her bir U-SQL dosyası için dosya adını almak için bir kullanıcı tanımlı değişken ekleyin. Ayarlama **dizin** dosya adını almak için 0. Bu örnekte, adında bir değişken tanımlayın `User::FileName`. Bu değişken, dinamik olarak U-SQL betik dosyası bağlantısını alın ve Azure Data Lake Analytics görevde U-SQL işi adı ayarlamak için kullanılır.

    ![Dosya adını almak için foreach döngüsü kapsayıcısını yapılandırın](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-variable-mapping.png)

### <a name="configure-azure-data-lake-analytics-task"></a>Azure Data Lake Analytics görevi yapılandırma 

1. Ayarlama **SourceType** için **FileConnection**.

2. Ayarlama **FileConnection** dosyasına dosya nesnelere işaret eden bağlantı Foreach döngüsü kapsayıcısından döndürdü.
    
    Bu dosya bağlantısı oluşturmak için:

   1. Seçin  **\<yeni bağlantı... >** FileConnection ayarı.
   2. Ayarlayın **kullanım türü** için **var olan dosya**ve **dosya** varolan bir dosyanın dosya yolu.

       ![Foreach döngüsü kapsayıcısını yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

   3. İçinde **bağlantı yöneticileri** görüntüleme, az önce oluşturulan dosya bağlantıyı sağ tıklatın ve seçin **özellikleri**.

   4. İçinde **özellikleri** penceresini genişletin **ifadeleri**ve **ConnectionString** Foreach döngüsü kapsayıcısında, örneğin, tanımlanan değişkenine `@[User::FileName]`.

       ![Foreach döngüsü kapsayıcısını yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-property-for-foreach-loop-container.png)

3. Ayarlama **AzureDataLakeAnalyticsConnection** göndermek istediğiniz Azure Data Lake Analytics hesabı. Daha fazla bilgi edinin [Azure Data Lake Analytics Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017).

4. Diğer iş yapılandırmaları ayarlayın. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017).

5. Kullanım **ifadeleri** U-SQL işi adı dinamik olarak ayarlamak için:

    1. İçinde **ifadeleri** sayfasında, yeni bir ifade anahtar-değer çifti eklemek için **JobName**.
    2. Değeri için JobName Foreach döngüsü kapsayıcısında, örneğin, tanımlı bir değişkene ayarlayın `@[User::FileName]`.
    
        ![SSIS ifade için U-SQL işi adı yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-expression-for-u-sql-job-name.png)

## <a name="scenario-3-use-u-sql-files-in-azure-blob-storage"></a>Senaryo 3 kullanımı U-SQL dosyaları Azure Blob Depolama alanında

Kullanarak U-SQL dosyaları Azure Blob Depolama alanında kullanabilirsiniz **Azure Blob indirme görev** Azure Feature Pack. Bu yaklaşım, bulutta betiklerini kullanarak sağlar.

İle benzer adımlarla [Senaryo 2: Azure Data Lake Store U-SQL dosyaları kullanma](#scenario-2-use-u-sql-files-in-azure-data-lake-store). Azure Blob indirme görev için Azure Data Lake Store dosya sistemi görevini değiştirin. [Azure Blob indirme görev hakkında daha fazla bilgi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task?view=sql-server-2017).

Denetim akışı aşağıdaki gibidir.

![Azure Data Lake Store U-SQL dosyalarını kullanma](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-blob-storage.png)

## <a name="scenario-4-use-u-sql-files-on-the-local-machine"></a>Senaryo 4 kullanımı U-SQL dosyaları yerel makinede

Yanında bulut üzerinde depolanan U-SQL dosyalarını kullanarak yerel makinenizde veya SSIS paketlerinizi dağıtılan dosyaları kullanabilirsiniz.

1. Sağ **bağlantı yöneticileri** SSIS projesini ve ardından **Yeni Bağlantı Yöneticisi**.

2. Seçin **dosya** yazın ve tıklayın **Ekle...** .

3. Ayarlama **kullanım türü** için **var olan dosya**, ayarlayıp **dosya** yerel makine üzerindeki dosyaya.

    ![Yerel dosya dosya Bağlantısı Ekle](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

4. Ekleme **Azure Data Lake Analytics'i** görev ve:
    1. Ayarlama **SourceType** için **FileConnection**.
    2. Ayarlama **FileConnection** için az önce oluşturulan dosya bağlantısı.

5. Azure Data Lake Analytics görev için diğer yapılandırmaları tamamlayın.

## <a name="scenario-5-use-u-sql-statement-in-ssis-variable"></a>Senaryo 5 kullanımı U-SQL deyiminde, SSIS değişkeni

Bazı durumlarda, dinamik olarak U-SQL deyimlerini oluşturmak gerekebilir. Kullanabileceğiniz **SSIS değişken** ile **SSIS ifade** ve betik dinamik olarak U-SQL deyimi oluşturmanıza yardımcı olmak için görev gibi diğer SSIS görevleri.

1. Açık değişkenleri araç penceresi aracılığıyla **SSIS > değişkenler** üst düzey menü.

2. Bir SSIS değişken ekleyebilir ve doğrudan ayarlayın ya da kullanmak **ifade** değer oluşturmak için.

3. Ekleme **Azure Data Lake Analytics görev** ve:
    1. Ayarlama **SourceType** için **değişken**.
    2. Ayarlama **SourceVariable** SSIS az önce oluşturduğunuz değişken için.

4. Azure Data Lake Analytics görev için diğer yapılandırmaları tamamlayın.

## <a name="scenario-6-pass-parameters-to-u-sql-script"></a>Senaryo 6-parametreleri için U-SQL betiği

Bazı durumlarda, U-SQL değişken değeri U-SQL betiği dinamik olarak ayarlamak isteyebilirsiniz. **Parametre eşleme** özelliği bu senaryo ile Azure Data Lake Analytics görev Yardım. Genellikle iki normal kullanıcı durum vardır:

- Giriş kümesi ve dosya yolu değişkenler dinamik olarak geçerli tarih ve saat göre çıktı.
- Saklı yordamlar parametresini ayarlayın.

[U-SQL betiği parametreleri ayarlama hakkında daha fazla bilgi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017#parameter-mapping-page-configuration).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da SSIS paketleri çalıştırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
- [Integration Services (SSIS) için Azure Feature Pack](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud)
- [Azure Data Factory kullanarak U-SQL işleri](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics)

