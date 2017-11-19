---
title: "Azure portalını kullanarak Azure veritabanı geçiş hizmet örneği oluşturma | Microsoft Docs"
description: "Azure veritabanı geçiş hizmeti örneğini oluşturmak için Azure portalını kullanma"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/17/2017
ms.openlocfilehash: 9faac0716334d627cdde4c0ef16262670333b5d4
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Azure portalı kullanarak Azure veritabanı geçiş hizmeti örneği oluşturma
Bu Hızlı Başlangıç, Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturmak için Azure Portalı'nı kullanın.  Hizmeti oluşturduktan sonra bir Azure SQL veritabanı için SQL Server şirket içi verileri geçirmeye kullanmanız mümkün olacaktır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kayıt kaynak sağlayıcısı
İlk veritabanı geçiş hizmetiniz oluşturmadan önce Microsoft.DataMigration kaynak sağlayıcısı kaydetmeniz gerekir.

1. Azure portalında seçin **tüm hizmetleri**ve ardından **abonelikleri**.

1. İçinde Azure veritabanı geçiş hizmeti örneğini oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.

1. Geçiş için arama yapın ve ardından Microsoft.DataMigration sağında **kaydetmek**.

![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti oluşturma
1. Tıklatın  **+**  yeni bir hizmet oluşturmak için.  Veritabanı geçiş hizmeti hala önizlemede değil.  

1. Market "geçiş" için arama, "Veritabanı geçiş hizmeti (Önizleme)" seçin ve ardından **oluşturma**.

    ![Geçiş hizmeti oluşturma](media/quickstart-create-data-migration-service-portal/dms-create-service.png)

    - Seçin bir **hizmet adı** etkileyici ve Azure veritabanı geçiş hizmeti örneği tanımlamak için benzersiz olmasıdır.
    - Azure seçin **abonelik** veritabanı geçiş hizmeti oluşturmak istediğiniz.
    - Yeni bir **ağ** benzersiz bir ada sahip.
    - Seçin **konumu** , kaynak veya hedef sunucuya en yakın olan.
    - Select Basic: 1 vCore için **fiyatlandırma katmanı**.

1. **Oluştur**'a tıklayın.

Birkaç dakika sonra Azure veritabanı geçiş hizmetiniz oluşturulur ve kullanıma hazır olacaktır.  Veritabanı geçiş hizmeti görüntüde gösterildiği gibi görürsünüz.

![Geçiş hizmeti oluşturuldu](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıcı silerek oluşturulan kaynakları temizlemek [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md).  Kaynak grubunu silmek için oluşturduğunuz veritabanına geçiş hizmetine gidin, tıklayın **kaynak grubu** adlandırın ve ardından **kaynak grubu Sil**.  Bu eylem tüm kaynak grubunu ve bunun yanı sıra grup ve varlıkları siler.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [SQL geçirmek sunucu şirket içi Azure SQL DB](tutorial-sql-server-to-azure-sql.md)