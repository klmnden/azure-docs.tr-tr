---
title: Azure portalı panoları oluşturma ve paylaşma | Microsoft Docs
description: Bu makalede, Azure Portalındaki Panolar oluşturup açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: doubeby
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: kfollis
ms.openlocfilehash: 693e973fb988a57c15b4ea2fae47f16b4ff39011
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60552706"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Azure portalında panoları oluşturma ve paylaşma
Birden çok Pano oluşturma ve Azure aboneliklerinizi erişimi olan diğer kişilerle paylaşabilirsiniz.  Bu makalede, oluşturma, düzenleme, yayımlama ve Pano erişimini yönetme temelleri gider.

## <a name="create-a-dashboard"></a>Bir pano oluşturma
Bir pano oluşturmak için geçerli pano adının yanındaki **Yeni pano** düğmesini seçin.  

![Pano oluşturma](./media/azure-portal-dashboards/new-dashboard.png)

Bu işlem yeni, boş ve özel bir pano oluşturur ve panonuzu adlandırıp kutucuklar ekleyebileceğiniz ya da yeniden düzenleyebileceğiniz özelleştirme modunu açar.  Bu modda, sol gezinti menüsü daraltılabilir kutucuk Galerisi alır.  Kutucuk Galerisi çeşitli yollarla Azure kaynaklarınız için kutucuklar bulmanıza olanak tanır: göre göz atabilirsiniz [kaynak grubu](../azure-resource-manager/resource-group-overview.md#resource-groups)kullanarak, kaynak türü, göre [etiketi](../azure-resource-manager/resource-group-using-tags.md), veya bir kaynak adına göre arama yapın.  

![Panoyu özelleştirin](./media/azure-portal-dashboards/customize-dashboard.png)

Kutucuklar, sürükleyip istediğiniz yere Pano yüzeyi bırakarak ekleyin.

Adlı yeni bir kategori yok **genel** belirli bir kaynakla ilişkili olmayan kutucukları için.  Bu örnekte biz Markdown kutucuğu sabitleyebilirsiniz.  Bu kutucuk, panonuza özel içerik eklemek için kullanın.  Düz metin, kutucuğu destekler [Markdown söz dizimi](https://daringfireball.net/projects/markdown/syntax)ve HTML sınırlı bir dizi.  (Ekleme gibi işlemler yapamayacağınız güvenliği için `<script>` etiketleri veya belirli portalıyla engelleyebilmesi CSS stil öğesi kullanın.) 

![markdown'ı Ekle](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Bir panoyu Düzenle
Pano oluşturduktan sonra kutucukları kutucuk Galerisi ya da dikey pencereleri döşe gösterimini sabitleyebilirsiniz. Şimdi kaynak grubumuz gösterimini sabitleyin. Öğe göz atarken veya kaynak grubu dikey penceresinden ya da PIN oluşturabilirsiniz. Her iki yaklaşım, kaynak grubunu kutucuk gösterimini sabitleme neden.

![Panoya Sabitle](./media/azure-portal-dashboards/pin-to-dashboard.png)

Öğeyi sabitleme sonra Panonuzda görüntülenir.

![Panoyu görüntüle](./media/azure-portal-dashboards/view-dashboard.png)

Markdown kutucuğu sunuyoruz ve bir kaynak grubu panoya sabitlenmiş göre biz yeniden boyutlandırabilir ve kutucuklar uygun bir düzen yeniden düzenleyin.

Vurgulama ve seçerek "..." veya bir kutucuğa sağ tıklayarak ilgili kutucuğa bağlamsal tüm komutları görebilirsiniz. Varsayılan olarak, iki öğe vardır:

1. **Panodan Kaldır** – kutucuğu panodan kaldırır
2. **Özelleştirme** – girer modu özelleştirme

![kutucuğu özelleştirme](./media/azure-portal-dashboards/customize-tile.png)

Seçerek özelleştirme, yeniden boyutlandırabilir ve kutucukları. Bir kutucuğu yeniden boyutlandırmak için aşağıdaki görüntüde gösterildiği gibi bağlam menüsünden Yeni boyutunu seçin.

![kutucuğu yeniden boyutlandırma](./media/azure-portal-dashboards/resize-tile.png)

Veya herhangi bir boyutta kutucuğu destekliyorsa, istenen boyuta sağ alt köşedeki sürükleyebilirsiniz.

![kutucuğu yeniden boyutlandırma](./media/azure-portal-dashboards/resize-corner.png)

Kutucukları yeniden boyutlandırıldıktan sonra panoyu görüntüleyebilir.

![kutucuğu görüntüle](./media/azure-portal-dashboards/view-tile.png)

Bir panoyu özelleştirme tamamladıktan sonra seçmeniz yeterlidir **özelleştirme Bitti** çıkmak için özelleştirme moduna veya sağ tıklayıp **özelleştirme Bitti** bağlam menüsünden.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Bir panoyu yayımlama ve erişim denetimini yönetme
Bir pano oluşturulduğunda varsayılan olarak gizlidir, yani onu yalnızca siz görebilirsiniz.  Panoyu başkalarının görebilmesi için, diğer pano komutlarıyla birlikte görünen **Paylaş** düğmesini kullanın.

![Panoyu Paylaş](./media/azure-portal-dashboards/share-dashboard.png)

Panonuzun yayımlanması için bir abonelik ve kaynak grubu seçmeniz istenir. (Bir e-posta adresini yazarak paylaşamazsınız şekilde) panolar ekosistemi ile sorunsuz bir şekilde tümleştirmek için paylaşılan panolar Azure kaynaklarını uyguladık.  Portalda kutucukları çoğu tarafından görüntülenen bilgilere erişim tabidir [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md). Bir erişim denetimi açısından bakıldığında, paylaşılan panoları farklı bir sanal makine ya da depolama hesabı yok.  

Diyelim ki bir Azure aboneliğiniz varsa ve takımınızın üyelerinin rollerine atanmış **sahibi**, **katkıda bulunan**, veya **okuyucu** abonelik.  Sahip veya katkıda bulunan kullanıcılar listesinde, görüntülemek, oluşturmak, değiştirmek veya bu Abonelikteki panoları silmesine olanağına sahip olursunuz.  Okuyucular kullanıcılar edebilir listeleyip görüntüleyin panolara, ancak değiştiremez veya silemezsiniz.  Okuyucu erişimi olan kullanıcılar için paylaşılan bir panoyu yerel düzenlemeler yapabilir, ancak bu değişiklikleri sunucuya geri yayımlamak mümkün değildir.  Ancak, bunlar panonun kendi kullanımları için özel bir kopyasını yapabilirsiniz.  Her zaman olduğu gibi tek tek kutucuklara Panoda için karşılık gelen kaynakları göre kendi erişim denetim kuralları uygular.  

Kolaylık olması için portalın yayımlama deneyimi, bir kaynak grubuna panoları yerleştirdiğiniz **panolar** adlı bir modelde size kılavuzluk eder.  

![Panoyu yayımlama](./media/azure-portal-dashboards/publish-dashboard.png)

Belirli bir kaynak grubu için bir Pano yayımlamayı seçebilirsiniz.  Bu Pano için erişim denetimi, kaynak grubu için erişim denetimi ile eşleşir.  Bu kaynak grubundaki kaynakları yönetebilir kullanıcılar panoları da erişebilir.

![kaynak grubu için panoyu yayımlama](./media/azure-portal-dashboards/publish-to-resource-group.png)

Panonuzu yayımlandıktan sonra **paylaşım ve erişim** kontrol bölmesini yenileyin ve panoya kullanıcı erişimini yönetmek için bir bağlantı da dahil olmak üzere yayımlanan Pano hakkında bilgi gösterir.  Bu bağlantı, tüm Azure kaynakları için erişimi yönetmek için kullanılan standart rol tabanlı erişim denetimi dikey penceresini başlatır.  Her zaman bu görünüme seçerek dönebilirsiniz **paylaşımı**.

![erişim denetimini yönetme](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kaynakları yönetmek için bkz: [Azure portalını kullanarak Azure kaynaklarınızı yönetme](../azure-resource-manager/manage-resources-portal.md).
* Kaynakları dağıtmak için bkz. [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md).

