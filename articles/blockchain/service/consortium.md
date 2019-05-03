---
title: Azure Blockchain hizmet Consortium
description: ''
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e745a4ee4789ef46a61b5cb0bbf806c41ef631ec
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027923"
---
# <a name="azure-blockchain-service-consortium"></a>Azure Blockchain hizmet Consortium

Azure Blockchain hizmetini kullanarak, blok zinciri ağları burada her blok zinciri ağ ağdaki belirli katılımcılara sınırlı olabilir özel consortium oluşturabilirsiniz. Yalnızca özel consortium blockchain ağ katılımcılar görebilir ve blok zinciri ile etkileşim. Azure Blockchain hizmet Consortium ağlarda iki tür üyesi katılımcı rollerini içerebilir:

* **Yönetici** -ayrıcalıklı consortium yönetim eylemleri gerçekleştirebilir ve blok zinciri işlemlerine katılabilmesi katılımcılar.

* **Kullanıcı** -herhangi bir konsorsiyum yönetim eylemi alınamaz ancak blok zinciri işlemlerine katılabilmesi katılımcılar.

Consortium ağları katılımcı rollerin bir karışımı olabilir ve her rol türünün rastgele bir sayı olabilir. En az bir yönetici olması gerekir.

Aşağıdaki diyagramda, birden fazla katılımcıları ile bir konsorsiyum ağı gösterilmektedir:

![Özel consortium Ağ Diyagramı](./media/consortium/network-diagram.png)

Azure Blockchain hizmetinde Consortium yönetimiyle Konsorsiyum ağı katılımcıları yönetebilirsiniz. Consortium yönetim ağının fikir birliğine varılmış modeline dayanır. Geçerli Önizleme sürümünde Azure blok zinciri hizmet consortium yönetimi için bir merkezi fikir birliğine varılmış modeli sağlar. Bir yönetici rolüne sahip herhangi bir ayrıcalıklı katılımcı ekleme veya ağdan katılımcıları kaldırma gibi consortium yönetim eylemleri alabilir.

## <a name="roles"></a>Roller

Konsorsiyumu katılımcıları, kişi ve kuruluşların olabilir ve bir kullanıcı rolü veya yönetici rolü atanabilir. Aşağıdaki tabloda, iki rol arasındaki üst düzey farklılıklar listelenmektedir:

| Eylem | Kullanıcı rolü | Yönetici rolü
|--------|:----:|:------------:|
| Yeni üye oluşturma | Evet | Evet |
| Yeni üyeler davet edin | Hayır | Evet |
| Katılımcı rolü üyesi veya | Hayır | Evet |
| Üye görünen adı değiştir | Yalnızca kendi üyesi için | Yalnızca kendi üyesi için |
| Üyeleri kaldır | Yalnızca kendi üyesi için | Evet |
| Blok zinciri işlemlere katılmasına | Evet | Evet |

### <a name="user-role"></a>Kullanıcı rolü

Hiçbir yönetim yetenekleri consortium katılımcılarıyla kullanıcılardır. Bunlar üyeleri için consortium ilgili yönetmek katılamaz. Kullanıcıların kendilerini Konsorsiyumu kaldırın ve kullanıcıların üye görünen adı değiştirebilirsiniz.

### <a name="administrator"></a>Yönetici

Bir yönetici, üyeleri consortium içinde yönetebilir. Yönetici üyeler davet üyeleri kaldırın veya üyeleri rolleri consortium içinde güncelleştirin.
Her zaman bir konsorsiyum içinde en az bir yönetici olmalıdır. Son yönetici başka bir katılımcının bir konsorsiyum ayrılmadan önce bir yönetici rolü olarak belirtmeniz gerekir.

## <a name="managing-members"></a>Üyeleri yönetme

Yalnızca Yöneticiler, diğer katılımcılara consortium davet edebilirsiniz. Yöneticiler, Azure abonelik kimliğini kullanarak katılımcıları davet

Davet sonra katılımcıları Azure blok zinciri hizmetinde yeni bir üye dağıtarak blok zinciri consortium katılabilirsiniz. Davet edilen consortium katılın ve görüntülemek için davette ağ yöneticisi tarafından kullanılan aynı Azure aboneliği kimliği belirtmeniz gerekir.

Yöneticiler, diğer yöneticiler dahil consortium herhangi bir katılımcı kaldırabilirsiniz. Üyeleri yalnızca kendilerini Konsorsiyumu kaldırabilirsiniz.

## <a name="consortium-management-smart-contract"></a>Consortium yönetim akıllı Sözleşmesi

Azure Blockchain hizmetinde Consortium Yönetimi consortium yönetim akıllı sözleşmeleri aracılığıyla gerçekleştirilir. Yeni bir blok zinciri üye dağıttığınızda nitelikli akıllı anlaşmalar düğümlerinizi için otomatik olarak dağıtılır.

Kök consortium yönetim akıllı sözleşme adresini Azure portalında görüntülenebilir. **RootContract adresi** blok zinciri üyesinin genel bakış bölümünde.

![RootContract adresi](./media/consortium/rootcontract-address.png)

Consortium Yönetimi'ni kullanarak consortium yönetim akıllı sözleşme ile etkileşim kurabilir [PowerShell Modülü](manage-consortium-powershell.md), Azure portal veya kullanarak akıllı sözleşme aracılığıyla doğrudan Azure blok zinciri hizmeti Ethereum oluşturulan hesabı.

## <a name="ethereum-account"></a>Ethereum hesabı

Bir üyesi oluşturulduğunda bir Ethereum hesap anahtarı oluşturulur. Azure Blockchain hizmeti anahtarı consortium yönetimiyle ilgili işlemler oluşturmak için kullanır. Ethereum hesap anahtarı Azure blok zinciri hizmeti tarafından otomatik olarak yönetilir.

Üye hesabı, Azure portalında görüntülenebilir. Blok zinciri üyesinin genel bakış bölümünde üye hesaptır.

![Üye hesabı](./media/consortium/member-account.png)

Üye hesabınıza tıklayarak ve yeni bir parola girerek Ethereum hesabınızı sıfırlayabilirsiniz. Hem Ethereum hesap adresi ve parola sıfırlanır.  

## <a name="next-steps"></a>Sonraki adımlar

[PowerShell kullanarak Azure blok zinciri hizmeti üyeleri yönetme](manage-consortium-powershell.md)
