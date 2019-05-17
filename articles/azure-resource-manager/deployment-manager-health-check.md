---
title: Azure Deployment Manager için sistem tümleştirme piyasaya tanıtmak
description: Azure Deployment Manager ile pek çok bölge üzerinde bir hizmet dağıtmayı açıklar. Bu, tüm bölgelere sunulmadan önce dağıtımınızı kararlılığını doğrulamak için güvenli dağıtım uygulamalarını gösterir.
services: azure-resource-manager
documentationcenter: na
author: mumian
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/19
ms.author: jgao
ms.openlocfilehash: 41b16498fb79166b2c77c77a517ee5c443ebec75
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796252"
---
# <a name="introduce-health-integration-rollout-to-azure-deployment-manager-public-preview"></a>Azure Deployment Manager (genel Önizleme) için sistem tümleştirme piyasaya tanıtmak

[Azure Deployment Manager](./deployment-manager-overview.md) Azure Resource Manager kaynakların aşamalı piyasaya çıkarma yapmanıza olanak tanır. Kaynak bölge bölgeye göre sıralı bir şekilde dağıtılır. Sorun giderme ve etkisi ölçek azaltma tümleşik sistem durumu denetimi Azure Deployment Manager'ın piyasaya çıkarma ve otomatik olarak durdurma sorunlu piyasaya çıkarma izleyebilirsiniz. Bu özellik güncelleştirmeleri depoda gerilemeyi nedeni hizmet kullanılamazlık azaltabilir.

## <a name="health-monitoring-providers"></a>Sistem durumu izleme sağlayıcıları

Sistem durumu tümleştirme mümkün olduğunca kolaylaştırmak için Microsoft bazı şirketler, sistem durumu denetimleri ile dağıtımlarınızı tümleştirmek için bir basit kopyala/yapıştır çözümü sağlamak için izleme üst hizmet durumu ile çalışmaktadır. Bir sistem durumu İzleyicisi zaten kullanıyorsanız, bunlar harika çözümleri ile başlaması ve:

| ![Azure deployment manager sistem durumu İzleyici sağlayıcısı datadog](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-datadog.svg) | ![Azure deployment manager sistem durumu İzleyici sağlayıcısı site24x7](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-site24x7.svg) | ![Azure dağıtım manager sistem durumu İzleyicisi sağlayıcı wavefront](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-wavefront.svg) |
|-----|------|------|
|Datadog, önde gelen izleme ve analiz platformu modern bulut ortamları için. Bkz: [Datadog Azure Deployment Manager ile nasıl tümleştirildiğini](https://www.datadoghq.com/azure-deployment-manager/).|Site24x7, izleme çözümü hepsi bir arada özel ve genel bulut Hizmetleri. Bkz: [Site24x7 Azure Deployment Manager ile nasıl tümleştirildiğini](https://www.site24x7.com/azure/adm.html).| Wavefront, çoklu bulut uygulama ortamları için izleme ve analiz platformu. Bkz: [Wavefront Azure Deployment Manager ile nasıl tümleştirildiğini](https://go.wavefront.com/wavefront-adm/).|

## <a name="how-service-health-is-determined"></a>Hizmet durumu belirleme

[Sistem durumu izleme sağlayıcılarını](#health-monitoring-providers) Hizmetleri izleme ve uyarı, hizmet sistem durumu sorunları için çeşitli mekanizmalar sunar. [Azure İzleyici](../azure-monitor/overview.md) söz konusu teklifle bir örneğidir. Azure İzleyici, belirli eşikleri aştığında uyarılar oluşturmak için kullanılabilir. Örneğin, hizmetiniz için yeni bir güncelleştirme dağıttığınızda, bellek ve CPU kullanımı beklenen düzeyleri dışında sağlayabilirsiniz. Bildirildiğinde, düzeltici eylemleri gerçekleştirebilirsiniz.

Bu sistem durumu sağlayıcıları, genellikle hizmetinizin İzleyici durumunu programlı bir şekilde incelenmesi böylece REST API'leri sunar. REST API ya da geri (HTTP yanıt koduna göre belirlenir) basit ve sorunsuz ve düzgün çalışmayan bir sinyali ve/veya sinyaller hakkında ayrıntılı bilgileri iletiyi almasını dönebilirsiniz.

Yeni *healthCheck* adım Azure Dağıtım Yöneticisi'nde, sistem durumu hizmetini gösteren HTTP kodları bildirmek izin verir veya daha karmaşık REST sonuçlar için hatta eşleşiyorlarsa gösteren, sağlıklı bir normal ifadeler belirtebilirsiniz yanıt.

Azure Deployment Manager sistem durumu denetimleri ile Kurulum başlangıç akışı:

1. Tercih ettiğiniz bir sistem durumu hizmeti sağlayıcısı aracılığıyla, sistem durumu izleyicileri oluşturun.
1. Azure Deployment Manager dağıtımınız bir parçası olarak bir veya daha fazla healthCheck adımları oluşturun. HealthCheck adımları aşağıdaki bilgilerle doldurun:

    1. (Sistem durumu hizmet sağlayıcınız tarafından tanımlanan şekilde), sistem durumu izleyicileri için REST API için URI.
    1. Kimlik doğrulama bilgileri. Şu anda yalnızca API anahtarını stili kimlik doğrulaması desteklenir.
    1. [HTTP durum kodları](https://www.wikipedia.org/wiki/List_of_HTTP_status_codes) veya iyi bir yanıt tanımlama normal ifadeler. Normal ifadeler, hangi tüm gereken sağlıklı olarak değerlendirilmesi için yanıt veya eşleşme ifadeleri herhangi yanıt sağlıklı olarak değerlendirilmesi hangi eşleşmelidir sağlayabilir sağlayabileceğiniz unutmayın. Her iki yöntem desteklenir.

    Aşağıdaki Json örneği verilmiştir:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

1. Azure Deployment Manager dağıtımınız içinde uygun zamanda healthCheck adımları çağırın. Aşağıdaki örnekte, bir sistem durumu denetimi adım çağrılması **postDeploymentSteps** , **stepGroup2**.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

Bir örnek üzerinde yürütmek için bkz [Öğreticisi: Azure Dağıtım Yöneticisi'nde sistem durumu denetimi kullanın](./deployment-manager-health-check.md).

## <a name="phases-of-a-health-check"></a>Sistem durumu denetimi aşamaları

Bu noktada Azure Deployment Manager sistem durumu hizmeti ve bunu yapmak için kullanıma sunma ne aşamaları sorgulamak nasıl bilir. Ancak, Azure Deployment Manager da bu denetimleri zamanlamasını ayrıntılı yapılandırılmasını sağlar. Tüm yapılandırılabilir süreler 3 sıralı aşamalarında healthCheck adım çalıştırılır: 

1. Bekle

    1. Dağıtım işlemi tamamlandıktan sonra Vm'leri yeniden başlatma, yeniden yeni veriler veya hatta ilk kez başlatıldı göre. Ayrıca, sistem durumu sinyal sağlayıcısı bir şeye yararlı izleme sistem tarafından toplanacak yayma başlatmak hizmetler zaman alır. Tumultuous bu işlem sırasında hizmet durumu için güncelleştirme henüz kararlı bir duruma ulaştı değil beri denetlemek için anlamlı olmayabilir. Aslında, kaynakları kapatma gibi hizmet sağlıklı ve sağlıksız durumlar arasında oscillating. 
    1. Bekleme aşamasında, hizmet durumu izlemesi yapılmaz. Bu, dağıtılan kaynaklar izin vermek için kullanılır sistem durumu onay işlemine başlamadan önce hazırlama süresi. 
1. Elastik

    1. Her durumda ne kadar bilmek olanaksızdır olduğundan kaynaklar kaynakları potansiyel olarak kararsız olduğunda arasında esnek süre boyunca esnek aşaması sağlayan kararlı, haline gelmeden önce ve sağlam bir kararlı korumak için gerekli olduğunda hazırlama yönlendirilirsiniz durumu.
    1. Esnek aşaması başladığında, Deployment Manager'ı Azure hizmet durumu için sağlanan REST uç noktasını düzenli aralıklarla yoklama başlar. Yoklama aralığı yapılandırılabilir. 
    1. Gösteren iyi durumda olmayan bir hizmettir, bu sinyaller göz ardı edilir, genel Durum izleyicisini geri sinyalleri geliyorsa, elastik aşamaya devam eder ve yoklama devam eder. 
    1. Genel Durum izleyicisini hizmetin iyi durumda olduğunu belirten bir sinyal ile gelir hemen sonra esnek aşaması sona erer ve HealthyState aşaması başlar. 
    1. Bu nedenle, elastik bir aşama için belirtilen iyi bir yanıt zorunlu olarak kabul edilmeden önce Hizmet durumu için yoklama için harcanabilecek en uzun süreyi süresidir. 
1. HealthyState

    1. HealthyState aşamasında, hizmet durumunu sürekli olarak elastik aşaması ile aynı aralıkta sorguladı. 
    1. Hizmet sistem durumu izleme sağlayıcısından sağlıklı sinyalleri belirtilen sürenin tamamı boyunca korumak için bekleniyor. 
    1. Sağlıksız bir yanıt herhangi bir noktada algılanırsa, Azure Deployment Manager tüm dağıtım durur ve iyi durumda olmayan hizmet sinyaller taşıyan REST yanıtı döndürür.
    1. HealthyState süresi sona erdikten sonra healthCheck tamamlandıktan ve dağıtım sonraki adıma devam eder.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Dağıtım Yöneticisi'nde sistem durumu izleme tümleştirme hakkında bilgi edindiniz. Deployment Manager ile dağıtma hakkında bilgi edinmek için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Dağıtım Yöneticisi'nde sistem durumu denetimini tümleştirin](./deployment-manager-tutorial-health-check.md)