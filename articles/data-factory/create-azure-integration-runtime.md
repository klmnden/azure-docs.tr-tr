---
title: Azure Data Factory'de Azure tümleştirme çalışma zamanı oluşturma | Microsoft Docs
description: Azure Integration runtime, Azure veri dönüştürme etkinliklerini gönderme ve veri kopyalama için kullanılan Factory'de oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 4b166ded3dcef4a89951eb81f7f1b321f89a0e67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66153409"
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Oluşturma ve Azure tümleştirme çalışma zamanını yapılandırma
Integration Runtime (IR) Azure Data Factory tarafından farklı ağ ortamları veri tümleştirme özellikleri sağlamak için kullanılan işlem altyapısıdır. IR hakkında daha fazla bilgi için bkz: [Integration runtime](concepts-integration-runtime.md).

Azure IR işlem HDInsight gibi hizmetleri için veri taşıma ve dağıtım veri dönüştürme etkinlikleri yerel olarak gerçekleştirilecek tam olarak yönetilen bir işlem sağlar. Bu, Azure ortamında barındırılan ve genel erişime açık Uç noktalara sahip genel ağ ortamında kaynaklarına bağlanmayı destekler.

Bu belge, nasıl oluşturabilir ve Azure Integration Runtime yapılandırma tanıtır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-azure-ir"></a>Varsayılan Azure IR
Varsayılan olarak, her veri fabrikasının bulut veri depoları ve işlem hizmetlerini ortak ağ işlemleri destekler arka Azure IR vardır. Bu Azure IR konumunu otomatik çözümleme ' dir. Varsa **connectVia** Özelliği Azure IR kullanılır. varsayılan bağlı hizmet tanımında belirtilmedi. Azure IR açıkça IR konumunu tanımlamak istediğiniz zaman ya da Yönetim amaçla farklı IRS üzerinde Etkinlik yürütme neredeyse Grup istiyorsanız açıkça oluşturmanız yeterlidir. 

## <a name="create-azure-ir"></a>Azure IR oluşturma
Tümleştirme çalışma zamanı kullanılarak oluşturulabilir **kümesi AzDataFactoryV2IntegrationRuntime** PowerShell cmdlet'i. Azure IR oluşturmak için adını, konumunu ve komut türü belirtin. Azure IR konumunu ayarla "Batı Avrupa'ya" oluşturmak için bir örnek komutu aşağıda verilmiştir:

```powershell
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Azure IR için türü ayarlanmalıdır **yönetilen**. Tam olarak esnek bir şekilde bulutta yönetilen işlem ayrıntılarını belirtmeniz gerekmez. İşlem belirtin, Azure-SSIS IR oluşturmak istediğiniz düğüm boyutunu ve düğüm sayısı gibi ayrıntıları Daha fazla bilgi için [oluşturma ve yapılandırma Azure-SSIS IR](create-azure-ssis-integration-runtime.md).

Set-AzDataFactoryV2IntegrationRuntime PowerShell cmdlet'ini kullanarak konumunu değiştirmek için mevcut Azure IR yapılandırabilirsiniz. Azure IR konumu hakkında daha fazla bilgi için bkz: [tümleştirme çalışma zamanı giriş](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Azure IR kullanın

Azure IR oluşturulduktan sonra bağlı hizmet tanımında başvuruda bulunabilir. Aşağıda Azure tümleştirme çalışma zamanının nasıl başvurabileceğiniz bir örnek yukarıda bir Azure depolama bağlı hizmet oluşturulur:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=...",
          "type": "SecureString"
        }
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
Tümleştirme çalışma zamanları diğer türleri oluşturma konusunda aşağıdaki makalelere bakın:

- [Kendinden konak tümleştirme çalışma zamanı oluşturma](create-self-hosted-integration-runtime.md)
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)
 
