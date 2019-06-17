---
title: RBAC kullanarak Azure portalı panoları paylaşma | Microsoft Docs
description: Bu makalede, rol tabanlı erişim denetimi kullanarak Azure portalında bir panoyu paylaşmak açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: fbbc8a4f636a95d18baa0dc5de541279ce36789b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60552015"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanarak Azure panoları paylaşma
Bir Pano yapılandırdıktan sonra uygulamayı yayımlayın ve kuruluşunuzdaki diğer kullanıcılarla paylaşabilirsiniz. Diğerlerinin Azure kullanarak panonuza görüntülemesine izin vermek [rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md). Bir role bir kullanıcı veya kullanıcı grubuna atayın ve bu role kullanıcılar görüntüleyebilir veya değiştirebilirsiniz Panosu yayımlandı olup olmadığını tanımlar. 

Yayımlanan tüm panoların, aboneliğiniz kapsamındaki yönetilebilir öğeleri olarak mevcut ve bir kaynak grubunda bulunan anlamına gelir, Azure kaynaklarının uygulanır.  Bir erişim denetimi açısından bakıldığında, panolar bir sanal makine veya bir depolama hesabı gibi diğer kaynaklar gereksinimlerinden farklı değildir.

> [!TIP]
> Panoda bulunan tek tek kutucuklara görüntüledikleri kaynaklara göre kendi erişim denetimi gereksinimleri uygular.  Bu nedenle, geniş çapta hala tek tek kutucuklara verileri korurken paylaşılan bir panoyu tasarlayabilirsiniz.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Panolar için erişim denetimi anlama
Rol tabanlı erişim denetimi (RBAC) ile kullanıcılar, rollerine kapsam üç farklı düzeylerde atayabilirsiniz:

* aboneliği
* kaynak grubu
* resource

Kaynak aşağı abonelikten atadığınız izinler devralınmıştır. Yayımlanan Pano bir kaynaktır. Bu nedenle, yayımlanmış panosunu da çalışan rollerine abonelik için atanan kullanıcılar zaten olabilir. 

Bir örnek aşağıda verilmiştir.  Diyelim ki bir Azure aboneliğiniz varsa ve takımınızın üyelerinin çeşitli rolleri atanmış **sahibi**, **katkıda bulunan**, veya **okuyucu** abonelik için. Sahip veya katkıda bulunan kullanıcılar listesinde, görüntülemek, oluşturmak, değiştirmek veya abonelik içindeki panoları silmesine olanağına sahip olursunuz.  Okuyucular kullanıcılar edebilir listeleyip görüntüleyin panolara, ancak değiştiremez veya silemezsiniz.  Okuyucu erişimi olan kullanıcılar, yayımlanmış bir panoya yerel düzenlemeler yapabilir (gibi bir sorun gidermede), ancak bu değişiklikleri sunucuya geri yayımlamak mümkün değildir.  Bunlar, kendileri için özel bir panonun kopyasını yapma seçeneğine sahip olursunuz

Ancak, ayrıca izinler birçok paneliniz içeren kaynak grubunu veya ayrı bir panoya atayabilirsiniz. Örneğin, bir kullanıcı grubu izinleri abonelik ancak daha fazla erişim belirli bir Pano üzerinde sınırlı olmalıdır, karar verebilirsiniz. Bu Pano için bir rol için bu kullanıcılara lisans atayın. 

## <a name="publish-dashboard"></a>Panoyu yayımlama
Aboneliğinizde bir kullanıcı grubuyla paylaşmak istediğiniz bir Pano yapılandırma tamamlandı varsayalım. Aşağıdaki adımlar, depolama yöneticileri olarak adlandırılan özel bir grup tarif, ancak ne olursa olsun istediğiniz grubunuzun adı verebilirsiniz. Bir Active Directory grubu oluşturma ve bu gruba kullanıcı ekleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'de grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

1. Panoda seçin **paylaşımı**.
   
     ![Paylaşımı seçin](./media/azure-portal-dashboard-share-access/select-share.png)
2. Erişim vermeden önce Pano yayımlamanız gerekir. Varsayılan olarak, adlı bir kaynak grubuna Pano yayımlanacak **panolar**. **Yayımla**’yı seçin.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Panonuz artık yayımlanır. Abonelikten devralınmış izinlere uygun olup olmadığını, daha fazla şey gerekmez. Kuruluşunuzdaki diğer kullanıcılara erişim ve abonelik düzeyinde rol tabanlı Pano değiştirmek mümkün olacaktır. Ancak, Bu öğretici için şimdi bir kullanıcı grubu için ilgili Pano bir role atayın.

## <a name="assign-access-to-a-dashboard"></a>Bir panoya erişim atama
1. Pano yayımladıktan sonra seçin **kullanıcıları yönetme**.
   
     ![Kullanıcıları yönetme](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Bu Pano için bir rol zaten atanmış olan mevcut kullanıcıların listesini görürsünüz. Mevcut kullanıcıların listesini, aşağıdaki resimde farklı olacaktır. Büyük olasılıkla atamaları abonelikten devralınır. Yeni bir kullanıcı veya grup eklemek için seçin **Ekle**.
   
     ![Kullanıcı Ekle](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Vermek istediğiniz izinleri temsil eden rolü seçin. Bu örnekte, seçin **katkıda bulunan**.
   
     ![rol seçin](./media/azure-portal-dashboard-share-access/select-role.png)
4. Kullanıcı veya role atamak istediğiniz grubu seçin. Kullanıcı veya Grup listesinde aradığınız görmüyorsanız, arama kutusunu kullanın. Kullanılabilir gruplar listesi, Active Directory'de oluşturduğunuz gruplar bağlıdır.
   
     ![kullanıcı seçin](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Kullanıcıları veya grupları eklemeyi tamamladığınızda, seçin **Tamam**. 
6. Yeni atama kullanıcıları listesine eklenir. Dikkat edin, **erişim** olarak listelenen **atanan** yerine **devralınan**.
   
     ![atanan roller](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Sonraki adımlar
* Rollerinin bir listesi için bkz. [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md).
* Kaynakları yönetme hakkında daha fazla bilgi için bkz: [Azure kaynaklarınızı portal üzerinden yönetme](resource-group-portal.md).

