---
title: Bir Azure Service Fabric kümesini yükseltme | Microsoft Docs
description: Sürüm veya bir Azure Service Fabric kümesi yapılandırmasını yükseltme hakkında bilgi edinin.  Bu makalede, sertifikaları yükseltme, uygulama bağlantı noktaları ekleme, işletim sistemi düzeltme ekleri ve yükseltmeleri gerçekleştirildiğinde gerçekleşmesini bekleyebilirsiniz ayarı küme güncelleştirme modu açıklanır.
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
ms.openlocfilehash: 3ddda89b19a04bdcd45f392f297ee5e930833538
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60711610"
---
# <a name="upgrading-and-updating-an-azure-service-fabric-cluster"></a>Yükseltme ve bir Azure Service Fabric kümesi güncelleştiriliyor

Herhangi bir modern sistemi için upgradability yönelik uzun vadeli ürününüzün başarısını ulaşmak için anahtardır. Bir Azure Service Fabric kümesine sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, otomatik olarak yönetilir ve sizin yapılandırabileceğiniz ayarlar açıklanır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan yapı sürümü denetleme

Kümenizi çalıştıran tutmaya dikkat bir [yapı sürümü desteklenen](service-fabric-versions.md) her zaman. Gibi ve biz service fabric yeni bir sürümünü duyurmaktan zaman önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni sürümler, service fabric Ekibi blogunda bildirilir. Yeni sürüm daha sonra seçmek kullanılabilir.

14 gün önce kümenizin çalıştığı, yayın sonu bir uyarı sistem durumu, kümenizin yerleştiren bir sistem durumu olayı oluşturulur. Desteklenen yapı sürümüne yükseltene kadar küme bir uyarı durumunda kalır.

Kümenizi otomatik yapı yükseltmeleri kümeniz üzerinde olmasını istediğiniz bir desteklenen yapı sürümü seçebilirsiniz veya Microsoft tarafından yayımlanır yayımlanmaz almak için ayarlayabilirsiniz.  Daha fazla bilgi edinmek için [kümenizin Service Fabric sürümüne yükseltme](service-fabric-cluster-upgrade-version-azure.md).

## <a name="fabric-upgrade-behavior-during-automatic-upgrades"></a>Fabric yükseltme davranışı otomatik yükseltmeler sırasında
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

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>3. Aşama: Yükseltme agresif bir sistem durumu ilkeleri kullanılarak gerçekleştirilir.
Bu sistem durumu ilkeleri bu aşamada, uygulamaların durumunu yerine yükseltme doğru olarak. Bu aşamada birkaç Küme yükseltme edersiniz. Bu aşama için kümenizin alır, / kötüleşir veya kullanılabilirlik kaybetmek uygulamanızı olasılığı yoktur.

3. Aşama yükseltmeleri benzer şekilde, diğer iki aşaması, bir yükseltme etki alanı aynı anda geçin.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltmeyi geri alınır. Herhangi bir yükseltmeyi altyapı nedenlerle başarısız olduysa yönelik aynı yükseltmeyi birkaç kez daha yürütün. deneyin. Destek ve/veya yükseltmeler artık alabileceklerdir bundan sonra küme, sabitlenir.

Abonelik sahibi, düzeltici eylemlerle birlikte bu bilgileri içeren bir e-posta gönderilir. 3. Aşama başarısız olduğu bir duruma getirmek için tüm kümeleri beklemiyoruz.

Küme sistem durumu ilkeleri karşılanırsa yükseltme başarılı olarak kabul ve tam olarak işaretlenmiş. Bu aşamada bu ilk yükseltme veya herhangi bir yükseltme tekrar bölümlerini sırasında ortaya çıkabilir. Başarılı çalıştırma hiç e-posta onayı yoktur.

## <a name="manage-certificates"></a>Sertifikaları yönetme
Service Fabric kullanıyor [X.509 sertifikaları](service-fabric-cluster-security.md) düğümler arasındaki iletişimler güvenli ve istemcilerin kimliğini doğrulamak için bir küme oluştururken belirtin. Ekleme, güncelleştirme veya istemci ve küme için sertifikaları silme [Azure portalında](https://portal.azure.com) veya PowerShell/Azure CLI kullanarak.  Daha fazla bilgi edinmek için [sertifika ekleme veya kaldırma](service-fabric-cluster-security-update-certs-azure.md)

## <a name="open-application-ports"></a>Açık uygulama bağlantı noktaları
Uygulama bağlantı noktaları, düğüm türü ile ilişkili olan yük dengeleyici kaynak özelliklerini değiştirerek değiştirebilirsiniz. Azure portalını kullanabilirsiniz veya PowerShell/Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için okuma [açık bir küme için bir uygulama bağlantı noktası](create-load-balancer-rule.md).

## <a name="define-node-properties"></a>Düğüm özellikleri tanımlama
Bazen, belirli iş yükleri yalnızca belirli türdeki düğümleri üzerinde çalıştığından emin olmak isteyebilirsiniz. Örneğin, başkalarının olmayabilir ancak bazı iş yükü GPU veya SSD gerektirebilir. Her bir kümedeki düğüm türleri için küme düğümleri için özel düğüm özellikleri ekleyebilirsiniz. Yerleştirme kısıtlamaları deyimleri için bir veya daha fazla düğüm Özellikler'i seçin, tek tek hizmetlerine bağlı olan. Yerleştirme kısıtlamaları Hizmetleri çalıştırdığı tanımlar.

Yerleştirme kısıtlamaları, düğüm özellikleri ve bunları tanımlama hakkında daha fazla bilgi için okuma [düğümü özellikleri ve yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="add-capacity-metrics"></a>Kapasite ölçüm Ekle
Her düğüm türü için yükü raporlamak üzere uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Yükü raporlamak için kapasite ölçümlerini kullanımı hakkında ayrıntılı bilgi için üzerinde Service Fabric Küme Kaynak Yöneticisi belgelerine bakın. [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md) ve [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md).

## <a name="set-health-policies-for-automatic-upgrades"></a>Otomatik yükseltmeler için sistem durumu ilkeleri ayarlama
Fabric yükseltmesi için özel sistem durumu ilkeleri belirtebilirsiniz. Otomatik yapı yükseltmeleri kümenizi ayarlarsanız bu ilkeleri otomatik yapı yükseltmeleri aşaması-1 olarak uygulanan.
Kümeniz için el ile yapı yükseltmeleri ayarladıysanız, bu ilkeler, kümenizdeki fabric yükseltmesi hız kazandırın sisteme tetikleme yeni bir sürüm seçmek her zaman uygulanan. İlkeleri geçersiz kılmaz içeriyorsa, varsayılan değerleri kullanılır.

Özel sistem durumu ilkeleri belirtebilir veya Gelişmiş Yükseltme Ayarları'nı seçerek "fabric yükseltmesi" dikey penceresinin altındaki geçerli ayarları gözden geçirin. Aşağıdaki resme nasıl gözden geçirin. 

![Özel durum ilkelerini yönetme][HealthPolices]

## <a name="customize-fabric-settings-for-your-cluster"></a>Kümeniz için yapı ayarları özelleştirme
Bir kümede küme ve düğüm özelliklerinin güvenilirlik düzeyi gibi birçok farklı yapılandırma ayarları özelleştirilebilir. Daha fazla bilgi için okuma [Service Fabric kümesi yapı ayarları](service-fabric-cluster-fabric-settings.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>İşletim sisteminde küme düğümlerine yama yapma
Düzeltme eki düzenleme uygulaması (POA) işletim sistemi düzeltme eki uygulama kapalı kalma süresi olmadan bir Service Fabric kümesinde otomatikleştiren bir Service Fabric uygulamasıdır. [Düzenleme uygulama Windows için düzeltme eki](service-fabric-patch-orchestration-application.md) veya [Linux için düzeltme eki düzenleme uygulaması](service-fabric-patch-orchestration-application-linux.md) kümenizdeki Hizmetleri koruyarak düzenli bir şekilde düzeltme eklerini yüklemeyi dağıtılabilir kullanılabilir tüm zamanlar. 


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
