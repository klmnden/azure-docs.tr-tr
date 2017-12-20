---
title: "Veri Fabrikası Öğreticisi: ilk veri ardışık | Microsoft Docs"
description: "Bu Azure Data Factory öğretici nasıl oluşturulacağı ve bir Hadoop kümesindeki Hive betiği kullanarak verileri işleyen bir veri fabrikası zamanlama gösterir."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: a1a0679b91304df5cbc3dcaec14abfeaaa25c04f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="tutorial-build-your-first-pipeline-to-transform-data-using-hadoop-cluster"></a>Öğretici: Verileri Hadoop kümesi kullanarak dönüştürmek için ilk işlem hattınızı oluşturma
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-build-your-first-pipeline.md)
> * [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Data Factory hizmetinin önizleme aşamasında olan 2. sürümünü kullanıyorsanız [Hızlı Başlangıç: Azure Data Factory sürüm 2’yi kullanarak veri fabrikası oluşturma](../quickstart-create-data-factory-dot-net.md) konusunu inceleyin.

Bu öğreticide, veri ardışık ile ilk Azure data factory'nizi derleme. Ardışık Düzen giriş verileri çıktı verileri üretemedi bir Azure Hdınsight (Hadoop) kümesinde Hive betiğini çalıştırarak dönüştürür.  

Bu makalede, genel bakış ve öğreticisi için Önkoşullar sağlar. Önkoşulları tamamladıktan sonra aşağıdaki araçlar/Sdk'lardan birini kullanarak öğreticiyi yapabilirsiniz: Azure portal, Visual Studio, PowerShell, Resource Manager şablonu, REST API. Açılan listenin başlangıcında (veya) bu seçeneklerden birini kullanarak öğreticiyi yapmak için bu makalenin sonunda bağlantılar seçeneklerden birini seçin.    

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası taşıyın ve veri dönüştürme bir veya daha fazla veri ardışık düzen içerebilir. 

    Bu öğreticide, data factory'de bir işlem hattı oluşturun. 
2. Oluşturma bir **ardışık düzen**. İşlem hattında bir veya daha fazla etkinlik bulunabilir (Örnek: Kopya Etkinliği, HDInsight Hive Etkinliği). Bu örnek, bir Hdınsight Hadoop kümesinde bir Hive betiği çalıştıran Hdınsight Hive etkinliği kullanmaktadır. Komut dosyası ilk Azure blob storage'da depolanan ham web günlüğü verilerini başvuruda bulunan bir tablo oluşturur ve sonra ham verileri ay ve yıl bölümler.

    Bu öğreticide, ardışık düzen Azure Hdınsight Hadoop kümesinde bir Hive sorgusu çalıştırarak veri dönüştürecek Hive etkinliği kullanır. 
3. Oluşturma **bağlantılı Hizmetleri**. Bir veri deposunu veya işlem hizmetini, veri fabrikasına bağlamak için bir bağlı hizmet oluşturursunuz. Azure Depolama gibi bir veri deposu, veri işlem hattındaki etkinliklerin giriş/çıkış verilerini tutar. Hdınsight Hadoop kümesi gibi işlem hizmeti işlemleri/veri dönüşümler.

    Bu öğreticide, iki bağlı hizmet oluşturacaksınız: **Azure Storage** ve **Azure Hdınsight**. Azure depolama hizmeti bağlantıları data factory girdi/çıktı verilerini tutan Azure Storage hesabını bağlı. Azure Hdınsight data factory veri dönüştürmek için kullanılan bir Azure Hdınsight Küme hizmeti bağlantıları bağlı. 
3. Girdi ve çıktı **veri kümeleri** oluşturun. Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder.

    Bu öğreticide girdi ve çıktı veri kümeleri girdi ve çıktı verilerini Azure Blob Depolama konumlarını belirtin. Azure Storage bağlı hizmeti Azure depolama hesabı nedir belirtir kullanılır. Bir giriş veri kümesi burada girdi dosyaları bulunan bir çıkış veri kümesi çıktı dosyaları yerleştirildiği belirtir. 


Bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) Azure Data Factory için ayrıntılı bir genel bakış makalesi.
  
Burada **diyagram görünümü** Bu öğreticide yapı örnek veri fabrikasının. **MyFirstPipeline** tüketir Hive türünde bir etkinlik sahip **Azureblobınput** veri kümesi oluşturur ve bir girdi olarak **AzureBlobOutput** olarak bir çıkış veri kümesi. 

![Data Factory öğreticisinde diyagram görünümü](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


Bu öğreticide **inputdata** klasöründe **adfgetstarted** Azure blob kapsayıcısı input.log adlı bir dosya içerir. Bu günlük dosyası üç ay girişlerinden vardır: Ocak, Şubat ve Mart 2016. Girdi dosyasındaki, her aya ait örnek satırlar şunlardır. 

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

Yukarıda gösterilen örnek satırlarından (ile 2016-01-01) ilk bir ay içinde 000000_0 dosyasına yazılır = 1 klasör. Benzer şekilde, ikinci satır month=2 klasöründeki dosyaya ve üçüncü satır month=3 klasöründeki dosyaya yazılır.  

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

1. **Azure aboneliği**: Aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Nasıl ücretsiz bir deneme hesabı edinebileceğinizi öğrenmek için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.
2. **Azure Depolama**: Bu öğreticide, verileri depolamak için bir Azure depolama hesabı kullanılmaktadır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın. Depolama hesabı oluşturduktan sonra aşağı Not **hesap adı** ve **erişim tuşu**. Bkz. [Depolama erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma](../../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).
3. Karşıdan yükle ve Hive sorgusu dosyasını gözden geçirin (**HQL**) konumunda bulunan: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Bu sorgu çıktı verileri üretemedi giriş verileri dönüştürür. 
4. Karşıdan yükle ve örnek girdi dosyası gözden geçirin (**input.log**) konumunda bulunan: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Adlı bir blob kapsayıcı oluşturun **adfgetstarted** Azure Blob Depolama. 
6. Karşıya yükleme **partitionweblogs.hql** dosya **betik** klasöründe **adfgetstarted** kapsayıcı. Gibi araçlar kullanın [Microsoft Azure Storage Gezgini](http://storageexplorer.com/). 
7. Karşıya yükleme **input.log** dosya **inputdata** klasöründe **adfgetstarted** kapsayıcı. 

Önkoşulları tamamladıktan sonra aşağıdaki araçlar/öğretici yapmak için SDK'ları birini seçin: 

- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure portalı ve Visual Studio, veri fabrikaları oluşturmanın GUI yolunu sağlar. Ancak, veri fabrikaları oluşturmanın scripting programlama yolu PowerShell, Resource Manager şablonu ve REST API seçeneklerini sağlar.

> [!NOTE]
> Bu öğreticideki veri işlem hattı, çıkış verileri üretmek üzere giriş verilerini dönüştürür. Bir kaynak veri deposundan hedef veri deposuna verileri kopyalamaz. Azure Data Factory kullanarak verileri kopyalama öğreticisi için bkz. [Öğretici: Blob Depolama’dan SQL Veritabanı’na veri kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md). 





  
