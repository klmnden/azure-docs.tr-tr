---
title: "Azure portalını kullanarak Azure Veritabanı Geçiş Hizmeti örneği oluşturma | Microsoft Docs"
description: "Azure Veritabanı Geçiş Hizmeti örneği oluşturmak için Azure portalını kullanma"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 12/13/2017
ms.openlocfilehash: 9dea80b0a6848bd69541aa9f7e0a0fe111fa0a28
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Veritabanı Geçiş Hizmeti örneği oluşturma
Bu Hızlı Başlangıç kılavuzunda Azure Veritabanı Geçiş Hizmeti’nin bir örneğini oluşturmak için Azure portalını kullanacaksınız.  Hizmeti oluşturduktan sonra, şirket içi SQL Server’daki verileri bir Azure SQL veritabanına geçirmek için bu hizmeti kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
Web tarayıcınızı açın, [Microsoft Azure portalına gidin](https://portal.azure.com/) ve kimlik bilgilerinizi girerek portalda oturum açın.

Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme
İlk Veritabanı Geçiş Hizmeti örneğinizi oluşturmadan önce Microsoft.DataMigration kaynak sağlayıcısını kaydedin.

1. Azure portalında, **Tüm hizmetler**’i seçin ve sonra **Abonelikler**’i seçin.

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

3. Geçiş öğesini arayın ve sonra Microsoft.DataMigration öğesinin sağ tarafındaki **Kaydet**’i seçin.

![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Hizmetin bir örneğini oluşturma
1. Şu anda önizleme sürümünde olan Azure Veritabanı Geçiş Hizmeti’nin bir örneğini oluşturmak için **+ Kaynak oluştur**’a tıklayın.

2. "Geçiş" için marketi seçin, **Azure Veritabanı Geçiş Hizmeti**’ni seçin ve **Azure Veritabanı Geçiş Hizmeti (önizleme)** ekranında, **Oluştur**’a tıklayın.

3. **Veritabanı Geçiş Hizmeti** ekranında: 

    - Azure Veritabanı Geçiş Hizmeti örneğinizi tanımlamak için akılda kalıcı ve benzersiz bir **Hizmet adı** seçin.
    - Örneği oluşturmak istediğiniz Azure **Aboneliğini** seçin.
    - Benzersiz bir ada sahip yeni bir **Ağ** oluşturun.
    - Kaynak veya hedef sunucunuza en yakın **Konum**’u seçin.
    - **Fiyatlandırma katmanı** için Temel: 1 Sanal Çekirdek seçeneğini belirleyin.

    ![Geçiş hizmetini oluşturma](media/quickstart-create-data-migration-service-portal/dms-create-service.png)
4. **Oluştur**’u seçin.

Birkaç dakika sonra Azure Veritabanı Geçiş Hizmeti örneğiniz oluşturulur ve kullanıma hazır hale gelir. Veritabanı Geçiş Hizmeti aşağıdaki resimde gösterildiği gibi görüntülenir:

![Geçiş hizmeti oluşturuldu](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
[Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silerek bu Hızlı Başlangıç kılavuzunda oluşturduğunuz kaynakları temizleyebilirsiniz.  Kaynak grubunu silmek için, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğine gidin. **Kaynak grubu** adını seçin ve **Kaynak grubunu sil** seçeneğini belirleyin.  Bu eylem kaynak grubundaki tüm varlıkları ve kaynak grubunu siler.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Şirket içi SQL Server’dan Azure SQL Veritabanı’na geçiş](tutorial-sql-server-to-azure-sql.md)