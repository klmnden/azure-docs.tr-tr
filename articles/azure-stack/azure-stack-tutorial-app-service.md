---
title: Web ve API apps, Azure Stack kullanıcıları için kullanılabilir hale | Microsoft Docs
description: App Service kaynak Sağlayıcısı'nı yükleme ve oluşturma Öğreticisi, Azure Stack kullanıcılarınıza web ve API uygulamaları oluşturma olanağı sunmak sunar.
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
ms.date: 03/11/2019
ms.author: jeffgilb
ms.reviewer: anwestg
ms.custom: mvc
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: f09ed70647eaa0275020bdac66e0fcf84d2ff00a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57760384"
---
# <a name="tutorial-make-web-and-api-apps-available-to-your-azure-stack-users"></a>Öğretici: web ve API apps, Azure Stack kullanıcılar için kullanılabilir yap

Azure Stack bulut yönetici olarak, kullanıcılarınızın sağlayan teklifler oluşturabilirsiniz (kiracılar), Azure işlevleri ve web ve API uygulamaları oluşturun. Bu isteğe bağlı, bulut tabanlı uygulamalar için kullanıcılarınızın erişim vererek bunları zamandan ve kaynaklardan tasarruf sağlayabilirsiniz.

Bunu ayarlamak için yapacaklarınız:

> [!div class="checklist"]
> * App Service kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Test teklifini

## <a name="deploy-the-app-service-resource-provider"></a>App Service kaynak sağlayıcısı dağıtma

1. [Azure Stack geliştirme Seti'ni konak hazırlama](azure-stack-app-service-before-you-get-started.md). Bu, bazı uygulamalar oluşturmak için gerekli olan SQL Server Kaynak sağlayıcısı dağıtma içerir.
2. [Yükleyici ve yardımcı betikleri indirin](azure-stack-app-service-deploy.md).
3. [Gerekli sertifikaları oluşturmak için yardımcı betiğini çalıştırın](azure-stack-app-service-deploy.md).
4. [App Service kaynak sağlayıcısı yüklemeniz](azure-stack-app-service-deploy.md) (yüklemek için birkaç saat sürer ve görünmesini tüm çalışan rolleri için.)
5. [Yüklemeyi doğrulama](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Teklif oluşturma

Örneğin, DNN web içerik yönetim sistemlerini oluşturmalarını sağlayan bir teklif oluşturabilirsiniz. SQL Server Kaynak Sağlayıcısı'nı yükleyerek zaten etkin SQL Server hizmetini gerektirir.

1.  [Kota ayarlamak](azure-stack-setting-quotas.md) ve adlandırın *AppServiceQuota*. Seçin **Microsoft.Web** için **Namespace** alan.
2.  [Bir plan oluşturmanız](azure-stack-create-plan.md). Adlandırın *TestAppServicePlan*seçin **Microsoft.SQL** hizmet ve **AppService kota** kota.

    > [!NOTE]
    > Kullanıcıların diğer uygulamalar için diğer hizmetleri planında gerekli olabilir. Örneğin, Azure işlevleri'ni gerektirir **Microsoft.Storage** Wordpress gerekirken hizmet planda **Microsoft.MySQL**.

3.  [Teklif oluşturma](azure-stack-create-offer.md), adlandırın **TestAppServiceOffer** seçip **TestAppServicePlan** planı.

## <a name="test-the-offer"></a>Test teklifini

App Service kaynak sağlayıcısı dağıtılan ve teklif oluşturduğunuza göre bir kullanıcı olarak oturum açın, teklife abone olma ve bir uygulama oluşturun.

Bu örnekte, bir DNN platformu içerik yönetim sistemi oluşturacağız. İlk olarak bir SQL veritabanı ve DNN web uygulaması oluşturun.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma

1. Azure Stack portalında oturum açın (https://portal.local.azurestack.external) Kiracı olarak.
2. Seçin **bir abonelik edinmeniz** >, girin **TestAppServiceSubscription** altında **görünen ad** > **bir teklif seçin**  >  **TestAppServiceOffer** > **oluşturma**.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. Seçin **+**  >  **veri + depolama** > **SQL veritabanı**.
2. Varsayılan değerleri, aşağıdaki alanlar hariç tut:

    - **Veritabanı adı**: DNNdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Abonelik**: TestAppServiceOffer
    - **Kaynak grubu**: DNN-RG

3. Seçin **oturum açma ayarları**veritabanı için kimlik bilgilerini girin ve ardından **Tamam**. Bu öğreticide daha sonra bu kimlik bilgilerini kullanacaksınız.
4. Altında **SKU** > SQL barındıran SQL Server için oluşturduğunuz SKU'ları seçin > ve ardından **Tamam**.
5. **Oluştur**’u seçin.

### <a name="create-a-dnn-app"></a>DNN uygulaması oluşturma

1. Seçin **+**  >  **tümünü gör** > **DNN platformu önizlemesi** > **Oluştur** .
2. Girin *DNNapp* altında **uygulama adı** seçip **TestAppServiceOffer** altında **abonelik**.
3. Seçin **gerekli ayarları Yapılandır** > **Yeni Oluştur** > girmek için bir **App Service planı** adı.
4. Seçin **fiyatlandırma katmanı** > **F1 ücretsiz** > **seçin** > **Tamam**.
5. Seçin **veritabanı** ve daha önce oluşturduğunuz SQL veritabanı için kimlik bilgilerini girin.
6. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * App Service kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Test teklifini

Bilgi edinmek için sonraki öğreticiye ilerleyin nasıl yapılır:

> [!div class="nextstepaction"]
> [Azure ve Azure uygulama dağıtma yığını](user/azure-stack-solution-pipeline.md)
