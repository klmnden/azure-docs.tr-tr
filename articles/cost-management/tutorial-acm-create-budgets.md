---
title: Öğretici - Azure bütçelerini yönetin ve oluşturun | Microsoft Docs
description: Bu öğretici, plan ve hesap Fiyatlar, kullandığınız Azure Hizmetleri için yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 6c6143cad04178fcafc825d9dae13c1a0620fb93
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033456"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Oluşturma ve Azure bütçelerini yönetin

Maliyet Yönetimi'nde bütçe planlaması ve sürücü Kurumsal sorumluluk bilincini yardımcı olur. Bütçeleriyle, kullanan veya belirli bir süre boyunca abone Azure Hizmetleri için hesap. Diğerleri kendi proaktif olarak maliyetleri yönetmek ve harcama zaman içinde nasıl ilerledikçe izlemek için harcama hakkında bildirmenize yardımcı olurlar. Zaman içinde nasıl harcama ilerledikçe görebilirsiniz. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimleri tetiklenir. Kaynaklarınızı hiçbiri etkilenir ve tüketiminiz durduruldu değil. Bütçe karşılaştırın ve maliyetleri analiz ederken harcama izlemek için kullanabilirsiniz.

Bütçe gelecekte bir sona erme tarihi seçtiğinizde aynı bütçe tutarını (aylık, üç aylık veya yıllık) bir süre sonunda otomatik olarak sıfırlama. Bütçelenen zaman ayrı bütçelerini oluşturmanıza gerek bunlar ile aynı bütçe miktarını sıfırlamak için para birimi tutarlar gelecek dönemlere ait farklı.

Bu öğreticideki örneklerde, oluşturma ve Azure Kurumsal Anlaşma (EA) aboneliğiniz için bütçe düzenleme işlemi açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

## <a name="prerequisites"></a>Önkoşullar

Bütçeleri, tüm Azure EA müşterileri tarafından kullanılabilir. Bir Azure EA aboneliği oluşturmak ve bütçelerini yönetmek için okuma erişimi olmalıdır. Kurumsal Anlaşma fatura hesapları bütçelerini tarafından desteklenmez.

Bütçe tek tek abonelik veya kaynak grubu düzeyinde oluşturulur. Aşağıdaki Azure izinleri bütçelerini için abonelik başına kullanıcı ve grup tarafından desteklenir:

- Sahip – oluşturmak, değiştirmek veya bir abonelik bütçelerini silin.
- Katkıda bulunan – oluşturmak, değiştirmek veya kendi bütçelerini silin. Başkaları tarafından oluşturulan bütçelerini için bütçe miktarını değiştirebilirsiniz.
- Okuyucu – iznine sahip oldukları bütçelerini görüntüleyebilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bir bütçe oluşturun

Aylık, üç aylık veya yıllık bir dönem için bir Azure aboneliği bütçe oluşturabilirsiniz. Azure portalında gezinme içerik için bir abonelik veya kaynak grubu için bütçe oluşturun belirler.

Azure portalında gidin **maliyet Yönetimi + faturalandırma** &gt; **abonelikleri** &gt; bir abonelik seçin &gt; **bütçelerini**. Bu örnekte, seçili abonelik için oluşturduğunuz bütçe olacaktır.

Bütçe oluşturduktan sonra bunlar geçerli bunlara karşı harcamalarınızı basit bir görünümü gösterir.

**Ekle**'ye tıklayın.

![Maliyet Yönetimi bütçelerini](./media/tutorial-acm-create-budgets/budgets01.png)

İçinde **Oluştur bütçe** penceresinde bir bütçe adı ve bütçe miktarı girin. Ardından, bir aylık, üç aylık, veya yıllık süresi seçin. Ardından, bir bitiş tarihi seçin. Bütçe en az bir maliyet eşiği (% bütçe) ve karşılık gelen e-posta adresi gerektirir. İsteğe bağlı olarak, en fazla beş eşikleri ve tek bir bütçe içinde beş e-posta adresi ekleyebilirsiniz.

4.500 için aylık bir bütçe oluşturma örneği aşağıda verilmiştir. %90 bütçenin ulaşıldığında bir e-posta uyarısı oluşturulan.

![Aylık bütçe örneği](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Üç aylık bir bütçe oluşturduğunuzda, bir aylık bütçe aynı şekilde çalışır. Üç aylık dönem arasında üç aylık üç aylık dönem için bütçe miktarını şekilde eşit bölünür farktır. Bekleyebileceğiniz gibi yıllık bir bütçe tutarı tüm 12 ay takvim yılı boyunca şekilde eşit bölünür.

Maliyet Yönetimi güncelleştirilmiş fatura veri aldığında, geçerli harcama bütçenize göre güncelleştirilir. Genellikle, her gün.

![Geçerli harcama bütçenize göre](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Bütçe oluşturduktan sonra maliyet analizi gösterilmektedir. Bütçenize göre harcama eğilimi görüntüleme olduğunda ilk adımlarından biri için başlangıç [maliyetlerinizi analiz edin ve harcama](quick-acm-cost-analysis.md).

![Maliyet analizi gösterilen bütçe](./media/tutorial-acm-create-budgets/cost-analysis.png)

Önceki örnekte, bir abonelik için bir bütçe oluşturuldu. Ancak, bir kaynak grubu için bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak istiyorsanız, gitmek **maliyet Yönetimi + faturalandırma** &gt; **abonelikleri** &gt; bir aboneliği seçin > **kaynak grupları** > bir kaynak grubu seçin > **bütçelerini** > ardından **Ekle** bütçe.

## <a name="edit-a-budget"></a>Bütçe Düzenle

Sahip olduğunuz erişim düzeyine bağlı olarak, bir bütçe özelliklerini değiştirmek için düzenleyebilirsiniz. Kullanıcı yalnızca abonelik için katkıda bulunan izni olduğundan aşağıdaki örnekte, bazı özellikleri salt okunurdur. Şu anda **sona erme tarihi** devre dışı bırakılır ve bir kez ayarlandıktan sonra değiştirilemez.

![Bütçe – katkıda bulunan izni Düzenle](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

Yinelenen bir dışarı aktarma, maliyet Yönetimi verilerine için oluşturmak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Oluşturma ve dışarı aktarılan verileri yönetme](tutorial-export-acm-data.md)
