---
title: Azure güvenlik duvarı hizmet etiketleri genel bakış
description: Bu makalede, Azure güvenlik duvarı hizmet etiketleri bir genel bakıştır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 2/25/2019
ms.author: victorh
ms.openlocfilehash: 1d03d896de947fcc938619c52a3690962a0d2d6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193739"
---
# <a name="azure-firewall-service-tags"></a>Azure güvenlik duvarı hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

Azure güvenlik duvarı hizmet etiketleri ağ kuralları hedef alanında kullanılabilir. Bunları belirli IP adreslerinin yerine kullanabilirsiniz.

## <a name="supported-service-tags"></a>Hizmet etiketleri desteklenir

Azure güvenlik duvarı ağ kuralları aşağıdaki hizmet etiketlerinden kullanılabilir:

* **AzureCloud** (yalnızca Resource Manager): Bu etiket, tüm dahil olmak üzere Azure için IP adresi alanını belirtir [veri merkezi genel IP adresleri](https://www.microsoft.com/download/details.aspx?id=41653). *AzureCloud* değerini belirtirseniz Azure genel IP adreslerine giden trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureCloud erişimine izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure AzureCloud hizmetine erişim izni vermek istiyorsanız *AzureCloud.EastUS* hizmet etiketini kullanabilirsiniz. 
* **AzureTrafficManager** (yalnızca Resource Manager): Bu etiket Azure Traffic Manager araştırma IP adresleri için IP adresi alanını belirtir. Traffic Manager yoklama IP adresleri hakkında daha fazla bilgi için [Azure Traffic Manager hakkında SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs) sayfasına göz atın. 
* **Depolama** (yalnızca Resource Manager): Bu etiket Azure Storage hizmeti için IP adresi alanını belirtir. *Depolama* değerini belirtirseniz depolama alanına gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions) depolama alanına erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure Depolama hizmetine erişim izni vermek istiyorsanız *Storage.EastUS* hizmet etiketini kullanabilirsiniz. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder.
* **SQL** (yalnızca Resource Manager): Bu etiket Azure SQL veritabanı ve Azure SQL veri ambarı hizmetlerinin adres ön eklerini belirtir. *Sql* değerini belirtirseniz Sql’e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions) Sql’e erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure SQL Veritabanı hizmetine erişim izni vermek istiyorsanız *Sql.EastUS* hizmet etiketini kullanabilirsiniz. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder.
* **AzureCosmosDB** (yalnızca Resource Manager): Bu etiket, Azure Cosmos veritabanı hizmetin adres ön eklerini belirtir. *AzureCosmosDB* değerini belirtirseniz AzureCosmosDB’ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir AzureCosmosDB erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), AzureCosmosDB biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı].
* **AzureKeyVault** (yalnızca Resource Manager): Bu etiket Azure KeyVault hizmetin adres ön eklerini belirtir. *AzureKeyVault* değerini belirtirseniz AzureKeyVault’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir AzureKeyVault erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), AzureKeyVault biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı].
* **EventHub** (yalnızca Resource Manager): Bu etiket, Azure Event Hubs'a hizmetin adres ön eklerini belirtir. *EventHub* değerini belirtirseniz EventHub’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir EventHub erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), EventHub biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı]. 
* **ServiceBus** (yalnızca Resource Manager): Bu etiket Azure ServiceBus hizmetin adres ön eklerini belirtir. *ServiceBus* değerini belirtirseniz ServiceBus'a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir ServiceBus erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), ServiceBus biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı].
* **MicrosoftContainerRegistry** (yalnızca Resource Manager): Bu etiket, Microsoft kapsayıcı kayıt defteri hizmetin adres ön eklerini belirtir. *MicrosoftContainerRegistry* değerini belirtirseniz MicrosoftContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir MicrosoftContainerRegistry erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), MicrosoftContainerRegistry biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı].
* **AzureContainerRegistry** (yalnızca Resource Manager): Bu etiket, Azure Container Registry hizmetin adres ön eklerini belirtir. *AzureContainerRegistry* değerini belirtirseniz AzureContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir AzureContainerRegistry erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), AzureContainerRegistry biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı]. 
* **AppService** (yalnızca Resource Manager): Bu etiket Azure AppService hizmetin adres ön eklerini belirtir. *AppService* değerini belirtirseniz AppService'e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir AppService erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), AppService biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı]. 
* **AppServiceManagement** (yalnızca Resource Manager): Bu etiket Azure AppService yönetim hizmetin adres ön eklerini belirtir. *AppServiceManagement* değerini belirtirseniz AppServiceManagement'a gelen trafiğe izin verilir veya trafik reddedilir. 
* **ApiManagement** (yalnızca Resource Manager): Bu etiket Azure API Management hizmetinin adres ön eklerini belirtir. *ApiManagement* değerini belirtirseniz ApiManagement'a gelen trafiğe izin verilir veya trafik reddedilir.  
* **AzureConnectors** (yalnızca Resource Manager): Bu etiket Azure bağlayıcılar hizmetin adres ön eklerini belirtir. *AzureConnectors* değerini belirtirseniz AzureConnectors’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir AzureConnectors erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/regions), AzureConnectors biçimini kullanarak bölgeyi belirtebilirsiniz. [ bölge adı].
* **AzureDataLake** (yalnızca Resource Manager): Bu etiket, Azure Data Lake hizmetin adres ön eklerini belirtir. *AzureDataLake* değerini belirtirseniz AzureDataLake’e gelen trafiğe izin verilir veya trafik reddedilir.
* **AzureActiveDirectory** (yalnızca Resource Manager): Bu etiket AzureActiveDirectory hizmetin adres ön eklerini belirtir. *AzureActiveDirectory* değerini belirtirseniz AzureActiveDirectory’ye gelen trafiğe izin verilir veya trafik reddedilir.
* **AzureMonitor** (yalnızca Resource Manager): Bu etiket AzureMonitor hizmetin adres ön eklerini belirtir. Belirtirseniz *AzureMonitor* değeri için trafiğe izin veya trafik için AzureMonitor reddedilir.
* **ServiceFabric** (yalnızca Resource Manager): Bu etiket ServiceFabric hizmetin adres ön eklerini belirtir. Belirtirseniz *ServiceFabric* değeri için trafiğe izin veya trafik için ServiceFabric reddedilir.
* **AzureMachineLearning** (yalnızca Resource Manager): Bu etiket AzureMachineLearning hizmetin adres ön eklerini belirtir. Belirtirseniz *AzureMachineLearning* değeri için trafiğe izin veya trafik için AzureMachineLearning reddedilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz: [işleme mantığı Azure güvenlik duvarı kuralı](rule-processing.md).