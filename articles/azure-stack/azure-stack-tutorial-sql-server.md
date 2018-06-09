---
title: SQL veritabanları Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs
description: Öğretici SQL Server Kaynak sağlayıcısı yüklemeniz ve oluşturmak için SQL veritabanları Azure yığın oluşturmalarını sağlayan sunar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: b9ba2bb89bb0d7e16a28a165cf14530a7a10f71b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234759"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Öğretici: SQL veritabanlarını Azure yığın kullanıcılarınıza erişilebilir

Azure yığın bulut yönetici olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) bulut yerel uygulamalar, Web siteleri ve iş yükleri kullanabilmeniz için SQL veritabanları oluşturun. Kullanıcılarınız için bu, isteğe bağlı, özel bulut tabanlı veritabanları sağlayarak bunları zaman ve kaynak kaydedebilirsiniz. Bunu ayarlamak için şunları yapacaksınız:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

## <a name="deploy-the-sql-server-resource-provider"></a>SQL Server Kaynak sağlayıcısı dağıtma

Dağıtım işlemi içinde ayrıntılı olarak açıklanmıştır [kullanım SQL veritabanları Azure yığın makale üzerinde](azure-stack-sql-resource-provider-deploy.md)ve birincil aşağıdaki adımlardan oluşur:

1. [SQL kaynak sağlayıcısı dağıtmak](azure-stack-sql-resource-provider-deploy.md).
2. [Dağıtımı doğrulama](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Bir barındırma SQL sunucusuna bağlanarak kapasitesi sağlar. Daha fazla bilgi için bkz: [barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Teklif oluşturma

1.  [Kota ayarlamak](azure-stack-setting-quotas.md) ve adlandırın *SQLServerQuota*. Seçin **Microsoft.SQLAdapter** için **Namespace** alan.
2.  [Bir plan oluşturmak](azure-stack-create-plan.md). Bu ad *TestSQLServerPlan*seçin **Microsoft.SQLAdapter** hizmeti ve **SQLServerQuota** kota.

    > [!NOTE]
    > Diğer uygulamalar oluşturmalarını sağlamak için diğer hizmetler planında gerekli olabilir. Örneğin, Azure işlevleri gerektirir **Microsoft.Storage** plandaki hizmet Wordpress gerektirse **Microsoft.MySQLAdapter**.

3.  [Bir teklif oluşturmak](azure-stack-create-offer.md), adlandırın **TestSQLServerOffer** seçip **TestSQLServerPlan** planı.

## <a name="test-the-offer"></a>Teklif test

SQL Server Kaynak sağlayıcısı dağıtılmış ve bir teklif oluşturulan göre bir kullanıcı olarak oturum açın, teklif abone ve bir veritabanı oluşturun.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma

1. Azure yığın portalında oturum açın (https://portal.local.azurestack.external) Kiracı olarak.
2. Seçin **bir abonelik edinmeniz** yazıp enter **TestSQLServerSubscription** altında **görünen adı**.
3. Seçin **bir teklif seçin** > **TestSQLServerOffer** > **oluşturma**.
4. Seçin **daha fazla hizmet** > **abonelikleri** > **TestSQLServerSubscription** > **kaynak sağlayıcıları**.
5. Seçin **kaydetmek** yanına **Microsoft.SQLAdapter** sağlayıcısı.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. Seçin **+**  >  **veri + depolama** > **SQL veritabanı**.
2. Varsayılan değerleri koruyun veya bu örnekler için aşağıdaki alanları kullanın:
    - **Veritabanı adı**: SQLdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Abonelik**: TestSQLOffer
    - **Kaynak grubu**: SQL RG
3. Seçin **oturum açma ayarları**veritabanı için kimlik bilgilerini girin ve ardından **Tamam**.
4. Seçin **SKU** > barındıran SQL Server için oluşturulan SQL SKU seçin > ve ardından **Tamam**.
5. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

Bilgi edinmek için sonraki öğretici İlerlet nasıl yapılır:

> [!div class="nextstepaction"]
> [Web, mobil ve API apps kullanıcılarınızın kullanımına sunun]( azure-stack-tutorial-app-service.md)