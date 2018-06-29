---
title: MySQL bağdaştırıcısı RP AzureStack üzerinde tarafından sağlanan veritabanlarını kullanma | Microsoft Docs
description: Oluşturma ve MySQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan MySQL veritabanlarını yönetme
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
ms.openlocfilehash: fdadc29aa1d25e90afe088053ae4fe2139fa7f16
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031846"
---
# <a name="create-mysql-databases"></a>MySQL veritabanı oluşturma

Oluşturun ve veritabanlarının Self Servis Kullanıcı Portalı'nda yönetin. Bir Azure yığın kullanıcı MySQL veritabanı hizmeti içeren bir teklif abonelikle gerekir.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Bir MySQL veritabanı oluşturarak dağıtımınızı test edin

1. Azure yığın kullanıcı portalında oturum açın.
2. Seçin **+ yeni** > **veri + depolama** > **MySQL veritabanı** > **eklemek**.
3. Altında **MySQL veritabanı oluşturma**, veritabanı adı girin ve ortamınız için gerektiği gibi diğer ayarları yapılandırın.

    ![MySQL veritabanı test oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Altında **Create Database**seçin **SKU**. Altında **MySQL SKU seçin**, veritabanınız için SKU seçin.

    ![Bir MySQL SKU seçin](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

    >[!Note]
    >Barındırma sunucuları Azure yığınına eklendikçe bir SKU atanmış oldukları. Veritabanlarını barındıran bir SKU sunucu havuzundaki oluşturulur.

5. Altında **oturum açma**seçin ***gerekli ayarları Yapılandır***.
6. Altında **bir oturum açma seçin**, var olan bir oturum seçin ya da seçin **+ yeni bir oturum açma oluşturmak** yeni bir oturum açma ayarlamak için.  Girin bir **veritabanı oturum açmayı** adı ve **parola**ve ardından **Tamam**.

    ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >Veritabanı oturum açma adının uzunluğu MySQL 5.7 32 karakteri aşamaz. Önceki sürümlerinde, 16 karakterden uzun olamaz.

7. Seçin **oluşturma** veritabanı kurulumunun tamamlanması için.

Veritabanı dağıtıldıktan sonra not edin **bağlantı dizesi** altında **Essentials**. Bu dize MySQL veritabanına erişmesi gereken herhangi bir uygulamada kullanabilirsiniz.

![MySQL veritabanı için bağlantı dizesi alma](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Yönetici parolasını güncelleştirin

Parola MySQL server örneğinde değiştirerek değiştirebilirsiniz.

1. Seçin **yönetim KAYNAKLARININ** > **sunucuları barındırma MySQL**. Barındırma sunucusu seçin.
2. Altında **ayarları**seçin **parola**.
3. Altında **parola**, yeni bir parola girin ve ardından **kaydetmek**.

![Yönetici parolasını güncelleştirin](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını güncelle](azure-stack-mysql-resource-provider-update.md)
