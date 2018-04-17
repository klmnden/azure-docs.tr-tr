---
title: Oluşturma ve Azure portalı panoları paylaşma | Microsoft Docs
description: Bu makalede, oluşturma ve Azure portalında panolar düzenleme açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 4e5a5ae944b5f0059ee78a2171a9688902aaf6db
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Oluşturma ve Azure portalındaki pano paylaşma
Birden çok panolar oluşturun ve bunları Azure aboneliklerinize erişimi başkalarıyla paylaşabilir.  Bu makalede oluşturma, düzenleme, yayımlama ve panolar erişimi yönetme temellerini geçer.

## <a name="create-a-dashboard"></a>Bir pano oluşturun
Bir Pano oluşturmak için seçin **yeni Pano** geçerli Panodaki adının yanındaki düğmesi.  

![bir pano oluşturun](./media/azure-portal-dashboards/new-dashboard.png)

Bu eylem, yeni, boş, özel bir Pano oluşturur ve burada panonuzu adlandırın ve ekleyebilir veya kutucukları yeniden özelleştirme moduna geçirir.  Bu modda olduğunda, sol gezinti menüsü daraltılabilir döşeme galeri alır.  Döşeme galeri Azure kaynaklarınızın çeşitli şekillerde döşeme bulmanıza olanak sağlar: tarafından göz atabilirsiniz [kaynak grubu](../azure-resource-manager/resource-group-overview.md#resource-groups), göre kaynak türü, göre [etiketi](../azure-resource-manager/resource-group-using-tags.md), ya da kaynak ada göre arayarak.  

![Pano özelleştirme](./media/azure-portal-dashboards/customize-dashboard.png)

Döşeme sürükleyip istediğiniz yere Pano yüzeyine bırakarak ekleyin.

Yeni bir kategori yok **genel** belirli bir kaynakla ilişkili olmayan kutucuklar için.  Bu örnekte, Markdown kutucuğu sabitleyin.  Özel içerik panonuza eklemek için bu kutucuğu kullanın.  Düz metin kutucuğu destekleyen [Markdown söz dizimi](https://daringfireball.net/projects/markdown/syntax)ve HTML sınırlı sayıda.  (Güvenlik için ekleme gibi işlemler yapamayacağı `<script>` etiketler veya belirli portalıyla engelleyebilmesi CSS stil öğesi kullanın.) 

![markdown Ekle](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Bir Pano Düzenle
Panonuz oluşturduktan sonra kutucukları döşeme galeri veya dikey pencereleri döşe gösterimini sabitleyebilirsiniz. Şimdi, kaynak grubu gösterimini sabitleyin. Öğe göz atarken veya kaynak grubu dikey ya da PIN olabilir. Her iki yaklaşım, kaynak grubu döşeme gösterimini sabitleme neden.

![Panoya Sabitle](./media/azure-portal-dashboards/pin-to-dashboard.png)

Öğeyi sabitleme sonra Panonuzda görüntülenir.

![Görünüm Panosu](./media/azure-portal-dashboards/view-dashboard.png)

Bir Markdown döşeme sahibiz ve bir kaynak grubu panosuna sabitlediğiniz göre biz yeniden boyutlandırma ve uygun bir düzen kutucukları yeniden.

Vurgulama ve seçerek "..." veya bir kutucuğa sağ tıklayarak bu kutucuğu tüm bağlamsal komutlarını görebilirsiniz. Varsayılan olarak, iki öğe vardır:

1. **Panodan sabitleme** – kutucuğu panodan kaldırır
2. **Özelleştirme** – girer modu özelleştirme

![Döşeme özelleştirme](./media/azure-portal-dashboards/customize-tile.png)

Seçerek özelleştirme, yeniden boyutlandırma ve kutucukları yeniden Sırala. Bir kutucuk yeniden boyutlandırmak için aşağıdaki görüntüde gösterildiği gibi bağlamsal menüsünden Yeni boyutunu seçin.

![Döşeme yeniden boyutlandırma](./media/azure-portal-dashboards/resize-tile.png)

Ya da döşeme herhangi bir boyutta destekliyorsa, istenen boyuta alt sağ köşesinde sürükleyebilirsiniz.

![Döşeme yeniden boyutlandırma](./media/azure-portal-dashboards/resize-corner.png)

Kutucukları yeniden boyutlandırma sonra Pano görüntüleyin.

![Görünümü kutucuğu](./media/azure-portal-dashboards/view-tile.png)

Bir Pano özelleştirme bittiğinde seçmeniz yeterlidir **özelleştirme Bitti** çıkmak için özelleştirme moduna veya sağ tıklatın ve seçin **özelleştirme Bitti** ve bağlam menüsünden.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Bir Pano yayımlayın ve erişim denetimi yönetin
Bir Pano oluşturduğunuzda, görebileceğiniz tek kişi olduğunuz anlamına gelir varsayılan olarak özeldir.  Başkaları için görünür yapmak için kullanın **paylaşımı** yanı sıra diğer Pano komutları görünür düğmesi.

![Pano paylaşımı](./media/azure-portal-dashboards/share-dashboard.png)

Abonelik ve kaynak grubu için yayımlanması panonuz için seçim istenir. (Size bir e-posta adresi yazarak paylaşamaz şekilde) sorunsuz bir şekilde panolar ekosistemi tümleştirmek için paylaşılan panoları Azure kaynaklarını uyguladık.  Portal döşemeleri çoğu tarafından görüntülenen bilgilere erişim tarafından yönetilir [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md). Bir erişim denetimi açısından bakıldığında, paylaşılan Pano sanal makine ya da bir depolama hesabı hiç farklı değildir.  

Bir Azure aboneliğiniz varsa ve ekibinizin üyeleri rolleri atanmış düşünelim **sahibi**, **katkıda bulunan**, veya **okuyucu** abonelik.  Sahipler veya katkıda bulunanlar kullanıcılar listesinde, görüntülemek, oluşturmak, değiştirmek veya bu abonelik içindeki panolar silmek kullanabilirsiniz.  Okuyucular kullanıcılar listesi ve görünümü panolarına kullanabilirsiniz ancak değiştirin veya silin.  Okuyucu erişimi olan kullanıcılar bir paylaşılan Pano yerel düzenlemeler yapabilir, ancak bu değişiklikleri geri sunucuya yayımlayın mümkün değildir.  Ancak, bunlar Pano kendi kullanmak için özel bir kopyasını yapabilirsiniz.  Her zaman olduğu gibi tek tek döşeme Panoda için karşılık gelen kaynaklara göre kendi erişim denetim kurallarını uygulayın.  

Kolaylık olması için portal, bir kaynak grubunda panolar nereye bir desen doğru olarak adlandırılan deneyimi kılavuzları yayımlama kullanıcının **panolar**.  

![Pano yayımlama](./media/azure-portal-dashboards/publish-dashboard.png)

Belirli bir kaynak grubunu için bir Pano yayımlamayı seçebilirsiniz.  Bu Pano için erişim denetimi, kaynak grubu için erişim denetimi ile eşleşir.  Bu kaynak grubundaki kaynakları yönetebilir kullanıcılar da panoları erişiminiz.

![kaynak grubu için Pano yayımlama](./media/azure-portal-dashboards/publish-to-resource-group.png)

Panonuz yayımlandıktan sonra **paylaşım + erişim** denetim bölmesinde yenileyin ve Pano kullanıcı erişimini yönetmek için bir bağlantı da dahil olmak üzere yayımlanan Pano hakkında bilgi gösterir.  Bu bağlantı, herhangi bir Azure kaynak erişimi yönetmek için kullanılan standart rol tabanlı erişim denetimi dikey penceresini başlatır.  Her zaman geri bu görünüme seçerek alabilirsiniz **paylaşımı**.

![erişim denetimi yönetme](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kaynakları yönetmek için bkz: [yönetmek Azure kaynakları portal üzerinden](../azure-resource-manager/resource-group-portal.md).
* Kaynakları dağıtmak için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md).

