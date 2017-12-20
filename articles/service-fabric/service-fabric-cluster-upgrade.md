---
title: "Azure Service Fabric kümesi yükseltme | Microsoft Docs"
description: "Service Fabric kod ve/veya küme güncelleştirme modunda, sertifikaları, uygulama bağlantı noktaları, işletim sistemi yamalarını yapılması ekleme yükseltme ayarı dahil olmak üzere bir Service Fabric kümesi çalıştıran yapılandırma yükseltin ve benzeri. Yükseltme gerçekleştirildiğinde beklentilerinizin ne?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 7ea71ab891583c51b3c07a4d0a9f0b4f54e56669
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Azure Service Fabric kümesi yükseltme
> [!div class="op_single_selector"]
> * [Azure küme](service-fabric-cluster-upgrade.md)
> * [Tek başına küme](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Tüm modern, tasarlama upgradability için uzun vadeli başarı ürününüzün ulaşmak için anahtar sistemidir. Azure Service Fabric kümesi sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, otomatik olarak yönetilir ve ne kendiniz yapılandırabileceğiniz anlatılmaktadır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan yapı sürümünü denetleme
Kümenizin Microsoft tarafından yayımlanan veya kümenizin üzerinde olmasını istediğiniz desteklenen fabric sürümü seçebileceğiniz gibi otomatik fabric yükseltmelerinin alacak şekilde ayarlayabilirsiniz.

Portalda "upgrademode öğesinin" Küme yapılandırma ayarı veya canlı bir küme oluşturma veya sonraki bir sürümü, aynı anda Resource Manager kullanarak bunu 

> [!NOTE]
> Desteklenen fabric sürümü her zaman çalışması kümenizi sakladığınızdan emin olun. Gibi ve size yeni bir service fabric sürümü sunmaktan olduğunda, önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni sürümler duyurulur [service fabric ekip blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/). Yeni sürüm, daha sonra seçmek kullanılabilir. 
> 
> 

14 gün önce kümenizi çalıştığından, yayın süre sonu, sistem durumu uyarısı kümenizi koyan bir sistem durumu olayı oluşturulur. Desteklenen doku sürümüne yükseltilene dek küme bir uyarı durumunda kalır.

### <a name="setting-the-upgrade-mode-via-portal"></a>Portal aracılığıyla yükseltme modunu ayarlama
Küme oluştururken küme otomatik veya el ile ayarlayabilirsiniz.

![Create_Manualmode][Create_Manualmode]

Yönetme deneyimi kullanarak otomatik veya el ile bir kümede dinamik olduğunda, küme ayarlayabilirsiniz. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Portal aracılığıyla el ile modu olarak ayarlanmış bir küme üzerinde yeni bir sürüme yükseltme.
Yeni bir sürüme yükseltmek için yapmanız gereken tek şey aşağı açılır listeden kullanılabilir sürümü seçin ve kaydedin. Fabric yükseltmesi otomatik olarak koparılan. Küme sistem durumu ilkeleri (düğüm durumu ve sistem bileşimini kümede çalışan tüm uygulamaların) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Daha fazla bilgi için bu belgenin aşağı bu özel durumu ilkeler ayarlama. 

Geri alma sonuçlandı sorunları düzelttikten sonra önce de aynı adımları izleyerek yükseltme yeniden başlatma gerekir.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Resource Manager şablonu aracılığıyla yükseltme modunu ayarlama
"Upgrademode öğesinin" yapılandırma Microsoft.ServiceFabric/clusters kaynak tanımına ekleyin ve aşağıda gösterildiği gibi desteklenen doku sürümlerinden birini "clusterCodeVersion" koymak ve şablonu dağıtmak. "Elle" veya "Otomatik" "upgrademode öğesinin" için geçerli değerler:

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Resource Manager şablonu aracılığıyla el ile modu olarak ayarlanmış bir küme üzerinde yeni bir sürüme yükseltme.
Küme el ile modunda olduğunda yeni bir sürüme yükseltmek için "clusterCodeVersion" desteklenen bir sürüm olarak değiştirin ve onu dağıtın. Şablon dağıtımı, Fabric yükseltmesi kicks başlayacağı zamana otomatik olarak. Küme sistem durumu ilkeleri (düğüm durumu ve sistem bileşimini kümede çalışan tüm uygulamaların) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Daha fazla bilgi için bu belgenin aşağı bu özel durumu ilkeler ayarlama. 

Geri alma sonuçlandı sorunları düzelttikten sonra önce de aynı adımları izleyerek yükseltme yeniden başlatma gerekir.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Belirli bir aboneliğe için tüm ortamlar için tüm kullanılabilir sürüm listesini al
Aşağıdaki komutu çalıştırın ve bunun benzer bir çıktı almalısınız.

"supportExpiryUtc" söyler, zaman belirli bir yayın doluyor veya süresi dolmuş. En son sürüm geçerli bir tarih yok - değerine sahip "9999-12-31T23:59:59.9999999", yalnızca başka bir deyişle, sona erme tarihi henüz ayarlanmadı.

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

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Küme yükseltme modu otomatik olduğunda doku yükseltme davranışı
Microsoft Azure bir kümede çalışan yapılandırma ve fabric kod tutar. Biz gerektiği ölçüde temeline göre yazılım otomatik izlenen yükseltme gerçekleştirin. Bu yükseltme, kod, yapılandırma veya her ikisi de olabilir. Uygulamanız herhangi bir etkisi veya bu yükseltme nedeniyle en az etki yükselmesine emin olmak için aşağıdaki aşamalarında yükseltmeleri biz gerçekleştirin:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>1. Aşama: Tüm küme sistem durumu ilkeleri kullanarak yükseltme gerçekleştirilir
Bu aşamasında, aynı anda tek bir yükseltme etki alanı yükseltme devam ve kümedeki çalışmakta olan uygulamalar herhangi kesinti olmadan çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve sistem bileşimini kümede çalışan tüm uygulamaların) yükseltme sırasında bağlı.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Ardından aboneliğin sahibi için bir e-posta gönderilir. E-posta, aşağıdaki bilgileri içerir:

* Bildirim biz küme yükseltmeyi geri alma gerekiyordu.
* Varsa önerilen düzeltici eylemler.
* Gün (biz Aşama 2 yürütme kadar n) sayısı.

Herhangi bir yükseltmesinin altyapı nedenlerden dolayı başarısız olduysa aynı yükseltme birkaç kez daha yürütülecek deneyin. E-posta gönderilip gönderilmediğini tarihinden itibaren n gün sonra Aşama 2'ye geçin.

Küme sistem durumu ilkeleri karşılanıyorsa, yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında meydana gelebilir. Hiçbir e-posta onayı başarılı bir çalışma yoktur. Bu, çok fazla e-postaların bir e-postayı--bir özel durum normal olarak görülmelidir gönderilmesini önlemek için yapılır. Uygulama kullanılabilirliği etkilemeden başarılı olması için Küme yükseltme çoğunu bekliyoruz.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>2. Aşama: Yalnızca varsayılan sistem durumu ilkeleri kullanarak yükseltme gerçekleştirilir
Sistem durumu ilkeleri bu aşamasında, yükseltme, başında sağlıklı uygulama sayısını yükseltme işlemi boyunca aynı kalır şekilde ayarlanır. 1. aşaması, olduğu gibi aynı anda tek bir yükseltme etki alanı Aşama 2 Yükseltme devam ve kümedeki çalışmakta olan uygulamalar herhangi kesinti olmadan çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve sistem bileşimini kümede çalışan tüm uygulamaların) yükseltme süresince bağlı.

Küme sistem durumu ilkeleri yürürlükte karşılanmazsa, yükseltme geri alınır. Ardından aboneliğin sahibi için bir e-posta gönderilir. E-posta, aşağıdaki bilgileri içerir:

* Bildirim biz küme yükseltmeyi geri alma gerekiyordu.
* Varsa önerilen düzeltici eylemler.
* Gün (biz aşama 3 yürütme kadar n) sayısı.

Herhangi bir yükseltmesinin altyapı nedenlerden dolayı başarısız olduysa aynı yükseltme birkaç kez daha yürütülecek deneyin. Birkaç gün n gün hazır olduğunuzda önce anımsatıcı e-posta gönderilir. E-posta gönderilip gönderilmediğini tarihinden itibaren n gün sonra aşama 3'e geçin. Aşama 2'de biz gönderdiğiniz e-postaları ciddi alınması ve düzeltici eylemler alınması gerekir.

Küme sistem durumu ilkeleri karşılanıyorsa, yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında meydana gelebilir. Hiçbir e-posta onayı başarılı bir çalışma yoktur.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>3. Aşama: Agresif sistem durumu ilkeleri kullanarak yükseltme gerçekleştirilir
Bu sistem durumu ilkeleri bu aşamasında, uygulamaların durumunu yerine yükseltme tamamlanmasından sağlamıştır. Bu aşamada çok az sayıda Küme yükseltme sonlanır. Kümeniz için bu aşamada alırsa, uygulamanızın bozulur ve/veya kullanılabilirlik kaybına olduğunu şansı yoktur.

Diğer iki aşamaya benzeyen, aşama 3 yükseltmeler bir yükseltme etki alanı aynı anda devam edin.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Herhangi bir yükseltmesinin altyapı nedenlerden dolayı başarısız olduysa aynı yükseltme birkaç kez daha yürütülecek deneyin. Böylece, destek ve/veya yükseltmeler almayacaksınız bundan sonra küme, sabitlenir.

Bu bilgileri içeren bir e-posta abonelik sahibi eylemlerden birlikte gönderilir. Aşama 3 başarısız olduğu bir duruma getirmek için tüm kümeleri bekliyoruz değil.

Küme sistem durumu ilkeleri karşılanıyorsa, yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında meydana gelebilir. Hiçbir e-posta onayı başarılı bir çalışma yoktur.

## <a name="cluster-configurations-that-you-control"></a>Denetim küme yapılandırmaları
Kümesi becerisine ek yükseltme modu, canlı bir küme üzerinde değiştirebileceğiniz yapılandırmalar şunlardır.

### <a name="certificates"></a>Sertifikalar
Yeni ekleyebilir veya sertifikaları Portalı aracılığıyla istemci ve küme için kolayca silebilirsiniz. Başvurmak [ayrıntılı yönergeler için bu belgenin](service-fabric-cluster-security-update-certs-azure.md)

![Azure portalında sertifika parmak izlerini gösteren ekran görüntüsü.][CertificateUpgrade]

### <a name="application-ports"></a>Uygulama bağlantı noktaları
Düğüm türü ile ilişkili yük dengeleyici kaynak özelliklerini değiştirerek uygulama bağlantı noktalarını değiştirebilirsiniz. Resource Manager PowerShell doğrudan kullanabilirsiniz veya Portalı'nı kullanabilirsiniz.

Bir düğüm türü tüm sanal makineleri yeni bir bağlantı noktasını açmak için aşağıdakileri yapın:

1. Yeni bir yoklama uygun yük dengeleyiciye ekleyin.
   
    Portalı kullanarak kümenizi dağıttıysanız, yük dengeleyici "LB-adının kaynak grubu-nodetypename adlı" adlı her düğüm türü için bir tane. Yük Dengeleyici adları yalnızca bir kaynak grubunda benzersiz olduğundan, bunlar için belirli bir kaynak grubu altında arama en iyisidir.
   
    ![Bir yük dengeleyiciye Portalı'nda bir araştırma ekleme gösteren ekran görüntüsü.][AddingProbes]
2. Yeni bir kural yük dengeleyiciye ekleyin.
   
    Yeni bir kural, önceki adımda oluşturduğunuz araştırmayı kullanarak aynı yük dengeleyiciye ekleyin.
   
    ![Bir yük dengeleyiciye portalında yeni bir kural ekleme.][AddingLBRules]

### <a name="placement-properties"></a>Yerleşim özellikleri
Her düğüm türleri için uygulamalarınızda kullanmak istediğiniz özel yerleşim özellikleri ekleyebilirsiniz. NodeType açıkça eklemeden kullanabileceğiniz varsayılan bir özelliktir.

> [!NOTE]
> Kısıtlamalarından, düğüm özellikleri ve bunları tanımlamak nasıl kullanımı hakkında daha fazla bilgi için "Yerleştirme kısıtlamaları ve düğüm özelliklerini" Service Fabric kümesi Kaynak Yöneticisi belgede bölüm üzerinde başvurmak [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Kapasite ölçümleri
Her düğüm türü için yükü raporlamak üzere uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Üzerindeki yükü raporlamak için kapasite ölçümlerini kullanımı hakkında daha fazla bilgi için Service Fabric kümesi Resource Manager belgelerine bakın [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md) ve [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Fabric yükseltme ayarları - sistem durumu ilkeleri
Fabric yükseltmesi için özel sistem durumu ilkeleri belirtebilirsiniz. Otomatik fabric yükseltmelerinin kümenizi ayarladıysanız, bu ilkeler otomatik fabric yükseltmelerinin aşaması-1 olarak uygulanan.
Yükseltmeler kümeniz için el ile doku ayarladıysanız, bu ilkeler kümenizdeki fabric yükseltmesi kapalı kazandırın sisteme tetikleme yeni bir sürüm seçin her zaman uygulanan. İlkeleri geçersiz varsayılan değerler kullanılır.

Özel sistem durumu ilkeleri belirtin veya Gelişmiş Ayarları Yükselt'i seçerek "fabric yükseltmesi" dikey geçerli ayarları gözden geçirin. Aşağıdaki resimde nasıl konusunda gözden geçirin. 

![Özel sistem durumu ilkeleri yönetme][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Yapı ayarları kümeniz için özelleştirme
Başvurmak [hizmet doku küme yapı ayarları](service-fabric-cluster-fabric-settings.md) ne ve bunları nasıl özelleştirebilirsiniz.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>İşletim sistemi yamalarını kümeyi olun VM'ler
Başvurmak [düzeltme eki Orchestration uygulama](service-fabric-patch-orchestration-application.md) hangi dağıtılabilir kümenizdeki düzeltme eklerini her zaman kullanılabilir hizmetler tutma bir orchestrated şekilde, Windows Update sitesinden yükleyin. 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>İşletim sistemi yükseltmeleri kümeyi olun VM'ler
İşletim sistemi görüntüsü kümenin sanal makinelerde yükseltmeniz gerekir, aynı anda bir VM yapmalısınız. Sorumlu olduğunuz bu yükseltmeyi--var. şu anda bu Otomasyon.

## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [hizmet doku küme yapı ayarları](service-fabric-cluster-fabric-settings.md)
* Bilgi edinmek için nasıl [kümenizi ölçeğini](service-fabric-cluster-scale-up-down.md)
* Hakkında bilgi edinin [uygulama yükseltme](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
