---
title: Web ve API uygulamaları Azure yığın kullanıcılarınız için kullanılabilir hale | Microsoft Docs
description: Uygulama hizmeti kaynak Sağlayıcısı'nı yüklemek ve oluşturmak için Öğreticisi, Azure yığın kullanıcılarınızın web ve API uygulamalarınızı oluşturma vermek sunar.
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
ms.openlocfilehash: 0171dba639e480a04cdd1c7f23d546d01121fb42
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247407"
---
# <a name="tutorial-make-web-and-api-apps-available-to-your-azure-stack-users"></a>Öğretici: web ve API uygulamaları Azure yığın kullanıcılarınıza kullanılabilmesini

Azure yığın bulut yönetici olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) Azure işlevleri ve web ve API uygulamaları oluşturun. Bu isteğe bağlı, bulut tabanlı uygulamalar için kullanıcılarınızın erişim vererek, bunları zaman ve kaynak tasarrufu yapabilirsiniz.

Bunu ayarlamak için şunları yapacaksınız:

> [!div class="checklist"]
> * Uygulama hizmeti kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

## <a name="deploy-the-app-service-resource-provider"></a>Uygulama hizmeti kaynak sağlayıcısı dağıtma

1. [Azure yığın Geliştirme Seti konak hazırlama](azure-stack-app-service-before-you-get-started.md). Bu, bazı uygulamalar oluşturmak için gerekli olan SQL Server Kaynak sağlayıcısı dağıtma içerir.
2. [Yükleyici ve yardımcı komut dosyalarını indirme](azure-stack-app-service-deploy.md).
3. [Gerekli sertifikaları oluşturmak için yardımcı betiğini çalıştırın](azure-stack-app-service-deploy.md).
4. [Uygulama hizmeti kaynak sağlayıcısı yüklemeniz](azure-stack-app-service-deploy.md) (yüklemek için birkaç saat sürecek ve görünmesi tüm çalışan rolleri için.)
5. [Yüklemeyi doğrulama](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Teklif oluşturma

Örnek olarak, DNN web içerik yönetim sistemleri oluşturmalarını sağlayan bir teklif oluşturabilirsiniz. SQL Server Kaynak sağlayıcısı yükleyerek zaten etkin SQL Server hizmetini gerektirir.

1.  [Kota ayarlamak](azure-stack-setting-quotas.md) ve adlandırın *AppServiceQuota*. Seçin **Microsoft.Web** için **Namespace** alan.
2.  [Bir plan oluşturmak](azure-stack-create-plan.md). Bu ad *TestAppServicePlan*seçin **Microsoft.SQL** hizmet ve **AppService kota** kota.

    > [!NOTE]
    > Diğer uygulamalar oluşturmalarını sağlamak için diğer hizmetler planında gerekli olabilir. Örneğin, Azure işlevleri gerektirir **Microsoft.Storage** plandaki hizmet Wordpress gerektirse **Microsoft.MySQL**.

3.  [Bir teklif oluşturmak](azure-stack-create-offer.md), adlandırın **TestAppServiceOffer** seçip **TestAppServicePlan** planı.

## <a name="test-the-offer"></a>Teklif test

Uygulama hizmeti kaynak sağlayıcısı dağıtılmış ve bir teklif oluşturulan göre bir kullanıcı olarak oturum açın, teklif abone ve bir uygulama oluşturun.

Bu örnekte, DNN Platform içerik yönetim sistemi oluşturacağız. İlk olarak, bir SQL veritabanı ve DNN web uygulaması oluşturun.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma

1. Azure yığın portalında oturum açın (https://portal.local.azurestack.external) Kiracı olarak.
2. Seçin **bir abonelik edinmeniz** >, girin **TestAppServiceSubscription** altında **görünen adı** > **bir teklif seçin**  >  **TestAppServiceOffer** > **oluşturma**.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. Seçin **+**  >  **veri + depolama** > **SQL veritabanı**.
2. Varsayılan değerleri aşağıdaki alanları hariç tutabilir:

    - **Veritabanı adı**: DNNdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Abonelik**: TestAppServiceOffer
    - **Kaynak grubu**: DNN RG

3. Seçin **oturum açma ayarları**veritabanı için kimlik bilgilerini girin ve ardından **Tamam**. Bu kimlik bilgileri daha sonra Bu öğreticide kullanacaksınız.
4. Altında **SKU** > barındıran SQL Server için oluşturulan SQL SKU seçin > ve ardından **Tamam**.
5. **Oluştur**’u seçin.

### <a name="create-a-dnn-app"></a>Bir DNN uygulaması oluşturma

1. Seçin **+**  >  **tümünü görmek** > **DNN Platform Önizleme** > **oluşturma** .
2. Girin *DNNapp* altında **uygulama adı** seçip **TestAppServiceOffer** altında **abonelik**.
3. Seçin **gerekli ayarları Yapılandır** > **Yeni Oluştur** > girmek için bir **uygulama hizmeti planı** adı.
4. Seçin **fiyatlandırma katmanı** > **F1 ücretsiz** > **seçin** > **Tamam**.
5. Seçin **veritabanı** ve daha önce oluşturduğunuz SQL veritabanı için kimlik bilgilerini girin.
6. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulama hizmeti kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Teklif test

Bilgi edinmek için sonraki öğretici İlerlet nasıl yapılır:

> [!div class="nextstepaction"]
> [Azure ve Azure uygulamaları dağıtmak yığını](user/azure-stack-solution-pipeline.md)
