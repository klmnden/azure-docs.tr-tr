---
title: "DevTest Labs kavramları | Microsoft Docs"
description: "DevTest Labs ve nasıl, oluşturmak, yönetmek ve Azure sanal makinelerini izlemek kolaylaştırmak temel kavramları hakkında bilgi"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: 46271c1122df852b37d4117f9d4008fd74f43d95
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="devtest-labs-concepts"></a>DevTest Labs kavramları
## <a name="overview"></a>Genel Bakış
Aşağıdaki liste, anahtar DevTest Labs kavramları ve tanımları içerir:

## <a name="labs"></a>Labs
Sınırları ve kotalar belirterek daha iyi olanak sağlayan sanal makineler (VM'ler), bu kaynakları yönetmek gibi bir laboratuvar kaynakların bir grubu kapsar altyapısıdır.

## <a name="virtual-machine"></a>Sanal makine
Bir Azure VM çeşitli türlerde biri [isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları](https://docs.microsoft.com/azure/app-service/choose-web-site-cloud-service-vm) Azure'un sunduğu. Azure VM'ler satın almak ve hala yapılandırma, düzeltme eki uygulama ve bunun üzerinde çalıştırılır yazılım yükleme gibi belirli görevleri gerçekleştirerek VM korumak gerek olmasa da, çalıştıran fiziksel donanımı sürdürmek zorunda kalmadan sanallaştırma esnekliği sağlar.

[Azure'da Windows sanal makineler genel bakış](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) verir önce dikkate almanız gerekenler hakkında bilgi bir VM, nasıl oluşturduğunuz ve yönettiğiniz nasıl oluşturma.

## <a name="claimable-vm"></a>Claimable VM
Bir Azure Claimable VM izinlerine sahip herhangi bir laboratuvar kullanıcı tarafından kullanılabilir olan bir sanal makinedir. Laboratuvar Yönetim VM'ler belirli taban görüntüleri ve yapıları hazırlamak ve bunları bir paylaşılan havuzuna kaydedin. Bu yapılandırmaya sahip bir gerektiğinde bir laboratuvar kullanıcı sonra çalışan bir VM havuzundan talep edebilir.

Claimable bir VM başlangıçta belirli bir kullanıcıya atanmış olmamalıdır, ancak her kullanıcının listesinde "Claimable sanal makineler" altında gösterilir. VM bir kullanıcı tarafından talep sonra kadar taşınır kendi "Benim sanal makineler" alanına ve artık herhangi bir kullanıcı tarafından claimable değil.

## <a name="environment"></a>Ortam
DevTest Labs'de bir ortamda bir laboratuvarda Azure kaynağı koleksiyonunu ifade eder. [Bu blog gönderisine](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) Azure Resource Manager şablonlarınızı çoklu VM ortamları oluşturma açıklanır.

## <a name="base-images"></a>Taban görüntüleri
Temel görüntüleri tüm araçlar ve hızlı bir şekilde bir VM oluşturmak için önceden yüklenmiş ve yapılandırılmış ayarlar ile VM görüntüleri bağımsızdır. Bir VM var olan bir temel çekme ve test aracısı yüklemek için bir yapı ekleyerek sağlayabilirsiniz. Böylece her VM sağlamak için test aracıyı yeniden yüklemek zorunda kalmadan temel kullanılabilir sağlanan VM'e sonra temel olarak kaydedebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Yapılar, dağıtmak ve bir VM sağlandıktan sonra Uygulamanızı yapılandırmak için kullanılır. Yapıtlar şunlar olabilir:

* Aracılar, Fiddler ve Visual Studio gibi VM - yüklemek istediğiniz araçları.
* Bir depoyu kopyalama gibi VM üzerinde-çalıştırmak istediğiniz eylemleri.
* Test etmek istediğiniz uygulamalar.

Ürünleridir [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) dağıtım gerçekleştirmek ve yapılandırmayı uygulamak için yönergeler içeren JSON dosyaları.

## <a name="artifact-repositories"></a>Yapı depoları
Yapı depoları yapıları burada iade git depoları ' dir. Yapı depoları, yeniden etkinleştirme ve paylaşımı, kuruluşunuzda birden çok laboratuarlara eklenebilir.

## <a name="formulas"></a>Formüller
Taban görüntüleri yanı sıra formüller hızlı VM sağlama için bir mekanizma sağlar. DevTest Labs formülde bir laboratuvar VM oluşturmak için kullanılan varsayılan özellik değerleri listesidir.
Formüllerle, sanal makineleri aynı kümesi özelliklerinin - temel görüntü, VM boyutunu, sanal ağ ve yapıları - gibi bu özellikleri her zaman belirtmek zorunda kalmadan oluşturulabilir. VM bir formülünden oluştururken, varsayılan değerleri olarak kullanılamaz-olduğundan veya değiştirilemiyor.

## <a name="policies"></a>İlkeler
İlkeleri laboratuvarınızda maliyetini denetleme yardımcı olur. Örneğin, sanal makineleri tanımlanmış bir zamanlamaya göre otomatik olarak kapatmak için bir ilke oluşturabilirsiniz.

## <a name="caps"></a>Büyük harfler
Büyük, atık laboratuvarınızda en aza indirmek için bir mekanizma olur. Örneğin, kullanıcı başına veya bir laboratuvarda oluşturulan VM sayısını sınırlamak için bir uç ayarlayabilirsiniz.

## <a name="security-levels"></a>Güvenlik düzeyleri
Güvenlik erişimi Azure rol tabanlı erişim denetimi (RBAC) tarafından belirlenir. Erişim nasıl çalıştığını anlamak için bu izin, rol ve RBAC tarafından tanımlandığı şekilde bir kapsam arasındaki farkları anlamak için yardımcı olur.

* Belirli bir eylemi (örneğin okuma tüm sanal makinelere erişim) için tanımlanmış bir erişim izni - izni yok.
* Rol - bir rolü gruplandırılır ve bir kullanıcıya atanan izinler kümesidir. Örneğin, *abonelik sahibi* rolüne sahip bir abonelik içindeki tüm kaynaklara erişim izni.
* Kapsamı - kapsam düzeyinde bir kaynak grubu, tek bir laboratuvar veya tüm abonelik gibi bir Azure kaynak hiyerarşi içinde değil.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak için rolleri iki tür vardır: Laboratuvar sahip ve Laboratuvar kullanıcı.

* Laboratuvar sahibi - bir laboratuvar sahibi Laboratuvar içinde herhangi bir kaynağa erişebilir. Bu nedenle, bir laboratuvar sahibi ilkeleri değiştirmek, okuma ve herhangi bir VM yazma, sanal ağı değiştirin ve benzeri.
* Laboratuvar kullanıcı - Laboratuvar kullanıcı VM'ler, ilkeleri ve sanal ağlar gibi tüm Laboratuvar kaynaklarını görüntüleyebilir ancak ilkeleri değiştiremez veya herhangi bir VM diğer kullanıcılar tarafından oluşturulan.

DevTest Labs'de özel roller oluşturma görmek için makaleyi için bakın [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kapsamları hiyerarşik olduğundan, bir kullanıcının belirli bir kapsamda izinleri olduğunda otomatik olarak çevrelenmiş her alt düzey kapsamındaki bu izinler verilir. Bir kullanıcı abonelik sahibi rolüne atanırsa, örneğin, ardından tüm sanal makineleri, tüm sanal ağları ve tüm labs içeren bir Abonelikteki tüm kaynaklar erişimi. Bu nedenle, abonelik sahibi, Laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, bunun tersi geçerli değildir. Bir laboratuvar sahibi abonelik düzeyinden daha düşük bir kapsam bir laboratuvar erişebilir. Bu nedenle, bir laboratuvar sahip sanal makine veya sanal ağlar ya da Laboratuvar dışında olan herhangi bir kaynağa görmeye olmaz.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Bu makalede açıklanan kavramları tümünün Azure çözümünüzün altyapı/yapılandırmasını tanımlayın ve sürekli olarak tutarlı bir durumda dağıtmanıza olanak sağlayan Azure Resource Manager şablonları kullanarak yapılandırılabilir.

[Yapı ve Azure Resource Manager şablonları sözdizimini anlamanız](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) yapısını bir Azure Resource Manager şablonu ve bir şablonu farklı bölümlerde özellikleri açıklar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
[DevTest Labs'de Laboratuvar oluşturma](devtest-lab-create-lab.md)
