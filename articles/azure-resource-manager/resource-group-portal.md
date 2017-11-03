---
title: "Azure kaynaklarınızı yönetmek için Azure portalını kullanma | Microsoft Docs"
description: "Azure portalı ve Azure Resource Manager kaynaklarınızı yönetmek için kullanın. Kaynakları izlemek için Panolar ile çalışmaya nasıl gösterir."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 7a94fd5065de93384460e851627a9813d439956b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-resources-through-portal"></a>Portal üzerinden Azure kaynaklarınızı yönetmek
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Bu konuda nasıl kullanılacağını gösterir [Azure portal](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı yönetmek için. Kaynakları portal üzerinden dağıtma hakkında bilgi edinmek için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).

Şu anda, her hizmeti Portalı'nı veya Kaynak Yöneticisi'ni destekler. Bu hizmetler için kullanmanız gerekir. [Klasik portal](https://manage.windowsazure.com). Her hizmet durumunu görmek [Azure portal kullanılabilirlik grafik](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Kaynak gruplarını yönetme

Bir kaynak grubu Azure çözümünü ilgili kaynaklara tutan bir kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, kolayca dağıtabilir, güncelleştirme ve bir grup olarak silmek için aynı kaynak grubunda aynı yaşam döngüsü paylaşan kaynak ekleyin. 

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

1. Aboneliğinizdeki tüm kaynak grupları görmek için seçin **kaynak grupları**.
   
    ![Kaynak grupları Gözat](./media/resource-group-portal/browse-groups.png)
2. Boş bir kaynak grubu oluşturmak için seçin **Ekle**.
   
    ![Kaynak Grubu Ekle](./media/resource-group-portal/add-resource-group.png)
3. Bir ad ve yeni kaynak grubu için konum belirtin. **Oluştur**’u seçin.
   
    ![kaynak grubu oluştur](./media/resource-group-portal/create-empty-group.png)
4. Seçmek için gerek duyabileceğiniz **yenileme** son oluşturduğunuz kaynak grubunu görmek için.
   
    ![kaynak grubu Yenile](./media/resource-group-portal/refresh-resource-groups.png)
5. Kaynak grupları için görüntülenen bilgileri özelleştirmek için seçin **sütunları**.
   
    ![sütunları özelleştirme](./media/resource-group-portal/select-columns.png)
6. Sütun ekleyin ve ardından seçin **güncelleştirme**.
   
    ![sütunlar ekleme](./media/resource-group-portal/add-columns.png)
7. Yeni kaynak grubunuz kaynakları dağıtma hakkında bilgi edinmek için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).
8. Bir kaynak grubu için hızlı erişim için dikey panonuza sabitleyebilirsiniz.
   
    ![PIN kaynak grubu](./media/resource-group-portal/pin-group.png)
9. Pano, kaynak grubu ve kaynaklarını görüntüler. Kaynak grupları veya herhangi birinde kaynaklarını öğesine gitmek üzere seçebilirsiniz.
   
    ![PIN kaynak grubu](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Etiket kaynakları
Varlıklarınızı mantıksal olarak düzenlemek için kaynak grupları ve kaynaklar için etiketler uygulayabilirsiniz. Etiketleri ile çalışma hakkında daha fazla bilgi için bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Kaynakları izleme
Bir kaynak seçtiğinizde, kaynak dikey penceresini varsayılan grafikleri ve izleme bu kaynak türü için tabloları gösterir.

1. Bir kaynak seçip bildirim **izleme** bölümü. Kaynak türü için uygun olan grafikleri içerir. Aşağıdaki resimde, varsayılan depolama hesabı için veri izleme gösterir.
   
    ![İzleme Göster](./media/resource-group-portal/show-monitoring.png)
2. Bölümün yukarısında üç nokta (...) seçerek dikey bir bölümünü panonuza sabitleyebilirsiniz. Ayrıca, dikey bölümde boyutunu özelleştirmek veya tamamen kaldırın. Aşağıdaki resimde, PIN, özelleştirme veya CPU ve bellek bölümü kaldırma gösterilmektedir.
   
    ![PIN bölümü](./media/resource-group-portal/pin-cpu-section.png)
3. Bölüm panoya sabitleme sonra Özet panosunda görürsünüz. Ve hemen seçerek, veriler hakkında daha fazla ayrıntı için alır.
   
    ![Görünüm Panosu](./media/resource-group-portal/view-startboard.png)
4. Tamamen Portalı aracılığıyla izleme verileri özelleştirmek için varsayılan panonuz gidin ve seçin **yeni Pano**.
   
    ![pano](./media/resource-group-portal/dashboard.png)
5. Yeni panonuz bir ad verin ve Pano kutucukları sürükleyin. Döşeme farklı seçenekler göre filtrelenir.
   
    ![pano](./media/resource-group-portal/create-dashboard.png)
   
     Panoları ile çalışma hakkında bilgi edinmek için [oluşturma ve Azure portalındaki Pano paylaşımı](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Kaynakları yönetme
Bir kaynak için dikey penceresinde kaynak yönetmek için seçenekler bakın. Portal bu belirli kaynak türü için yönetim seçenekleri sunar. Üstte yer alan kaynak dikey penceresinin ve sol tarafındaki yönetimi komutları bakın.

![Kaynakları yönetme](./media/resource-group-portal/manage-resources.png)

Aşağıdaki seçeneklerden birini başlatılıyor ve bir sanal makineyi durdurmak veya sanal makinenin özelliklerini yeniden gibi işlemler gerçekleştirebilirsiniz.

## <a name="move-resources"></a>Kaynakları taşıma
Başka bir kaynak grubu veya başka bir abonelik kaynaklarını taşımak gerekirse bkz [yeni kaynak grubu veya abonelik kaynaklarını taşıma](resource-group-move-resources.md).

## <a name="lock-resources"></a>Kaynakları kilitleme
Abonelik, kaynak grubu veya kaynak yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzda bulunan diğer kullanıcıların önlemek için kilitleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Aboneliğiniz ve maliyetleri görüntüleyin
Tüm kaynaklar için aboneliğinizi ve aktarılmış maliyetleri hakkında bilgileri görüntüleyebilirsiniz. Seçin **abonelikleri** ve görmek istediğiniz abonelik. Yalnızca seçmek için bir abonelik olabilir.

![aboneliği](./media/resource-group-portal/select-subscription.png)

Abonelik dikey penceresinde içinde yazma oranını bakın.

![yazma oranı](./media/resource-group-portal/burn-rate.png)

Ayrıca, kaynak türüne göre maliyetlerin dökümünü.

![kaynak maliyeti](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Şablonu dışarı aktarma
Kaynak grubunuzun ayarladıktan sonra kaynak grubu için Resource Manager şablonu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarma iki avantajları sunar:

1. Şablondaki tüm eksiksiz altyapı içerdiğinden çözümün gelecekteki dağıtımlar kolayca otomatik hale getirebilirsiniz.
2. JavaScript nesne gösterimi (çözümünüzü temsil eden JSON adresindeki) bakarak şablon söz dizimi ile tanıdık.

Adım adım yönergeler için bkz: [Azure Resource Manager şablonunu dışarı mevcut kaynaklardan](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Kaynak grubunu veya kaynakları silme
Bir kaynak grubunu silme içerdiği tüm kaynakları siler. Kaynakların kaynak grubunda silebilirsiniz. Olabileceğinden kaynaklara bağlı olan diğer kaynak gruplarının, bir kaynak grubu sildiğinizde dikkatli istiyor. Resource Manager bağlı kaynaklar silinmez, ancak bunlar beklenen kaynakları doğru çalışmayabilir.

![Grup silme](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Sonraki adımlar
* Etkinlik günlükleri görüntülemek için bkz: [denetim işlemleri Resource Manager ile](resource-group-audit.md).
* Bir dağıtımı hakkındaki ayrıntıları görüntülemek için bkz: [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
* Kaynakları portal üzerinden dağıtmak için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).
* Kaynaklara erişimi yönetmek için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../active-directory/role-based-access-control-configure.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

