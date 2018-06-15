---
title: "Öğretici: Azure Data Factory kullanarak Hdınsight'ta isteğe bağlı Hadoop kümeleri oluşturma | Microsoft Docs"
description: Azure Data Factory kullanarak Hdınsight'ta isteğe bağlı Hadoop kümeleri oluşturmayı öğrenin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: 53ff14e00b88f6d182579ba0d9df630fae9b3d78
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33771142"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Öğretici: Azure Data Factory kullanarak Hdınsight'ta isteğe bağlı Hadoop kümeleri oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, Azure Data Factory kullanarak Azure hdınsight'ta isteğe bağlı bir Hadoop kümesi oluşturmayı öğrenin. Ardından Hive işlerini çalıştırmak ve küme silmek için veri ardışık Azure Data Factory'de kullanın. Bu öğreticide sonuna kadar küme oluşturma, işi çalıştırma ve küme silinmesini bir zamanlamaya göre burada gerçekleştirilen bir büyük veri iş faaliyete öğrenin.

Bu öğretici aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Azure Data Factory etkinlik anlama
> * Azure portalını kullanarak bir veri fabrikası oluşturun
> * Bağlı hizmetler oluşturma
> * İşlem hattı oluşturma
> * Tetikleyici bir ardışık düzen
> * İşlem hattını izleme
> * Çıktıyı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* Azure PowerShell. Yönergeler için bkz: [yükleyin ve Azure PowerShell yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Bir Azure Active Directory hizmet asıl. Hizmet sorumlusu oluşturduktan sonra almak mutlaka **uygulama kimliği** ve **kimlik doğrulama anahtarı** bağlantılı makalesindeki yönergeleri kullanarak. Bu öğreticide daha sonra bu değerler gerekir. Ayrıca, hizmet sorumlusu bir üyesi olduğundan emin olun *katkıda bulunan* rol abonelik veya kaynak grubunun küme oluşturulur. Gerekli değerleri almak ve doğru rolleri atamak yönergeler için bkz: [bir Azure Active Directory Hizmet sorumlusu oluşturmak](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

Bu bölümde, talep üzerine oluşturmak Hdınsight kümesi için varsayılan depolama alanı olarak kullanılacak bir depolama hesabı oluşturun. Bu depolama hesabını örnek HiveQL betiğini de içerir (**hivescript.hql**), küme üzerinde çalışan bir örnek Hive işi benzetimini yapmak için kullanın.

Bu bölüm, gerekli dosyaları depolama hesabındaki üzerinden depolama hesabı ve kopya oluşturmak için bir Azure PowerShell betiğini kullanır. Bu bölümde Azure PowerShell örnek betiği aşağıdaki görevleri gerçekleştirir:

1. Azure günlükleri.
2. Bir Azure kaynak grubu oluşturur.
3. Azure Depolama hesabı oluşturur.
4. Depolama hesabında bir Blob kapsayıcısını oluşturur
5. Örnek HiveQL betiğini kopyalar (**hivescript.hql**) Blob kapsayıcısı. Komut dosyası şu adresten edinilebilir [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Örnek komut dosyasını başka bir ortak Blob kapsayıcısında zaten var. Aşağıdaki PowerShell komut dosyası oluşturduğu Azure Storage hesabınıza bu dosyaları bir kopyasını oluşturur.


**Depolama hesabı oluşturma ve Azure PowerShell kullanarak dosyaları kopyalamak için:**
> [!IMPORTANT]
> Azure kaynak grubu ve komut dosyası tarafından oluşturulan Azure depolama hesabı adlarını belirtin.
> Yazma **kaynak grubu adı**, **depolama hesabı adı**, ve **depolama hesabı anahtarı** komut dosyası tarafından yüzdelik. Sonraki bölümde gereksinim duyarsınız.

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
<<<<<<< HEAD
Login-AzureRmAccount
=======
try{Get-AzureRmContext}
catch{Connect-AzureRmAccount}
>>>>>>> refs/remotes/MicrosoftDocs/release-build-hdinsight-2018
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Depolama hesabı oluşturmayı doğrulamak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Seçin **kaynak grupları** sol bölmede.
3. PowerShell komut dosyasında oluşturulan kaynak grubu adı çift tıklatın. Listelenen çok fazla kaynak grupları varsa, filtrenin kullanın.
4. Üzerinde **kaynakları** kutucuğu, bir kaynak kaynak grubu diğer projelerle paylaşmak sürece listelenen bakın. Bu kaynak, daha önce belirttiğiniz ada sahip bir depolama hesabıdır. Depolama hesabı adı seçin.
5. Seçin **BLOB'lar** döşeme.
6. Seçin **adfgetstarted** kapsayıcı. Adlı bir klasör gördüğünüz **hivescripts**.
7. Klasörü açın ve örnek komut dosyası içerdiğinden emin olun **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Azure Data Factory etkinlik anlama

[Azure Data Factory](../data-factory/introduction.md) düzenleyen ve taşınmasını ve dönüştürülmesini veri otomatikleştirir. Azure Data Factory bir Hdınsight Hadoop kümesi yalnızca bir giriş veri dilimi işlemek ve işlem tamamlandıktan sonra kümeyi silmek için zaman oluşturabilirsiniz. 

Azure Data Factory'de veri fabrikası bir veya daha fazla veri işlem hattı olabilir. Veri ardışık bir veya daha fazla etkinlikler içeriyor. İki tür etkinlik vardır:

* [Veri taşıma etkinlikleri](../data-factory/copy-activity-overview.md) -veri kaynağına veri deposundan hedef veri deposuna taşımak için veri taşıma etkinlikleri kullanın.
* [Veri dönüştürme etkinlikleri](../data-factory/transform-data.md). Veri dönüştürme/işlem için veri dönüştürme etkinlikleri kullanın. Hdınsight Hive etkinliği Data Factory ile desteklenen dönüştürme etkinlikleri biridir. Bu öğreticide Hive dönüşüm etkinliği kullanın.

Bu makalede, bir isteğe bağlı Hdınsight Hadoop kümesi oluşturmak için Hive etkinliği yapılandırın. Etkinlik veri işlemek için çalıştırıldığında, şunlar olur:

1. Bir Hdınsight Hadoop kümesi, yalnızca dilim işlemek zaman için otomatik olarak oluşturulur. 

2. Giriş verisi HiveQL betiğini küme üzerinde çalışan tarafından işlenir. Bu öğreticide, hive etkinliği ile ilişkili HiveQL betiğini aşağıdaki eylemleri gerçekleştirir:

    * Varolan tablonun kullanır (*hivesampletable*) başka bir tablo oluşturmak için **HiveSampleOut**.
    * Doldurur **HiveSampleOut** yalnızca belirli sütunları özgün tabloyla *hivesampletable*.

3. İşlem tamamlandıktan sonra kümeyi yapılandırılan süreyi (timeToLive ayarını) için boşta Hdınsight Hadoop kümesi silindi. Sonraki veri dilimi bu timeToLive boşta kalma süresi ile işlemek için kullanılabilir durumda, aynı küme dilim işlemek için kullanılır.  

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.

2. Azure portalında seçin **kaynak oluşturma** > **veri + analiz** > **Data Factory**.

    ![Azure Data Factory portalındaki](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "portalındaki Azure veri fabrikası")

2. Girin veya aşağıdaki ekran görüntüsünde gösterilen değerleri seçin:

    ![Azure portalını kullanarak Azure Data Factory oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "Azure veri Azure portalını kullanarak fabrikası oluşturma")

    Aşağıdaki değerleri yazın veya seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Ad** |  Veri Fabrikası için bir ad girin. Bu ad genel olarak benzersiz olmalıdır.|
    |**Abonelik**     |  Azure aboneliğinizi seçin. |
    |**Kaynak grubu**     | Seçin **var olanı kullan** ve PowerShell Betiği kullanılarak oluşturulan kaynak grubu seçin. |
    |**Sürüm**     | Seçin **V2 (Önizleme)** |
    |**Konum**     | Konum, daha önce kaynak grubu oluşturulurken belirtilen konuma otomatik olarak ayarlanır. Bu öğretici için konum ayarlamak **Doğu ABD 2**. |
    

3. Seçin **panoya Sabitle**ve ardından **oluşturma**. Başlıklı yeni bir kutucuk göreceksiniz **dağıtım gönderme** portal panosunda. Veri Fabrikası oluşturma, herhangi bir yere 2 ila 4 dakika arasında sürebilir.

    ![Şablon dağıtımı ilerleme](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "şablon dağıtımı devam ediyor") 
 
4. Veri Fabrikası oluşturulduktan sonra portal data factory için genel bakışı göstermektedir.

    ![Azure Data Factory genel bakış](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory genel bakış")

5. Seçin **Yazar & İzleyici** yazma ve portal izleme Azure Data Factory başlatmak için.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu bölümde, iki bağlı hizmet veri Fabrikanızda yazar.

- Bir Azure depolama hesabını veri fabrikasına bağlayan **Azure Depolama bağlı hizmeti**. Bu depolama alanı, isteğe bağlı HDInsight kümesi tarafından kullanılır. Ayrıca kümede çalışan Hive betiğini de içerir.
- **İsteğe bağlı HDInsight bağlı hizmeti**. Azure Data Factory otomatik olarak bir Hdınsight kümesi oluşturur ve Hive betiği çalıştırır. Daha sonra, küme önceden yapılandırılmış bir süre boyunca boşta kaldığında HDInsight kümesini siler.

###  <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

1. Sol bölmesinden **başlayalım** sayfasında, **Düzenle** simgesi.

    ![Bir Azure Data Factory bağlantılı hizmeti oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "bir Azure Data Factory bağlantılı hizmeti oluşturma")

2. Seçin **bağlantıları** penceresinin ve ardından sol alt köşeden **+ yeni**.

    ![Azure Data Factory'de bağlantıları oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Azure Data Factory'de bağlantıları oluşturma")

3. İçinde **yeni bağlı hizmet** iletişim kutusunda **Azure Blob Storage** ve ardından **devam**.

    ![Create Azure depolama bağlantılı hizmeti veri fabrikası için](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "oluşturmak Azure depolama bağlantılı hizmeti için veri fabrikası")

4. Depolama bağlantılı hizmeti için bir ad, PowerShell Betiği bir parçası olarak, oluşturduğunuz Azure depolama hesabı seçin ve ardından **son**.

    ![Sağlama adı Azure depolama bağlantılı hizmeti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "sağla adı Azure depolama bağlı hizmeti")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>İsteğe bağlı bir HDInsight bağlı hizmeti oluşturma

1. Başka bir bağlı hizmet oluşturmak için **+ Yeni** düğmesini tekrar seçin.

2. **Yeni Bağlı Hizmet** penceresinde **İşlem** > **Azure HDInsight**’ı ve sonra **Devam**’ı seçin.

    ![Create Hdınsight bağlı hizmeti Azure Data Factory için](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "oluşturma Hdınsight bağlı hizmeti Azure Data Factory için")

3. İçinde **yeni bağlı hizmet** penceresinde, gerekli değerleri girin.

    ![Hdınsight için değerler sağlayın bağlantılı hizmeti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Hdınsight için değerler sağlayın bağlantılı hizmeti")

    Aşağıdaki değerleri girin ve geri kalan varsayılan olarak bırakın.

    | Özellik | Açıklama |
    | --- | --- |
    | Ad | Hdınsight bağlı hizmeti için bir ad girin |
    | Tür | Seçin **isteğe bağlı Hdınsight** |
    | Azure Storage Bağlı Hizmeti | Daha önce oluşturduğunuz depolama bağlı hizmeti seçin. |
    | Küme türü | Seçin **hadoop** |
    | Yaşam süresi | Hdınsight küme otomatik olarak silinmeden önce kullanılabilir olmasını istediğiniz süreyi belirtin.|
    | Hizmet sorumlusu kimliği | Önkoşullar bir parçası olarak oluşturulan Azure Active Directory Hizmet Asıl uygulama Kimliğini belirtin |
    | Hizmet asıl anahtarı | Azure Active Directory hizmet asıl için kimlik doğrulama anahtarı sağlayın |
    | Küme adı öneki | Veri fabrikası tarafından oluşturulan tüm küme türleri için önek olarak bir değer girin |
    | Kaynak grubu | Daha önce kullanılan PowerShell komut dosyasının bir parçası, oluşturduğunuz kaynak grubunu seçin| 
    | Küme SSH kullanıcı adı | Bir SSH kullanıcı adı girin |
    | Küme SSH parolası | SSH kullanıcı için bir parola sağlayın |

    **Son**’u seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **+** (artı) düğmesini seçip **İşlem Hattı**'nı seçin.

    ![Azure Data Factory'de işlem hattı oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Azure Data Factory'de işlem hattı oluşturma")

2. İçinde **etkinlikleri** araç kutusu, genişletin **Hdınsight**, sürükleyin **Hive** ardışık düzen Tasarımcı yüzeyine etkinlik. İçinde **genel** sekmesinde, etkinliği için bir ad sağlayın.

    ![Data Factory işlem hattı etkinlikler ekleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Data Factory işlem hattı etkinlikler ekleme")

3. Seçili Hive etkinliği sahip, seçim yapmak **HDI kümesi** sekmesi, gelen ve giden **Hdınsight bağlı hizmeti** açılan listesinde, önceden oluşturduğunuz için Hdınsight bağlı hizmeti seçin.

    ![Ardışık düzeni için Hdınsight küme ayrıntıları sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "ardışık düzeni için sağlayan Hdınsight küme ayrıntıları")

4. Seçin **betik** sekmesinde ve aşağıdaki adımları tamamlayın:

    a. İçin **betik bağlantılı hizmet**seçin **HDIStorageLinkedService**. Bu değer, daha önce oluşturduğunuz depolama bağlı hizmeti olur.

    b. İçin **dosya yolu**seçin **Gözat depolama** ve örnek Hive betiğini olduğu kullanılabilir konuma gidin. Bu konum, PowerShell Betiği daha önce çalıştıysa olmalıdır `adfgetstarted/hivescripts/hivescript.hql`.

    ![Ardışık düzeni için Hive komut dosyası ayrıntılarını sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "komut dosyası ayrıntılarını sağlayın Hive ardışık düzeni")

    c. Altında **Gelişmiş** > **parametreleri**seçin **otomatik olarak doldurulması komut dosyasından**. Bu seçenek değerleri çalışma zamanında gerektiren herhangi bir parametre Hive betiğini de arar. Kullandığınız komut dosyası (**hivescript.hql**) sahip bir **çıkış** parametresi. Değer biçimde sağlayın `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` Azure depolama alanınızı üzerinde var olan klasöre işaret edecek şekilde. Bu yol büyük/küçük harfe duyarlıdır. Bu komut dosyası çıkışını depolanacağı yoludur.

    ![Hive betiği için parametreleri sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Hive betiğini için parametreleri sağlayın")

5. Seçin **doğrulama** ardışık düzen doğrulanacak. Doğrulama penceresini kapatmak için **>>** (sağ ok) düğmesini seçin.

    ![Azure Data Factory işlem hattı doğrulamak](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Azure Data Factory işlem hattı doğrula")

5. Son olarak, seçin **tümünü Yayımla** yapıları için Azure Data Factory yayımlamak için.

    ![Azure Data Factory işlem hattı yayımlama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Azure Data Factory işlem hattı yayımlama")

## <a name="trigger-a-pipeline"></a>Tetikleyici bir ardışık düzen

1. Tasarımcı yüzeyinde araç çubuğundan seçin **tetikleyici** > **şimdi tetikleyici**.

    ![Azure Data Factory işlem hattı tetiklemek](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Azure Data Factory işlem hattı Tetikle")

2. Seçin **son** açılır yan çubuğunda.

## <a name="monitor-a-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. **İşlem Hattı Çalıştırmaları** listesinde bir işlem hattı çalıştırması görürsünüz. Altında çalıştır durumunu fark **durum** sütun.

    ![Azure Data Factory işlem hattını izleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Azure Data Factory işlem hattını izleme")

2. Durumu yenilemek için **Yenile**’yi seçin.

3. Öğesini de seçebilirsiniz **görünüm etkinlik çalışması** etkinliğin çalışma görmek için simgesini ilişkili sahip işlem hattı. Aşağıdaki ekran görüntüsünde, oluşturduğunuz ardışık düzeninde yalnızca bir etkinlik olduğundan yalnızca bir etkinlik bakın. Önceki görünüme dönmek için seçin **ardışık düzen** sayfanın üstünde bulunun.

    ![Azure Data Factory işlem hattı etkinliğini izleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Azure Data Factory işlem hattı etkinliğini izleme")


## <a name="verify-the-output"></a>Çıktıyı doğrulama

1. Çıktı doğrulamak için Bu öğretici için kullandığınız depolama hesabı Azure portalında gidin. Aşağıdaki klasörler veya kapsayıcıları görmeniz gerekir:

    - Gördüğünüz bir **adfgerstarted/outputfolder** potansiyel bir parçası olarak çalıştırılan Hive betiği çıktısını içerir.

    - Gördüğünüz bir **adfhdidatafactory -\<bağlı-service-name >-\<zaman damgası >** kapsayıcı. Ardışık Düzen Çalıştır bir parçası oluşturulmuş Hdınsight kümesi varsayılan depolama konumunu kapsayıcıdır.

    - Gördüğünüz bir **adfjobs** Azure Data Factory işin kapsayıcı günlüğe kaydeder.  

        ![Azure Data Factory işlem hattı çıkış doğrulayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Azure Data Factory işlem hattı çıkış doğrulayın")


## <a name="clean-up-the-tutorial"></a>Öğreticiyi silme

İle üzerindeki-Hdınsight kümesi oluşturma deman, açıkça Hdınsight kümesini silmek gerekmez. Küme, ardışık düzen oluşturulurken sağlanan yapılandırmasını temel alarak silinir. Ancak, küme bile silindikten sonra kümeyle ilişkili depolama hesapları var olmaya devam. Böylece, verilerinizi sağlam tutabilirsiniz Bu davranış tasarım gereğidir. Ancak, veri kalıcı hale getirmek istemiyorsanız, oluşturduğunuz depolama hesabı silebilirsiniz.

Alternatif olarak, Bu öğretici için oluşturduğunuz tüm kaynak grubunu silebilirsiniz. Bu depolama hesabı ve oluşturduğunuz Azure Data Factory siler.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Seçin **kaynak grupları** sol bölmede.
3. PowerShell komut dosyasında oluşturulan kaynak grubu adı seçin. Listelenen çok fazla kaynak grupları varsa, filtrenin kullanın. Kaynak grubu açar.
4. Üzerinde **kaynakları** kutucuğu elinizde varsayılan depolama hesabı ve kaynak grubu diğer projelerle paylaşmak sürece listelenen veri fabrikası.
5. **Kaynak grubunu sil**'i seçin. Bunun yapılması, depolama hesabı ve depolama hesabında depolanan verileri siler.

    ![Kaynak grubunu silmek](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "kaynak grubu Sil")

6. Silme işlemini onaylayın ve ardından seçmek için kaynak grubu adı girin **silmek**.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Factory isteğe bağlı Hdınsight kümesi oluşturmak ve Hive işlerini çalıştırmak için nasıl kullanılacağı hakkında bilgi edindiniz. Hdınsight kümeleri ile özel yapılandırma oluşturma konusunda bilgi almak için sonraki artciel ilerleyin.

> [!div class="nextstepaction"]
>[Azure Hdınsight kümeleri ile özel yapılandırma oluşturma](hdinsight-hadoop-provision-linux-clusters.md)


