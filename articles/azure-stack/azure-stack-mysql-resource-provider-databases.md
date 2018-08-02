---
title: AzureStack üzerinde MySQL bağdaştırıcısı RP tarafından sağlanan veritabanları kullanılarak | Microsoft Docs
description: Nasıl MySQL bağdaştırıcısı kaynak Sağlayıcısı'nı kullanarak sağlanan MySQL veritabanı oluşturma ve yönetme
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 77dca29b0c60726f0a072dd662aba0d12730502a
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413697"
---
# <a name="create-mysql-databases"></a>MySQL veritabanı oluşturma

Oluşturun ve Kullanıcı Portalı'nda, Self Servis veritabanlarını yönetme. Azure Stack kullanıcı MySQL veritabanı hizmeti içeren bir teklif olan bir abonelik gerekir.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Bir MySQL veritabanı oluşturma dağıtımınızı test edin

1. Azure Stack kullanıcı portalında oturum açın.
2. Seçin **+ yeni** > **veri + depolama** > **MySQL veritabanı** > **ekleme**.
3. Altında **MySQL veritabanı oluşturma**, veritabanı adını girin ve ortamınız için gerektiği gibi diğer ayarları yapılandırın.

    ![Test MySQL veritabanı oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Altında **Create Database**seçin **SKU**. Altında **MySQL SKU seçin**, veritabanınız için SKU seçin.

    ![Bir MySQL SKU'ları seçin](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >Azure Stack için barındırma sunucuları eklendikçe, bir SKU atanmış oldukları. Bir SKU sunucuları bulundurma havuzdaki veritabanları oluşturulur.

5. Altında **oturum açma**seçin ***gerekli ayarları Yapılandır***.
6. Altında **bir oturum açma seçin**, var olan bir oturum seçin ya da seçin **+ yeni bir oturum açma Oluştur** yeni bir oturum açma ayarlamak için.  Girin bir **veritabanı oturum açma** adı ve **parola**ve ardından **Tamam**.

    ![Yeni bir veritabanı oturumu oluştur](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >Veritabanı oturum açma adının MySQL 5.7 32 karakter uzunluğunda olabilir. Önceki sürümlerinde, 16 karakterden uzun olamaz.

7. Seçin **Oluştur** veritabanı ayarlama işlemini sonlandırmak için.

Veritabanı dağıtıldıktan sonra Not **bağlantı dizesi** altında **Essentials**. Bu dize MySQL veritabanına erişmesi gereken herhangi bir uygulamada kullanabilirsiniz.

![MySQL veritabanı için bağlantı dizesini alın](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Yönetici parolasını güncelleştirme

Parola MySQL server örneğinde değiştirerek değiştirebilirsiniz.

1. Seçin **yönetim kaynakları** > **MySQL barındırma sunucuları**. Barındırma sunucusu seçin.
2. Altında **ayarları**seçin **parola**.
3. Altında **parola**, yeni bir parola girin ve ardından **Kaydet**.

![Yönetici parolasını güncelleştirme](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını güncelle](azure-stack-mysql-resource-provider-update.md)
