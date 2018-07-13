---
title: Azure kaynaklarını yönetmek için Azure portalını kullanma | Microsoft Docs
description: Azure portalı ve Azure Resource Manager kaynaklarınızı yönetmek için kullanın. Kaynakları izlemek için Pano ile çalışma işlemi gösterilmektedir.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2016
ms.author: tomfitz
ms.openlocfilehash: 7398e01a46b5d296f26905e2063acdb98383f567
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38600370"
---
# <a name="manage-azure-resources-through-portal"></a>Azure kaynaklarınızı portal üzerinden yönetme

Bu makalede nasıl kullanılacağını gösterir [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı yönetmek için. Kaynakları portal üzerinden dağıtma hakkında bilgi edinmek için [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="manage-resource-groups"></a>Kaynak gruplarını yönetme

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, kolayca dağıtabilir, güncelleştirme ve onları bir grup olarak silmek için aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynakların ekleyin. 

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

1. Aboneliğinizdeki tüm kaynak grupları görmek için seçin **kaynak grupları**.
   
    ![Kaynak grupları göz atın](./media/resource-group-portal/browse-groups.png)
2. Boş bir kaynak grubu oluşturmak için Seç **Ekle**.
   
    ![Kaynak Grubu Ekle](./media/resource-group-portal/add-resource-group.png)
3. Bir ad ve konum için yeni kaynak grubu sağlar. **Oluştur**’u seçin.
   
    ![Kaynak grubu oluşturun](./media/resource-group-portal/create-empty-group.png)
4. Seçmek için gerek duyabileceğiniz **Yenile** son oluşturduğunuz kaynak grubunu görmek için.
   
    ![kaynak grubunu Yenile](./media/resource-group-portal/refresh-resource-groups.png)
5. Kaynak gruplarınızın için görüntülenen bilgileri özelleştirmek için seçin **sütunları**.
   
    ![Sütunları Özelleştir](./media/resource-group-portal/select-columns.png)
6. Sütunları ekleyin ve ardından seçin **güncelleştirme**.
   
    ![Sütun Ekle](./media/resource-group-portal/add-columns.png)
7. Kaynakları yeni kaynak grubunuz dağıtma hakkında bilgi edinmek için [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).
8. Bir kaynak grubu, hızlı erişim için kaynak grubunu panonuza sabitleyebilirsiniz.
   
    ![PIN kaynak grubu](./media/resource-group-portal/pin-group.png)
9. Pano, kaynak grubunu ve kaynaklarını görüntüler. Kaynak grupları veya any kaynaklarının öğe için gidilecek seçebilirsiniz.
   
    ![PIN kaynak grubu](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Kaynakları etiketleme
Varlıklarınızı mantıksal olarak düzenlemek için kaynak grupları ve kaynaklar için etiketler ekleyebilirsiniz. Etiketleri ile çalışma hakkında daha fazla bilgi için bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Kaynakları izleme
Bir kaynak seçtiğinizde, portal varsayılan grafikleri ve tabloları bu kaynak türü izleme sunar.

1. Bir kaynak seçip bildirimi **izleme** bölümü. Bu kaynak türü için uygun olan grafikleri içerir. Aşağıdaki görüntüde, varsayılan depolama hesabı için veri izleme gösterilmektedir.
   
    ![İzleme Göster](./media/resource-group-portal/show-monitoring.png)
2. Yukarıdaki bölümde üç nokta (...) seçerek bir bölümü panonuza sabitleyebilirsiniz. Ayrıca bölüm boyutu özelleştirebilir ya da tamamen kaldırın. Aşağıdaki görüntüde, sabitleme, özelleştirme veya CPU ve bellek bölümü kaldırma işlemi gösterilmektedir.
   
    ![PIN bölümü](./media/resource-group-portal/pin-cpu-section.png)
3. Bölüm panoya sabitleme sonra Panoda Özet görürsünüz. Ve bunu hemen seçtiğinizde veriler hakkında daha fazla ayrıntı için açılır.
   
    ![Panoyu görüntüle](./media/resource-group-portal/view-startboard.png)
4. Portal üzerinden izleme verileri tamamen özelleştirmek için varsayılan panonuza gidin ve seçin **yeni Pano**.
   
    ![pano](./media/resource-group-portal/dashboard.png)
5. Yeni panonuz bir ad verin ve panoya kutucukları sürükleyin. Kutucukları farklı seçeneklere göre filtrelenir.
   
    ![pano](./media/resource-group-portal/create-dashboard.png)
   
     Panolar ile çalışma hakkında bilgi edinmek için [oluşturma ve Azure Portalı'ndaki pano paylaşma](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Kaynakları yönetme
Bir kaynak portalda görüntülerken, bu kaynağa yönetmek için seçeneklere bakın.

![Kaynakları yönetme](./media/resource-group-portal/manage-resources.png)

Bu seçenekler arasında sanal makinenin özelliklerini yeniden yapılandırmak ve bir sanal makineyi durdurma veya başlatma gibi işlemler gerçekleştirebilirsiniz.

## <a name="move-resources"></a>Kaynakları taşıma
Kaynakları başka bir kaynak grubu veya başka bir aboneliğe taşımak ihtiyacınız varsa bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="lock-resources"></a>Kaynakları kilitleme
Bir aboneliğe, kaynak grubuna ya da kaynak yanlışlıkla kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcıların önlemek için kilitleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Aboneliğiniz ve maliyetleri görüntüleyin
Tüm kaynaklar için aboneliğinizi ve aktarılmış maliyetleri hakkında bilgileri görüntüleyebilirsiniz. Seçin **abonelikleri** ve görmek istediğiniz aboneliği. Yalnızca seçmek için bir aboneliğiniz.

![aboneliği](./media/resource-group-portal/select-subscription.png)

Yazma hızı görürsünüz.

![yazma hızı](./media/resource-group-portal/burn-rate.png)

Ve maliyetlerini kaynak türlerine göre dökümünü.

![kaynak maliyeti](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Şablonu dışarı aktarma
Kaynak grubunuzun ayarladıktan sonra kaynak grubu için Resource Manager şablonu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarma iki avantajı sunar:

1. Şablon tüm eksiksiz altyapı içerdiğinden çözümün gelecekteki dağıtımlar kolayca otomatik hale getirebilirsiniz.
2. JavaScript nesne gösterimi (çözümünüzü temsil eden JSON konumunda) bakarak şablon söz dizimi ile aşina olabilirsiniz.

Adım adım yönergeler için bkz. [dışarı Azure Resource Manager şablonu var olan kaynaklardan](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Kaynak grubunu veya kaynakları Sil
Bir kaynak grubunun silinmesi, içerdiği tüm kaynakları siler. Bir kaynak grubu içindeki tek tek kaynakları da silebilirsiniz. Bir kaynak grubunu silerken dikkatli olun. Bu kaynak grubu, diğer kaynak gruplarındaki kaynaklara bağımlı kaynakları içerebilir.

![Grubu Sil](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Sonraki adımlar
* Etkinlik günlüklerini görüntülemek için bkz: [Resource Manager denetim işlemleri](resource-group-audit.md).
* Bir dağıtım ayrıntılarını görüntülemek için bkz: [dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md).
* Kaynakları portal üzerinden dağıtmak için bkz. [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).
* Kaynaklara erişimi yönetmek için bkz: [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

