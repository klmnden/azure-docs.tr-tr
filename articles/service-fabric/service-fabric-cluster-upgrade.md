---
title: Bir Azure Service Fabric kümesini yükseltme | Microsoft Docs
description: Service Fabric kodu ve/veya küme güncelleştirme modunda, sertifikaları, uygulama bağlantı noktaları, işletim sistemi düzeltme ekleri, yapılması ekleme yükseltme ayarı dahil olmak üzere, bir Service Fabric kümesi çalıştıran yapılandırma yükseltin ve benzeri. Yükseltmeleri gerçekleştirildiğinde ne yönde?
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: aljo
ms.openlocfilehash: 2fd62f8709bddfd981f4b1358c97d0acbaf7f12d
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269115"
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Bir Azure Service Fabric kümesini yükseltme
> [!div class="op_single_selector"]
> * [Azure kümesine](service-fabric-cluster-upgrade.md)
> * [Tek başına küme](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Herhangi bir modern sistemi için upgradability yönelik uzun vadeli ürününüzün başarısını ulaşmak için anahtardır. Bir Azure Service Fabric kümesine sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, otomatik olarak yönetilir ve sizin yapılandırabileceğiniz ayarlar açıklanır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan yapı sürümü denetleme
Kümenizi otomatik yapı yükseltmeleri kümeniz üzerinde olmasını istediğiniz bir desteklenen yapı sürümü seçebilirsiniz veya Microsoft tarafından yayımlanır yayımlanmaz almak için ayarlayabilirsiniz.

Portalı "upgradeMode" Küme yapılandırması ayarlama veya canlı bir küme oluşturma veya daha sonra anında Resource Manager kullanarak bunu 

> [!NOTE]
> Kümenizi desteklenen yapı sürümü her zaman çalışır durumda tutmak emin olun. Gibi ve biz service fabric yeni bir sürümünü duyurmaktan zaman önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [üzerinde service fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/). Yeni sürüm daha sonra seçmek kullanılabilir. 
> 
> 

14 gün önce kümenizin çalıştığı, yayın sonu bir uyarı sistem durumu, kümenizin yerleştiren bir sistem durumu olayı oluşturulur. Desteklenen yapı sürümüne yükseltene kadar küme bir uyarı durumunda kalır.

### <a name="setting-the-upgrade-mode-via-portal"></a>Portal aracılığıyla yükseltme modunu ayarlama
Kümeyi oluştururken, kümeyi otomatik veya el ile ayarlayabilirsiniz.

![Create_Manualmode][Create_Manualmode]

Küme yönetme deneyimini kullanarak otomatik veya el ile dinamik bir kümeye olduğunda, şirket için ayarlayabilirsiniz. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Portal aracılığıyla el ile modu olarak ayarlanmış bir kümede yeni bir sürüme yükseltiliyor.
Yeni bir sürüme yükseltmek için gerçekleştirmeniz gereken şey kullanılabilir sürüm açılan listeden seçin ve kaydedin. Fabric yükseltmesi otomatik olarak başlatıldı. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Daha fazla bilgi için bu belgede aşağı bu özel sistem durumu ilkeleri ayarlama. 

Geri almaya sonuçlandı. sorunları düzelttikten sonra önceki adımları izleyerek yükseltmeyi yeniden başlatmak gerekir.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Resource Manager şablonu aracılığıyla yükseltme modunu ayarlama
"UpgradeMode" yapılandırma Microsoft.ServiceFabric/clusters kaynak tanımına ekleyin ve aşağıda gösterildiği gibi desteklenen yapı sürümleri "clusterCodeVersion" ayarlayın ve ardından şablonu dağıtmanız. "Elle" veya "Otomatik" "upgradeMode" için geçerli değerler:

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Resource Manager şablonu aracılığıyla el ile modu olarak ayarlanmış bir kümede yeni bir sürüme yükseltiliyor.
Küme el ile modunda olduğunda yeni bir sürüme yükseltmek için "clusterCodeVersion"'ı desteklenen bir sürüm olarak değiştirin ve dağıtın. Şablon dağıtımı, doku yükseltmenin kicks başlatıldı otomatik olarak. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Daha fazla bilgi için bu belgede aşağı bu özel sistem durumu ilkeleri ayarlama. 

Geri almaya sonuçlandı. sorunları düzelttikten sonra önceki adımları izleyerek yükseltmeyi yeniden başlatmak gerekir.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Belirli bir aboneliği için tüm ortamlar için kullanılabilir tüm sürüm'ın listesini alın
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

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>' % S'küme yükseltme modu otomatik olduğunda fabric yükseltme davranışı
Microsoft, bir Azure kümesinde çalışır yapılandırma ve yapı kodu tutar. Gerektiğinde bir temelinde otomatik yükselten yazılım gerçekleştiririz. Bu yükseltme, kod, yapılandırma veya her ikisi de olabilir. Uygulamanızı hiçbir etkisi veya bu yükseltme işlemleri nedeniyle en az etki alternatife emin olmak için yükseltmeleri aşağıdaki aşamalar gerçekleştiririz:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>1. Aşama: Tüm küme sistem durumu ilkeleri kullanarak bir yükseltme gerçekleştirilmeden
Bu aşamasında, yükseltmeleri bir yükseltme etki alanı, aynı anda devam etmek ve kümede çalışan uygulamalar kapalı kalma süresi çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Daha sonra bir e-posta, aboneliğin sahibine gönderilir. E-posta, aşağıdaki bilgileri içerir:

* Bildirim, bir küme yükseltmesi geri almak vardı.
* Varsa önerilen düzeltici eylemler.
* Gün (biz 2. Aşama yürütene kadar n) sayısı.

Herhangi bir yükseltmeyi altyapı nedenlerle başarısız olduysa yönelik aynı yükseltmeyi birkaç kez daha yürütün. deneyin. 2. Aşama ilerlemeden n gün sonra tarihten itibaren e-posta gönderildi.

Küme sistem durumu ilkeleri karşılanırsa yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında ortaya çıkabilir. Başarılı çalıştırma hiç e-posta onayı yoktur. Bu, çok fazla e-postalar bir e-postayı--özel durum normal olarak başlatmasında gösterilmelidir gönderilmesini önlemek için yapılır. Çoğu uygulama, kullanılabilirliği etkilemeden başarılı olması için Küme yükseltme bekliyoruz.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>2. Aşama: Yalnızca varsayılan sistem durumu ilkeleri kullanarak bir yükseltme gerçekleştirilmeden
Sistem durumu ilkeleri bu aşamada, yükseltme başında sağlıklı uygulama sayısını yükseltme işlemi boyunca aynı kalır şekilde ayarlanır. 1. Aşama, olduğu gibi 2. Aşama yükseltmeleri bir yükseltme etki alanı aynı anda devam etmek ve kümede çalışan uygulamalar kapalı kalma süresi çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve sistem kümede çalıştırılan tüm uygulamalar) yükseltme süresince bağlı.

Küme sistem durumu ilkeleri etkili karşılanmazsa, yükseltmeyi geri alınır. Daha sonra bir e-posta, aboneliğin sahibine gönderilir. E-posta, aşağıdaki bilgileri içerir:

* Bildirim, bir küme yükseltmesi geri almak vardı.
* Varsa önerilen düzeltici eylemler.
* Gün (biz 3.Aşama yürütene kadar n) sayısı.

Herhangi bir yükseltmeyi altyapı nedenlerle başarısız olduysa yönelik aynı yükseltmeyi birkaç kez daha yürütün. deneyin. Birkaç yedekleme n gün önce gün bir anımsatıcı e-posta gönderilir. 3. Aşama ilerlemeden n gün sonra tarihten itibaren e-posta gönderildi. 2. Aşama gönderdiğimiz e-postaları ciddi atılmalıdır ve düzeltici eylemler atılmalıdır.

Küme sistem durumu ilkeleri karşılanırsa yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında ortaya çıkabilir. Başarılı çalıştırma hiç e-posta onayı yoktur.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>3. Aşama: Agresif bir sistem durumu ilkeleri kullanarak bir yükseltme gerçekleştirilmeden
Bu sistem durumu ilkeleri bu aşamada, uygulamaların durumunu yerine yükseltme doğru olarak. Bu aşamada çok az sayıda Küme yükseltme edersiniz. Bu aşama için kümenizin alır, / kötüleşir veya kullanılabilirlik kaybetmek uygulamanızı olasılığı yoktur.

3. Aşama yükseltmeleri benzer şekilde, diğer iki aşaması, bir yükseltme etki alanı aynı anda geçin.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Herhangi bir yükseltmeyi altyapı nedenlerle başarısız olduysa yönelik aynı yükseltmeyi birkaç kez daha yürütün. deneyin. Destek ve/veya yükseltmeler artık alabileceklerdir bundan sonra küme, sabitlenir.

Abonelik sahibi, düzeltici eylemlerle birlikte bu bilgileri içeren bir e-posta gönderilir. 3. Aşama başarısız olduğu bir duruma getirmek için tüm kümeleri beklemiyoruz.

Küme sistem durumu ilkeleri karşılanırsa yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında ortaya çıkabilir. Başarılı çalıştırma hiç e-posta onayı yoktur.

## <a name="cluster-configurations-that-you-control"></a>Denetim küme yapılandırmaları
Yükseltme modu kümesi özelliğine ek olarak, canlı bir küme üzerinde değişiklik yapabileceğiniz yapılandırmalar şunlardır.

### <a name="certificates"></a>Sertifikalar
Yeni Ekle veya portal aracılığıyla istemci ve küme için sertifikaları kolayca silin. Başvurmak [ayrıntılı yönergeler için bu belgenin](service-fabric-cluster-security-update-certs-azure.md)

![Azure Portalı'nda sertifika parmak izleri gösteren ekran görüntüsü.][CertificateUpgrade]

### <a name="application-ports"></a>Uygulama bağlantı noktaları
Uygulama bağlantı noktaları, düğüm türü ile ilişkili olan yük dengeleyici kaynak özelliklerini değiştirerek değiştirebilirsiniz. Resource Manager PowerShell doğrudan kullanabilir veya portalı kullanabilirsiniz.

Bir düğüm türündeki tüm sanal makineler yeni bir bağlantı noktası açmak için aşağıdakileri yapın:

1. Yeni bir araştırma uygun yük dengeleyiciye ekleyin.
   
    Portalı kullanarak kümenize dağıtılan, yük Dengeleyiciler "LB-adının kaynak grubu-nodetypename adlı" adlı bir her düğüm türü. Yük Dengeleyici adları yalnızca bir kaynak grubu içinde benzersiz olduğundan, bunlar için belirli bir kaynak grubu altında arama en iyisidir.
   
    ![Portalda bir yük dengeleyiciye bir araştırma ekleme gösteren ekran görüntüsü.][AddingProbes]
2. Yük dengeleyiciye yeni bir kural ekleyin.
   
    Yeni bir kural, önceki adımda oluşturduğunuz araştırmayı kullanarak aynı yük dengeleyiciye ekleyin.
   
    ![Portalda bir yük dengeleyiciye yeni bir kural ekleme.][AddingLBRules]

### <a name="placement-properties"></a>Yerleşim özellikleri
Her düğüm türü, uygulamalarınızda kullanmak istediğiniz özel yerleşim özellikleri ekleyebilirsiniz. NodeType açıkça eklemeden kullanabileceğiniz varsayılan bir özelliktir.

> [!NOTE]
> Yerleştirme kısıtlamaları, düğüm özellikleri ve bunları tanımlama hakkında daha fazla bilgi için "Yerleştirme kısıtlamaları ve düğüm özelliklerinde" Service Fabric Küme Kaynak Yöneticisi belgenin bölüm üzerinde başvurmak [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Kapasite ölçümleri
Her düğüm türü için yükü raporlamak üzere uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Yükü raporlamak için kapasite ölçümlerini kullanımı hakkında ayrıntılı bilgi için üzerinde Service Fabric Küme Kaynak Yöneticisi belgelerine bakın. [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md) ve [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Yapı yükseltme ayarları - sistem durumu ilkeleri
Fabric yükseltmesi için özel sistem durumu ilkeleri belirtebilirsiniz. Otomatik yapı yükseltmeleri kümenizi ayarlarsanız bu ilkeleri otomatik yapı yükseltmeleri aşaması-1 olarak uygulanan.
Kümeniz için el ile yapı yükseltmeleri ayarladıysanız, bu ilkeler, kümenizdeki fabric yükseltmesi hız kazandırın sisteme tetikleme yeni bir sürüm seçmek her zaman uygulanan. İlkeleri geçersiz kılmaz içeriyorsa, varsayılan değerleri kullanılır.

Özel sistem durumu ilkeleri belirtebilir veya Gelişmiş Yükseltme Ayarları'nı seçerek "fabric yükseltmesi" dikey penceresinin altındaki geçerli ayarları gözden geçirin. Aşağıdaki resme nasıl gözden geçirin. 

![Özel durum ilkelerini yönetme][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Kümeniz için yapı ayarları özelleştirme
Başvurmak [service fabric kümesi yapı ayarları](service-fabric-cluster-fabric-settings.md) ne ve bunları nasıl özelleştirebilirsiniz.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Kümeyi oluşturan vm'lerde işletim sistemi düzeltme ekleri
Başvurmak [düzeltme düzenleme uygulaması](service-fabric-patch-orchestration-application.md) kümenizdeki her zaman kullanılabilen hizmetler tutma düzenlenmiş bir biçimde Windows Update'ten düzeltme eklerini yüklemeyi dağıtılabilecek. 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Kümeyi oluşturan vm'lerde işletim sistemi yükseltmeleri
İşletim sistemi görüntüsü kümenin sanal makinelerde yükseltmeniz gerekir, aynı anda bir VM yapmalısınız. Sorumlu olduğunuz için bu yükseltme--şu anda bu Otomasyon olmadığından.

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
