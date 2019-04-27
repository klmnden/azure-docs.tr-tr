---
title: Silme ve Azure DevTest Labs ' kişisel verileri dışarı aktarma | Microsoft Docs
description: Sizin yükümlülükleriniz genel veri koruma yönetmeliği (GDPR) altında desteklemek için Azure DevLast Labs hizmetinden kişisel verileri dışarı aktarma ve silme hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: spelluru
ms.openlocfilehash: e681652c13e521bd33524e247db65088f47a794c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60395008"
---
# <a name="export-or-delete-personal-data-from-azure-devtest-labs"></a>Dışarı aktarma veya Azure DevTest Labs ' kişisel verilerini silme
Bu makalede Azure DevTest Labs hizmetinden kişisel verileri dışarı aktarma ve silme için adımları sağlar. 

## <a name="what-personal-data-does-devtest-labs-collect"></a>DevTest Labs kişisel hangi verileri toplar?
DevTest Labs, kullanıcıdan iki ana parçalarını kişisel verileri toplar. Bunlar: kullanıcı e-posta adresi ve kullanıcı nesnesi kimliği Bu bilgiler, hizmetin Laboratuvar Yöneticiler ve kullanıcılar Laboratuvar hizmet içi özellikler sağlamak önemlidir.

### <a name="user-email-address"></a>Kullanıcı e-posta adresi
DevTest Labs Laboratuvar kullanıcılara otomatik kapatma e-posta bildirimleri göndermek için kullanıcı e-posta adresini kullanır. E-posta, kendi makine kapatılmadan kullanıcıları uyarır. Kullanıcıların erteleyebilir veya bunu yapmak istedikleri kapatma atlayın. Laboratuvar düzeyinde veya VM düzeyinde e-posta adresi yapılandırın.

**Laboratuvar e-posta ayarı:**

![E-posta Laboratuvar düzeyinde ayarlama](./media/personal-data-delete-export/lab-user-email.png)

**E-posta, sanal Makineyi ayar:**

![E-posta VM düzeyinde ayarlama](./media/personal-data-delete-export/vm-user-email.png)

### <a name="user-object-id"></a>Kullanıcı nesnesi kimliği
DevTest Labs Laboratuvar yöneticilere kaynak bilgileriyle aya aylık maliyet eğilimlerinizi ve maliyet göstermek için kullanıcı nesne kimliği kullanır. Maliyetleri izlemenize ve bunların Laboratuvar için eşikler yönetmek için sağlar. 

**Geçerli Takvim ayı için tahmini maliyet eğilimi:**
![geçerli Takvim ayı için tahmini maliyet eğilimi](./media/personal-data-delete-export/estimated-cost-trend-per-month.png)

**Kaynak tarafından ay başından bu yana maliyet tahmini:**
![kaynak tarafından ay başından bu yana maliyet tahmini](./media/personal-data-delete-export/estimated-month-to-date-cost-by-resource.png)


## <a name="why-do-we-need-this-personal-data"></a>Bu kişisel verileri neden ihtiyacımız var?
DevTest Labs hizmet kişisel verileri işlemsel amaçlar için kullanır. Bu veri hizmetinin temel özellikleri sağlamak için önemlidir. Kullanıcı e-posta adresinde bir bekletme ilkesi ayarlamak, e-posta adresi bizim sistemden silindikten sonra Laboratuvar kullanıcıları zamanında otomatik kapatma e-posta bildirimleri almazsınız. Benzer şekilde, Laboratuvar Yönetimi, ay, Labs'de makineler için kaynak tarafından kimlikleri silinmiş kullanıcı nesnesinin bir bekletme ilkesi temel alınarak, maliyet ve maliyet eğilimlerini aya görüntüleyemezsiniz. Bu nedenle, bu veriler için kullanıcının kaynak laboratuvarda etkin olduğu sürece saklanması gerekir.

## <a name="how-can-i-have-the-system-to-forget-my-personal-data"></a>Kişisel verilerimi unutursanız sistemin nasıl olabilir mi?
Bu kişisel verileri silindi, isterseniz bir laboratuvar kullanıcı olarak Laboratuvar karşılık gelen kaynak silerek bunu yapabilirsiniz. DevTest Labs hizmeti silinmiş kişisel verileri 30 gün sonra kullanıcı tarafından silinmiş anonimleştirir.

Örneğin, sanal makinenizin silmek ya da e-posta adresiniz kaldırıldı, DevTest Labs hizmet bu veri kaynak silindikten sonraki 30 gün anonimleştirir. Bir Laboratuvar Yöneticisi doğru aya aylık Maliyet Yansıtma sağladığımız emin olmak için silme sonra 30 günlük bekletme ilkesi

## <a name="how-can-i-request-an-export-on-my-personal-data"></a>Nasıl bir dışarı aktarma kişisel verilerimi isteyebilir miyim?
Bir laboratuvar kullanıcı olarak DevTest Labs hizmeti depolar kişisel veriler üzerinde bir dışarı aktarım isteğinde bulunabilirsiniz. Bir dışarı aktarma için istek için gidin **kişisel verileri** seçeneğini **genel bakış** laboratuvarınızın sayfası. Seçin **dışarı aktarma isteği** düğmesi devreye Laboratuvar yöneticinizin depolama hesabındaki bir indirilebilir bir excel dosyası oluşturulmasını devre dışı. Ardından bu verileri görüntülemek için laboratuvar yöneticinize başvurabilirsiniz.

1. Seçin **kişisel verileri** sol menüsünde. 

    ![Kişisel veri sayfası](./media/personal-data-delete-export/personal-data-page.png)
2. Seçin **kaynak grubu** Laboratuvar içeren.

    ![Kaynak grubu seçin](./media/personal-data-delete-export/select-resource-group.png)
3. Seçin **depolama hesabı** kaynak grubunda.
4. Üzerinde **depolama hesabı** sayfasında **Blobları**.

    ![Blobları kutucuk seçin](./media/personal-data-delete-export/select-blobs-tile.png)
5. Adlı kapsayıcıyı seçin **labresourceusage** kapsayıcılar listesinde.

    ![Blob kapsayıcısı seçin](./media/personal-data-delete-export/select-blob-container.png)
6. Seçin **klasör** Laboratuvarınızı sonra adlı. Bulduğunuz **csv** dosyaları **diskleri** ve **sanal makineler** laboratuvarınızda bu klasördeki. Bu csv dosyaları indirmek, bir erişim isteğinde Laboratuvar kullanıcı için içerik filtreleme ve onlarla paylaşmanız.

    ![CSV dosyalarını indirme](./media/personal-data-delete-export/download-csv-file.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Bir laboratuvara yönelik ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
