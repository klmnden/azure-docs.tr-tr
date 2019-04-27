---
title: Azure portalını kullanarak Azure Resource Manager grupları yönetme | Microsoft Docs
description: Azure Resource Manager gruplarınızı yönetmek için Azure portalını kullanın.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: cb1eb5ac27c53f4c0d48fe3644febc62f848486d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551319"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Resource Manager kaynak gruplarını yönetme

Nasıl kullanacağınızı öğrenin [Azure portalında](https://portal.azure.com) ile [Azure Resource Manager](resource-group-overview.md) , Azure kaynak gruplarını yönetme. Azure kaynaklarını yönetmek için bkz: [Azure portalını kullanarak Azure kaynaklarınızı yönetme](./manage-resources-portal.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure CLI kullanarak Azure kaynak gruplarını yönetme](./manage-resources-cli.md)
- [Azure PowerShell kullanarak Azure kaynak gruplarını yönetme](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir

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

## <a name="list-resource-groups"></a>Kaynak gruplarını listele

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

Kaynak grubunuz başarıyla ayarladıktan sonra kaynak grubu için Resource Manager şablonu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarma iki avantajı sunar:

- Şablon tüm eksiksiz altyapı içerdiğinden çözümün gelecekteki dağıtımlar otomatikleştirin.
- Şablon söz dizimi, JavaScript nesne gösterimi (çözümünüzü temsil eden JSON konumunda) bakarak öğrenin.

Şablonu dışarı aktarmanın iki yolu vardır:

- Dağıtım için kullanılan gerçek şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bu yaklaşım kaynakları portal üzerinden dağıttığınızda ve bu kaynakları oluşturmak için kullanılan şablonu görmek istediğinizde yararlıdır. Bu şablon kullanıma hazırdır. 
- Kaynak grubunun geçerli durumunu temsil eden oluşturulmuş bir şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullanılan herhangi bir şablonu temel değil. Bunun yerine "snapshot" veya "yedek" kaynak grubu olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Kaynaklar aynı kaynak grubuna yeniden dağıtmak için bu seçeneği kullanın. Başka bir kaynak grubu için bu şablonu kullanmak için önemli ölçüde değiştirmeniz gerekebilir.

### <a name="export-templates-from-deployment-history"></a>Dağıtım geçmişinden dışarı aktarma şablonları

Bu yöntem, bazı dağıtımlar için şablonları dışarı aktarır. Portal ya da birden çok dağıtım kaynağı ekleme/kaldırma yapıldığında kaynakları değiştirdiyseniz bkz [dışarı aktarma şablonları kaynak gruplarınızdaki](#export-templates-from-resource-groups).

1. Dışarı aktarmak istediğiniz kaynak grubunu açın.  Bkz: [açık kaynak grupları](#open-resource-groups).
2. Sol bölmede seçin **dağıtımları**, veya altındaki bağlantıyı seçin **dağıtımları**.  Aşağıdaki ekran gösterilir **4 başarılı** olmadığı için dört farklı dağıtım adlarına sahip dört ayrılmış dağıtımları. Görebileceğiniz **1 başarılı**.

    ![Azure kaynak grubunu dışarı aktarma şablonları](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history.png)

3. Dağıtımlardan biri listeden seçin.
4. Sol bölmede seçin **şablon**. Resource Manager sizin için aşağıdaki altı dosyayı alır:

   - **Şablon** - Çözümünüze ait altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.
   - **Parametreler**: Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. İlk dağıtım sırasında sağladığınız değerleri içerir. Şablonu yeniden dağıtırken bu değerlerden herhangi birini değiştirebilirsiniz.
   - **CLI** -şablonu dağıtmak için kullanabileceğiniz bir Azure CLI betiği.
   - **PowerShell**: Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.
   - **.NET**: Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.
   - **Ruby** - Şablonu dağıtmak için kullanabileceğiniz bir Ruby sınıfı.

     Varsayılan olarak, portal şablonunu görüntüler.

5. Seçin **indirme** yerel bilgisayarınıza bir şablonu dışarı aktarmak için.

    ![Azure kaynak grubunu dışarı aktarma şablonları](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history-download.png)

<a name="export-templates-from-resource-groups"></a>
### <a name="export-templates-from-resource-groups"></a>Kaynak grubundan dışarı aktarma şablonları

Dağıtım geçmişinden bir şablonu alınırken, kaynaklarınızı portal değiştirdiyseniz veya birden çok dağıtım kaynakları eklenen/Kaldır, kaynak grubunun geçerli durumunu yansıtmaz. Bu bölümde kaynak grubunun geçerli durumunu yansıtan bir şablonun nasıl dışarı aktarıldığı gösterilir. Aynı kaynak grubuna yeniden dağıtmak için kullanabileceğiniz kaynak grubunun bir anlık görüntü olarak tasarlanmıştır. Diğer çözümler için dışarı aktarılan şablon kullanmak için önemli ölçüde değiştirmeniz gerekir.

1. Dışarı aktarmak istediğiniz kaynak grubunu açın.  Bkz: [açık kaynak grupları](#open-resource-groups).
2. Sol bölmede seçin **şablonu dışarı aktarma**. Resource Manager sizin için aşağıdaki altı dosyayı alır:

   - **Şablon** - Çözümünüze ait altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.
   - **Parametreler**: Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. İlk dağıtım sırasında sağladığınız değerleri içerir. Şablonu yeniden dağıtırken bu değerlerden herhangi birini değiştirebilirsiniz.
   - **CLI** -şablonu dağıtmak için kullanabileceğiniz bir Azure CLI betiği.
   - **PowerShell**: Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.
   - **.NET**: Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.
   - **Ruby** - Şablonu dağıtmak için kullanabileceğiniz bir Ruby sınıfı.

     Varsayılan olarak, portal şablonunu görüntüler.
3. Seçin **indirme** yerel bilgisayarınıza bir şablonu dışarı aktarmak için.

Dışarı aktarılan bazı şablonlar kullanılabilmesi için önce bazı düzenlemeler gerekir. Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).

### <a name="export-template-before-deploying"></a>Dağıtmadan önce şablonu dışarı aktarma

Bir kaynağı tanımlamak için portalı kullanabilirsiniz.  Kaynak dağıtmadan önce görüntüleyebilir ve bir şablonu dışarı aktarma. Yönergeler için bkz. [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](./resource-manager-quickstart-create-templates-use-the-portal.md).

### <a name="fix-export-issues"></a>Dışarı aktarma sorunlarını düzeltme

Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Yalnızca sorunları, dağıtım geçmişiniz yerine bir kaynak grubundan dışarı aktarılırken dışarı aktarma. Son dağıtımınız kaynak grubunun geçerli durumunu doğru şekilde temsil ediyorsa, şablonu kaynak grubu yerine dağıtım geçmişinden dışarı aktarmanız gerekir. Yalnızca tek bir şablonda tanımlı olmayan kaynak grubunda değişiklikler yaptığınızda kaynak grubundan dışarı aktarın.

Dışarı aktarma sorunlarını gidermek için el ile eksik kaynakları şablonunuza ekleyin. Hata iletisi, dışarı aktarılamayan kaynak türlerini içerir. Bu kaynak türünü [Şablon başvurusunda](/azure/templates/) bulun. Örneğin, bir sanal ağ geçidini el ile eklemek için [Microsoft.Network/virtualNetworkGateways şablon başvurusuna](/azure/templates/microsoft.network/virtualnetworkgateways) bakın. Şablon başvurusu kaynak şablonunuza eklemek için JSON sağlar.

JSON biçimi için kaynak aldıktan sonra kaynak değerlerini almanız gerekir. Kaynak türü için REST API'SİNDE alma işlemi kullanarak kaynak değerlerini görebilirsiniz. Örneğin, sanal ağ geçidiniz için değerleri almak için bkz: [sanal ağ geçitleri - alma](/rest/api/network-gateway/virtualnetworkgateways/get).

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).