---
title: "Bir Analysis Services sunucusuna oluşturma | Microsoft Docs"
description: "Azure Analysis Services sunucu örneği oluşturmayı öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: 54b9c9df1c368575c271a44e4738a2fc25ac4798
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Azure portalında bir Azure Analysis Services sunucusu oluşturma
Bu makalede, Azure aboneliğinizde bir Analysis Services sunucu kaynağı oluşturmada size anlatılmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
* **Azure Active Directory**: aboneliğinizi bir Azure Active Directory Kiracı ile ilişkilendirilmiş olması gerekir. Ve Azure'a, Azure Active Directory'de bir hesapla oturum açmanız gerekir. Microsoft hesapları desteklenmez. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).
* **Kaynak grubu**: zaten bir kaynak grubunu kullanın veya [yeni bir tane oluşturun](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Sunucu oluşturulması ek hizmet ücretlerine neden olabilir. Daha fazla bilgi için bkz. [Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="to-create-a-server-in-azure-portal"></a>Azure portalında bir sunucu oluşturmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Tıklatın **+ yeni** > **veri + analiz** > **Analysis Services**.
3. İçinde **Analysis Services** dikey penceresinde, gerekli alanları doldurun ve sonra basın **oluşturma**.
   
    ![Sunucu oluşturma](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Sunucu adı**: sunucu başvurmak için kullanılan benzersiz bir ad yazın.
   * **Abonelik**: Bu sunucunun bills için aboneliği seçin.
   * **Kaynak grubu**: Bu kapsayıcılar Azure kaynağı koleksiyonunu yönetmenize yardımcı olmak için tasarlanmıştır. Daha fazla bilgi için bkz: [kaynak grupları](../azure-resource-manager/resource-group-overview.md).
   * **Konum**: Bu Azure veri merkezi konum barındıran sunucu. En büyük kullanıcı tabanınızı en yakın bir konum seçin.
   * **Fiyatlandırma katmanı**: bir fiyatlandırma katmanı seçin. Tablolu modeller 400 GB'a kadar desteklenir. Daha fazla bilgi için bkz: [Azure Analysis Services fiyatlandırma](https://azure.microsoft.com/pricing/details/analysis-services/).
4. **Oluştur**’a tıklayın.

Oluşturma genellikle bir dakika altında; alır genellikle yalnızca birkaç saniye sayısı. Seçtiyseniz **eklemek için Portal**, yeni sunucunuzu görmek için portalınıza gidin. Ya da gidin **tüm hizmetleri** > **Analysis Services** sunucunuz hazır olup olmadığını görmek için.

 ![Pano](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Sonraki adımlar
Sunucunuz oluşturduktan sonra [bir model dağıtma](analysis-services-deploy.md) ona SSDT kullanarak veya SSMS ile.

Sunucunuza dağıttığınız bir model şirket içi veri kaynaklarına bağlanıyorsa, yüklemek gereken bir [şirket içi veri ağ geçidi](analysis-services-gateway.md) ağınızdaki bir bilgisayara.

