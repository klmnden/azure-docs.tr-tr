---
title: 'Öğretici: Azure Maliyet Yönetimi’nde erişim atama | Microsoft Docs'
description: Bu öğreticide, varlıklara erişim düzeylerini tanımlayan kullanıcı hesapları ile maliyet yönetimi verilerine erişim atamayı öğrenirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: dougeby
ms.openlocfilehash: c1be4d649bf4b69a9f749003b5c66142006b78e0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-assign-access-to-cost-management-data"></a>Öğretici: Maliyet yönetimi verilerine erişim atama

Maliyet yönetimi verilerine erişim, kullanıcı ya da varlık yönetimi tarafından sağlanır. Cloudyn kullanıcı hesapları, *varlıklara* ve yönetim işlevlerine erişimi belirler. İki tür erişim vardır: yönetici ve kullanıcı. Kullanıcı özelinde değiştirilmediği sürece yönetici, bir kullanıcıya Cloudyn portalındaki kullanıcı yönetimi, alıcı listesi yönetimi ve tüm varlık verilerine kök varlık erişimi dahil olmak üzere tüm işlevlerin sınırsız kullanımı için erişim izni verir. Kullanıcı erişimi, son kullanıcıların varlık verileri üzerinde sahip oldukları erişimi kullanarak raporları görüntülemesine ve rapor oluşturmasına yöneliktir.

Varlıklar, kuruluşunuzun hiyerarşik yapısını yansıtmak için kullanılır. Bunlar Cloudyn’de, kuruluşunuzdaki departmanları, bölümleri ve takımları tanımlar. Varlık hiyerarşisi, harcamaları varlıklara göre doğru şekilde izlemenize yardımcı olur.

Azure sözleşmenizi veya hesabınızı kaydettiğinizde, bu öğreticideki tüm adımları uygulayabilmeniz için Cloudyn’de yönetici iznine sahip bir hesap oluşturulmuştur. Bu öğretici, kullanıcı yönetimi ve varlık yönetimi dahil olmak üzere maliyet yönetimi verilerine erişimi ele almaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlık oluşturma

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

## <a name="create-entities"></a>Varlık oluşturma

Maliyet varlığı hiyerarşinizi tanımlarken en iyi uygulama, kuruluşunuzun yapısını tanımlamaktır.

Ağacı oluştururken, maliyetlerin iş birimi, maliyet merkezi, ortam ve satış bölümlerine göre nasıl ayrılmasını istediğinizi veya nasıl ayrılması gerektiğini düşünün. Cloudyn’deki varlık ağacı, varlık devralma nedeniyle esnektir. Bulut hesaplarınız için bireysel abonelikler belirli varlıklarla bağlantılıdır. Bu nedenle, varlıklar çok kiracılıdır. Belirli kullanıcılara yalnızca işletmenizin varlıkları kullanan kesimlerine erişim hakkı atayabilirsiniz. Bunun yapılması, bir işletmenin yan kuruluşlar gibi büyük bölümlerinde bile verilerin yalıtılmış olarak kalmasını sağlar. Ayrıca, veri yalıtımı idareye yardımcı olur.  

Azure sözleşmenizi veya hesabınızı Cloudyn’e kaydettiğinizde, aboneliklerinizdeki kullanım, performans, faturalandırma ve etiket verileri gibi Azure kaynak verileri Cloudyn hesabınıza kopyalanmıştır. Ancak, varlık ağacınızı el ile oluşturmanız gerekir. Azure Resource Manager kaydını atladıysanız, Cloudyn portalında yalnızca fatura verileri ve birkaç varlık raporu gösterilir.

Cloudyn portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Bulut Hesapları**'nı seçin. Tek bir varlık (kök) ile başlayıp kök altında varlık ağacınızı oluşturabilirsiniz. Ağaç tamamlandıktan sonra birçok BT kuruluşunun benzeyebileceği bir varlık hiyerarşisi örneği aşağıda verilmiştir:

![varlık ağacı](.\media\tutorial-user-access\entity-tree.png)

**Varlıklar**’ın yanındaki **Varlık Ekle** öğesine tıklayın. Eklemek istediğiniz kişi veya departmana ilişkin bilgileri girin. **Tam Ad** ve **E-posta** alanları, mevcut kullanıcılarla aynı olmamalıdır. Erişim düzeylerinin bir listesini görüntülemek isterseniz *Varlık ekleme* yardımını arayın.

![varlık ekleme](.\media\tutorial-user-access\add-entity.png)

İşiniz bittiğinde varlığı **Kaydedin**.


Maliyet varlık hiyerarşisi oluşturma hakkında öğretici bir video izlemek için bkz. [Azure Maliyet Yönetimi’nde Maliyet Varlık Hiyerarşisi Oluşturma](https://youtu.be/dAd9G7u0FmU).

Azure Kurumsal Anlaşma kullanıcısıysanız, [Azure Maliyet Yönetimi ile Azure Resource Manager’a bağlanma](https://youtu.be/oCIwvfBB6kk) bölümünde hesap ve abonelikleri varlıklarla ilişkilendirme hakkında öğretici bir video izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlık oluşturma

Hesaplarınız için Azure Resource Manager API’si erişimini etkinleştirmediyseniz, aşağıdaki makaleden devam edin.

> [!div class="nextstepaction"]
> [Azure aboneliklerini ve hesaplarını etkinleştirme](activate-subs-accounts.md)
