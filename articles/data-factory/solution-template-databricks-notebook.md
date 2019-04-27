---
title: Azure Data Factory'de Databricks kullanarak verileri dönüştürme | Microsoft Docs
description: Azure Data Factory'de Databricks Not Defteri kullanarak verileri dönüştürmek için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: 562ce675acc43002ce468d60f8a8c412410be86c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60395416"
---
# <a name="transform-data-by-using-databricks-in-azure-data-factory"></a>Azure Data Factory'de Databricks kullanarak verileri dönüştürme

Bu öğreticide, içeren bir uçtan uca işlem hattı oluşturma **arama**, **kopyalama**, ve **Databricks not defteri** Data factory'de etkinlik.

-   **Arama** veya GetMetadata etkinliği, kaynak veri kümesi kopyalama ve analiz iş tetiklemeden önce aşağı akış kullanılmaya hazır olduğundan emin olmak için kullanılır.

-   **Kopyalama etkinliği** kaynak dosyayı kopyalar / havuz depolama birimine veri kümesi. Böylece veri kümesi doğrudan Spark tarafından tüketilebilecek havuz depolama Databricks not defteri DBFS bağlanmıştır.

-   **Databricks not defteri etkinliğini** dataset dönüştüren ve işlenen bir klasöre ekler Databricks not defteri tetikler / SQL DW.

Bu şablon basit tutmak için zamanlanmış bir tetikleyici şablonu oluşturmaz. Gerekirse, ekleyebilirsiniz.

![1](media/solution-template-Databricks-notebook/Databricks-tutorial-image01.png)

## <a name="prerequisites"></a>Önkoşullar

1.  Oluşturma bir **blob depolama hesabı** ve adlı bir kapsayıcı `sinkdata` olarak kullanılmak üzere **havuz**. Not ın **depolama hesabı adı**, **kapsayıcı adı**, ve **erişim anahtarı**, şablon başvurulduğundan.

2.  Olduğundan emin olun bir **Azure Databricks çalışma alanı** veya yeni bir tane oluşturun.

1.  **ETL için Not defterini alma**. İçeri aktarma dönüştürme Not Databricks çalışma alanı altında. (Bunu aşağıda gösterildiği gibi aynı konumda olması, ancak için daha sonra seçtiğiniz yolu yok.) URL alanını bu URL'yi girerek aşağıdaki URL'den not alın: `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`. Seçin **alma**.

    ![2](media/solution-template-Databricks-notebook/Databricks-tutorial-image02.png)

    ![3](media/solution-template-Databricks-notebook/Databricks-tutorial-image03.png)  

1.  Artık güncelleştirelim **dönüştürme** not defteri ile **depolama bağlantı bilgilerini** (adını ve erişim anahtarı). Git **komut 5** yukarıdaki içeri aktarılan not defterinde, değiştirin aşağıda vurgulanan değerler değiştirdikten sonra kod parçacığı. Bu hesap daha önce oluşturduğunuz aynı depolama hesabı ve içerdiğinden olun `sinkdata` kapsayıcı.

    ```python
    # Supply storageName and accessKey values  
    storageName = "<storage name>"  
    accessKey = "<access key>"  

    try:  
      dbutils.fs.mount(  
        source = "wasbs://sinkdata\@"+storageName+".blob.core.windows.net/",  
        mount_point = "/mnt/Data Factorydata",  
        extra_configs = {"fs.azure.account.key."+storageName+".blob.core.windows.net": accessKey})  

    except Exception as e:  
      # The error message has a long stack track. This code tries to print just the relevant line indicating what failed.

    import re
    result = re.findall(r"\^\s\*Caused by:\s*\S+:\s\*(.*)\$", e.message, flags=re.MULTILINE)
    if result:
      print result[-1] \# Print only the relevant error message
    else:  
      print e \# Otherwise print the whole stack trace.  
    ```

1.  Oluşturmak bir **Databricks erişim belirteci** Data factory'nin Databricks erişin. **Erişim belirtecini kaydetmesine** bir Databricks oluştururken daha sonra kullanmak için şuna benzer 'dapi32db32cbb4w6eee18b7d87e45exxxxxx' hizmetine bağlı

    ![4](media/solution-template-Databricks-notebook/Databricks-tutorial-image04.png)

    ![5](media/solution-template-Databricks-notebook/Databricks-tutorial-image05.png)

## <a name="create-linked-services-and-datasets"></a>Bağlı hizmetleri ve veri kümeleri oluşturma

1.  Yeni Oluştur **bağlı hizmetler** Data Factory kullanıcı arabiriminde giderek *bağlantıları bağlı hizmetler + yeni*

    1.  **Kaynak** – kaynak verilerine erişmek için. Bu örnek için kaynak dosyaları içeren ortak blob depolamayı kullanabilirsiniz.

        Seçin **Blob Depolama**, kullanın aşağıda **SAS URI'sini** (salt okunur erişimi) kaynak depolama alanına bağlanmak için.

        `https://storagewithdata.blob.core.windows.net/?sv=2017-11-09&ss=b&srt=sco&sp=rl&se=2019-12-31T21:40:53Z&st=2018-10-24T13:40:53Z&spr=https&sig=K8nRio7c4xMLnUV0wWVAmqr5H4P3JDwBaG9HCevI7kU%3D`

        ![6](media/solution-template-Databricks-notebook/Databricks-tutorial-image06.png)

    1.  **Havuz** : verileri kopyalamak için.

        ' % S'önkoşul 1, havuz bağlantılı hizmet olarak oluşturulan bir depolama alanını seçin.

        ![7](media/solution-template-Databricks-notebook/Databricks-tutorial-image07.png)

    1.  **Databricks** – Databricks kümesine bağlanma

        Önkoşul 2.c içinde oluşturulan erişim anahtarı kullanarak bir Databricks bağlı hizmeti oluşturun. Varsa bir *etkileşimli küme*, şunları seçebilirsiniz. (Bu örnekte *yeni iş küme* seçeneği.)

        ![8](media/solution-template-Databricks-notebook/Databricks-tutorial-image08.png)

2.  Oluşturma **veri kümeleri**

    1.  Oluşturma **'sourceAvailability_Dataset'** kaynak verilerin kullanılabilir olup olmadığını denetlemek için

    ![9](media/solution-template-Databricks-notebook/Databricks-tutorial-image09.png)

    1.  **Kaynak veri kümesi –** (ikili kopyalama kullanarak) kaynak veri kopyalama

    ![10](media/solution-template-Databricks-notebook/Databricks-tutorial-image10.png)

    1.  **Havuz veri kümesi** – havuz kopyalama / hedef konum

        1.  Bağlı hizmeti - '1.b içinde oluşturulan sinkBlob_LS' seçin

        2.  Dosya yolu - ' sinkdata/staged_sink'

        ![11](media/solution-template-Databricks-notebook/Databricks-tutorial-image11.png)

## <a name="create-activities"></a>Etkinlikleri oluşturun

1.  Arama etkinliği oluşturun '**kullanılabilirlik bayrağı**' kaynak kullanılabilirlik denetimi yapmak için (arama veya GetMetadata kullanılabilir). '2.a oluşturulan sourceAvailability_Dataset' seçin.

    ![12](media/solution-template-Databricks-notebook/Databricks-tutorial-image12.png)

1.  Kopyalama etkinliği oluşturma '**dosya blob**' kaynağından havuz veri kümesi kopyalama. Bu durumda, ikili dosya verilerdir. Başvuru için kopyalama etkinliği kaynak ve havuz yapılandırma ekran görüntüsü aşağıda.

    ![13](media/solution-template-Databricks-notebook/Databricks-tutorial-image13.png)

    ![14](media/solution-template-Databricks-notebook/Databricks-tutorial-image14.png)

1.  Tanımlama **işlem hattı parametreleri**

    ![15](media/solution-template-Databricks-notebook/Databricks-tutorial-image15.png)

1.  Oluşturma bir **Databricks etkinlik**

    Bir önceki adımda oluşturduğunuz bağlı hizmeti seçin.

    ![16](media/solution-template-Databricks-notebook/Databricks-tutorial-image16.png)

    Yapılandırma **ayarları**. Oluşturma **temel parametreleri** ekran görüntüsünde gösterildiği gibi ve Data Factory tarafından Databricks not defterine geçirilecek parametreler oluşturun. Gözat ve **seçin** **doğru not defteri yolu** içinde karşıya **önkoşul 2**.

    ![17](media/solution-template-Databricks-notebook/Databricks-tutorial-image17.png)

1.  **İşlem hattı çalıştırma**. Daha ayrıntılı Spark günlükleri için Databricks günlükleri bağlantısını bulabilirsiniz.

    ![18](media/solution-template-Databricks-notebook/Databricks-tutorial-image18.png)

    Depolama Gezgini'ni kullanarak veri dosyası da doğrulayabilirsiniz. (Data Factory işlem hattı çalıştırması ile ilişkilendirmek için bu örnek çıktı klasörüne çalıştırma kimliği data factory'deki işlem hattı ekler. Bu şekilde geri her bir çalıştırmanın oluşturulan dosyaları takip edebilirsiniz.)

![19](media/solution-template-Databricks-notebook/Databricks-tutorial-image19.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
