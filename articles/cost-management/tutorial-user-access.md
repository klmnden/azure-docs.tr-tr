---
title: 'Öğretici: Azure Maliyet Yönetimi’nde erişim atama | Microsoft Docs'
description: Bu öğreticide, varlıklara erişim düzeylerini tanımlayan kullanıcı hesapları ile maliyet yönetimi verilerine erişim atamayı öğrenirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/17/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: dougeby
ms.openlocfilehash: 3ceed8b88b9c81954c967d3d7ddd964c532867ab
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301616"
---
# <a name="tutorial-assign-access-to-cost-management-data"></a>Öğretici: Maliyet yönetimi verilerine erişim atama

Maliyet yönetimi verilerine erişim, kullanıcı ya da varlık yönetimi tarafından sağlanır. Cloudyn kullanıcı hesapları, *varlıklara* ve yönetim işlevlerine erişimi belirler. İki tür erişim vardır: yönetici ve kullanıcı. Kullanıcı özelinde değiştirilmediği sürece yönetici, bir kullanıcıya Cloudyn portalındaki kullanıcı yönetimi, alıcı listesi yönetimi ve tüm varlık verilerine kök varlık erişimi dahil olmak üzere tüm işlevlerin sınırsız kullanımı için erişim izni verir. Kullanıcı erişimi, son kullanıcıların varlık verileri üzerinde sahip oldukları erişimi kullanarak raporları görüntülemesine ve rapor oluşturmasına yöneliktir.

Varlıklar, kuruluşunuzun hiyerarşik yapısını yansıtmak için kullanılır. Bunlar Cloudyn’de, kuruluşunuzdaki departmanları, bölümleri ve takımları tanımlar. Varlık hiyerarşisi, harcamaları varlıklara göre doğru şekilde izlemenize yardımcı olur.

Azure sözleşmenizi veya hesabınızı kaydettiğinizde, bu öğreticideki tüm adımları uygulayabilmeniz için Cloudyn’de yönetici iznine sahip bir hesap oluşturulmuştur. Bu öğretici, kullanıcı yönetimi ve varlık yönetimi dahil olmak üzere maliyet yönetimi verilerine erişimi ele almaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlık oluşturma ve yönetme


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Azure hesabınız olmalıdır.
- Azure Maliyet Yönetimi için bir deneme kaydı veya ücretli aboneliğe sahip olmalısınız.

## <a name="create-a-user-with-admin-access"></a>Yönetici erişimi olan bir kullanıcı oluşturma

Siz zaten yönetici erişimine sahip olsanız da, kuruluşunuzdaki iş arkadaşlarınızın da yönetici erişimine sahip olması gerekebilir. Cloudyn portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Kullanıcı Yönetimi**'ni seçin. Yeni bir kullanıcı eklemek için **Yeni Kullanıcı Ekle**’ye tıklayın.

Kullanıcı hakkında gerekli bilgileri girin. Oturum Açma Kimliği, geçerli bir e-posta adresi olmalıdır. Kullanıcının başka kullanıcılar oluşturabilmesi ve diğer kullanıcıları değiştirebilmesi için Kullanıcı Yönetimi’ne izin vermek üzere izinleri seçin. Alıcı Listeleri Yönetimi, kullanıcının alıcı listelerini düzenlemesine olanak tanır. **Kullanıcıya e-posta ile bildir**’i seçtiğinizde, kullanıcıya Cloudyn’den oturum açma bilgilerini içeren bir bağlantı e-posta ile gönderilir. İlk oturum açma işleminde kullanıcı bir parola ayarlar.

**Kullanıcının yönetici erişimi var** altında, kuruluşunuzun kök varlığı seçilidir. Kökü seçili durumda bırakın ve sonra kullanıcı bilgilerini kaydedin. Kök varlığın seçilmesi, kullanıcının yalnızca ağaçtaki kök varlıkta değil, aynı zamanda onun içinde bulunan tüm varlıklarda da yönetici iznine sahip olmasını sağlar.  
  ![yönetici erişimi olan yeni kullanıcı oluşturma](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Kullanıcı erişimi olan bir kullanıcı oluşturma
Pano ve raporlar gibi maliyet yönetimi verilerine erişmesi gereken tipik kullanıcılar, bunları görüntüleme erişimine sahip olmalıdır. Aşağıdaki farklılıklar dışında, yönetici erişimi ile oluşturduğunuz kullanıcıya benzer şekilde kullanıcı erişimi olan yeni bir kullanıcı oluşturun:

- **Kullanıcı Yönetimine İzin Ver**, **Alıcı Listesi Yönetimine İzin Ver** seçeneklerinin işaretini kaldırın ve **Kullanıcının yönetici erişimi var** listesinin tümünü temizleyin.
- **Kullanıcının kullanıcı erişimi var** listesinde kullanıcının erişmesi gereken varlıkları seçin.
- Ayrıca, gerektiğinde yöneticinin belirli varlıklara erişmesine izin verebilirsiniz.

![kullanıcı erişimi olan yeni kullanıcı oluşturma](.\media\tutorial-user-access\new-user-access.png)

Kullanıcı ekleme hakkında öğretici bir video izlemek için bkz. [Azure Maliyet Yönetimi’ne Kullanıcı Ekleme](https://youtu.be/Nzn7GLahx30).

## <a name="create-and-manage-entities"></a>Varlık oluşturma ve yönetme

Maliyet varlığı hiyerarşinizi tanımlarken en iyi uygulama, kuruluşunuzun yapısını tanımlamaktır. Varlıklar, bireysel hesap veya aboneliklere göre harcamaları ayırmanıza olanak tanır. Mantıksal gruplar oluşturmak ve harcamaları izlemek için maliyet varlıkları oluşturursunuz. Ağacı oluştururken, maliyetlerin iş birimi, maliyet merkezi, ortam ve satış bölümlerine göre nasıl ayrılmasını istediğinizi veya nasıl ayrılması gerektiğini düşünün. Cloudyn’deki varlık ağacı, varlık devralma nedeniyle esnektir.

Bulut hesaplarınız için bireysel abonelikler belirli varlıklarla bağlantılıdır. Bir varlığı, bulut hizmeti sağlayıcı hesabı ya da aboneliği ile ilişkilendirebilirsiniz. Bu nedenle, varlıklar çok kiracılıdır. Belirli kullanıcılara yalnızca işletmenizin varlıkları kullanan kesimlerine erişim hakkı atayabilirsiniz. Bunun yapılması, bir işletmenin yan kuruluşlar gibi büyük bölümlerinde bile verilerin yalıtılmış olarak kalmasını sağlar. Ayrıca, veri yalıtımı idareye yardımcı olur.  

Azure sözleşmenizi veya hesabınızı Cloudyn’e kaydettiğinizde, aboneliklerinizdeki kullanım, performans, faturalandırma ve etiket verileri gibi Azure kaynak verileri Cloudyn hesabınıza kopyalanmıştır. Ancak, varlık ağacınızı el ile oluşturmanız gerekir. Azure Resource Manager kaydını atladıysanız, Cloudyn portalında yalnızca fatura verileri ve birkaç varlık raporu gösterilir.

Cloudyn portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin. Tek bir varlık (kök) ile başlayıp kök altında varlık ağacınızı oluşturabilirsiniz. Ağaç tamamlandıktan sonra birçok BT kuruluşunun benzeyebileceği bir varlık hiyerarşisi örneği aşağıda verilmiştir:

![varlık ağacı](.\media\tutorial-user-access\entity-tree.png)

**Varlıklar**’ın yanındaki **Varlık Ekle** öğesine tıklayın. Eklemek istediğiniz kişi veya departmana ilişkin bilgileri girin. **Tam Ad** ve **E-posta** alanları, mevcut kullanıcılarla aynı olmamalıdır. Erişim düzeylerinin bir listesini görüntülemek isterseniz *Varlık ekleme* yardımını arayın.

![varlık ekleme](.\media\tutorial-user-access\add-entity.png)

İşiniz bittiğinde varlığı **Kaydedin**.

### <a name="entity-access-levels"></a>Varlık erişim düzeyleri

Bir kullanıcının erişimi ile birlikte varlık erişim düzeylerini kullanarak, Cloudyn portalındaki kullanılabilir eylem türlerini tanımlayabilirsiniz.

- **Kurumsal** - Alt maliyet varlıkları oluşturma ve yönetme olanağı sağlar.
- **Kurumsal ve Maliyet Ayırma** - Birleşik hesaplar için maliyet ayırma dahil olmak üzere alt maliyet varlıkları oluşturma ve yönetme olanağı sağlar.
- **Kurumsal, Üst maliyet ayırmaya göre maliyet** - Alt maliyet varlıkları oluşturma ve yönetme olanağı sağlar. Hesabın maliyetleri, üst maliyet ayırma modelini temel alır.
- **Yalnızca Özel Panolar** - Kullanıcıya yalnızca önceden tanımlı özel panoları görme olanağı sağlar.
- **Yalnızca Panolar** - Kullanıcıya yalnızca panoları görme olanağı sağlar.

### <a name="create-a-cost-entity-hierarchy"></a>Maliyet varlık hiyerarşisi oluşturma

Maliyet varlık hiyerarşisi oluşturmak için kurumsal veya kurumsal ve maliyet ayırma erişimine sahip bir hesabınızın olması gerekir.

Cloudyn portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin. **Varlıklar** ağacı, sol bölmede gösterilir. Gerekirse, bir hesapla ilişkilendirmek istediğiniz varlığı görüntüleyebilmek için varlık ağacını genişletin.  Bulut hizmeti sağlayıcı hesaplarınız sağ bölmedeki sekmelerde gösterilir. Bir sekme seçin ve sonra varlığa bir hesabı/aboneliği sürükleyip bırakın. **Taşı** kutusu, hesabın başarıyla taşındığını size bildirir. **Tamam**’a tıklayın.

Bir varlıkla birden fazla hesabı da ilişkilendirebilirsiniz. Hesapları seçin ve **Taşı**’ya tıklayın. Hesapları Taşı kutusunda, hesabı taşımak istediğiniz varlığı seçin ve ardından **Kaydet**’e tıklayın. Hesapları taşı kutusu, hesapları taşımak isteğinizi doğrulamanızı ister. **Evet**’e ve ardından **Tamam**’a tıklayın.

Maliyet varlık hiyerarşisi oluşturma hakkında öğretici bir video izlemek için bkz. [Azure Maliyet Yönetimi’nde Maliyet Varlık Hiyerarşisi Oluşturma](https://youtu.be/dAd9G7u0FmU).

Azure Kurumsal Anlaşma kullanıcısıysanız, [Azure Maliyet Yönetimi ile Azure Resource Manager’a bağlanma](https://youtu.be/oCIwvfBB6kk) bölümünde hesap ve abonelikleri varlıklarla ilişkilendirme hakkında öğretici bir video izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlık oluşturma ve yönetme


Hesaplarınız için Azure Resource Manager API’si erişimini etkinleştirmediyseniz, aşağıdaki makaleden devam edin.

> [!div class="nextstepaction"]
> [Azure aboneliklerini ve hesaplarını etkinleştirme](activate-subs-accounts.md)
