---
title: Azure portalını kullanarak Azure Resource Manager grupları yönetme | Microsoft Docs
description: Azure Resource Manager gruplarınızı yönetmek için Azure portalını kullanın.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: bc3c1a05c64edea260bd177dd7eaefc003db5310
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296301"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Resource Manager kaynak gruplarını yönetme

Nasıl kullanacağınızı öğrenin [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) , Azure kaynak gruplarını yönetme. Azure kaynaklarını yönetmek için bkz: [Azure portalını kullanarak Azure kaynaklarınızı yönetme](./manage-resources-portal.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure CLI kullanarak Azure kaynak gruplarını yönetme](./manage-resources-cli.md)
- [Azure PowerShell kullanarak Azure kaynak gruplarını yönetme](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Bir kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, kolayca dağıtabilir, güncelleştirme ve onları bir grup olarak silmek için aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynakların ekleyin.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğinizde meta verilerin depolandığı belirlediniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturun

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **kaynak grupları**

    ![Kaynak Grubu Ekle](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. **Add (Ekle)** seçeneğini belirleyin.
4. Aşağıdaki değerleri girin:

   - **Abonelik**: Azure aboneliğinizi seçin. 
   - **Kaynak grubu**: Yeni bir kaynak grubu adı girin. 
   - **Bölge**: Bir Azure konumu seçin **Orta ABD**.

     ![Kaynak grubu oluşturun](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. Seçin **gözden geçir + oluştur**
6. **Oluştur**’u seçin. Bir kaynak grubu oluşturmak için birkaç saniye sürer.
7. Seçin **Yenile** üstteki menüden, kaynak grubu listesini yenileyin ve sonra açmak için yeni oluşturulan kaynak grubunu seçin. Veya **bildirim**(zil simgesi) seçin ve üst **kaynak grubuna gidin** yeni oluşturulan kaynak grubu açmak için

    ![kaynak grubuna Git](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Kaynak gruplarını listeleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kaynak gruplarını listelemek için **kaynak grupları**

    ![Kaynak grupları göz atın](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Kaynak grupları için görüntülenen bilgileri özelleştirmek için seçin **sütunları Düzenle**. Aşağıdaki ekran görüntüsüne ekleyebilirsiniz toplama sütunları gösterir:

## <a name="open-resource-groups"></a>Açık kaynak grupları

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Kaynak grupları**’nı seçin.
3. Açmak istediğiniz kaynak grubunu seçin.

## <a name="delete-resource-groups"></a>Kaynak gruplarını silin

1. Silmek istediğiniz kaynak grubunu açın.  Bkz: [açık kaynak grupları](#open-resource-groups).
2. **Kaynak grubunu sil**'i seçin.

    ![Azure kaynak grubunu sil](./media/manage-resource-groups-portal/delete-group.png)

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna dağıtma

Resource Manager şablonu oluşturduktan sonra Azure portalını kullanarak Azure kaynaklarınızı dağıtmak için kullanabilirsiniz. Bir şablon oluşturmak için bkz: [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](./resource-manager-quickstart-create-templates-use-the-portal.md). Portalı kullanarak bir şablonu dağıtmak için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneliğe taşıma

Kaynak grubunda başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="lock-resource-groups"></a>Kilit kaynak grupları

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

1. Silmek istediğiniz kaynak grubunu açın.  Bkz: [açık kaynak grupları](#open-resource-groups).
2. Sol bölmede seçin **kilitler**.
3. Bir kilit kaynak grubuna eklemek için seçin **Ekle**.
4. Girin **kilit adı**, **kilit türü**, ve **notları**. Kilit türlerini dahil **salt okunur**, ve **Sil**.

    ![Kilit azure kaynak grubu](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Daha fazla bilgi için [beklenmeyen değişiklikleri önlemek için kaynakları kilitleme](./resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Etiket kaynak grupları

Varlıklarınızı mantıksal olarak düzenlemek için kaynak grupları ve kaynaklar için etiketler ekleyebilirsiniz. Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#portal).

## <a name="export-resource-groups-to-templates"></a>Kaynak grupları için şablonları dışarı aktarma

Şablonları dışarı aktarma hakkında daha fazla bilgi için bkz: [şablonu - Portal için tek ve birden çok kaynak dışarı aktarma](export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).