---
title: "Bir bulut hizmeti güncelleştirmek nasıl | Microsoft Docs"
description: "Azure bulut Hizmetleri güncelleştirmesi öğrenin. Nasıl kullanılabilirliğini sağlamak için bir bulut hizmeti üzerinde güncelleştirme işlemi devam eder öğrenin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 2ba9676ed2afce7f18446642527971f5001b5ca7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-update-a-cloud-service"></a>Bir bulut hizmeti güncelleştirme

Kendi rolleri ve konuk işletim sistemi, dahil olmak üzere bir bulut hizmeti güncelleştirme üç adım bir işlemdir. İlk olarak, ikili dosyaları ve yapılandırma dosyalarını yeni bulut hizmeti veya işletim sistemi sürümü yüklenmelidir. Ardından, Azure yeni bulut hizmeti sürümü gereksinimlerine bağlı olarak bulut hizmeti için işlem ve ağ kaynakları ayırır. Son olarak, Azure artımlı olarak Kiracı yeni sürümü veya konuk işletim sistemi, kullanılabilirliği koruyarak güncelleştirmek için bir yükseltme gerçekleştirir. Bu makalede bu son adım – yükseltme ayrıntılarını anlatılmaktadır.

## <a name="update-an-azure-service"></a>Bir Azure hizmetini güncelleştirme
Azure rolü örneklerinizi yükseltme etki alanları (UD) adı verilen mantıksal gruplar düzenler. Yükseltme etki alanları (UD) bir grup olarak güncelleştirilen rol örnekleri mantıksal kümeleridir.  Bulut bir Azure güncelleştirmeleri bir UD, her seferinde trafiği hizmet veren devam etmek için diğer UDs örnekleri sağlayan hizmet.

Yükseltme etki alanlarının varsayılan sayısı 5'tir. Hizmet tanım dosyası (.csdef) upgradeDomainCount özniteliği dahil olmak üzere farklı bir yükseltme etki alanlarının sayısını belirtebilirsiniz. UpgradeDomainCount özniteliği hakkında daha fazla bilgi için bkz: [WebRole şema](https://msdn.microsoft.com/library/azure/gg557553.aspx) veya [örn şema](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Bir veya daha fazla rolün bir yerinde güncelleştirme hizmetinizi gerçekleştirdiğinizde, Azure kümeleri rol örneklerinin ait oldukları yükseltme etki alanına göre güncelleştirir. Tüm bunları getirilmesi güncelleştirme onları durdurma belirli bir yükseltme etki alanındaki – örneklerinin çevrimiçi – geri azure güncelleştirmeleri sonra sonraki etki alanında oturum taşır. Yalnızca geçerli yükseltme etki alanında çalışan örnekleri durdurarak Azure bir güncelleştirme en az olumsuz etkiyi ile çalışan hizmet olduğunda emin olur. Daha fazla bilgi için bkz: [nasıl güncelleştirme işlemi devam eder](#howanupgradeproceeds) bu makalenin ilerisinde yer.

> [!NOTE]
> While koşulları **güncelleştirme** ve **yükseltme** Azure bağlamda biraz farklı bir anlama sahip, bu belgedeki özelliklerin açıklamaları ve işlemler için birbirinin yerine kullanılabilir.
>
>

Hizmetinizi güncelleştirilmesi bu rol için bir rolün en az iki örnek tanımlamalısınız yerinde kapalı kalma süresi olmadan. Hizmet bir rolü yalnızca bir örneği oluşuyorsa, yerinde güncelleştirme tamamlanana kadar hizmet kullanılamaz.

Bu konu, Azure güncelleştirmeler hakkında aşağıdaki bilgileri içerir:

* [Hizmet değişikliklerini güncelleştirme sırasında izin verilen](#AllowedChanges)
* [Nasıl bir yükseltme işlemi devam eder](#howanupgradeproceeds)
* [Bir güncelleştirme geri alma](#RollbackofanUpdate)
* [Devam eden bir dağıtımda birden çok mutating işlemleri başlatma](#multiplemutatingoperations)
* [Yükseltme etki alanları arasında rollerin dağıtım](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Hizmet değişikliklerini güncelleştirme sırasında izin verilen
Aşağıdaki tabloda, güncelleştirme sırasında bir hizmeti izin verilen değişiklikler gösterilir:

| Barındırma, hizmetler ve roller değişikliklere izin | Yerinde güncelleştirme | Hazırlanmış (VIP takas) | Silme ve yeniden Dağıt |
| --- | --- | --- | --- |
| İşletim sistemi sürümü |Evet |Evet |Evet |
| .NET güven düzeyi |Evet |Evet |Evet |
| Sanal makine boyutu<sup>1</sup> |Evet<sup>2</sup> |Evet |Evet |
| Yerel depolama ayarları |Yalnızca artırmak<sup>2</sup> |Evet |Evet |
| Rolleri bir hizmet olarak ekleyip |Evet |Evet |Evet |
| Belirli bir rol örneklerinin sayısını |Evet |Evet |Evet |
| Sayı veya bir hizmet için uç nokta türü |Evet<sup>2</sup> |Hayır |Evet |
| Adları ve yapılandırma ayarlarının değerleri |Evet |Evet |Evet |
| Değerleri (ancak değil adları) yapılandırma ayarları |Evet |Evet |Evet |
| Yeni sertifika Ekle |Evet |Evet |Evet |
| Mevcut sertifikalarını değiştirme |Evet |Evet |Evet |
| Yeni kod dağıtın |Evet |Evet |Evet |

<sup>1</sup> boyut bulut hizmeti için kullanılabilir boyutları alt sınırlı değişiklik.

<sup>2</sup> Azure SDK 1.5 gerektirir veya sonraki sürümler.

> [!WARNING]
> Sanal makine boyutunu değiştirme yerel verileri silecektir.
>
>

Aşağıdaki öğeler, güncelleştirme sırasında desteklenmez:

* Bir rolün adını değiştirme. Kaldırın ve ardından yeni adıyla rolünü ekleyin.
* Yükseltme etki alanı sayısı değiştirme.
* Yerel kaynakların boyutu kısaltır.

Yerel kaynak boyutu azalan gibi hizmet tanımı, diğer güncelleştirmeleri yapıyorsanız bir VIP takas güncelleştirmesi yerine gerçekleştirmeniz gerekir. Daha fazla bilgi için bkz: [takas dağıtım](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Nasıl bir yükseltme işlemi devam eder
Tüm hizmetiniz rollerinde veya tek bir rol hizmetinde güncelleştirmek isteyip istemediğinize karar verebilirsiniz. Her iki durumda da, yükseltilmekte olan ve ilk yükseltme etki alanına ait her bir rol tüm örneklerini durduruldu, yükseltme ve tekrar çevrimiçi. Yeniden çevrimiçi olduktan sonra ikinci yükseltme etki alanındaki örnekleri durduruldu, yükseltme ve tekrar çevrimiçi. Bir bulut hizmeti en çok bir yükseltme aynı anda etkin olabilir. Yükseltme her zaman en son sürümünü bulut hizmeti karşı gerçekleştirilir.

Aşağıdaki diyagramda, nasıl tüm hizmet rollerin yükseltiyorsanız yükseltmeye devam eder gösterilmektedir:

![Hizmet yükseltme](media/cloud-services-update-azure-service/IC345879.png "yükseltme hizmeti")

Sonraki Bu diyagramda, yalnızca tek bir rol yükseltiyorsanız nasıl güncelleştirme işlemi devam eder gösterilir:

![Yükseltme rol](media/cloud-services-update-azure-service/IC345880.png "yükseltme rolü")  

Bir otomatik güncelleştirme sırasında Azure yapı denetleyicisi sonraki UD yol güvenli olduğunda belirlemek için bulut hizmeti düzenli olarak değerlendirir. Bu sistem durumu değerlendirmesi bir rol başına temelinde gerçekleştirilir ve yalnızca en son sürümünü (yani zaten gitti UDs örneklerinden) örneği göz önünde bulundurur. Olduğunu doğrular en az sayıda her rol için rol örneği elde tatmin edici terminal durumuna.

### <a name="role-instance-start-timeout"></a>Rol örneği başlangıç zaman aşımı
Yapı denetleyicisi Başlatıldı durumuna ulaşması her rol örneği için 30 dakika bekler. Zaman aşımı süresi aşılırsa, yapı denetleyicisi sonraki rol örneği taramasını devam eder.

### <a name="impact-to-drive-data-during-cloud-service-upgrades"></a>Bulut hizmeti sırasında sürücü verilere etkisi yükseltir

Yükseltme yolu Azure yükseltmeler Hizmetleri nedeniyle gerçekleştirilirken bir hizmet birden çok örneği için tek bir örnekten yükseltirken hizmetinizi gidersiniz. Hizmet düzeyi sözleşmesi guaranteeing hizmet kullanılabilirliği, yalnızca birden fazla örneği ile dağıtılan Hizmetleri için geçerlidir. Aşağıdaki listede, her sürücüdeki verilerin her Azure hizmet yükseltme senaryosu nasıl etkilendiği açıklanmaktadır:

|Senaryo|C sürücüsü|D sürücüsü|E sürücüsü|
|--------|-------|-------|-------|
|VM yeniden başlatma|Korunur|Korunur|Korunur|
|Portal yeniden başlatma|Korunur|Korunur|Yok|
|Portal yeniden görüntü oluşturma|Korunur|Yok|Yok|
|Yerinde yükseltme|Korunur|Korunur|Yok|
|Düğüm geçiş|Yok|Yok|Yok|

Yukarıdaki listede E: sürücü rolün kök sürücüsünde temsil eder ve sabit kodlanmış olmamalıdır, unutmayın. Bunun yerine, kullanın **RoleRoot %** sürücü temsil etmek için ortam değişkeni.

Tek Örnekli bir hizmeti yükseltirken kapalı kalma süresini en aza indirmek için yeni bir çok örnekli hizmet hazırlama sunucusuna dağıtır ve bir VIP takası gerçekleştirin.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Bir güncelleştirme geri alma
Azure tarafından Azure yapı denetleyicisi ilk güncelleştirme isteği kabul edildikten sonra bir hizmete ek işlemleri başlatmak izin vererek güncelleştirme sırasında hizmetlerini yönetme esneklik sağlar. Bir geri alma, yalnızca bir güncelleştirme olduğunda (yapılandırma değişikliği) gerçekleştirilebilir veya yükseltme işlemi **devam eden** dağıtımda durum. Bir güncelleştirme veya yükseltme henüz yeni sürüme güncelleştirilmemiş hizmetinin en az bir örnek var olduğu sürece devam ediyor olarak kabul edilir. Bir geri alma izin verilip verilmeyeceğini sınamak için tarafından döndürülen RollbackAllowed bayrak değerini denetleyin [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özellik alma](https://msdn.microsoft.com/library/azure/ee460806.aspx) işlemleri ayarlanır true.

> [!NOTE]
> Yalnızca geri alma çağırmak için bir anlam bir **yerinde** güncelleştirmek veya bir çalışan örneğinin tamamını hizmetinizi diğeriyle VIP takas yükseltmeler içerdiğinden dolayı yükseltin.
>
>

Geri alma devam eden güncelleştirme dağıtımda aşağıdaki etkileri gösterir:

* Henüz güncelleştirildi veya en yeni sürüme yükseltildiğinden tüm rol örnekleri güncelleştirilmez veya bu örneklerde hizmetinin hedefi sürümü zaten çalıştığından, yükseltme.
* Zaten güncelleştirildi veya yeni hizmet paketi sürümüne tüm rol örnekleri (\*.cspkg) dosyası ya da hizmet yapılandırması (\*.cscfg) dosya (veya her iki dosya) Bu dosyalar yükseltme öncesi sürümüne geri.

Bu, işlevsel olarak aşağıdaki özellikler tarafından sağlanır:

* [Geri alma güncelleştirme veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) bir yapılandırma güncelleştirme çağrılabilir işlemi (çağırarak tetiklenen [dağıtım yapılandırmasını değiştirme](https://msdn.microsoft.com/library/azure/ee460809.aspx)) veya yükseltme (çağırarak tetiklenen [ Yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx)) var olduğu sürece en az bir örnek hizmetindeki henüz yeni sürüme güncelleştirilmemiş.
* Yanıt gövdesi bir parçası olarak döndürülen kilitli öğesi ve RollbackAllowed öğesi [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özellik alma](https://msdn.microsoft.com/library/azure/ee460806.aspx) işlemleri:

  1. Kilitli öğesi mutating işlemi üzerinde belirli bir dağıtım çağrılabilir algılamasını sağlar.
  2. RollbackAllowed öğesi algılama sayesinde [geri alma güncelleştirme veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) işlemi üzerinde belirli bir dağıtım çağrılabilir.

  Bir geri alma işlemi gerçekleştirmek için kilitli ve RollbackAllowed öğeleri denetleme gerekmez. RollbackAllowed ayarlandığını doğrulamak için son eklerini true. Bu yöntemleri kümesine istek üstbilgisi kullanarak çağrılan bu öğeleri yalnızca döndürülür "x-ms-version: 2011-10-01" veya sonraki bir sürümü. Sürüm oluşturma üstbilgileri hakkında daha fazla bilgi için bkz: [Hizmet Yönetimi sürüm](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Bazı durumlar vardır geri alma bir güncelleştirme olduğunda veya yükseltme desteklenmiyor, bunlar şu şekildedir:

* Yerel kaynaklar - güncelleştirme rolü Azure platformu için yerel kaynak artarsa azalma geri izin vermiyor.
* Kota sınırlamalarını - güncelleştirme işlemi artık olabilecek bir ölçek idiyse, geri alma işlemi tamamlamak için yeterli işlem kotası sahip. Her Azure aboneliğinin bu aboneliğe ait tüm barındırılan hizmetler tarafından kullanılan çekirdek sayısını belirten ilişkili kota sahiptir. Verilen güncelleştirmesi işlemin geri alınmasının aboneliğinizi kota sonra koyabilirsiniz, bir geri alma etkin değil.
* Yarış durumu - ilk güncelleştirme tamamladıysa, bir geri alma mümkün değildir.

Kullanıyorsanız, bir güncelleştirme geri alma ne zaman yararlı olabilir, bir örnek verilmiştir [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx) işlemi, Azure ana yerinde yükseltme sırasında hizmet barındırılan hızı denetlemek için el ile modunda alındı.

Yükseltme piyasaya sürme sırasında çağırmanız [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx) el ile modunda ve yükseltme etki alanlarının gitmeye başlayın. Yükseltme, izleme gibi belirli bir noktada, Not Bazı rol örnekleri, inceleyin ilk yükseltme etki alanlarında yanıt veremez duruma gelmiş, çağırabilirsiniz [geri alma güncelleştirme veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) bırakacaktır dağıtım işlemi önceki hizmet paketi ve yapılandırma için yükseltilmiş geri alma örneklerini ve henüz yükseltme örnekleri olduğu gibi kalmıştır.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Devam eden bir dağıtımda birden çok mutating işlemleri başlatma
Bazı durumlarda, devam eden bir dağıtımda birden çok eşzamanlı mutating işlemi başlatmak isteyebilirsiniz. Örneğin, bir hizmet güncelleştirmesi gerçekleştirebilir ve bu güncelleştirmeyi hizmetiniz arasında alınmakta olsa da, bazı değişiklik örneğin hale getirmek istediğiniz güncelleştirme geri dönmek için farklı bir güncelleştirme uygulamak veya hatta dağıtımı silin. Hizmet yükseltmesi yükseltilen rol örneğini art arda çökmesine neden olan buggy kodu içeriyorsa, bu gerekli olabilir durumu gösterir. Bu durumda, Azure yapı denetleyicisi yükseltilmiş etki alanındaki örneklerinin sayısı yetersiz sağlıklı olduğundan yükseltme yapan uygulamada ilerleme mümkün olmaz. Bu durum olarak adlandırılır bir *takılmış dağıtım*. Güncelleştirme geri veya yeni bir güncelleştirme başarısız olan üst uygulama dağıtımı sürüklemeniz biri.

Güncelleştirme veya hizmet yükseltmek için ilk istek Azure yapı denetleyicisi tarafından alınan sonra sonraki diziyi işlemleri başlatabilirsiniz. Diğer bir deyişle, başka bir mutating işlemi başlamadan önce ilk işlemin tamamlanmasını beklemek zorunda değildir.

İlk güncelleştirme devam eden durumdayken ikinci bir güncelleştirme işlemi başlatma benzer şekilde geri alma işlemi gerçekleştirir. İkinci güncelleştirmeyi otomatik modda ise, ilk yükseltme etki alanı muhtemelen zamanında aynı noktada çevrimdışı olmasından birden fazla yükseltme etki alanlarından örneklerine baştaki hemen yükseltilir.

Mutating işlem aşağıdaki gibidir: [dağıtım yapılandırmasını değiştirme](https://msdn.microsoft.com/library/azure/ee460809.aspx), [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx), [güncelleştirme dağıtım durumunu](https://msdn.microsoft.com/library/azure/ee460808.aspx), [dağıtımı Sil ](https://msdn.microsoft.com/library/azure/ee460815.aspx), ve [geri alma güncelleştirme veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx).

İki işlem [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özellik alma](https://msdn.microsoft.com/library/azure/ee460806.aspx), mutating işlemi üzerinde belirli bir dağıtım çağrılan olup olmadığını belirlemek için denetlenen kilitli bayrak döndürür.

Kilitli bayrağı döndüren sürümü bu yöntemleri çağırmak için istek üstbilgisi ayarlamanız gerekir "x-ms-version: 2011-10-01" ya da daha sonra. Sürüm oluşturma üstbilgileri hakkında daha fazla bilgi için bkz: [Hizmet Yönetimi sürüm](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Yükseltme etki alanları arasında rollerin dağıtım
Azure bir rolün örnekleri sayıda hizmet açıklaması (.csdef) dosyasının bir parçası olarak yapılandırılmış yükseltme etki alanları arasında eşit olarak dağıtır. Yükseltme etki alanlarının en büyük sayı 20'dir ve varsayılan 5'tir. Hizmet tanımı dosyası değiştirme hakkında daha fazla bilgi için bkz: [Azure Hizmet tanım Şeması (.csdef dosyası)](cloud-services-model-and-package.md#csdef).

Örneğin, rolünüze on örneği varsa, varsayılan olarak her bir yükseltme etki alanı iki örneklerini içerir. Rolünüze 14 örneği varsa, dört yükseltme etki alanlarının üç örneklerini içerir ve iki beşinci bir etki alanı içerir.

Yükseltme etki alanları, sıfır tabanlı bir dizin ile tanımlanır: ilk yükseltme etki alanı kimliği 0 ve 1 vb. kimliği ikinci yükseltme etki alanı varsa.

Aşağıdaki diyagramda, nasıl iki rolü içeren bir hizmet dağıtılmış hizmet iki yükseltme etki alanlarının tanımladığında gösterilmektedir. Hizmet, sekiz örneklerini web rolü ve çalışan rolü dokuz örneklerini çalışıyor.

![Yükseltme etki alanlarının dağıtım](media/cloud-services-update-azure-service/IC345533.png "yükseltme etki alanlarının dağılımı")

> [!NOTE]
> Azure örnekleri yükseltme etki alanları arasında nasıl ayrıldığını denetleyen unutmayın. Hangi girişlerin hangi etki alanı için ayrılan belirlemek mümkün değil.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Cloud Services nasıl yönetilir?](cloud-services-how-to-manage.md)  
[Bulut Hizmetleri izleme](cloud-services-how-to-monitor.md)  
[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure.md)  
