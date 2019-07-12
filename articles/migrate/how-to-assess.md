---
title: Azure geçişi değerlendirme araçları Ekle | Microsoft Docs
description: Değerlendirme araçları Azure geçişi Merkezi'nde eklemeyi açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: d176e6276d69cd3465aa4943efa86ea1e6b0736d
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811654"
---
# <a name="add-assessment-tools"></a>Değerlendirme araçları ekleme

Bu makalede, değerlendirme araçlarını eklemeyi açıklar [Azure geçişi](migrate-overview.md).

Azure geçişi, Azure'a Araçları'nın bir hub'ı değerlendirme ve geçiş için sağlar. Bu araçlar, diğer Azure Hizmetleri ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri tarafından sağlanan yerel araçlar içerir.

Bir değerlendirme aracı eklemek istediğiniz ve henüz bir Azure geçişi projesi yoksa, bu izleyin [makale](how-to-add-tool-first-time.md).

## <a name="selecting-an-isv-tool"></a>Bir ISV aracı seçme

Seçerseniz bir [ISV aracı](migrate-services-overview.md#isv-integration) değerlendirmesi için bir lisans almak veya ISV ilkesine uygun olarak ücretsiz bir deneme için kaydolma başlatabilirsiniz. Her Aracı'nda Azure geçişi için bağlanmak için bir seçenek yoktur. Azure geçişi ile aracı çalışma alanına bağlamak için aracı yönergeleri ve belgeleri izleyin. 


## <a name="select-an-assessment-scenario"></a>Değerlendirme senaryosu seçin

1. Azure geçişi projesinde tıklayın **genel bakış**.
2. Kullanmak istediğiniz değerlendirme senaryosu seçin:

    - Bulma ve makineler ve iş yüklerini Azure'a geçiş için değerlendirme için seçin **değerlendirin ve sunucularını geçirme**.
    - Şirket içi SQL makineleri değerlendirmek için seçin **değerlendirin ve veritabanlarını geçirme**.
    - Şirket içi web uygulamaları değerlendirmek için seçin **değerlendirin ve web uygulamaları geçirme**.

    ![Değerlendirme senaryosu](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Bir sunucu Değerlendirme Aracı'nı seçin 

1. Tıklayın **değerlendirmek ve sunucularını geçirme**.
2. İçinde **Azure geçişi - sunucuları**, bir Değerlendirme Aracı'nı altında eklemediyseniz **değerlendirme Araçları**seçin **Değerlendirme Aracı eklemek için burayı tıklatın**. Değerlendirme araçları, önceden eklediğiniz varsa **daha fazla değerlendirme araçları ekleme**seçin **değişiklik**.

    > [!NOTE]
    > Farklı bir projeye gidin gerekip gerekmediğini **Azure geçişi - sunucuları**yanındaki **farklı geçiş projesi için ayrıntılara bakın**, tıklayın **Buraya**.

3. İçinde **Azure geçişi**, kullanmak istediğiniz Değerlendirme Aracı'nı seçin.

    
    ![Değerlendirme araçları](./media/how-to-assess/assess-tool.png)

    - Azure geçişi Server değerlendirmesi kullanırsanız, ayarladığınız çalıştırın ve doğrudan Azure geçişi projesinde değerlendirmeleri görüntüle.
    - Bir üçüncü taraf değerlendirme aracı kullanırsanız, kendi site için sağlanan bağlantıya gidin ve sağladıkları yönergelere uygun olarak değerlendirmeyi çalıştırın.


## <a name="select-a-database-assessment-tool"></a>Bir veritabanı Değerlendirme Aracı'nı seçin

1. Tıklayın **değerlendirin ve veritabanlarını geçirme**
2. İçinde **veritabanları**, tıklayın **ekleme Araçları**.
3. Bir araç eklentisi > **Select Değerlendirme Aracı**, veritabanınızı değerlendirmek için kullanmak istediğiniz aracı seçin.

## <a name="select-a-web-app-assessment-tool"></a>Bir web uygulamasını Değerlendirme Aracı'nı seçin

1. Tıklayın **değerlendirin ve web uygulamaları geçirme**.
2. Bağlantı için Azure App Service için geçiş aracı izleyin. Geçiş Aracı için kullanın:

    - **Çevrimiçi uygulamalar değerlendirmek**: Azure App Service Migration Yardımcısı'nı kullanarak çevrimiçi bir genel URL ile uygulamaları değerlendirebilirsiniz.
    - **.NET/PHP**: Dahili .NET ve PHP uygulamaları için indirin ve geçiş Yardımcısı'nı çalıştırın.



## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi Server değerlendirmesi için kullanarak bir değerlendirme denemek [Hyper-V](tutorial-prepare-hyper-v.md) veya [VMware](tutorial-prepare-vmware.md) VM'ler.
