---
title: "Machine Learning toplu yürütme hizmeti işleri için kapasite ayrılmış | Microsoft Docs"
description: "Machine Learning işleri için Azure Batch hizmetlerine genel bakış."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl
ms.openlocfilehash: 4a4c5e6bf44fb4774d9ba501479383d6c7d3b128
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a>Machine Learning işleri için Azure Batch hizmeti

Machine Learning Batch havuzundaki işlem Azure Machine Learning toplu yürütme hizmeti için müşteri tarafından yönetilen ölçek sağlar. Machine learning eşzamanlı iş sayısını sınırlayan bir çok kiracılı ortamında, gerçekleşir için Klasik toplu işleme gönderebilir ve işler ilk olarak ilk çıkar temelinde kuyruğa alınır. Bu belirsizlik, doğru şekilde işinizi ne zaman çalışacağını tahmin edilemez olduğunu anlamına gelir.

Batch havuzu işleme toplu işleri gönderebilmek için havuzları oluşturmanıza olanak sağlar. Havuz boyutunu denetlemek ve hangi havuz iş gönderildiğinde. BES işinizi tahmin edilebilir işlem performansı ve gönderdiğiniz işleme yükünü karşılık gelen kaynak havuzları oluşturmanıza olanak sağlayan kendi işleme alan çalıştırır.

## <a name="how-to-use-batch-pool-processing"></a>Batch havuzundaki işlem kullanma

Azure portalı üzerinden yapılandırma havuzu toplu işlem şu anda kullanılamıyor. Batch havuzundaki işlem kullanmak için şunları yapmalısınız:

-   Batch havuzu hesabı oluşturmak ve bir havuz hizmet URL'sini ve bir yetkilendirme anahtarını elde etmek için CSS çağırın
-   Yeni kaynak tabanlı Manager web hizmeti ve fatura planı oluşturma

Hesabınızı oluşturmak için Microsoft Müşteri Hizmetleri ve desteği (CSS) arayın ve abonelik kimliğinizi sağlayın CSS senaryonuz için uygun kapasitesini belirlemek için sizinle birlikte çalışır. CSS sonra hesabınızı maksimum sayısı havuzu oluşturabilirsiniz ve her havuzda yerleştirebilirsiniz sanal makineler (VM'ler) üst sınırını yapılandırır. Hesabınızı yapılandırıldıktan sonra havuzu hizmet URL'niz ve bir yetkilendirme anahtarı sağlanır.

Hesabınızı oluşturduktan sonra Batch havuzundaki havuzu yönetim işlemlerini gerçekleştirmek için havuz hizmet URL'sini ve yetkilendirme tuşunu kullanın.

![Batch havuzu hizmet mimarisi.](./media/dedicated-capacity-for-bes-jobs/pool-architecture.png)

CSS size sağlanan havuz hizmet URL'si havuzu oluştur işlemi çağırarak havuzları oluşturun. Bir havuz oluşturduğunuzda, Machine Learning web hizmeti VM'ler ve Yeni Kaynak Yöneticisi'nin swagger.json URL'sini sayısına dayalı belirtin. Bu web Hizmeti'nin, fatura ilişkisi kurmak için sağlanır. Batch havuzu hizmeti swagger.json havuzunu bir faturalandırma planı ile ilişkilendirmek için kullanır. Web hizmeti, iki yeni Resource Manager temelli herhangi BES çalıştırabilir ve klasik, havuzu seçin.

Tüm yeni Resource Manager temelli web hizmetini kullanır, ancak işleri için fatura ücretlendirilirsiniz, hizmetle ilişkili faturalandırma planı karşı unutmayın. Bir web hizmeti ve özellikle Batch havuzu işlerini çalıştırmak için yeni fatura planı oluşturmak isteyebilirsiniz.

Web hizmetleri oluşturma hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

Bir havuzu oluşturduktan sonra toplu iş isteklerini URL için web hizmetini kullanarak BES işi gönderin. Bir havuz veya Klasik toplu işleme göndermek seçebilirsiniz. Toplu işlem havuzu için bir işi göndermek için iş gönderme istek gövdesi aşağıdaki parametresini ekleyin:

"AzureBatchPoolId":"&lt;pool ID&gt;"

Parametresini eklemezseniz iş Klasik toplu işlem ortamında çalıştırılır. Kullanılabilir kaynak havuzu varsa, iş çalıştırdıktan hemen başlar. Havuz kaynakları serbest bırakmak yoksa, bir kaynak kullanılabilir hale gelene kadar işinizi sıraya alındı.

Düzenli olarak, havuzlarınızı kapasitesini ulaşmak ve artan kapasite gerektiğini fark ederseniz, CSS çağırın ve, kotalar artırmak için bir temsilcisi ile çalışır.

Örnek isteği:

https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a>Batch havuzundaki işlem kullanmayla ilgili konular

Havuz toplu işleme bir her zaman açık Faturalanabilir hizmetidir ve bir Resource Manager temelli fatura planı ile ilişkilendirmenizi gerektirir. Havuz çalışan saat tutarında işlem için yalnızca faturalandırılır; o zaman havuzunu sırasında çalıştırmak işlerin sayısı ne olursa olsun. Bir havuzu oluşturursanız, havuzu silinene kadar toplu iş havuzda çalıştırıyor olsanız bile, her bir sanal makine havuzundaki işlem saatlik için faturalandırılır. Sanal makineler için fatura sağlama bittiğinde, başlatır ve silinmiş zaman durdurur. Bulunan planları birini kullanabilirsiniz [Machine Learning fiyatlandırması sayfa](https://azure.microsoft.com/pricing/details/machine-learning/).

Faturalama örnek:

2 sanal makinelerle Batch havuzu oluşturma ve 24 saat sonra silerseniz, faturalandırma planınıza 48 işlem saatleri düşülür; kaç tane işleri bağımsız olarak, bu süre boyunca çalıştırılmadı.

4 sanal makinelerle Batch havuzu oluşturma ve 12 saat sonra silerseniz, faturalandırma planınıza ayrıca borç kaydedilen 48 işlem saattir.

İşlerini tamamlandığında belirlemek için işin durumunu yoklama öneririz. Çalışan tüm işleri tamamladığınızda, sanal makine sayısı sıfır havuzuna ayarlamak için havuzu yeniden boyutlandırma işlemi çağırın. Çalışan tüm işleri tamamladığınızda havuzu kaynaklardaki kısa ve örneğin farklı bir fatura planı karşı faturalandırmak yeni bir havuz oluşturmak ihtiyacınız varsa havuzu yerine silebilirsiniz.


| **Batch havuzundaki işlemesini kullanın**    | **Klasik toplu ne zaman işleme kullanın**  |
|---|---|
|Çok sayıda işlerini çalıştırmanız gerekir<br>Veya<br/>İşleriniz hemen çalışacağını bilmeniz gerekir<br/>Veya<br/>Garantili işleme gerekir. Örneğin, belirli bir zaman dilimi içinde iş sayısı çalıştırın ve ihtiyaçlarınızı karşılamak için işlem kaynaklarınızı ölçeklendirmek istediğiniz gerekir.    | Yalnızca birkaç işleri çalıştırma<br/>Ve<br/> İşlerin hemen çalıştırmanız gerekmez |
