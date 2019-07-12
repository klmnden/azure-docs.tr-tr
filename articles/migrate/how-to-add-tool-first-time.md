---
title: Azure Geçişi'ndeki ilk kez bir değerlendirme/geçiş aracı ekleyin | Microsoft Docs
description: Bir Azure geçişi projesi oluşturun ve bir değerlendirme/geçiş aracı ekleme açıklanmaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: b226f7c5879673b573133cde45db78d8d1f2fffa
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812031"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Bir değerlendirmeyi/geçiş aracı ilk kez Ekle

Bu makalede, bir değerlendirme veya geçiş aracı eklemeyi açıklar bir [Azure geçişi](migrate-overview.md) ilk kez bir proje.  
Azure geçişi, bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve private/public bulut Vm'lerinden azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf, bağımsız yazılım satıcısı (ISV) sağlar. [teklifleri](migrate-services-overview.md#isv-integration) . 

## <a name="create-a-project-and-add-a-tool"></a>Bir proje oluşturma ve bir aracı ekleme

Bir Azure aboneliğinde yeni bir Azure geçişi projesi ayarlama ve bir aracı ekleyebilirsiniz.

- Azure geçişi projesinde, bulma, değerlendirme ve toplanan geçirme değerlendirerek veya kullandığınız ortamdan geçiş meta verileri depolamak için kullanılır. 
- Bir projede bulunan varlıkları izlemek ve değerlendirme ve geçiş düzenleyin.

1. Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**.
2. Altında **Hizmetleri**seçin **Azure geçişi**.

    ![Azure Geçişi ' ayarlayın](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. İçinde **genel bakış**, tıklayın **değerlendirin ve sunucularını geçirme**.
4. Altında **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **değerlendirin ve sunucularını geçirme**.

    ![Bulma ve değerlendirme sunucuları](./media/how-to-add-tool-first-time/assess-migrate.png)

1. İçinde **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **ekleme Araçları**.
2. İçinde **geçiş projesi**, Azure aboneliğinizi seçin ve bir yoksa, bir kaynak grubu oluşturun.
3. İçinde **Project Details**, proje adı ve projeyi oluşturmak istediğiniz coğrafi konum belirtin. 

    ![Bir Azure geçişi projesi oluşturun](./media/how-to-add-tool-first-time/migrate-project.png)

    Azure geçişi projesinde bu coğrafyalar hiçbirinde oluşturabilirsiniz.

    **Coğrafya** | **Depolama konumu bölge**
    --- | ---
    Asya | Güneydoğu Asya veya Doğu Asya
    Avrupa | Güney Avrupa veya Batı Avrupa
    Birleşik Krallık | UK Güney veya UK Batı
    Amerika Birleşik Devletleri | Orta ABD ve Batı ABD 2

    Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.

4. Tıklayın **sonraki**ve bir değerlendirme veya geçiş aracı ekleyin.

    > [!NOTE]
    > Bir proje oluşturduğunuzda en az bir değerlendirme veya geçiş aracını eklemeniz gerekir.

5. İçinde **Select Değerlendirme Aracı**, bir değerlendirme aracı ekleyin. Bir değerlendirme aracı gerekmiyorsa seçin **şimdilik bir değerlendirme aracı eklemeyi atlamak mı** > **sonraki**. 
2. İçinde **Select geçiş aracı**, geçiş aracı gerektiği gibi ekleyin. Geçiş Aracı şu anda gerekmiyorsa, seçin **şimdilik geçiş aracı eklemeyi atlamak mı** > **sonraki**.
3. İçinde **gözden + araçları Ekle**, ayarları gözden geçirin ve tıklayın **ekleme Araçları**.

Projeyi oluşturduktan sonra değerlendirme ve geçiş sunucuları ve iş yükleri, veritabanları ve web uygulamaları için ek araçlar seçebilirsiniz.

## <a name="create-additional-projects"></a>Ek projeleri oluşturma

Bazı durumlarda, Azure geçişi ek projeleri oluşturmanız gerekebilir. Örneğin, farklı coğrafi bölgelerde veri merkezleri varsa veya farklı bir coğrafi bölge meta verileri depolamak gerekir. Ek bir proje şu şekilde oluşturun:

1. Geçerli Azure geçişi projesinde tıklayın **sunucuları** veya **veritabanları**.
2. Sağ üst köşede tıklayın **değişiklik** geçerli proje adı yanında.
3. İçinde **ayarları**seçin **yeni bir proje oluşturmak için buraya tıklayın**.
4. Yeni bir proje oluşturun ve önceki yordamda açıklandığı gibi yeni bir aracı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Ek eklemeyi öğrenin [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları. 
