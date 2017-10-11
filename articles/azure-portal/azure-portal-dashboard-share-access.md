---
title: "RBAC kullanarak Azure portalı panoları paylaşmak | Microsoft Docs"
description: "Bu makalede, Azure portal panosunda rol tabanlı erişim denetimi kullanarak paylaşmak açıklanmaktadır."
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: ea0cf7ad074f95c2b49a92f9a8e32270a1d39b3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanarak Azure panoları paylaşmak
Bir Pano yapılandırdıktan sonra yayımlayabilir ve kuruluşunuzdaki diğer kullanıcılarla paylaşmak. Diğerlerinin Azure kullanarak panonuz görüntülemesine izin vermek [rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md). Bir kullanıcı veya kullanıcılar grubunun bir role atayın ve bu rol kullanıcılarla değiştirebilir veya görüntüleyebilirsiniz yayımlanan Pano olup olmadığını tanımlar. 

Tüm yayımlanan panolar Azure kaynakları yönetilebilir öğeleri aboneliğinizi içinde mevcut ve bir kaynak grubunda bulunan yani uygulanır.  Bir erişim denetimi açısından bakıldığında, panolar sanal makine ya da bir depolama hesabı gibi başka kaynaklar gereksinimlerinden farklı değildir.

> [!TIP]
> Bir Pano üzerinde tek tek döşeme görüntüledikleri kaynaklara göre kendi erişim denetimi gereksinimlerine uygulayın.  Bu nedenle, geniş çapta hala tek tek döşeme verileri korurken paylaşılan bir Pano tasarlayabilirsiniz.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Panolar için erişim Denetimi'ni anlama
Rol tabanlı erişim denetimi (RBAC) ile kullanıcıları kapsamının üç farklı düzeylerde roller atayabilirsiniz:

* aboneliği
* kaynak grubu
* Kaynak

Atadığınız izinler kaynak aşağıya doğru abonelikten devralınır. Yayımlanan Pano bir kaynaktır. Bu nedenle, aynı zamanda yayımlanan Pano çalışan rollerine abonelik için atanan kullanıcılar zaten sahip olabilir. 

Bir örnek verilmiştir.  Bir Azure aboneliğiniz varsa ve çeşitli ekibinizin üyeleri rollerini atamış olduğunuz düşünelim **sahibi**, **katkıda bulunan**, veya **okuyucu** abonelik için. Sahipler veya katkıda bulunanlar kullanıcılar listesinde, görüntülemek, oluşturmak, değiştirmek veya abonelik içindeki panolar silmek kullanabilirsiniz.  Okuyucular kullanıcılar listesi ve görünümü panolarına kullanabilirsiniz ancak değiştirin veya silin.  Okuyucu erişimi olan kullanıcılar için yayımlanan bir Pano yerel düzenlemeler mümkün (gibi bir sorun gidermede), ancak bu değişiklikleri geri sunucuya yayımlayın mümkün değildir.  Kendileri için panoyu özel bir kopyasını oluşturmak için seçeneği sağlanır.

Ancak, aynı zamanda izinleri birkaç panolar içeren kaynak grubunu veya tek bir Pano atayabilirsiniz. Örneğin, bir kullanıcı grubuna izinleri abonelik ancak belirli bir Panoda daha fazla erişim arasında sınırlı olmalıdır, karar verebilirsiniz. Kullanıcılar bu Pano için bir rol atayın. 

## <a name="publish-dashboard"></a>Pano yayımlama
Şimdi, aboneliğinizde kullanıcı grubuyla paylaşmak istediğiniz bir Pano yapılandırmayı tamamladıktan varsayalım. Depolama yöneticileri adlı özel bir grup aşağıdaki adımları tarif, ancak ne olursa olsun ister misiniz grubunuzun adı verebilirsiniz. Bir Active Directory grubu oluşturma ve bu gruba kullanıcı ekleme hakkında daha fazla bilgi için bkz: [Azure Active Directory içinde grupları yönetme](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. Panoda seçin **paylaşımı**.
   
     ![Paylaşımı seçin](./media/azure-portal-dashboard-share-access/select-share.png)
2. Erişim vermeden önce Pano yayımlamanız gerekir. Varsayılan olarak, bir kaynak grubu için panoyu yayımlanacak **panolar**. Seçin **yayımlama**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Panonuz artık yayımlanır. Abonelikten devralınan izinleri uygun varsa, başka bir işlem yapmanız gerekmez. Kuruluşunuzdaki diğer kullanıcılar erişebilir ve kendi abonelik düzeyi rolüne dayalı Pano değiştirme olacaktır. Ancak, bu öğreticide, şirketinizdeki kullanıcılar grubunun bu Pano için bir rol atayın.

## <a name="assign-access-to-a-dashboard"></a>Bir panoya erişimi atayın
1. Pano yayımlandıktan sonra Seç **kullanıcıları yönetme**.
   
     ![Kullanıcıları yönetme](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Bu Pano için bir rol zaten atanmış olan kullanıcıların bir listesini görürsünüz. Varolan kullanıcılar listesi aşağıdaki görüntü farklı olacaktır. Büyük olasılıkla atamaları abonelikten devralınır. Yeni bir kullanıcı veya grup eklemek için seçin **Ekle**.
   
     ![Kullanıcı Ekle](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Vermek istediğiniz izinleri temsil eden rolü seçin. Bu örnek için select **katkıda bulunan**.
   
     ![Rol Seç](./media/azure-portal-dashboard-share-access/select-role.png)
4. Kullanıcı veya role atamak istediğiniz grubu seçin. Kullanıcı veya grup listede aradığınız görmüyorsanız arama kutusunu kullanın. Kullanılabilir grupları listesi, Active Directory'de oluşturulan grupları bağlıdır.
   
     ![Kullanıcı Seç](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Kullanıcıları veya grupları eklemeyi tamamladığınızda seçin **Tamam**. 
6. Yeni atama kullanıcılar listesine eklenir. Şunlara dikkat edin, **erişim** olarak listelenen **atanan** yerine **devralınan**.
   
     ![atanan roller](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Sonraki adımlar
* Rollerinin listesi için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md).
* Kaynakları yönetme hakkında bilgi edinmek için [yönetmek Azure kaynakları portal üzerinden](resource-group-portal.md).

