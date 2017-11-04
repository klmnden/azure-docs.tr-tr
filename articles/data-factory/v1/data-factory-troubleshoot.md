---
title: "Azure Data Factory sorunlarını giderme"
description: "Azure Data Factory kullanarak sorunları gidermek öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 8273647aa1cf8f7d35a9c645d44c64455e554cdb
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="troubleshoot-data-factory-issues"></a>Data Factory'de sorun giderme
> [!NOTE]
> Bu makale, genel olarak kullanılabilir (GA) olduğu Azure Data Factory, 1 sürümü için geçerlidir. 

Bu makalede Azure Data Factory kullanırken sorunlarıyla ilgili sorun giderme ipuçları sağlar. Bu makalede tüm olası sorunları hizmetini kullanırken listelenmez, ancak bazı sorunlar ve genel sorun giderme ipuçları kapsar.   

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Hata: Abonelik ‘Microsoft.DataFactory’ ad alanını kullanacak şekilde kaydedilmemiş
Bu hatayı aldıysanız Azure Data Factory kaynak sağlayıcısı makinenizde kayıtlı değildir. Şunları yapın:

1. Azure PowerShell’i çalıştırın.
2. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın.

    ```powershell
    Login-AzureRmAccount
    ```
3. Azure Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın.

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Sorun: bir Data Factory cmdlet çalıştırıldığında yetkisiz hata
Azure PowerShell ile doğru Azure hesabını veya aboneliğini kullanmıyor olabilirsiniz. Azure PowerShell ile kullanılacak doğru Azure hesabını ve aboneliğini seçmek için aşağıdaki cmdlet’leri kullanın.

1. Login-AzureRmAccount - kullanım doğru kullanıcı kimliği ve parolası
2. Get-AzureRmSubscription - hesap için tüm abonelikleri görüntüleyin.
3. Select-AzureRmSubscription &lt;abonelik adı&gt; -doğru abonelik seçin. Aynı Azure Portal'da data factory oluşturmak için kullandığınız kullanın.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Sorun: Azure Portalı'ndan veri yönetimi ağ geçidi Express Kurulumu başlatılamadı
Veri Yönetimi Ağ Geçidi Hızlı Kurulumu için Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı gerekir. Hızlı Kurulum başlatılamıyorsa, aşağıdakilerden birini yapın:

* Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı kullanın.

    Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.

    Aynı Firefox (yükleme eklenti) yapın. Araç çubuğundaki Menüyü Aç düğmesine tıklayın (sağ üst köşede yer alan üç yatay çizgi), Eklentiler’e tıklayın, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
* Kullanım **el ile Kurulum** Portalı'nda aynı dikey penceresinde gösterilen bağlantı. Yükleme dosyasını indirin ve el ile çalıştırmak için bu yaklaşımı kullanın. Yükleme başarılı olduktan sonra veri yönetimi ağ geçidi yapılandırması iletişim kutusu görebilirsiniz. Portal ekranındaki **anahtarı** kopyalayın ve ağ geçidini hizmete elle kaydetmek için yapılandırma yöneticisi içinde kullanın.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Sorun: şirket içi SQL Server'a bağlanmak başarısız
Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** kullanın ve ağ geçidi makinesi **sorun giderme** SQL Server ağ geçidi makineden bağlantıyı sınamak için sekmesi. Bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) ilgili sorunlar bağlantı/ağ geçidi sorun giderme ipuçları için.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Sorun: Durumu için herhangi bir zamanda bekleyen giriş dilimler olur
Dilimler olabilir **bekleyen** durum çeşitli nedenlerden dolayı. En yaygın nedenlerinden biri olan **dış** özelliği ayarlı değil **doğru**. Azure Data Factory kapsamı dışında üretilen herhangi bir veri kümesi ile işaretlenmelidir **dış** özelliği. Bu özellik, verilerin dış ve data factory içinde herhangi bir ardışık düzen tarafından yedeklenen olduğunu gösterir. İlgili depoda veriler kullanılabilir duruma geldiğinde veri dilimleri **Hazır** olarak işaretlenir.

**External** özelliğinin kullanımı ile ilgili olarak aşağıdaki örneğe bakın. İsteğe bağlı olarak belirtebilirsiniz **externalData*** dış true olarak ayarlandığında.

Bkz: [veri kümeleri](data-factory-create-datasets.md) bu özellik hakkında daha fazla ayrıntı için makale.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

Hatayı gidermek için girdi tablosunun JSON tanımına **external** özelliğini ve isteğe bağlı **externalData** bölümünü ekleyin ve tabloyu yeniden oluşturun.

### <a name="problem-hybrid-copy-operation-fails"></a>Sorun: Karma kopyalama işlemi başarısız
Bkz: [ağ geçidi sorunlarını giderme ](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) /şirket içi veri kopyalama ile sorunlarını giderme adımları için depolama veri yönetimi ağ geçidini kullanma.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Sorun: başarısız sağlama isteğe bağlı Hdınsight
Bağlı hizmet türü HDInsightOnDemand kullanırken, Azure Blob depolama alanına işaret eden bir linkedServiceName belirtmeniz gerekir. Data Factory hizmeti isteğe bağlı HDInsight kümeniz için günlükleri ve destekleyici dosyaları depolamak için bu depoyu kullanır.  Bazen aşağıdaki hatayla isteğe bağlı bir HDInsight kümesinin sağlanması başarısız olur:

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Bu hata genellikle linkedServiceName içinde belirtilen depolama hesabı konumunun HDInsight sağlama işleminin gerçekleştiği veri merkezi konumu ile aynı olmadığını gösterir. Örnek: veri fabrikanızın Batı ABD, Doğu ABD Azure depolama ise isteğe bağlı sağlama Batı ABD başarısız olur.

Buna ek olarak, isteğe bağlı HDInsight içinde ek depolama hesaplarının belirlenebileceği ikinci bir JSON özelliği (additionalLinkedServiceNames) bulunmaktadır. Bu ek bağlantılı depolama hesapları Hdınsight kümesi ile aynı konumda olmalıdır veya aynı hatasıyla başarısız oluyor.

### <a name="problem-custom-net-activity-fails"></a>Sorun: Özel .NET etkinliği başarısız
Bkz: [özel etkinliği ile işlem hattı Debug](data-factory-use-custom-activities.md#troubleshoot-failures) ayrıntılı adımlar için.

## <a name="use-azure-portal-to-troubleshoot"></a>Sorun giderme için Azure portalını kullanma
### <a name="using-portal-blades"></a>Portal dikey penceresi kullanılarak
Bkz: [işlem hattını izleme](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) adımlar için.

### <a name="using-monitor-and-manage-app"></a>İzleme ve Yönetme Uygulamasını kullanma
Bkz: [İzleyici ve data factory işlem hatlarını izleme ve yönetme uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md) Ayrıntılar için.

## <a name="use-azure-powershell-to-troubleshoot"></a>Sorun giderme için Azure PowerShell'i kullanma
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Bir hatayı gidermek için Azure PowerShell'i kullanma
Bkz: [İzleyicisi veri fabrikasında ardışık düzenleri Azure PowerShell kullanarak](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) Ayrıntılar için.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
