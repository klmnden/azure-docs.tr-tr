---
title: Azure İlkesi'nde güvenlik ilkelerini düzenleme | Microsoft Docs
description: Azure İlkesi'nde güvenlik ilkeleri düzenleyin.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: rkarlin
ms.openlocfilehash: d6cc216f71efcd3b3973cd37349dd5145237f02f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839339"
---
# <a name="edit-security-policies-in-azure-policy"></a>Azure İlkesi'nde güvenlik ilkelerini düzenleme
Güvenlik Merkezi güvenlik ilkeleri ve yüklerinize nasıl uygulanacağını durumunu görüntülemenize yardımcı olur. Azure Güvenlik Merkezi otomatik olarak atar, [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md) eklendikten her abonelikte. Bunları yapılandırabilirsiniz [Azure İlkesi](../azure-policy/azure-policy-introduction.md), ya da Yönetim grupları arasında ve birden çok aboneliğe ilkeleri ayarlamanıza olanak sağlayan REST API'sini kullanarak. Daha fazla bilgi için bkz. [Azure İlkesi ile Güvenlik Merkezi güvenlik ilkelerini tümleştirme](security-center-azure-policy.md). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * REST API kullanarak bir güvenlik ilkesi yapılandırma
> * Kaynaklarınızın güvenliğini değerlendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ele alınan özellikleri adım adım görmek için Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Ücretsiz olarak Güvenlik Merkezi standart deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

## <a name="configure-a-security-policy-using-the-rest-api"></a>REST API kullanarak bir güvenlik ilkesi yapılandırma

Azure İlkesi ile yerel tümleştirme bir parçası olarak, Azure Güvenlik Merkezi ilke ataması oluşturmak için avantajı Azure İlkesi'nin REST API yararlanmanıza olanak sağlar. Aşağıdaki yönergeler, ilke atamaları oluşturulmasını yanı sıra mevcut atamaları özelleştirmesini yol. 

Azure İlkesi önemli Kavramları: 

- A **ilke tanımı** bir kuralı 

- Bir **girişim** ilke tanımları (kuralları) oluşan bir koleksiyondur 

- Bir **atama** girişim veya bir ilke bir uygulama belirli bir kapsama (Yönetim grubu, abonelik, vb.) 

Güvenlik Merkezi, güvenlik ilkelerini içeren yerleşik bir girişim sahiptir. Azure kaynaklarınızın Güvenlik Merkezi'nin ilkelerini değerlendirmek için yönetim grubu veya abonelik değerlendirmek istediğiniz atama oluşturmanız gerekir.  

Yerleşik girişim varsayılan olarak etkin Güvenlik Merkezi'nin ilkeler vardır. Yerleşik girişim belirli ilkelerden devre dışı bırakmayı seçebilirsiniz, örneğin Güvenlik Merkezi'nin ilkeler uygulayabilirsiniz **web uygulaması güvenlik duvarı**, ilkenin etkisi parametresinin değerini değiştirerek için **Devre dışı bırakılmış**. 

### <a name="api-examples"></a>API örnekleri

Aşağıdaki örneklerde, bu değişkenleri değiştirin:

- **{} kapsamı**  yönetim grubu adını girin veya abonelik ilkeyi uygulamak.
- **{poicyAssignmentName}**  girin [ilgili ilke ataması adı](#policy-names).
- **{name}**  adınızı veya ilke değişikliği onaylayan yöneticisinin adını girin.

Bu örnek, bir abonelik veya yönetim grubu yerleşik Güvenlik Merkezi girişimine atama gösterir
 
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 

Bu örnekte, yerleşik Güvenlik Merkezi girişimine aboneliği devre dışı aşağıdaki ilkeleriyle atama gösterir: 

- Sistem güncelleştirmeleri ("systemUpdatesMonitoringEffect") 

- Güvenlik yapılandırmalarını ("systemConfigurationsMonitoringEffect") 

- Uç nokta Koruması ("endpointProtectionMonitoringEffect") 

 
      PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

      Request Body (JSON) 

      { 

        "properties":{ 

      "displayName":"Enable Monitoring in Azure Security Center", 

      "metadata":{ 

      "assignedBy":"{Name}" 

      }, 

      "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

      "parameters":{ 

      "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 

      "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 

      "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 

      }, 

       } 

      } 

Bu örnek, bir atamayı silmeyi işlemini göstermektedir:

    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 


## İlke adları Başvurusu <a name="policy-names"></a>

|Güvenlik Merkezi'ndeki ilke adı|Azure İlkesi'nde görüntülenen bir ilke adı |İlke etkisi parametresi adı|
|----|----|----|
|SQL Şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle |sqlEncryptionMonitoringEffect| 
|SQL Denetimi |Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme |sqlAuditingMonitoringEffect|
|Sistem güncelleştirmeleri |Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme |systemUpdatesMonitoringEffect|
|Depolama şifrelemesi |Depolama hesapları için eksik blob şifrelemesini denetler |storageEncryptionMonitoringEffect|
|JIT ağ erişimi |Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme |jitNetworkAccessMonitoringEffect |
|Uyarlamalı uygulama denetimleri |Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme |adaptiveApplicationControlsMonitoringEffect|
|Ağ güvenlik grupları |Azure Güvenlik Merkezi'nde esnek ağın genelini erişimi izleme |networkSecurityGroupsMonitoringEffect| 
|Güvenlik yapılandırmaları |Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izleyin |systemConfigurationsMonitoringEffect| 
|Uç nokta koruması |Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme |endpointProtectionMonitoringEffect |
|Disk şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş VM disklerini izleyin |diskEncryptionMonitoringEffect|
|Güvenlik açığı değerlendirmesi |Azure Güvenlik Merkezi'nde izleme VM güvenlik açıklarını |vulnerabilityAssesmentMonitoringEffect|
|Web uygulaması güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme |webApplicationFirewallMonitoringEffect |
|Yeni nesil güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izleme| |





## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure İlkesi'nde güvenlik ilkelerini düzenleme öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi için kiracı genelinde görünürlük kazanma](security-center-management-groups.md): Yönetim gruplarını Azure Güvenlik Merkezi için ayarlamayı öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
