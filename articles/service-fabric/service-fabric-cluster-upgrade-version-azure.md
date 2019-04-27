---
title: Bir Azure Service Fabric kümesini yükseltme | Microsoft Docs
description: Service Fabric kodu ve/veya küme güncelleştirme modunda, sertifikaları, uygulama bağlantı noktaları, işletim sistemi düzeltme ekleri, yapılması ekleme yükseltme ayarı dahil olmak üzere, bir Service Fabric kümesi çalıştıran yapılandırma yükseltin ve benzeri. Yükseltmeleri gerçekleştirildiğinde ne yönde?
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: 234bff5049babf0c4b1d036b40201720b2736228
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714734"
---
# <a name="upgrade-the-service-fabric-version-of-a-cluster"></a>Service Fabric bir küme sürümünü yükseltin

Herhangi bir modern sistemi için upgradability yönelik uzun vadeli ürününüzün başarısını ulaşmak için anahtardır. Bir Azure Service Fabric kümesine sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, Azure kümenizde çalışan Service Fabric sürümüne yükseltmek açıklar.

Kümenizi otomatik yapı yükseltmeleri kümeniz üzerinde olmasını istediğiniz bir desteklenen yapı sürümü seçebilirsiniz veya Microsoft tarafından yayımlanır yayımlanmaz almak için ayarlayabilirsiniz.

Portalı "upgradeMode" Küme yapılandırması ayarlama veya canlı bir küme oluşturma veya daha sonra anında Resource Manager kullanarak bunu 

> [!NOTE]
> Kümenizi desteklenen yapı sürümü her zaman çalışır durumda tutmak emin olun. Gibi ve biz service fabric yeni bir sürümünü duyurmaktan zaman önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [üzerinde service fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/). Yeni sürüm daha sonra seçmek kullanılabilir. 
> 
> 

14 gün önce kümenizin çalıştığı, yayın sonu bir uyarı sistem durumu, kümenizin yerleştiren bir sistem durumu olayı oluşturulur. Desteklenen yapı sürümüne yükseltene kadar küme bir uyarı durumunda kalır.

## <a name="set-the-upgrade-mode-in-the-azure-portal"></a>Azure portalında yükseltme modunu ayarlayın
Kümeyi oluştururken, kümeyi otomatik veya el ile ayarlayabilirsiniz.

![Create_Manualmode][Create_Manualmode]

Küme yönetme deneyimini kullanarak otomatik veya el ile dinamik bir kümeye olduğunda, şirket için ayarlayabilirsiniz. 

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Portal aracılığıyla el ile modu olarak ayarlanmış bir kümede yeni bir sürüme yükseltiliyor.
Yeni bir sürüme yükseltmek için gerçekleştirmeniz gereken şey kullanılabilir sürüm açılan listeden seçin ve kaydedin. Fabric yükseltmesi otomatik olarak başlatıldı. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Daha fazla bilgi için bu belgede aşağı bu özel sistem durumu ilkeleri ayarlama. 

Geri almaya sonuçlandı. sorunları düzelttikten sonra önceki adımları izleyerek yükseltmeyi yeniden başlatmak gerekir.

![Manage_Automaticmode][Manage_Automaticmode]

## <a name="set-the-upgrade-mode-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak yükseltme modunu ayarlayın
"UpgradeMode" yapılandırma Microsoft.ServiceFabric/clusters kaynak tanımına ekleyin ve aşağıda gösterildiği gibi desteklenen yapı sürümleri "clusterCodeVersion" ayarlayın ve ardından şablonu dağıtmanız. "Elle" veya "Otomatik" "upgradeMode" için geçerli değerler:

![ARMUpgradeMode][ARMUpgradeMode]

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Resource Manager şablonu aracılığıyla el ile modu olarak ayarlanmış bir kümede yeni bir sürüme yükseltiliyor.
Küme el ile modunda olduğunda yeni bir sürüme yükseltmek için "clusterCodeVersion"'ı desteklenen bir sürüm olarak değiştirin ve dağıtın. Şablon dağıtımı, doku yükseltmenin kicks başlatıldı otomatik olarak. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır.  

Geri almaya sonuçlandı. sorunları düzelttikten sonra önceki adımları izleyerek yükseltmeyi yeniden başlatmak gerekir.

## <a name="set-custom-health-polices-for-upgrades"></a>Yükseltmeleri için kümesi özel sistem durumu ilkeleri
Fabric yükseltmesi için özel sistem durumu ilkeleri belirtebilirsiniz. Kümenizi otomatik yapı yükseltmeleri için ayarladığınız sonra bu ilkeler için uygulandığından [otomatik yapı yükseltmeleri Aşama 1](service-fabric-cluster-upgrade.md#fabric-upgrade-behavior-during-automatic-upgrades).
Kümeniz için el ile yapı yükseltmeleri ayarladıysanız, bu ilkeler, kümenizdeki fabric yükseltmesi hız kazandırın sisteme tetikleme yeni bir sürüm seçmek her zaman uygulanan. İlkeleri geçersiz kılmaz içeriyorsa, varsayılan değerleri kullanılır.

Özel sistem durumu ilkeleri belirtebilir veya Gelişmiş Yükseltme Ayarları'nı seçerek "fabric yükseltmesi" dikey penceresinin altındaki geçerli ayarları gözden geçirin. Aşağıdaki resme nasıl gözden geçirin. 

![Özel durum ilkelerini yönetme][HealthPolices]

## <a name="list-all-available-versions-for-all-environments-for-a-given-subscription"></a>Belirli bir aboneliği için tüm ortamlar için kullanılabilir tüm sürümlerin listesi
Aşağıdaki komutu çalıştırın ve buna benzer bir çıktı almalısınız.

"supportExpiryUtc" söyler, zaman belirli bir sürüm süresi doluyor veya süresi dolmuş. En son sürüm geçerli bir tarih yok - değeri olan "9999-12-31T23:59:59.9999999", yalnızca başka bir deyişle, sona erme tarihi öğesi henüz ayarlanmamış.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }
```

## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [service fabric kümesi yapı ayarları](service-fabric-cluster-fabric-settings.md)
* Bilgi edinmek için nasıl [kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md)
* Hakkında bilgi edinin [uygulama yükseltmeleri](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
