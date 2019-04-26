---
title: DevTest Labs kavramları | Microsoft Docs
description: DevTest Labs ve nasıl, oluşturmak, yönetmek ve Azure sanal makinelerini izlemek kolaylaştırmak temel kavramlarını öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 1e35513d5a5a799b1f5e45cf9a5aa97c083e2087
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201848"
---
# <a name="devtest-labs-concepts"></a>DevTest Labs kavramları
## <a name="overview"></a>Genel Bakış
Aşağıdaki liste, temel DevTest Labs kavramları ve tanımları içerir:

## <a name="labs"></a>Labs
Limitler ve kotalar belirterek daha iyi sayesinde sanal makineler (VM'ler), bu kaynakları yönetmelerini gibi bir laboratuvar kaynaklarını bir grup kapsayan altyapısıdır.

## <a name="virtual-machine"></a>Sanal makine
Bir Azure VM türlerinden biri [isteğe bağlı ve ölçeklenebilir işlem kaynağı](https://docs.microsoft.com/azure/app-service/overview-compare) Azure'un sunduğu. Azure sanal makineleri size sanallaştırma esnekliği satın alma ve rağmen yine de yapılandırma, düzeltme eki uygulama ve üzerinde çalıştığı yazılım yükleme gibi belirli görevleri gerçekleştirerek VM'nin bakımını yapmanız gereken, onu çalıştıran fiziksel donanımı korumak zorunda kalmadan sunar .

[Azure'da Windows sanal makineleri genel bakış](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) önce dikkat etmeniz gereken hakkında bilgi verir oluşturma bir VM, oluşturma ve yönetme biçiminizi.

## <a name="claimable-vm"></a>Talep edilebilir VM
Azure talep edilebilir VM izinlerine sahip herhangi bir laboratuvar kullanıcı tarafından kullanılabilir bir sanal makinedir. Bir Laboratuvar Yöneticisi, belirli temel görüntüleri ve yapıtları ile Vm'leri hazırlama ve paylaşılan bir havuz için kaydedebilirsiniz. Belirli bir yapılandırma ile gerektiğinde bir laboratuvar kullanıcı çalışan bir VM havuzundan talep edebilir.

Talep edilebilir VM başlangıçta belirli bir kullanıcıya atanmadı, ancak her kullanıcının listesinde "Talep edilebilir sanal makineler" altında gösterilir. Bir VM, bir kullanıcı tarafından talep edildikten sonra en fazla taşınması bunların "Sanal makinelerim" alanı ve artık herhangi bir kullanıcı tarafından talep edilebilir.

## <a name="environment"></a>Ortam
DevTest Labs'de bir ortam bir Azure kaynak koleksiyonunu belirtir. [Bu blog gönderisini](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) , Azure Resource Manager şablonlarından çoklu VM ortamları oluşturma anlatılmaktadır.

## <a name="base-images"></a>Temel görüntüler
Temel görüntüler, tüm araçları ve hızlı bir şekilde bir VM oluşturmak için önceden yüklenmiş ve yapılandırılmış ayarları ile VM görüntüleridir. Var olan bir taban çekme ve test aracısını yüklemek için bir yapıt ekleyerek, bir sanal makine sağlayabilirsiniz. Böylece her sanal Makinenin sağlanması için test aracısı yeniden yüklemek zorunda kalmadan temel kullanılabilir sağlanan VM ardından temel olarak kaydedebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Yapıtlar, VM sağlandıktan sonra Uygulamanızı yapılandırmak ve dağıtmak için kullanılır. Yapıtlar şunlar olabilir:

* Aracılar, fiddler'ı ve Visual Studio gibi - VM üzerinde yüklemek istediğiniz araçlar.
* Bir depoyu kopyalama gibi VM üzerinde-çalıştırmak istediğiniz eylemler.
* Test etmek istediğiniz uygulamalar.

Yapıt [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) dağıtımını gerçekleştirme ve yapılandırmayı uygulamak için yönergeleri içeren bir JSON dosyaları.

## <a name="artifact-repositories"></a>Yapıt deposu
Yapıt deposu burada yapıtları iade edildiği git depolarıdır ' dir. Yapıt deposu, yeniden etkinleştirmek ve paylaşımı kuruluşunuzdaki birden çok laboratuvarlara eklenebilir.

## <a name="formulas"></a>Formüller
Temel görüntüler, yanı sıra formüller hızlı VM sağlama için bir mekanizma sağlar. Bir formül DevTest Labs'de Laboratuvar sanal makinesi oluşturmak için kullanılan varsayılan özellik değerlerini listesidir.
Formüllerle özellikleri - temel görüntü, VM boyutunu, sanal ağ ve yapıtları - gibi aynı küme Vm'leriyle her zaman bu özellikler belirtmek gerek kalmadan oluşturulabilir. Bir formülle bir VM oluştururken, varsayılan değerleri olarak kullanılamaz-olduğu veya değiştirilemez.

## <a name="policies"></a>İlkeler
Laboratuvarınızda maliyet denetleme ilkeleri Yardım. Örneğin, Vm'leri tanımlanmış bir zamanlamaya göre otomatik olarak kapatmak için bir ilke oluşturabilirsiniz.

## <a name="caps"></a>CAPS
Büyük harf olan laboratuvarınızda israfı en aza indirmek için bir mekanizma. Örneğin, kullanıcı başına veya bir laboratuvarda oluşturulan VM'lerin sayısını sınırlamak için bir sınır ayarlayabilirsiniz.

## <a name="security-levels"></a>Güvenlik düzeyleri
Güvenlik erişimi, Azure rol tabanlı Access Control (RBAC) göre belirlenir. Erişimi nasıl çalıştığını anlamak için bu izin, bir rolü olan ve RBAC tarafından tanımlandığı şekilde bir kapsam farklarını anlamak için yardımcı olur.

* İzni - izin, belirli bir eylem (örneğin okuma tüm sanal makinelere erişimi) için tanımlanmış bir erişimdir.
* Rol - rol gruplandırılmış ve bir kullanıcıya atanmış izinler kümesidir. Örneğin, *abonelik sahibi* rolü bir Abonelikteki tüm kaynaklara erişebilir.
* Kapsam - kapsam, bir kaynak grubu, tek bir laboratuvar veya tüm abonelik gibi bir Azure kaynak hiyerarşi içinde düzeyidir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak için rolleri iki tür vardır: Laboratuvar sahibi ve Laboratuvar kullanıcı.

* Laboratuvar sahibi - Laboratuvar sahibi bir laboratuvar içindeki herhangi bir kaynağa erişebilir. Bu nedenle, Laboratuvar sahibi ilkeleri değiştirebilir, okuma ve yazma herhangi bir VM, sanal ağı değiştirin ve benzeri.
* Laboratuvar kullanıcı - Laboratuvar kullanıcı Vm'leri, ilkeleri ve sanal ağlar gibi tüm Laboratuvar kaynaklarını görüntüleyebilir ancak ilkeleri değiştirilemiyor veya herhangi bir VM, diğer kullanıcılar tarafından oluşturulmuş.

DevTest Labs'de özel roller oluşturmanızı da hakkında bilgi için makaleye bakın [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kapsamları hiyerarşik olduğundan, bir kullanıcı belirli bir kapsamda izinlere sahip olduğunda bunlar otomatik olarak dahil her alt düzey kapsamda bu izinleri verilir. Abonelik sahibi rolüne atanmış bir kullanıcı, örneğin, ardından tüm sanal makineler, tüm sanal ağları ve tüm laboratuvarlar içeren bir Abonelikteki tüm kaynaklara erişim izni sahiptirler. Bu nedenle, abonelik sahibi, Laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, bunun tersi doğru değildir. Laboratuvar sahibi abonelik düzeyinden daha düşük bir kapsam bir laboratuvar erişebilir. Bu nedenle, bir laboratuvar sahip sanal makineler veya sanal ağ veya Laboratuvar dışında olan kaynakları görmek mümkün olmayacaktır.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Tüm bu makalede ele alınan kavramları kullanarak Azure çözümünüzün altyapısını/yapılandırmasını tanımlamak ve tutarlı bir durumda sürekli dağıtıma olanak veren Azure Resource Manager şablonlarını kullanarak yapılandırılabilir.

[Azure Resource Manager şablonları, söz dizimi ve yapısı anlamak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) bir Azure Resource Manager şablonu ve bir şablonu farklı bölümlerde kullanılabilir olan özellikleri yapısını açıklar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
[DevTest Labs'de Laboratuvar oluşturma](devtest-lab-create-lab.md)
