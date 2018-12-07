---
title: Öğretici - Azure Maliyet Yönetimi'nden dışarı aktarılan verileri oluşturma ve yönetme | Microsoft Docs
description: Bu makalede, dış sistemlerde kullanabilmeniz için dışarı aktarılan Azure Maliyet Yönetimi verilerini nasıl oluşturup yönetebileceğiniz gösterilir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/05/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 734020f2e70d09b125867faafebf1e7f7332fec4
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52995563"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Öğretici: Dışarı aktarılan verileri oluşturma ve yönetme

Maliyet Analizi öğreticisini okuduysanız, Maliyet Yönetimi verilerinizi el ile indirme konusunda bilgi sahibisiniz. Ancak, günlük, haftalık veya aylık olarak Azure depolama için maliyet Yönetimi verilerinizi otomatik olarak dışarı aktarır, yinelenen bir görev oluşturun. Dışarı aktarılan veriler CSV biçimindedir ve Maliyet Yönetimi tarafından toplanan tüm bilgileri içerir. Daha sonra Azure depolama alanındaki dışarı aktarılan verileri dış sistemlerle kullanabilir ve kendi özel verilerinizle birleştirebilirsiniz. Ayrıca dışarı aktarılan verilerinizi pano gibi bir dış sistemde veya diğer mali sistemde kullanabilirsiniz.

Bu öğreticideki örnek, maliyet yönetimi verilerinizi dışarı aktarmada ve ardından verilerin başarılı bir şekilde dışarı aktarıldığını doğrulamada size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Günlük bir dışarı aktarma oluşturma
> * Verilerin toplandığını doğrulama

## <a name="prerequisites"></a>Önkoşullar

Veri dışarı aktarma tüm [Kurumsal Sözleşme (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşterileri tarafından kullanılabilir. Aşağıdaki Azure izinleri abonelik başına kullanıcı ve grup tarafından yapılan veri dışarı aktarımı için desteklenir:

- Sahip – Bir abonelik için zamanlanan dışarı aktarmaları oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan – Kendi zamanlanan dışarı aktarmalarını oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan zamanlanmış dışarı aktarmaların adını değiştirebilir.
- Okuyucu – İzni oldukları dışarı aktarmaları zamanlayabilir.

Azure Depolama hesapları için:
- Yazma izinlerinin, dışarı aktarma üzerindeki izinlerden bağımsız olarak yapılandırılmış depolama hesabını değiştirmesi gerekir.
- Azure depolama hesabınızın blob veya dosya depolama için yapılandırılmış olması gerekir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="create-a-daily-export"></a>Günlük bir dışarı aktarma oluşturma

Maliyet Yönetimi + Faturalama &gt; Maliyet Yönetimi &gt; bir abonelik veya bir abonelikteki kaynak grubunu &gt; seçin &gt; Dışarı Aktar **Ekle**.

Dışarı aktarma için bir ad yazın ve "Günlük ay başından bu yana maliyetlerini ver" seçeneğini belirleyin. **İleri**’ye tıklayın.

![Yeni dışarı aktarma: türünü dışarı aktarın](./media/tutorial-export-acm-data/basics_exports.png)

Azure depolama hesabınız için bir abonelik belirtin, ardından depolama hesabınızı seçin.  Depolama kapsayıcısı ve dışarı aktarma dosyası gitmek istediğiniz dizin yolunu belirtin.  **İleri**’ye tıklayın.

![Yeni dışarı aktarma: depolama](./media/tutorial-export-acm-data/storage_exports.png)

Dışarı aktarma ayrıntılarını gözden geçirin ve tıklayın **Oluştur**.

Yeni dışarı aktarmanız, dışarı aktarma listesinde görünür. Varsayılan olarak, yeni dışarı aktarmaları etkinleştirilir. Zamanlanmış bir dışarı aktarmayı devre dışı bırakmak veya silmek istiyorsanız, listedeki herhangi bir öğeye ve ardından **Devre dışı bırak** veya **Sil** seçeneklerinden birine tıklayın.

Başlangıçta, dışarı aktarmanın çalışmaya başlaması bir ila iki saat arası sürebilir. Ancak, verilerin dışarı aktarılan dosyalarda gösterilmesi en fazla dört saat sürebilir.

## <a name="verify-that-data-is-collected"></a>Verilerin toplandığını doğrulama

Maliyet Yönetimi verilerinizin toplandığını kolaylıkla doğrulayabilir ve Azure Depolama Gezgini’ni kullanarak dışarı aktarılan CSV dosyasını görüntüleyebilirsiniz.

Dışarı aktarma listesinde depolama hesabı adına tıklayın. Depolama hesabı sayfasında Gezgin'de Aç seçeneğine tıklayın. Bir onay kutusu görürseniz, dosyayı Azure Depolama Gezgini’nde açmak için **Evet**’e tıklayın.

![Depolama hesabı sayfası](./media/tutorial-export-acm-data/storage-account-page.png)

Depolama Gezgini'nde, açmak istediğiniz kapsayıcıya gidin ve bulunduğunuz aya karşılık gelen klasörü seçin. CSV dosyaları listesi gösterilir. Birini seçin ve **Aç**’a tıklayın.

![Depolama Gezgini](./media/tutorial-export-acm-data/storage-explorer.png)

Dosya, CSV dosyası uzantılarını açmak üzere ayarlanmış program veya uygulama ile açılır. Bir örneği Excel’de verilmiştir.

![Örnek dışarı aktarma verileri](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Dışarı aktarılan verilere diğer sistemlerden erişme

Maliyet Yönetimi verilerinizi dışarı aktarma amaçlarından biri, verilere dış sistemlerden erişmektir. Bir pano sistemi veya diğer finansal sistemi kullanabilirsiniz. Bu sistemler büyük ölçüde farklılık gösterdiğinden bir örnek vermek uygun olmaz.  Bununla birlikte, [Azure Depolama'ya giriş](../storage/common/storage-introduction.md) bölümünde uygulamalarınızdan verilerinize erişmeye başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Günlük bir dışarı aktarma oluşturma
> * Verilerin toplandığını doğrulama

Boşta olan ve az kullanılan kaynakları belirleyerek verimliliği iyileştirmek ve geliştirmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Önerileri gözden geçirme ve gerekli eylemleri gerçekleştirme](tutorial-acm-opt-recommendations.md)
