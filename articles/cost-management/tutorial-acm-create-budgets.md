---
title: Öğretici - Azure bütçelerini yönetin ve oluşturun | Microsoft Docs
description: Bu öğretici, plan ve hesap Fiyatlar, kullandığınız Azure Hizmetleri için yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/11/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: ab2ea762ac8cc289ff24136f3cafac2b80fa1a9f
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268453"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Oluşturma ve Azure bütçelerini yönetin

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Diğerleri kendi proaktif olarak maliyetleri yönetmek ve harcama zaman içinde nasıl ilerledikçe izlemek için harcama hakkında bildirmenize yardımcı olurlar. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimleri tetiklenir. Kaynaklarınızı hiçbiri etkilenir ve tüketiminiz durduruldu değil. Bütçe karşılaştırın ve maliyetleri analiz ederken harcama izlemek için kullanabilirsiniz.

Aylık bütçeleri dört saatte bir harcama karşı değerlendirilir. Ancak, veri ve tüketilen kaynaklar için bildirimler sekiz saat içinde kullanılabilir.  

Bütçe gelecekte bir sona erme tarihi seçtiğinizde aynı bütçe tutarını (aylık, üç aylık veya yıllık) bir süre sonunda otomatik olarak sıfırlama. Bütçelenen zaman ayrı bütçelerini oluşturmanıza gerek bunlar ile aynı bütçe miktarını sıfırlamak için para birimi tutarlar gelecek dönemlere ait farklı.

Bu öğreticideki örneklerde oluşturmak ve düzenlemek için Azure Kurumsal Anlaşma (EA) aboneliğiniz bir bütçe size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

## <a name="prerequisites"></a>Önkoşullar

Bütçeleri, tüm Azure EA müşterileri tarafından kullanılabilir. Bir Azure EA aboneliği görünümü bütçelerini okuma erişimi olmalıdır. Oluşturma ve bütçelerini yönetmek için katkıda bulunan izni olmalıdır. EA aboneliklerinde ve kaynak gruplarınız için ayrı ayrı bütçeden oluşturabilirsiniz. Ancak, EA hesapları faturalama bütçelerini oluşturulamıyor.

Aşağıdaki Azure izinleri bütçelerini için abonelik başına kullanıcı ve grup tarafından desteklenir:

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve maliyet Yönetimi katkıda bulunan – oluşturmak, değiştirmek veya kendi bütçelerini silin. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve maliyet Yönetimi okuyucu – iznine sahip oldukları bütçelerini görüntüleyebilirsiniz.

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [maliyet Yönetimi verilerine erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bir bütçe oluşturun

Aylık, üç aylık veya yıllık bir dönem için bir Azure aboneliği bütçe oluşturabilirsiniz. Azure portalında gezinme içerik için bir abonelik veya kaynak grubu için bütçe oluşturun belirler.

Azure portalında gidin **maliyet Yönetimi + faturalandırma** &gt; **abonelikleri** &gt; bir abonelik seçin &gt; **bütçelerini**. Bu örnekte, seçili abonelik için oluşturduğunuz bütçe olacaktır.

Bütçe oluşturduktan sonra bunlar geçerli bunlara karşı harcamalarınızı basit bir görünümü gösterir.

**Ekle**'ye tıklayın.

![Azure portalında gösterilen yönetim bütçelerini maliyeti](./media/tutorial-acm-create-budgets/budgets01.png)

İçinde **Oluştur bütçe** penceresinde bir bütçe adı ve bütçe miktarı girin. Ardından, bir aylık, üç aylık, veya yıllık süresi seçin. Ardından, bir bitiş tarihi seçin. Bütçe en az bir maliyet eşiği (% bütçe) ve karşılık gelen e-posta adresi gerektirir. İsteğe bağlı olarak, en fazla beş eşikleri ve tek bir bütçe içinde beş e-posta adresi ekleyebilirsiniz. Bütçe eşiği karşılandığında, e-posta bildirimleri normal olarak sekiz saatten kısa bir süre içinde alınır.

4.500 için aylık bir bütçe oluşturma örneği aşağıda verilmiştir. %90 bütçenin ulaşıldığında bir e-posta uyarısı oluşturulan.

![Oluştur bütçe kutusunda gösterilen örnek bilgileri](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Üç aylık bir bütçe oluşturduğunuzda, bir aylık bütçe aynı şekilde çalışır. Üç aylık dönem arasında üç aylık üç aylık dönem için bütçe miktarını şekilde eşit bölünür farktır. Bekleyebileceğiniz gibi yıllık bir bütçe tutarı tüm 12 ay takvim yılı boyunca şekilde eşit bölünür.

Maliyet Yönetimi güncelleştirilmiş fatura veri aldığında, geçerli harcama bütçenize göre güncelleştirilir. Genellikle, her gün.

![Geçerli harcama bütçenize göre gösteren örnek bilgiler](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Bütçe oluşturduktan sonra maliyet analizi gösterilmektedir. Bütçenize göre harcama eğilimi görüntüleme olduğunda ilk adımlarından biri için başlangıç [maliyetlerinizi analiz edin ve harcama](quick-acm-cost-analysis.md).

![Örnek bütçe ve maliyet analizi gösterilen harcama](./media/tutorial-acm-create-budgets/cost-analysis.png)

Önceki örnekte, bir abonelik için bir bütçe oluşturuldu. Ancak, bir kaynak grubu için bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak istiyorsanız, gitmek **maliyet Yönetimi + faturalandırma** &gt; **abonelikleri** &gt; bir aboneliği seçin > **kaynak grupları** > bir kaynak grubu seçin > **bütçelerini** > ardından **Ekle** bütçe.

## <a name="edit-a-budget"></a>Bütçe Düzenle

Sahip olduğunuz erişim düzeyine bağlı olarak, bir bütçe özelliklerini değiştirmek için düzenleyebilirsiniz. Kullanıcı yalnızca abonelik için katkıda bulunan izni olduğundan aşağıdaki örnekte, bazı özellikleri salt okunurdur. Şu anda **sona erme tarihi** devre dışı bırakılır ve bir kez ayarlandıktan sonra değiştirilemez.

![Örneği, çeşitli özelliklerini değiştirmek için bir bütçe düzenleme](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

Yinelenen bir dışarı aktarma, maliyet Yönetimi verilerine için oluşturmak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Oluşturma ve dışarı aktarılan verileri yönetme](tutorial-export-acm-data.md)
