---
title: Azure Batch hizmeti işleri
titleSuffix: Azure Machine Learning Studio
description: Machine Learning Studio işleri için Azure Batch hizmetlerine genel bakış. Batch havuzu işleme toplu işleri gönderebilmek için havuzları oluşturmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18, previous-title='Dedicated capacity for batch execution service jobs - Azure Machine Learning Studio | Microsoft Docs'
ms.date: 04/19/2017
ms.openlocfilehash: 24efa3caba3918a38c09b1c921c600b117dedbc1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60751163"
---
# <a name="azure-batch-service-for-azure-machine-learning-studio-jobs"></a>Azure Machine Learning Studio işleri için Azure Batch hizmeti

Machine Learning Batch havuzu işleme, Azure Machine Learning Batch yürütme hizmeti için müşteri tarafından yönetilen ölçek sağlar. Machine learning eşzamanlı iş sayısını sınırlayan bir çok kiracılı ortamında, gerçekleşir için Klasik toplu işleme gönderebilir ve ilk-giren ilk çıkar temelinde işi kuyruğa alındı. Bu bir belirsizlik, doğru bir şekilde iş ne zaman çalışacağını tahmin edemezsiniz, anlamına gelir.

Batch havuzu işleme toplu işleri gönderebilmek için havuzları oluşturmanıza olanak sağlar. Havuz boyutunu denetlemek ve hangi havuzuna iş gönderilir. BES iş işleme tahmin edilebilir performans ve gönderdiğiniz işleme yükünü karşılık gelen kaynak havuzları oluşturma olanağı sağlayarak kendi işlem alanı çalıştırır.

> [!NOTE]
> Yeni bir kaynak yöneticisi olmalıdır. havuz oluşturmak için Machine Learning web hizmeti tabanlı. Oluşturulduktan sonra herhangi bir BES web hizmeti, her iki yeni Resource Manager tabanlı ve klasik, havuzda çalıştırabilirsiniz.

## <a name="how-to-use-batch-pool-processing"></a>Batch havuzundaki işlem kullanma

Azure portalı üzerinden yapılandırma Batch havuzu işleme şu anda kullanılamıyor. Batch havuzu işleme kullanmak için yapmanız gerekir:

-   Bir Batch havuzu hesabı oluşturmak ve bir havuz hizmet URL'sini ve bir yetkilendirme anahtarını elde etmek için CSS çağırın
-   Yeni Resource Manager tabanlı web hizmeti ve fatura planı oluşturma

Hesabınızı oluşturmak için Microsoft Müşteri Hizmetleri ve desteği (CSS) arayın ve abonelik kimliğinizi sağlayın CSS senaryonuz için uygun kapasitesini belirlemek için sizinle birlikte çalışır. CSS, ardından hesabınızı havuzlarını oluşturabileceğiniz en fazla sayısını ve her havuzda yerleştirebilirsiniz, sanal makineler (VM) sayısı ile yapılandırır. Hesabınız yapılandırıldıktan sonra havuzu hizmet URL'niz ve bir yetkilendirme anahtar sağlanır.

Hesabınızı oluşturduktan sonra Batch havuzu havuzu yönetim işlemlerini gerçekleştirmek için havuz hizmet URL'sini ve yetkilendirme anahtarını kullanın.

![Batch havuzu hizmet mimarisi.](./media/dedicated-capacity-for-bes-jobs/pool-architecture.png)

CSS size sağlanan havuzu hizmet URL'si havuzu oluşturma işlemi çağırarak, havuzları oluşturun. Bir havuz oluşturduğunuzda, VM'ler ve swagger.json, yeni bir kaynak yöneticisi URL'si sayısı Machine Learning web hizmeti tabanlı belirtin. Bu web hizmetini, fatura ilişki kurmak için sağlanır. Batch havuzu hizmet swagger.json havuzu bir fatura planı ile ilişkilendirmek için kullanır. Bir BES web hizmeti, her iki yeni Resource Manager tabanlı ve klasik, havuzda çalıştırabilirsiniz.

Herhangi bir yeni Resource Manager tabanlı web hizmeti kullanır, ancak işlerin faturalandırması ücretlendirilir, hizmetle ilişkili faturalandırma planı karşı unutmayın. Bir web hizmeti ve Batch havuzu işleri çalıştırmak için özellikle yeni faturalandırma planı oluşturmak isteyebilirsiniz.

Web hizmetleri oluşturma hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

Havuz oluşturulduktan sonra web hizmeti için toplu iş isteklerini URL'yi kullanarak BES iş gönderin. Bir havuz veya Klasik toplu işlem gönderme seçebilirsiniz. Toplu işlem havuzuna bir işi göndermek için iş gönderme istek gövdesi için aşağıdaki parametreyi ekleyin:

"AzureBatchPoolId": "&lt;kimliği havuz&gt;"

Parametresini eklemezseniz, iş Klasik toplu işlem ortamında çalıştırılır. Havuzu kullanılabilir kaynaklarınız varsa, iş çalıştırdıktan hemen başlar. Havuz ücretsiz kaynaklara sahip değilse bir kaynak kullanılabilir hale gelene kadar iş kuyruğa alınır.

Düzenli olarak, havuzlarınızı kapasitesini ulaşın ve daha fazla kapasiteye ihtiyaç bulursanız, CSS çağırın ve kotanızı artırmak için bir temsilcisi ile çalışma.

Örnek istek:

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

## <a name="considerations-when-using-batch-pool-processing"></a>Batch havuzu işleme kullanmayla ilgili konular

Batch havuzu işleme bir her zaman açık Faturalanabilir hizmetidir ve bir Resource Manager tabanlı faturalandırma planı ile ilişkilendirmenizi gerektirir. Yalnızca havuzu çalışan bilgi işlem saati sayısı için faturalandırılırsınız; işleri çalıştırma sırasında o zaman havuzu sayısı ne olursa olsun. Bir havuzu oluşturursanız, havuzu silinene kadar havuzda çalışan toplu iş olsa bile havuzdaki her bir sanal makinenin işlem saatleri için faturalandırılırsınız. Sanal makineler için faturalandırma sağlama işiniz bittiğinde başlar ve silinmiş sona erer. Bulunan planlardan herhangi birini kullanabilirsiniz [Machine Learning fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/machine-learning/).

Faturalandırma örnek:

2 sanal makine ile bir Batch havuzu oluşturma ve bu 24 saat sonra silerseniz, faturalandırma planınızı 48 işlem saatleri düşülür; kaç tane işin ne olursa olsun, bu süre içinde çalıştırılamadı.

4 sanal makine ile bir Batch havuzu oluşturma ve 12 saat sonra silin, faturalandırma planınızı borç kaydedilen 48 işlem saatleri de olur.

İşleri tamamlandığında belirlemek için iş durumunu yoklamak öneririz. Çalışan tüm işleri tamamladığınızda, sıfıra havuzundaki sanal makinelerin sayısını ayarlamak için havuz yeniden boyutlandırma işlemi çağırın. Çalışan tüm işler tamamladıktan sonra havuzu kaynaklardaki kısa ve örneğin farklı bir fatura planı karşı faturalandırmak yeni bir havuz oluşturmak ihtiyacınız varsa havuzu yerine silebilirsiniz.


| **İşlemesini Batch havuzu kullanma**    | **İşleme zamanı Klasik Batch'i kullanma**  |
|---|---|
|Çok sayıda işlerini çalıştırmanız gerekir<br>Veya<br/>İşleriniz hemen çalışacağını bilmeniz gerekir<br/>Veya<br/>Garantili aktarım hızı gerekir. Örneğin, belirli bir zaman dilimi içinde çalıştırılan bir iş sayısı ve ihtiyaçlarınızı karşılamak üzere işlem kaynaklarınızı ölçeklendirmek istediğinizden gerekir.    | Yalnızca birkaç iş çalışıyor<br/>Ve<br/> İşleri hemen çalıştırmanız gerekmez. |
