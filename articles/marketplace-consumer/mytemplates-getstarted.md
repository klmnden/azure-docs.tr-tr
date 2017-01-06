---
title: "Özel Şablonları kullanmaya başlama | Microsoft Belgeleri"
description: "Azure portalı, Azure CLI veya PowerShell kullanarak özel şablonlarınızı ekleyin, yönetin ve paylaşın."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: 01657619cbe579c6818a790cc3ab95a33936a565


---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Azure Portal'da özel Şablonları kullanmaya başlama
[Azure Resource Manager ](../azure-resource-manager/resource-group-authoring-templates.md) şablonu, dağıtımınızı tanımlamak için kullanılan bildirim temelli bir şablondur. Bir çözümü dağıtmak amacıyla kaynaklarınızı tanımlayabilir ve farklı ortamlar için değer girmenizi sağlayan parametreler ve değişkenleri belirtebilirsiniz. Şablonda, JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler bulunur.

Kullanıcıların kişisel bir kitaplıktan özel şablonları oluşturmalarını, yönetmelerini ve dağıtmalarını sağlamak için [Azure Portal](https://portal.azure.com)'daki yeni **Şablonlar** işlevinin yanı sıra [Azure Marketi](https://azure.microsoft.com/marketplace/)'nin bir uzantısı olarak **Microsoft.Gallery** kaynak sağlayıcısını kullanabilirsiniz.

Bu belge, Azure Portal'ı kullanarak özel bir **Şablon** ekleme, yönetme ve paylaşma konusunda size rehberlik sunar.

## <a name="guidance"></a>Rehber
Aşağıdaki öneriler çözümleriniz üzerinde çalışırken **Şablonlar**'dan tam anlamıyla yararlanmanıza yardımcı olur.

* Bir **Şablon**, Resource Manager şablonu ve ek meta verileri içeren kapsayıcı bir kaynaktır. Market'te bulunan bir öğeye çok benzer şekilde davranır. En önemli fark, genel Market öğelerinin aksine özel bir öğe olmasıdır.
* **Şablonlar** kitaplığı, dağıtımlarını özelleştirme ihtiyacı duyan kullanıcılar için iyi çalışır.
* **Şablonlar**, Azure içinde basit bir depoya ihtiyaç duyan kullanıcılar için iyi çalışır.
* Var olan bir Resource Manager şablonu ile başlayın. [GitHub](https://github.com/Azure/azure-quickstart-templates)'da şablon bulun veya var olan bir kaynak grubundan [Şablonu dışarı aktarın](../azure-resource-manager/resource-manager-export-template.md).
* **Şablonlar**, kendilerini yayımlayan kullanıcıya bağlıdır. Yayımcı adı, okuma erişimi olan herkes tarafından görülebilir.
* **Şablonlar**, Resource Manager kaynaklarıdır ve yayımlandıktan sonra yeniden adlandırılamaz.

## <a name="add-a-template-resource"></a>Şablon kaynağı ekleme
Azure portalında bir **Şablon** kaynağı oluşturmanın iki yolu vardır.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>1. Yöntem: Çalışan bir kaynak grubundan yeni bir Şablon kaynağı oluşturma
1. Azure Portal'da var olan bir kaynak grubuna gidin. **Ayarlar**'da **Şablonu dışarı aktarma** öğesini seçin.
2. Resource Manager şablonu dışarı aktarıldıktan sonra, şablonu **Şablonlar** deposuna kaydetmek için **Şablonu Kaydetme** düğmesini kullanın. Şablonu dışarı aktarmayla ilgili tüm ayrıntıları [burada](../azure-resource-manager/resource-manager-export-template.md) bulabilirsiniz.
   <br /><br />
   ![Kaynak grubunu dışarı aktarma](media/rg-export-portal1.PNG)  <br />
3. **Şablona Kaydet** komut düğmesini seçin.
   <br /><br />
4. Aşağıdaki bilgileri girin:
   
   * Ad - Şablon nesnesinin adı (NOT: Bu Azure Resource Manager temelli bir addır. Tüm adlandırma kısıtlamaları geçerlidir ve oluşturulduktan sonra değiştirilemez).
   * Açıklama - Şablonla ilgili kısa bir özet.
     
     ![Şablonu kaydetme](media/save-template-portal1.PNG)  <br />
5. **Save (Kaydet)** düğmesine tıklayın.
   
   > [!NOTE]
   > Dışarı aktarma şablonu dikey penceresi, dışarı aktarılan Resource Manager şablonu hatalarla karşılaştığında bildirim gösterir ancak bu Resource Manager şablonunu yine de Şablonlara kaydedebilirsiniz. Dışarı aktarılan Resource Manager şablonunu yeniden dağıtmadan önce tüm Resource Manager şablonu sorunlarını denetleyip düzelttiğinizden emin olun.
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a>2. Yöntem: Gözat bölümünden yeni bir Şablon kaynağı ekleme
**Gözat > Şablonlar**'daki +Ekle komut düğmesini kullanarak da sıfırdan yeni bir **Şablon** ekleyebilirsiniz. Bir Ad, Açıklama ve Resource Manager şablon JSON'ı sağlamanız gerekir.

![Şablon ekleme](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> Microsoft.Gallery, Kiracı bazlı bir Azure kaynak sağlayıcısıdır. Şablon kaynağı, şablonu oluşturan kullanıcıya bağlı olur. Herhangi bir belirli aboneliğe bağlı değildir. Yalnızca bir Şablon dağıtılırken bir aboneliğin seçilmesi gerekir.
> 
> 

## <a name="view-template-resources"></a>Şablon kaynaklarını görüntüleme
Kullanabileceğiniz tüm **Şablonlar**'ı, **Gözat > Şablonlar**'da görebilirsiniz. Buna, oluşturduğunuz **Şablonlar**'ın yanı sıra, farklı izin düzeyleriyle sizinle paylaşılanlar da dahildir. Aşağıdaki [erişim denetimi](#access-control-for-a-tenant-resource-provider) bölümünde daha çok ayrıntı bulunmaktadır.

![Şablonu görüntüleme](media/view-template-portal1.PNG)  <br />

Listedeki bir öğeye tıklayarak bir **Şablon**'un ayrıntılarını görüntüleyebilirsiniz.

![Şablonu görüntüleme](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Şablon kaynağını düzenleme
Gözat listesinde bir öğeye sağ tıklayarak veya Düzenle komutu düğmesini seçerek bir **Şablon** için düzenleme akışını başlatabilirsiniz.

![Şablon düzenleme](media/edit-template-portal1a.PNG)  <br />

Açıklamayı veya Resource Manager şablonu metnini düzenleyebilirsiniz. Ad bir Resource Manager kaynak adı olduğundan, adı düzenleyemezsiniz. Resource Manager şablonu JSON'ını düzenlediğinizde, bunun geçerli JSON olduğundan emin olmak için doğrularız. Güncelleştirilmiş şablonunuzu kaydetmek için **Tamam**'ı ve ardından **Kaydet**'i seçin.

![Şablon düzenleme](media/edit-template-portal2a.PNG)  <br />

**Şablon** kaydedildikten sonra, bir onay bildirimi görürsünüz.

![Şablon düzenleme](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Bir Şablon kaynağını dağıtma
**Okuma** izniniz olan herhangi bir **Şablon**'u dağıtabilirsiniz. Dağıtım akışı, standart Azure Şablon dağıtımı dikey penceresini başlatır. Dağıtıma devam etmek için Resource Manager şablonu parametrelerinin değerlerini doldurun.

![Şablon dağıtma](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Şablon kaynağı paylaşma
Bir **Şablon** kaynağı iş arkadaşlarınızla paylaşılabilir. Paylaşma, [Azure'daki herhangi bir kaynak için rol ataması](../active-directory/role-based-access-control-configure.md) ile benzer şekilde davranır. **Şablon** sahibi, bir Şablon kaynağıyla etkileşim kurabilen diğer kullanıcılara izinler sağlar. **Şablon**'u paylaştığınız kişi veya bir grup kişi, Resource Manager şablonunu ve bunun galeri özelliklerini görebilir.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Microsoft.Gallery kaynakları için erişim denetimi
| Rol | İzinler |
| --- | --- |
| Sahip |Paylaşma dahil olmak üzere Şablon kaynağında tüm denetime izin verir |
| Okuyucu |Şablon kaynağında Okuma ve Yürütme (Dağıtma) izni verir |
| Katılımcı |Şablon kaynağında Düzenleme ve Silme izni verir. Kullanıcı, Şablon'u başkalarıyla Paylaşamaz |

Sağ tıklayarak veya belirli bir öğenin görüntüleme dikey penceresindeki göz atma öğesinde **Paylaş**'ı seçin. Bu, bir Paylaşma deneyimini çalıştırır.

![Şablonu paylaşma](media/share-template-portal1a.png)  <br />

 Artık belirli bir **Şablon**'a erişim sağlamak için bir rol ve kullanıcı veya grup seçebilirsiniz. Kullanılabilir roller Sahip, Okuyucu ve Katılımcıdır. Yukarıdaki [erişim denetimi](#access-control-for-a-tenant-resource-provider) bölümünde daha çok ayrıntı bulunmaktadır.

![Şablonu paylaşma](media/share-template-portal2b.png)  <br />

![Şablonu paylaşma](media/share-template-portal3b.png)  <br />

**Seç**'e ve **Tamam**'a tıklayın. Artık kaynağa eklediğiniz kullanıcıları veya grupları görebilirsiniz.

![Şablonu paylaşma](media/share-template-portal4b.png)  <br />

> [!NOTE]
> Bir Şablon kullanıcılar ve gruplar ile yalnızca aynı Azure Active Directory kiracısında paylaşılabilir. Kiracınızda olmayan bir e-posta adresine sahip bir Şablon'u paylaşırsanız kullanıcının kiracıya bir konuk olarak katılmasını soran bir davet gönderilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için bkz. [Şablon yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Bir Resource Manager şablonunda kullanabileceğiniz işlevleri anlamak için bkz. [Şablon işlevleri](../azure-resource-manager/resource-group-template-functions.md).
* Şablonlarınızı tasarlama konusunda rehberlik için bkz. [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](../azure-resource-manager/best-practices-resource-manager-design-templates.md)




<!--HONumber=Jan17_HO1-->


