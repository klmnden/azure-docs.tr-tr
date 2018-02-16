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
ms.openlocfilehash: c890339ba7677b23717a6e0437b5e936fdf8ab03
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Azure Portal'da özel Şablonları kullanmaya başlama
[Azure Resource Manager ](../azure-resource-manager/resource-group-authoring-templates.md) şablonu, dağıtımınızı tanımlamak için kullanılan bildirim temelli bir şablondur. Bir çözümü dağıtmak amacıyla kaynaklarınızı tanımlayabilir ve farklı ortamlar için değer girmenizi sağlayan parametreler ve değişkenleri belirtebilirsiniz. Şablonda, JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler bulunur.

Kullanıcıların kişisel bir kitaplıktan özel şablonları oluşturmalarını, yönetmelerini ve dağıtmalarını sağlamak için [Azure Portal](https://portal.azure.com)'daki yeni **Şablonlar** işlevinin yanı sıra **Azure Marketi**'nin bir uzantısı olarak [Microsoft.Gallery](https://azure.microsoft.com/marketplace/) kaynak sağlayıcısını kullanabilirsiniz.

> [!NOTE]
> Portalda özel şablonları kullanmak yerine Microsoft, [Yönetilen Uygulamalar](../managed-applications/overview.md) aracılığıyla bir hizmet kataloğu uygulaması oluşturmanızı önerir. Hizmet kataloğu uygulamasını kuruluşunuzdaki kullanıcıların kullanımına sunabilirsiniz.

Bu belge, Azure Portal'ı kullanarak özel bir **Şablon** ekleme, yönetme ve paylaşma konusunda size rehberlik sunar.

## <a name="guidance"></a>Rehber
Aşağıdaki öneriler çözümleriniz üzerinde çalışırken **Şablonlar**'dan tam anlamıyla yararlanmanıza yardımcı olur:

* **Şablon**, bir Resource Manager şablonu ve ek meta verileri içeren kapsayıcı bir kaynaktır. Market'te bulunan bir öğeye benzer şekilde davranır. En önemli fark, genel Market öğelerinin aksine özel bir öğe olmasıdır.
* **Şablonlar** kitaplığı, dağıtımlarını özelleştirme ihtiyacı duyan kullanıcılar için iyi çalışır.
* **Şablonlar**, Azure içinde basit bir depoya ihtiyaç duyan kullanıcılar için iyi çalışır.
* Var olan bir Resource Manager şablonu ile başlayın. [GitHub](https://github.com/Azure/azure-quickstart-templates)'da şablon bulun veya var olan bir kaynak grubundan [Şablonu dışarı aktarın](../azure-resource-manager/resource-manager-export-template.md).
* **Şablonlar**, kendilerini yayımlayan kullanıcıya bağlıdır. Yayımcı adı, okuma erişimi olan herkes tarafından görülebilir.
* **Şablonlar**, Resource Manager kaynaklarıdır ve yayımlandıktan sonra yeniden adlandırılamaz.

## <a name="add-a-template-resource"></a>Şablon kaynağı ekleme
Azure portalında bir **Şablon** kaynağı oluşturmanın iki yolu vardır.

### <a name="method-1-create-a-new-template-resource-from-a-running-resource-group"></a>1. Yöntem: Çalışan bir kaynak grubundan yeni Şablon kaynağı oluşturma
1. Azure Portal'da var olan bir kaynak grubuna gidin. **Ayarlar**'da **Şablonu dışarı aktarma** öğesini seçin.
2. Resource Manager şablonu dışarı aktarıldıktan sonra, şablonu **Şablonlar** deposuna kaydetmek için **Şablonu Kaydetme** düğmesini kullanın. Şablonu dışarı aktarmayla ilgili tüm ayrıntıları [burada](../azure-resource-manager/resource-manager-export-template.md) bulabilirsiniz.
   <br /><br />
   ![Kaynak grubunu dışarı aktarma](media/rg-export-portal1.PNG)
3. **Şablona Kaydet** komut düğmesini seçin.
   <br /><br />
4. Aşağıdaki bilgileri girin:
   
   * Ad - Şablon nesnesinin adı (NOT: Bu alan Azure Resource Manager tabanlı bir addır. Tüm adlandırma kısıtlamaları geçerlidir ve oluşturulduktan sonra değiştirilemez).
   * Açıklama - Şablonla ilgili kısa bir özet.
     
     ![Şablonu kaydetme](media/save-template-portal1.PNG)
5. **Kaydet**’e tıklayın.
   
   > [!NOTE]
   > Portal, dışarı aktarılan Resource Manager şablonu hatalarla karşılaştığında bildirim gösterir ancak bu Resource Manager şablonunu yine de Şablonlara kaydedebilirsiniz. Dışarı aktarılan Resource Manager şablonunu yeniden dağıtmadan önce tüm Resource Manager şablonu sorunlarını denetleyip düzelttiğinizden emin olun.
   > 
   > 

### <a name="method-2-add-a-new-template-resource-from-browse"></a>2. Yöntem: Göz atarak yeni bir Şablon kaynağı ekleme
**Gözat > Şablonlar**'daki +Ekle komut düğmesini kullanarak da sıfırdan yeni bir **Şablon** ekleyebilirsiniz. Ad, Açıklama ve Resource Manager şablon JSON değeri sağlayın.

![Şablon ekleme](media/add-template-portal1.PNG)

> [!NOTE]
> Microsoft.Gallery, Kiracı tabanlı bir Azure kaynak sağlayıcısıdır. Şablon kaynağı, şablonu oluşturan kullanıcıya bağlı olur. Herhangi bir belirli aboneliğe bağlı değildir. Şablonu dağıtırken bir abonelik seçin.
> 
> 

## <a name="view-template-resources"></a>Şablon kaynaklarını görüntüleme
Kullanabileceğiniz tüm **Şablonlar**'ı, **Gözat > Şablonlar**'da görebilirsiniz. Kullanılabilir şablonlar, hem kendi oluşturduğunuz hem de çeşitli izin düzeyleriyle sizinle paylaşılan şablonlardan oluşur. Daha fazla bilgi için bkz. [erişim denetimi](#access-control-for-a-tenant-resource-provider).

![Şablonu görüntüleme](media/view-template-portal1.PNG)

Listedeki bir öğeye tıklayarak bir **Şablon**'un ayrıntılarını görüntüleyebilirsiniz.

![Şablonu görüntüleme](media/view-template-portal2c.png)

## <a name="edit-a-template-resource"></a>Şablon kaynağını düzenleme
Gözat listesinde bir öğeye sağ tıklayarak veya Düzenle komutu düğmesini seçerek bir **Şablon** için düzenleme akışını başlatabilirsiniz.

![Şablon düzenleme](media/edit-template-portal1a.PNG)

Açıklamayı veya Resource Manager şablonu metnini düzenleyebilirsiniz. Ad bir Resource Manager kaynak adı olduğundan, adı düzenleyemezsiniz. Resource Manager şablonu JSON'ını düzenlediğinizde, bunun geçerli JSON olduğundan emin olmak için doğrulama yaparız. Güncelleştirilmiş şablonunuzu kaydetmek için **Tamam**'ı ve ardından **Kaydet**'i seçin.

![Şablon düzenleme](media/edit-template-portal2a.PNG)

Şablon kaydedildikten sonra, bir onay bildirimi görürsünüz.

![Şablon düzenleme](media/edit-template-portal3b.png)

## <a name="deploy-a-template-resource"></a>Bir Şablon kaynağını dağıtma
**Okuma** izniniz olan herhangi bir **Şablon**'u dağıtabilirsiniz. Dağıtıma devam etmek için Resource Manager şablonu parametrelerinin değerlerini doldurun.

![Şablon dağıtma](media/deploy-template-portal1b.png)

## <a name="share-a-template-resource"></a>Şablon kaynağı paylaşma
Bir **Şablon** kaynağı iş arkadaşlarınızla paylaşılabilir. Paylaşma, [Azure'daki herhangi bir kaynak için rol ataması](../active-directory/role-based-access-control-configure.md) ile benzer şekilde davranır. **Şablon** sahibi, bir Şablon kaynağıyla etkileşim kurabilen diğer kullanıcılara izinler sağlar. **Şablon**'u paylaştığınız kişi veya grup, Resource Manager şablonunu ve bunun galeri özelliklerini görebilir.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Microsoft.Gallery kaynakları için erişim denetimi
| Rol | İzinler |
| --- | --- |
| Sahip |Paylaşma dahil olmak üzere Şablon kaynağında tüm denetime izin verir |
| Okuyucu |Şablon kaynağında Okuma ve Yürütme (Dağıtma) izni verir |
| Katılımcı |Şablon kaynağında Düzenleme ve Silme izni verir. Kullanıcı, Şablon'u başkalarıyla Paylaşamaz |

Sağ tıklayarak veya belirli bir öğeyi görüntülerken, göz atma öğesinde **Paylaş**'ı seçin.

![Şablonu paylaşma](media/share-template-portal1a.png)

 Artık belirli bir **Şablon**'a erişim sağlamak için bir rol ve kullanıcı veya grup seçebilirsiniz. Kullanılabilir roller Sahip, Okuyucu ve Katılımcıdır.

![Şablonu paylaşma](media/share-template-portal2b.png)

![Şablonu paylaşma](media/share-template-portal3b.png)

**Seç**'e ve **Tamam**'a tıklayın. Artık kaynağa eklediğiniz kullanıcıları veya grupları görebilirsiniz.

![Şablonu paylaşma](media/share-template-portal4b.png)

> [!NOTE]
> Bir Şablon kullanıcılar ve gruplar ile yalnızca aynı Azure Active Directory kiracısında paylaşılabilir. Kiracınızda olmayan bir e-posta adresine sahip bir Şablon paylaşırsanız kullanıcıdan kiracıya bir konuk olarak katılmasını isteyen bir davet gönderilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için bkz. [Şablon yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Resource Manager şablonunda kullanabileceğiniz işlevleri anlamak için bkz. [Şablon işlevleri](../azure-resource-manager/resource-group-template-functions.md).

