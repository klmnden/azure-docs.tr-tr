---
title: Azure Data Factory'de sorun giderme
description: Azure Data Factory kullanarak sorunlarını giderme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
author: gauravmalhot
ms.author: gamal
ms.reviewer: maghan
manager: craigg
robots: noindex
ms.openlocfilehash: cc880885777cbca67d6fb39b90feadc889339f76
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836169"
---
# <a name="troubleshoot-data-factory-issues"></a>Data Factory'de sorun giderme
> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. 

Bu makalede Azure Data Factory kullanırken sorunları için sorun giderme ipuçları sağlar. Bu makalede hizmet kullanırken olası tüm sorunları listelenmez, ancak bazı sorunlar ve sorun giderme hakkında genel ipuçları kapsar.   

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Hata: Abonelik 'Microsoft.DataFactory' ad alanını kullanacak şekilde kaydedilmemiş
Bu hatayı aldıysanız Azure Data Factory kaynak sağlayıcısı makinenizde kayıtlı değildir. Şunları yapın:

1. Azure PowerShell’i çalıştırın.
2. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```
3. Azure Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın.

    ```powershell        
    Register-AzResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Sorun: Bir Data Factory cmdlet çalıştırırken yetkilendirilmemiş hatası
Azure PowerShell ile doğru Azure hesabını veya aboneliğini kullanmıyor olabilirsiniz. Azure PowerShell ile kullanılacak doğru Azure hesabını ve aboneliğini seçmek için aşağıdaki cmdlet’leri kullanın.

1. Bağlanma-AzAccount - doğru kullanıcı Kimliğini kullanın ve parola
2. Get-AzSubscription - hesap için tüm abonelikleri görüntüleyin.
3. Select-AzSubscription &lt;abonelik adı&gt; -doğru aboneliği seçin. Azure Portal'da veri fabrikası oluşturmak için kullandığınız aynı kullanın.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Sorun: Azure Portalı'ndan veri yönetimi ağ geçidi hızlı kurulumunu başlatılamadı
Veri Yönetimi Ağ Geçidi Hızlı Kurulumu için Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı gerekir. Hızlı Kurulum başlatılamıyorsa, aşağıdakilerden birini yapın:

* Internet Explorer veya Microsoft ClickOnce uyumlu web tarayıcısı kullanın.

    Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.

    Aynı Firefox (eklenti yükleme) yapın. Araç çubuğundaki Menüyü Aç düğmesine tıklayın (sağ üst köşede yer alan üç yatay çizgi), Eklentiler’e tıklayın, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
* Kullanım **el ile Kurulum** portaldaki aynı dikey pencerede gösterilen bağlantı. Yükleme dosyasını indirin ve el ile çalıştırmak için bu yaklaşımı kullanın. Yükleme başarılı olduktan sonra veri yönetimi ağ geçidi yapılandırma iletişim kutusunu görürsünüz. Portal ekranındaki **anahtarı** kopyalayın ve ağ geçidini hizmete elle kaydetmek için yapılandırma yöneticisi içinde kullanın.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Sorun: Şirket içi SQL Server'a bağlanma başarısız
Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** kullanın ve ağ geçidi makinesi üzerinde **sorun giderme** bağlantısını için SQL Server ağ geçidi makinesinden test için sekmesinde. Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Sorun: Giriş dilimleri bekleme durumundan çıkmıyor
Dilimler olabilir **bekleyen** durum çeşitli nedenlerden dolayı. Sık karşılaşılan nedenlerden biri olan **dış** özelliği ayarlanmamış **true**. Azure Data Factory kapsamı dışında oluşturulan herhangi bir veri kümesi ile işaretlenmelidir **dış** özelliği. Bu özellik, verilerin dış ve veri fabrikası içinde herhangi bir işlem hattı tarafından desteklenmediğini olduğunu gösterir. İlgili depoda veriler kullanılabilir duruma geldiğinde veri dilimleri **Hazır** olarak işaretlenir.

**External** özelliğinin kullanımı ile ilgili olarak aşağıdaki örneğe bakın. İsteğe bağlı olarak belirtebilirsiniz **externalData*** dış true olarak ayarlandığında.

Bu özellikle ilgili daha ayrıntılı bilgi için [Veri kümeleri](data-factory-create-datasets.md) makalesine bakın.

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
Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) gönderip buralardan şirket içi verileri kopyalama sorunlarını giderme adımları için depolama veri yönetimi ağ geçidi kullanma.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Sorun: İsteğe bağlı HDInsight sağlama başarısız
Hdınsightondemand türünde bağlantılı bir hizmet kullanırken bir Azure Blob depolama alanına işaret eden linkedServiceName belirlemeniz gerekir. Data Factory hizmeti isteğe bağlı HDInsight kümeniz için günlükleri ve destekleyici dosyaları depolamak için bu depoyu kullanır.  Bazen aşağıdaki hatayla isteğe bağlı bir HDInsight kümesinin sağlanması başarısız olur:

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Bu hata genellikle linkedServiceName içinde belirtilen depolama hesabı konumunun HDInsight sağlama işleminin gerçekleştiği veri merkezi konumu ile aynı olmadığını gösterir. Örnek: veri fabrikanıza Batı ABD ve Doğu ABD bölgesinde Azure depolama ise isteğe bağlı sağlama Batı ABD'de başarısız olur.

Buna ek olarak, isteğe bağlı HDInsight içinde ek depolama hesaplarının belirlenebileceği ikinci bir JSON özelliği (additionalLinkedServiceNames) bulunmaktadır. Bu ek bağlantılı depolama hesapları HDInsight kümesi ile aynı konumda olmalıdır veya aynı hata ile başarısız oluyor.

### <a name="problem-custom-net-activity-fails"></a>Sorun: Özel .NET etkinliği başarısız
Bkz: [özel etkinliği ile işlem hattı hata ayıklama](data-factory-use-custom-activities.md#troubleshoot-failures) ayrıntılı adımlar için.

## <a name="use-azure-portal-to-troubleshoot"></a>Sorun gidermek için Azure portalını kullanma
### <a name="using-portal-blades"></a>Portal dikey penceresi kullanılarak
Bkz: [işlem hattını izleme](data-factory-monitor-manage-pipelines.md) adımlar.

### <a name="using-monitor-and-manage-app"></a>İzleme ve Yönetme Uygulamasını kullanma
Bkz [İzleyicisi ve izleme ve yönetme uygulaması'nı kullanarak data factory işlem hatlarını yönetmek](data-factory-monitor-manage-app.md) Ayrıntılar için.

## <a name="use-azure-powershell-to-troubleshoot"></a>Sorun gidermek için Azure PowerShell kullanma
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Bir hatayı gidermek için Azure PowerShell'i kullanma
Bkz: [İzleyici Data Factory işlem hatları Azure PowerShell kullanarak](data-factory-monitor-manage-pipelines.md) Ayrıntılar için.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: https://go.microsoft.com/fwlink/?LinkId=516971

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
