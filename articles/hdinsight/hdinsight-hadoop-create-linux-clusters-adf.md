---
title: 'Öğretici: İsteğe bağlı Apache Hadoop kümeleri Azure Data Factory kullanarak HDInsight oluşturma '
description: Azure Data Factory kullanarak HDInsight isteğe bağlı Apache Hadoop kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/29/2018
ms.author: hrasheed
ms.openlocfilehash: 11827d8e355483b278a4c5ab5fcce41c03f0565a
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62102042"
---
# <a name="tutorial-create-on-demand-apache-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Öğretici: İsteğe bağlı Apache Hadoop kümeleri Azure Data Factory kullanarak HDInsight oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, şunların nasıl oluşturulacağı bir [Apache Hadoop](https://hadoop.apache.org/) isteğe bağlı, Azure Data Factory kullanarak Azure HDInsight kümesi. Ardından Hive işleri çalıştırmayı ve kümeyi silmek için veri işlem hatları Azure Data Factory'de kullanın. Bu öğreticinin sonunda, küme oluşturma işi çalıştırma ve küme silme zaman çizelgesinde nerede gerçekleştirilen çalıştırma bir büyük veri iş kullanıma hazır hale getirme hakkında bilgi edinin.

Bu öğretici aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Azure Data Factory etkinliğini anlama
> * Azure portalını kullanarak veri fabrikası oluşturma
> * Bağlı hizmetler oluşturma
> * İşlem hattı oluşturma
> * Tetikleyici işlem hattı
> * İşlem hattını izleme
> * Çıktıyı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- Azure PowerShell. Yönergeler için [yüklemek ve Azure PowerShell yapılandırma](https://docs.microsoft.com/powershell/azure/install-az-ps).

- Azure Active Directory Hizmet sorumlusu. Hizmet sorumlusu oluşturulduktan sonra almak mutlaka **uygulama kimliği** ve **kimlik doğrulama anahtarı** bağlantılı makaledeki yönergeleri kullanarak. Bu öğreticinin ilerleyen bölümlerinde bu değerlere ihtiyacınız olur. Ayrıca, hizmet sorumlusu üyesi olduğundan emin olun *katkıda bulunan* rolü abonelikte ya da kümenin oluşturulduğu kaynak grubu. Gereken değerleri almak ve doğru rollere atamak yönergeler için bkz: [Azure Active Directory Hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

Bu bölümde, oluşturduğunuz isteğe bağlı HDInsight kümesi için varsayılan depolama alanı olarak kullanılacak bir depolama hesabı oluşturun. Bu depolama hesabı da örnek içeren [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) betik (**hivescript.hql**) bir örnek benzetimini yapmak için kullandığınız [Apache Hive](https://hive.apache.org/) küme üzerinde çalışan iş.

Bu bölümde Azure PowerShell Betiği, gerekli dosyaları depolama hesabında kopyalama ve depolama hesabı oluşturmak için kullanılır. Bu bölümde Azure PowerShell örnek betiği aşağıdaki görevleri gerçekleştirir:

1. Azure için günlükleri.
1. Bir Azure kaynak grubu oluşturur.
1. Azure Depolama hesabı oluşturur.
1. Depolama hesabındaki bir Blob kapsayıcısını oluşturur
1. Örnek HiveQL betiğini kopyalar (**hivescript.hql**) Blob kapsayıcısı. Betik kullanılabilir [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Örnek betik, zaten başka bir ortak Blob kapsayıcısında kullanılabilir. Aşağıdaki PowerShell Betiği, oluşturduğu Azure depolama hesabına bu dosyaları bir kopyasını oluşturur.


**Depolama hesabı oluşturma ve Azure PowerShell kullanarak dosyaları kopyalamak için:**
> [!IMPORTANT]  
> Azure kaynak grubu ve komut dosyası tarafından oluşturulacak Azure depolama hesabı adları belirtin.
> Not **kaynak grubu adı**, **depolama hesabı adı**, ve **depolama hesabı anahtarı** komut dosyası tarafından yüzdelik. Sonraki bölümde ihtiyaç.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzAccount
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Depolama hesabı oluşturmayı doğrulamak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
1. Seçin **kaynak grupları** sol bölmede.
1. PowerShell betiğinizde oluşturduğunuz kaynak grubu adına çift tıklayın. Listelenen çok fazla kaynak grubu varsa, filtreyi kullanın.
1. Üzerinde **kaynakları** kutucuk, bir kaynak, kaynak grubu ile diğer projelerin dünya genelindeki Çalışanlarımız sürece listelenen görürsünüz. Bu kaynağın daha önce belirttiğiniz ada sahip bir depolama hesabıdır. Depolama hesabı adını seçin.
1. Seçin **Blobları** kutucukları.
1. Seçin **adfgetstarted** kapsayıcı. Adlı bir klasör gördüğünüz **hivescripts**.
1. Klasörü açın ve örnek komut dosyası içerdiğinden emin olun **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Azure Data Factory etkinliğini anlama

[Azure Data Factory](../data-factory/introduction.md) taşımayı ve dönüştürmeyi düzenleyen ve otomatikleştiren. Azure Data Factory, bir HDInsight Hadoop kümesi yalnızca bir giriş veri dilimi işlemek ve işleme tamamlandıktan sonra kümeyi silmek için zamanında oluşturabilirsiniz. 

Azure Data Factory'de veri fabrikası, bir veya daha fazla veri işlem hattı olabilir. Bir veri işlem hattı, bir veya daha fazla etkinlik içerir. İki tür etkinlik vardır:

- [Veri taşıma etkinlikleri](../data-factory/copy-activity-overview.md) -bir kaynak veri deposundan hedef veri deposuna veri taşımak veri taşıma etkinlikleri kullanın.
- [Veri dönüştürme etkinlikleri](../data-factory/transform-data.md). Dönüştürebilen/verileri için veri dönüştürme etkinlikleri kullanın. HDInsight Hive etkinliği, Data Factory tarafından desteklenen dönüştürme etkinlikleri biridir. Bu öğreticide Hive dönüştürme etkinliği kullandığınız.

Bu makalede, bir isteğe bağlı HDInsight Hadoop kümesi oluşturmak için Hive etkinliği yapılandırın. Etkinlik verileri işlemek için çalıştırıldığında, şunlar olur:

1. Bir HDInsight Hadoop kümesi, yalnızca dilimi işlemesi zamanında için otomatik olarak oluşturulur. 

1. Giriş veri kümesinde HiveQL betiğini çalıştırarak işlenir. Bu öğreticide, hive etkinliği ile ilişkili HiveQL betiğini aşağıdaki eylemleri gerçekleştirir:
    
    - Varolan tablonun kullanır (*hivesampletable*) başka bir tablo oluşturmak için **HiveSampleOut**.
    - Doldurur **HiveSampleOut** orijinal yalnızca belirli sütunlar içeren tablo *hivesampletable*.
    
1. HDInsight Hadoop kümesi, işlem tamamlandıktan sonra kümeyi yapılandırılmış zaman miktarı (timeToLive ayarını) için boşta silinir. Bu timeToLive boşta kalma süresi ile işleme için sonraki veri dilimi kullanılabilir haldeyse, aynı kümede dilimi işlemesi için kullanılır.  

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.

1. Sol menüden **+ kaynak Oluştur**.

1. Altında **Azure Marketi**seçin **Analytics**.

1.  Altında **öne çıkan**seçin **Data Factory**.

    ![Azure Data Factory portalında](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Portal'daki Data Factory")

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi değerleri seçin veya girin:

    ![Azure portalını kullanarak Azure veri fabrikası oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "oluşturma Azure Azure portalı kullanarak Data factory'yi")

    Aşağıdaki değerleri yazın veya seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Ad** |  Veri Fabrikası için bir ad girin. Bu adın küresel olarak benzersiz olması gerekir.|
    |**Abonelik**     |  Azure aboneliğinizi seçin. |
    |**Kaynak grubu**     | Seçin **var olanı kullan** ve ardından PowerShell betiğini kullanarak oluşturduğunuz kaynak grubunu seçin. |
    |**Sürüm**     | Seçin **V2** |
    |**Konum**     | Konum, daha önce bir kaynak grubu oluşturulurken belirtilen konuma otomatik olarak ayarlanır. Bu öğreticide, konumu ayarlamak **Doğu ABD**. |
    

1. **Oluştur**’u seçin. Herhangi bir veri fabrikası oluşturma 2 ila 4 dakika arasında sürebilir.


1. Veri Fabrikası oluşturulduktan sonra alacak bir **dağıtım başarılı** bildirimi bir **kaynağa Git** düğmesi.  Seçin **kaynağa Git** Data Factory varsayılan görünümünü açın.

1. Seçin **yazar ve İzleyici** Azure Data Factory yazma ve izleme Portalı'nı başlatmak için.

    ![Azure Data Factory'ye genel bakış](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory'ye genel bakış")

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu bölümde iki bağlı hizmet içinde veri fabrikanızı oluşturacaksınız.

- Bir Azure depolama hesabını veri fabrikasına bağlayan **Azure Depolama bağlı hizmeti**. Bu depolama alanı, isteğe bağlı HDInsight kümesi tarafından kullanılır. Ayrıca, küme üzerinde çalışan Hive betiğini içerir.
- **İsteğe bağlı HDInsight bağlı hizmeti**. Azure Data Factory otomatik olarak bir HDInsight kümesi oluşturur ve Hive betiği çalıştırır. Daha sonra, küme önceden yapılandırılmış bir süre boyunca boşta kaldığında HDInsight kümesini siler.

###  <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

1. Sol bölmeden **başlayalım** sayfasında **Düzenle** simgesi.

    ![Bir Azure Data Factory, bağlı hizmet oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "bir Azure Data Factory, bağlı hizmet oluşturma")

1. Seçin **bağlantıları** penceresi ve ardından sol alt köşesinden **+ yeni**.

    ![Azure Data Factory'de bağlantıları oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Azure Data Factory'de bağlantıları oluşturma")

1. İçinde **yeni bağlı hizmet** iletişim kutusunda **Azure Blob Depolama** seçip **devam**.

    ![Oluşturma Azure depolama bağlı hizmeti Data factory'nin](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "oluşturma Azure depolama bağlı hizmeti Data Factory")

1. Depolama bağlı hizmeti için bir ad belirtin, PowerShell betiğinin bir parçası oluşturduğunuz Azure depolama hesabını seçin ve ardından **son**.

    ![Sağlama adı Azure depolama bağlı hizmeti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "adını sağlayın Azure depolama bağlı hizmeti")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>İsteğe bağlı bir HDInsight bağlı hizmeti oluşturma

1. Başka bir bağlı hizmet oluşturmak için **+ Yeni** düğmesini tekrar seçin.

1. **Yeni Bağlı Hizmet** penceresinde **İşlem** > **Azure HDInsight**’ı ve sonra **Devam**’ı seçin.

    ![Oluşturma HDInsight bağlı hizmeti Azure Data Factory için](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "oluşturma HDInsight bağlı hizmeti Azure Data Factory için")

1. İçinde **yeni bağlı hizmet** penceresi, gerekli değerleri sağlayın.

    ![Sağlama değerleri için HDInsight bağlı hizmeti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "sağlama değerleri için HDInsight bağlı hizmeti")

    Aşağıdaki değerleri girin ve geri kalan varsayılan olarak bırakın.

    | Özellik | Açıklama |
    | --- | --- |
    | Ad | HDInsight bağlı hizmeti için bir ad girin |
    | Tür | Seçin **isteğe bağlı HDInsight** |
    | Azure Storage Bağlı Hizmeti | Daha önce oluşturduğunuz depolama bağlı hizmeti seçin. |
    | Küme türü | Seçin **hadoop** |
    | Yaşam süresi | HDInsight küme otomatik olarak silinmeden önce kullanılabilir olmasını istediğiniz süreyi belirtin.|
    | Hizmet sorumlusu kimliği | Önkoşulların bir parçası oluşturduğunuz Azure Active Directory Hizmet sorumlusunun uygulama Kimliğini girin |
    | Hizmet sorumlusu anahtarı | Azure Active Directory Hizmet sorumlusunun kimlik doğrulama anahtarı sağlayın |
    | Küme adı ön eki | Data factory tarafından oluşturulan tüm küme türleri için önek alacaktır bir değer belirtin |
    | Kaynak grubu | Daha önce kullanılan PowerShell betiğinin bir parçası oluşturduğunuz kaynak grubunu seçin| 
    | Küme SSH kullanıcı adı | Bir SSH kullanıcı adı girin |
    | Küme SSH parolası | SSH kullanıcısı için bir parola sağlayın |

    **Son**’u seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **+** (artı) düğmesini seçip **İşlem Hattı**'nı seçin.

    ![Azure Data Factory'de işlem hattı oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Azure Data Factory'de işlem hattı oluşturma")

1. İçinde **etkinlikleri** araç, genişletin **HDInsight**, sürükleyin **Hive** etkinliğini işlem hattı Tasarımcı yüzeyine bırakın. İçinde **genel** sekmesinde, etkinliği için bir ad sağlayın.

    ![Data Factory işlem hattı için etkinlik ekleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Data Factory işlem hattı için etkinlik ekleme")

1. Seçeneğini seçili Hive etkinliği sahip emin olun **HDI kümesi** sekmesi, gelen ve giden **HDInsight bağlı hizmeti** açılan listesinde, daha önce oluşturduğunuz için HDInsight bağlı hizmeti seçin.

    ![HDInsight küme ayrıntıları sağlamak için işlem hattı](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "işlem hattının sağlamak HDInsight küme ayrıntıları")

1. Seçin **betik** sekmesini ve aşağıdaki adımları tamamlayın:
    
    1. İçin **betik bağlı hizmeti**seçin **HDIStorageLinkedService**. Daha önce oluşturduğunuz depolama bağlı hizmeti değerdir.
    
    1. İçin **dosya yolu**seçin **depolamaya Gözat** ve örnek Hive betiği kullanılabildiği konumuna gidin. Daha önce PowerShell betiğini çalıştırdıysanız bu konuma olmalıdır `adfgetstarted/hivescripts/hivescript.hql`.
    
        ![İşlem hattı için Hive komut dosyası ayrıntılarını sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "işlem hattının sağlamak Hive betik ayrıntıları")
    
    1. Altında **Gelişmiş** > **parametreleri**seçin **betikten otomatik olarak Doldur**. Bu seçenek değerleri çalışma zamanında gerektiren herhangi bir parametre Hive betiğinde arar. Kullandığınız komut dosyası (**hivescript.hql**) sahip bir **çıkış** parametresi. Biçiminde bir değer sağlayın `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` Azure depolama hesabınızda var olan klasöre işaret edecek şekilde. Bu yol büyük/küçük harfe duyarlıdır. Bu betik çıktısı depolanacağı yoludur.
    
        ![Parametreleri sağlamak için Hive betiği](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "parametreleri sağlamak için Hive betiği")

1. Seçin **doğrulama** işlem hattını doğrulamak için. Doğrulama penceresini kapatmak için **>>** (sağ ok) düğmesini seçin.

    ![Azure Data Factory işlem hattı doğrulama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Azure Data Factory işlem hattını doğrulama")

1. Son olarak, seçin **tümünü Yayımla** yapıtları Azure Data Factory'de yayımlamak için.

    ![Azure Data Factory işlem hattı yayımlama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Azure Data Factory işlem hattı yayımlama")

## <a name="trigger-a-pipeline"></a>Tetikleyici işlem hattı

1. Tasarımcı yüzeyinde araç çubuğundan seçin **tetikleyici** > **şimdi Tetikle**.

    ![Azure Data Factory işlem hattını tetikleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Azure Data Factory işlem hattını tetikleme")

1. Seçin **son** açılır kenar çubuğu olarak.

## <a name="monitor-a-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. **İşlem Hattı Çalıştırmaları** listesinde bir işlem hattı çalıştırması görürsünüz. Altında çalıştır durumunu fark **durumu** sütun.

    ![Azure Data Factory işlem hattını izleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Azure Data Factory işlem hattını izleme")

1. Durumu yenilemek için **Yenile**’yi seçin.

1. Belirleyebilirsiniz **etkinlik çalıştırmalarını görüntüle** etkinlik çalıştırma görmek için işlem hattı çalıştırmasıyla ilişkili. Aşağıdaki ekran görüntüsünde, yalnızca bir etkinlik, oluşturduğunuz işlem hattında yalnızca bir etkinlik olduğundan çalıştırma görürsünüz. Önceki görünüme dönmek için seçin **işlem hatları** sayfasının en üste.

    ![Azure Data Factory işlem hattı etkinliğini](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Azure Data Factory işlem hattı etkinliğini izleme")


## <a name="verify-the-output"></a>Çıktıyı doğrulama

1. Çıktıyı doğrulamak için Azure portalında Bu öğretici için kullandığınız depolama hesabına gidin. Aşağıdaki klasörleri veya kapsayıcıları görmeniz gerekir:

    - Gördüğünüz bir **adfgerstarted/outputfolder** işlem hattının bir parçası çalıştırılan Hive betiğinin çıktısı içerir.

    - Gördüğünüz bir **adfhdidatafactory -\<bağlı-hizmet-adı >-\<zaman damgası >** kapsayıcı. İşlem hattı çalıştırmasını bir parçası oluşturulmuş HDInsight kümesinin varsayılan depolama konumu kapsayıcıdır.

    - Gördüğünüz bir **adfjobs** Azure Data Factory işin kapsayıcı günlüğe kaydeder.  

        ![Azure Data Factory işlem hattı çıktı doğrulayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Azure Data Factory işlem hattı çıktı doğrulayın")


## <a name="clean-up-the-tutorial"></a>Öğreticiyi silme

İle şirket-HDInsight küme oluşturma deman, açıkça HDInsight kümeyi silmeniz gerekmez. Küme, işlem hattını oluştururken sağladığınız yapılandırmasına göre silinir. Ancak, hatta kümesi silindikten sonra kümeyle ilişkili depolama hesapları var olmaya devam. Böylece, verilerinizi korumak Bu davranış tasarım gereğidir. Ancak, verileri kalıcı hale getirmek istemiyorsanız, oluşturduğunuz depolama hesabını silebilirsiniz.

Alternatif olarak, Bu öğretici için oluşturduğunuz tüm kaynak grubunu silebilirsiniz. Bu depolama hesabı ve Azure Data Factory, oluşturduğunuz siler.

### <a name="delete-the-resource-group"></a>Kaynak grubunu sil

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
1. Seçin **kaynak grupları** sol bölmede.
1. PowerShell komut dosyanızda oluşturduğunuz kaynak grubu adı seçin. Listelenen çok fazla kaynak grubu varsa, filtreyi kullanın. Kaynak grubunu açar.
1. Üzerinde **kaynakları** kutucuk, sahip varsayılan depolama hesabı ve kaynak grubu diğer projeleriyle paylaşın sürece listelenen data factory.
1. **Kaynak grubunu sil**'i seçin. Bunun yapılması, depolama hesabı ve depolama hesabında depolanan verileri siler.

    ![Kaynak grubunu silme](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "kaynak grubunu sil")

1. Silme işlemini onaylayın ve ardından seçmek için kaynak grubu adı girin **Sil**.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, isteğe bağlı HDInsight kümesi ve çalışma oluşturmak için Azure Data Factory kullanmayı öğrendiniz [Apache Hive](https://hive.apache.org/) işler. Özel yapılandırma ile HDInsight kümeleri oluşturma hakkında bilgi edinmek için sonraki artciel ilerleyin.

> [!div class="nextstepaction"]
>[Azure HDInsight kümeleri ile özel yapılandırma oluşturma](hdinsight-hadoop-provision-linux-clusters.md)


