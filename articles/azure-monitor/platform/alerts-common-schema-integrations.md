---
title: Ortak uyarı şema Logic Apps ile tümleştirme
description: Tüm uyarılarınız işlemek için ortak uyarı şema yararlanan bir mantıksal uygulama oluşturmayı öğrenin.
author: ananthradhakrishnan
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 13cb3880662e1665b03dd63f009645acbe97fc75
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734883"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Ortak uyarı şema Logic Apps ile tümleştirme

Bu makalede, tüm uyarıları yönetmek için ortak uyarı şema yararlanan bir mantıksal uygulama oluşturma işlemini gösterir.

## <a name="overview"></a>Genel Bakış

[Ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs) tüm, farklı uyarı türleri arasında standartlaştırılmış ve Genişletilebilir bir JSON şeması sağlar. Ortak uyarı şema program aracılığıyla – Web kancaları, runbook'ları ve logic apps kullanılabilir olduğunda yararlıdır. Bu makalede, tüm uyarıları yönetmek için tek bir mantıksal uygulama'nın nasıl yazılabilir gösterir. Programlı diğer yöntemler ile aynı ilkeler uygulanabilir. Bu makalede açıklanan mantıksal uygulama için iyi tanımlanmış değişkenler oluşturur ['önemli' alanlar](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields)ve ayrıca nasıl işleyebileceğini açıklar [uyarı türü](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) belirli mantığı.


## <a name="prerequisites"></a>Önkoşullar 

Bu makalede okuyucu alışkın olduğu varsayılır. 
* Uyarı kurallarını ayarlama ([ölçüm](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [günlük](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [etkinlik günlüğü](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Ayarlama [Eylem grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* Etkinleştirme [ortak uyarı şeması](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) Gelen Eylem grupları içinde

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Ortak uyarı şema yararlanarak bir mantıksal uygulama oluşturma

1. İzleyin [özetlenen adımları mantıksal uygulamanızı oluşturmak için](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app). 

1.  Tetikleyiciyi seçin: **Bir HTTP isteği alındığında**.

    ![Mantıksal uygulama Tetikleyicileri](media/action-groups-logic-app/logic-app-triggers.png "mantıksal uygulama Tetikleyicileri")

1.  Seçin **Düzenle** HTTP isteği tetikleyicisi değiştirmek için.

    ![HTTP isteği Tetikleyicileri](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP isteği Tetikleyicileri")


1.  Aşağıdaki şema kopyalayıp yeniden oluştur:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. Seçin **+** **yeni adım** seçip **Eylem Ekle**.

    ![Eylem Ekle](media/action-groups-logic-app/add-action.png "Eylem Ekle")

1. Bu aşamada, belirli iş gereksinimlerinize göre çeşitli bağlayıcılar (Microsoft Teams, Slack, Salesforce, vb.) ekleyebilirsiniz. 'Gerekli alanları' nın-hazır kullanabilirsiniz. 

    ![Önemli alanlar](media/alerts-common-schema-integrations/logic-app-essential-fields.png "önemli alanlar")
    
    Alternatif olarak, koşullu mantık 'Expression' seçeneği kullanılarak uyarı türüne göre yazabilirsiniz.

    ![Mantıksal uygulama ifade](media/alerts-common-schema-integrations/logic-app-expressions.png "mantıksal uygulama ifadesi")
    
     ['MonitoringService' alanı](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) uyarı türü benzersiz olarak tanımlanabilmesi tanır temel üzerinde koşullu mantık oluşturabilirsiniz.

    
    Örneğin, aşağıdaki kod parçacığını denetler uyarı bir Application Insights tabanlı günlük uyarı ve bu durumda, arama sonuçlarını yazdırır. Aksi takdirde 'NA' yazdırır.

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     Daha fazla bilgi edinin [logic app ifadeleri yazarken](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions).

    


## <a name="next-steps"></a>Sonraki adımlar

* [Eylem grupları hakkında daha fazla bilgi](../../azure-monitor/platform/action-groups.md).
* [Ortak uyarı şeması hakkında daha fazla bilgi](https://aka.ms/commonAlertSchemaDocs).

