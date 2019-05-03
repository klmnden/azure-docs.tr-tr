---
title: Açık veri kümeleri nelerdir? Seçkin ortak veri kümeleri
titleSuffix: Azure Open Datasets (preview)
description: Azure açık veri kümeleri (Önizleme), machine learning ve analytics çözümleri kullanıma hazır olan seçkin veri kümeleri ortak etki alanından hakkında bilgi edinin. Veri kümeleri, hava durumu, görselleştirmenizdeki, tatiller ve Tahmine dayalı çözüm zenginleştirin yardımcı olması için konum gibi genel verileri içerir.
ms.topic: overview
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: 439c25363d4c3b24b391b49811d3806c98171034
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026952"
---
# <a name="what-are-azure-open-datasets-preview-and-how-can-you-use-them"></a>Azure açık veri kümeleri (Önizleme) nedir ve bunları nasıl kullanabileceğiniz?

[Azure açık veri kümeleri](https://azure.microsoft.com/services/open-datasets/) seçkin daha doğru modeller için makine öğrenimi çözümleri senaryoya özgü özellikler eklemek için kullanabileceğiniz ortak veri kümelerinin olduğunu. Açık veri kümeleri, Microsoft Azure bulutta bulunan ve Azure Databricks, Machine Learning hizmeti ve Machine Learning Studio kullanıma hazırdır. Ayrıca, veri kümeleri API'leri aracılığıyla erişmesini ve bunları Power BI ve Azure Data Factory gibi diğer ürünleri de kullanabilirsiniz.

Genel etki alanı veri hava durumu, görselleştirmenizdeki, tatil, kamu güvenliği için veri kümeleri içerir ve yardımcı konumu makine öğrenimi modellerini eğitmenize ve Tahmine dayalı çözüm zenginleştirin. Ayrıca, açık veri kümeleri Azure üzerinde genel veri kümelerinizi paylaşabilirsiniz. 

![Azure açık veri kümeleri bileşenleri](./media/overview-what-are-open-datasets/open-datasets-components.png)

## <a name="curated-prepared-datasets"></a>Seçkin, hazır veri kümeleri
Seçkin açık genel veri kümeleri Azure kümelerindeki açın, machine learning iş akışlarında tüketim için iyileştirilmiştir. 

Veri bilimcileri, temizleme ve Gelişmiş analiz için verileri hazırlama zamanlarının çoğunu genellikle ayırın. Açık veri kümeleri Azure bulutuna kopyalanır ve size zaman kazandırmak için önceden işlenmiş. Düzenli aralıklarla veri kaynaklarından gibi bir FTP bağlantısı tarafından Ulusal Okyanus ve Atmosfer Yönetimi (NOAA), yapılandırılmış bir biçime ayrıştırılmış çekilen ve ardından posta kodu veya konumu gibi özellikler ile uygun şekilde zenginleştirilmiş en yakın hava durumu istasyonu.

Veri kümeleri, Azure yapmayı erişim ve daha kolay işleme ile bulut işlem cohosted.  

Veri kümeleri kullanılabilir örnekleri aşağıda verilmiştir. 

### <a name="weather-data"></a>Hava durumu verileri
 
|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[Tümleşik NOAA yüzey veri (ISD)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureDatabricks) | Dünya çapında saatlik hava durumu verileri NOAA Kuzey Amerika, Avrupa, Avustralya ve Asya bölümleri içinde uzamsal en iyi kapsamına sahip. Her gün güncelleştirilir. |
|[NOAA genel sistem (GFS) tahmin](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureDatabricks) | 15 günlük ABD saatlik hava durumu tahminini verilerden NOAA. Her gün güncelleştirilir. |

### <a name="calendar-data"></a>Takvim verileri

|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[Genel tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureDatabricks) | 41 ülkeler ya da 1970 bölgelerden 2099 için kapsayan, dünya çapında genel tatil verileri. Ülke ve çoğu kişi olup olmadığı zaman ödemesi içerir. |

## <a name="access-to-datasets"></a>Veri kümelerine erişim  
Bir Azure hesabıyla erişebilirsiniz kod kullanarak veri kümelerini açın veya arabirimi Azure hizmet. Azure bulut işlem kaynaklarının kullanılmak üzere makine öğrenimi çözüm ile birlikte verilerdir.  

Açık veri kümeleri, verileri Azure Databricks ve Azure Machine Learning hizmeti ile bağlanmak için kullanabileceğiniz Azure not defterleri ve Azure Databricks not defterleri sağlar. Veri kümeleri, bir Python SDK'sı da erişilebilir. 

Bununla birlikte, açık veri kümelerine erişim için bir Azure hesabınızın gerek yoktur; herhangi bir Python ortamı olmadan veya Spark olmadan erişim olabilirler.

## <a name="request-or-contribute-datasets"></a>İstek veya katkıda bulunan veri kümeleri

Verileri bulamazsanız, bize e-posta istediğiniz [bir veri kümesi istek](mailto:aod@microsoft.com?Subject=Contribute%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A) veya [bir veri kümesi katkıda](mailto:aod@microsoft.com?Subject=Request%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A). 

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek Not Defteri](samples.md)
* [Öğretici: Regresyon NY ile taksi verileri modelleme](tutorial-opendatasets-automl.md)
* [Açık veri kümeleri için Python SDK](https://aka.ms/open-datasets-api)
