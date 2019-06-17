---
title: 'Veri Fabrikası Öğreticisi: İlk veri işlem hattı | Microsoft Docs'
description: Bu Azure Data Factory öğreticide oluşturma ve bir Hadoop kümesinde Hive betiği kullanarak veri işleyen bir veri fabrikası zamanlama gösterilmektedir.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: d9d9e68b7e74ba7725e97162d01e1a35314fdd0f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60564596"
---
# <a name="tutorial-build-your-first-pipeline-to-transform-data-using-hadoop-cluster"></a>Öğretici: Hadoop kümesi kullanarak verileri dönüştürmek için ilk işlem hattınızı oluşturma
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-build-your-first-pipeline.md)
> * [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [hızlı başlangıç: Azure Data Factory kullanarak veri fabrikası oluşturma](../quickstart-create-data-factory-dot-net.md).

Bu öğreticide, bir veri işlem hattı ile ilk Azure veri fabrikanızı oluşturun. İşlem hattı, çıkış verileri üretmek üzere bir Azure HDInsight (Hadoop) kümesinde Hive betiği çalıştırarak giriş verilerini dönüştürür.  

Bu makalede, genel bakış ve öğreticisi için Önkoşullar sağlar. Önkoşulları tamamladıktan sonra öğretici aşağıdaki araçlardan/Sdk'lardan birini kullanarak bunu yapabilirsiniz: Azure portal, Visual Studio, PowerShell, REST API Resource Manager şablonu. Başlangıcında (veya) bağlantıları aşağıdaki seçeneklerden birini kullanarak öğreticiyi uygulamak için bu makalenin sonunda açılır listedeki seçeneklerden birini belirleyin.    

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, taşıma ve dönüştürme bir veya daha fazla veri işlem hattı içerebilir. 

    Bu öğreticide, data factory'de bir işlem hattı oluşturun. 
2. Oluşturma bir **işlem hattı**. İşlem hattında bir veya daha fazla etkinlik olabilir (örnek: Kopyalama) etkinliği, HDInsight Hive etkinliği. Bu örnek, bir HDInsight Hadoop kümesinde bir Hive betiği çalıştıran HDInsight Hive etkinliği kullanır. Betik ilk olarak Azure blob depolamada depolanan ham web günlüğü verilerine başvuran bir tablo oluşturur ve ardından ham verileri yıl ve aya göre bölümler.

    Bu öğreticide, işlem hattı, bir Azure HDInsight Hadoop kümesinde Hive sorgusu çalıştırarak verileri dönüştürme için Hive etkinliği kullanır. 
3. Oluşturma **bağlı hizmetler**. Bir veri deposunu veya işlem hizmetini, veri fabrikasına bağlamak için bir bağlı hizmet oluşturursunuz. Azure Depolama gibi bir veri deposu, veri işlem hattındaki etkinliklerin giriş/çıkış verilerini tutar. HDInsight Hadoop kümesi gibi bir işlem hizmeti verileri işler/dönüştürür.

    Bu öğreticide, iki bağlı hizmet oluşturursunuz: **Azure depolama** ve **Azure HDInsight**. Azure depolama veri fabrikasına girdi/çıktı verilerini tutan Azure depolama hesabı bağlı hizmeti. Azure HDInsight, data factory verileri dönüştürmek için kullanılan bir Azure HDInsight kümesi bağlı hizmeti. 
3. Girdi ve çıktı **veri kümeleri** oluşturun. Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder.

    Bu öğreticide, girdi ve çıktı veri kümeleri, girdi ve çıktı verilerini Azure Blob Depolama konumlarını belirtin. Azure depolama bağlı hizmeti Azure depolama hesabı nedir belirtir kullanılır. Girdi veri kümesi, burada giriş dosyaları bulunan ve çıkış dosyalarının yerleştirildiği bir çıktı veri kümesi belirtir belirtir. 


Bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) Azure Data Factory ayrıntılı bir genel bakış makalesi.
  
İşte **diyagram görünümü** Bu öğreticide oluşturduğunuz örnek veri fabrikasının. **MyFirstPipeline** tüketen Hive türünde bir etkinlik olan **Azureblobınput** veri kümesi olarak bir giriş ve üretir **AzureBlobOutput** olarak bir çıkış veri kümesi. 

![Data Factory öğreticisinde diyagram görünümü](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


Bu öğreticide **inputdata** klasörü **adfgetstarted** Azure blob kapsayıcısı, input.log adlı bir dosya içerir. Bu günlük dosyasında üç aya ait girişler vardır: Ocak, Şubat ve Mart 2016. Girdi dosyasındaki, her aya ait örnek satırlar şunlardır. 

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Dosya, işlem hattı tarafından HDInsight Hive etkinliği ile işlendiğinde etkinlik, giriş verilerini yıl ve aya göre bölümlere ayıran HDInsight kümesi üzerinde bir Hive betiği çalıştırır. Betik, her bir aya ait girişlerin bulunduğu bir dosya içeren üç çıktı klasörü oluşturur.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Yukarıda gösterilen örnek satırlardan ilki (2016-01-01) 000000_0 dosyasına yazılır = 1 klasöründe. Benzer şekilde, ikinci satır month=2 klasöründeki dosyaya ve üçüncü satır month=3 klasöründeki dosyaya yazılır.  

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

1. **Azure aboneliği**: Aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Nasıl ücretsiz bir deneme hesabı edinebileceğinizi öğrenmek için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.
2. **Azure Depolama**: Bu öğreticide, verileri depolamak için bir Azure depolama hesabı kullanılmaktadır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın. Depolama hesabı oluşturduktan sonra Not **hesap adı** ve **erişim anahtarı**. Bkz. [Depolama erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma](../../storage/common/storage-account-manage.md#access-keys).
3. İndirin ve Hive sorgu dosyasını gözden geçirin (**HQL**) konumunda bulunan: [ https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql ](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Bu sorgu, çıkış verileri üretmek üzere giriş verilerini dönüştürür. 
4. İndirme ve örnek giriş dosyasını gözden geçirin (**input.log**) konumunda bulunan: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Adlı bir blob kapsayıcısı oluşturursunuz **adfgetstarted** Azure Blob Depolama alanınızda. 
6. Karşıya yükleme **partitionweblogs.hql** dosyasını **betik** klasöründe **adfgetstarted** kapsayıcı. Gibi araçları kullanın [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/). 
7. Karşıya yükleme **input.log** dosyasını **inputdata** klasöründe **adfgetstarted** kapsayıcı. 

Önkoşulları tamamladıktan sonra aşağıdaki araçlardan/öğreticiyi uygulamak için Sdk'lardan birini seçin: 

- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure portalı ve Visual Studio, veri fabrikaları oluşturma GUI yoludur. PowerShell ve Resource Manager şablonu REST API seçenekleri sağlar ise, veri fabrikaları oluşturmayı komut programlama yolu.

> [!NOTE]
> Bu öğreticideki veri işlem hattı, çıkış verileri üretmek üzere giriş verilerini dönüştürür. Bir kaynak veri deposundan hedef veri deposuna verileri kopyalamaz. Azure Data Factory kullanarak veri kopyalama hakkında bir öğretici için bkz. [Öğreticisi: Blob depolama alanından SQL veritabanı'na veri kopyalamak](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md). 





  
