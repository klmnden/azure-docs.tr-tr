---
title: "Web ve API uygulamaları Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs"
description: "Uygulama hizmeti kaynak Sağlayıcısı'nı yüklemek ve oluşturmak için Öğreticisi, Azure yığın kullanıcılarınızın web ve API uygulamalarınızı oluşturma vermek sunar."
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
ms.openlocfilehash: 5978706f2cab69c83a49bfd0e15ae904a38c7bab
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web ve API uygulamaları Azure yığın kullanıcılarınızın kullanımına sunun

Azure yığın bulut yönetici olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) Azure işlevleri ve web ve API uygulamaları oluşturun. Kullanıcılarınız için bu isteğe bağlı, bulut tabanlı uygulamalara erişim sağlayarak bunları zaman ve kaynak kaydedebilirsiniz. Bunu ayarlamak için şunları yapacaksınız:

> [!div class="checklist"]
> * Uygulama hizmeti kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

## <a name="deploy-the-app-service-resource-provider"></a>Uygulama hizmeti kaynak sağlayıcısı dağıtma

1. [Azure yığın Geliştirme Seti konak hazırlama](azure-stack-app-service-before-you-get-started.md). Bu, bazı uygulamalar oluşturmak için gerekli olan SQL Server Kaynak sağlayıcısı dağıtma içerir.
2. [Yükleyici ve yardımcı komut dosyalarını indirme](azure-stack-app-service-deploy.md).
3. [Gerekli sertifikaları oluşturmak için yardımcı betiğini çalıştırın](azure-stack-app-service-deploy.md).
4. [Uygulama hizmeti kaynak sağlayıcısı yüklemeniz](azure-stack-app-service-deploy.md) (yüklemek için birkaç saat sürecek ve görünmesi tüm çalışan roller için).
5. [Yüklemeyi doğrulama](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Teklif oluşturma

Örnek olarak, DNN web içerik yönetim sistemleri oluşturmalarını sağlayan bir teklif oluşturabilirsiniz. SQL Server Kaynak sağlayıcısı yükleyerek zaten etkin SQL Server hizmetini gerektirir.

1.  [Kota ayarlamak](azure-stack-setting-quotas.md) ve adlandırın *AppServiceQuota*. Seçin **Microsoft.Web** için **Namespace** alan.
2.  [Bir plan oluşturmak](azure-stack-create-plan.md). Bu ad *TestAppServicePlan*seçin **Microsoft.SQL** hizmeti ve **AppService kota** kota.

    > [!NOTE]
    > Diğer uygulamalar oluşturmalarını sağlamak için diğer hizmetler planında gerekli olabilir. Örneğin, Azure işlevleri plan eklemenizi gerektirir **Microsoft.Storage** Wordpress gerekirken, hizmet **Microsoft.MySQL**.
    > 
    >

3.  [Bir teklif oluşturmak](azure-stack-create-offer.md), adlandırın **TestAppServiceOffer** seçip **TestAppServicePlan** planı.

## <a name="test-the-offer"></a>Teklif test

Uygulama hizmeti kaynak sağlayıcısı dağıtılmış ve bir teklif oluşturulan göre bir kullanıcı olarak oturum açın, teklif abone ve bir uygulama oluşturun. Bu örnekte, DNN Platform içerik yönetim sistemi oluşturacağız. Önce bir SQL veritabanı ve DNN web uygulaması oluşturmanız gerekir.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma
1. Bir kiracı olarak Azure yığın Portalı'na (https://portal.local.azurestack.external) oturum açın.
2. Tıklatın **bir abonelik edinmeniz** > türü **TestAppServiceSubscription** altında **görünen adı** > **bir teklif seçin**  >  **TestAppServiceOffer** > **oluşturma**.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. Tıklatın  **+**   >  **veri + depolama** > **SQL veritabanı**.
2. Alanlar için varsayılan ayarları aşağıdaki gibi dışında bırakın:
    - **Veritabanı adı**: DNNdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Subscription**: TestAppServiceOffer
    - **Kaynak grubu**: DNN RG
3. Tıklatın **oturum açma ayarları**, veritabanı için kimlik bilgilerini girin ve ardından **Tamam**. Bu adımlarda daha sonra bu kimlik bilgileri kullanacaksınız.
4. Tıklatın **SKU** > barındıran SQL Server için oluşturulan SQL SKU seçin > **Tamam**.
5. **Oluştur**’a tıklayın.

### <a name="create-a-dnn-app"></a>Bir DNN uygulaması oluşturma    

1. Tıklatın  **+**   >  **tümünü görmek** > **DNN Platform Önizleme** > **oluşturma**.
2. Tür *DNNapp* altında **uygulama adı** seçip **TestAppServiceOffer** altında **abonelik**.
3. Tıklatın **gerekli ayarları Yapılandır** > **Yeni Oluştur** > türü bir **uygulama hizmeti planı** adı.
4. Tıklatın **fiyatlandırma katmanı** > **F1 ücretsiz** > **seçin** > **Tamam**.
5. Tıklatın **veritabanı** ve daha önce oluşturduğunuz SQL veritabanı bilgilerini girin.
6. **Oluştur**’a tıklayın.

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulama hizmeti kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

Bilgi edinmek için sonraki öğretici İlerlet nasıl yapılır:

> [!div class="nextstepaction"]
> [Azure ve Azure uygulamaları dağıtmak yığını](user/azure-stack-solution-pipeline.md)
