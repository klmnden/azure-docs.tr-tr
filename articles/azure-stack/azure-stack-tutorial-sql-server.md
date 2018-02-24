---
title: "SQL veritabanları Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs"
description: "Öğretici SQL Server Kaynak sağlayıcısı yüklemeniz ve oluşturmak için SQL veritabanları Azure yığın oluşturmalarını sağlayan sunar."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 7/03/2017
ms.author: brenduns
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: e9fd74fa44bb9482ee2285f4305085ee6ff2fb73
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL veritabanları Azure yığın kullanıcılarınızın kullanımına sunun

Azure yığın bulut yönetici olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) bulut yerel uygulamalar, Web siteleri ve iş yükleri kullanabilmeniz için SQL veritabanları oluşturun. Kullanıcılarınız için bu, isteğe bağlı, özel bulut tabanlı veritabanları sağlayarak bunları zaman ve kaynak kaydedebilirsiniz. Bunu ayarlamak için şunları yapacaksınız:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

## <a name="deploy-the-sql-server-resource-provider"></a>SQL Server Kaynak sağlayıcısı dağıtma

Dağıtım işlemi içinde ayrıntılı olarak açıklanmıştır [kullanım SQL veritabanları Azure yığın makale üzerinde](azure-stack-sql-resource-provider-deploy.md)ve birincil aşağıdaki adımlardan oluşur:

1. [SQL kaynak sağlayıcısı dağıtmak]( azure-stack-sql-resource-provider-deploy.md#deploy-the-resource-provider).
2. [Dağıtımı doğrulama]( azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Bir barındırma SQL sunucusuna bağlanarak kapasitesi sağlar.

## <a name="create-an-offer"></a>Teklif oluşturma

1.  [Kota ayarlamak](azure-stack-setting-quotas.md) ve adlandırın *SQLServerQuota*. Seçin **Microsoft.SQLAdapter** için **Namespace** alan.
2.  [Bir plan oluşturmak](azure-stack-create-plan.md). Bu ad *TestSQLServerPlan*seçin **Microsoft.SQLAdapter** hizmeti ve **SQLServerQuota** kota.

    > [!NOTE]
    > Diğer uygulamalar oluşturmalarını sağlamak için diğer hizmetler planında gerekli olabilir. Örneğin, Azure işlevleri plan eklemenizi gerektirir **Microsoft.Storage** Wordpress gerekirken, hizmet **Microsoft.MySQLAdapter**.
    > 
    >

3.  [Bir teklif oluşturmak](azure-stack-create-offer.md), adlandırın **TestSQLServerOffer** seçip **TestSQLServerPlan** planı.

## <a name="test-the-offer"></a>Teklif test

SQL Server Kaynak sağlayıcısı dağıtılmış ve bir teklif oluşturulan göre bir kullanıcı olarak oturum açın, teklif abone ve bir veritabanı oluşturun.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma
1. Bir kiracı olarak Azure yığın Portalı'na (https://portal.local.azurestack.external) oturum açın.
2. Tıklatın **bir abonelik edinmeniz** ve ardından **TestSQLServerSubscription** altında **görünen adı**.
3. Tıklatın **bir teklif seçin** > **TestSQLServerOffer** > **oluşturma**.
4. Tıklatın **daha fazla hizmet** > **abonelikleri** > **TestSQLServerSubscription** > **kaynak sağlayıcıları**.
5. Tıklatın **kaydetmek** yanına **Microsoft.SQLAdapter** sağlayıcısı.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. Tıklatın ** + **  >  **veri + depolama** > **SQL veritabanı**.
2. Alanlar için varsayılanları bırakabilir veya bu örnekler kullanabilirsiniz:
    - **Veritabanı adı**: SQLdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Abonelik**: TestSQLOffer
    - **Kaynak grubu**: SQL RG
3. Tıklatın **oturum açma ayarları**, veritabanı için kimlik bilgilerini girin ve ardından **Tamam**.
4. Tıklatın **SKU** > barındıran SQL Server için oluşturulan SQL SKU seçin > **Tamam**.
5. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

Bilgi edinmek için sonraki öğretici İlerlet nasıl yapılır:

> [!div class="nextstepaction"]
> [Web, mobil ve API apps kullanıcılarınızın kullanımına sunun]( azure-stack-tutorial-app-service.md)

