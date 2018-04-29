---
title: Bir Analysis Services sunucusuna oluşturma | Microsoft Docs
description: Azure Analysis Services sunucu örneği oluşturmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c49e886ee5b980e8fd059d72eb2e4a3f0dc895c4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-analysis-services-server-in-azure-portal"></a>Azure portalında bir Analysis Services sunucusu oluşturma
Bu makalede, Azure aboneliğinizde bir Analysis Services sunucu kaynağı oluşturmada size anlatılmaktadır.

Başlamadan önce gerekir: 

* **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
* **Azure Active Directory**: aboneliğinizi bir Azure Active Directory Kiracı ile ilişkilendirilmiş olması gerekir. Ve Azure'a, Azure Active Directory'de bir hesapla oturum açmanız gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma 

[Azure portalı](https://portal.azure.com)’nda oturum açın


## <a name="create-a-server"></a>Sunucu oluşturma

1. Tıklatın **+ kaynak oluşturma** > **veri + analiz** > **Analysis Services**.

    ![Portal](./media/analysis-services-create-server/aas-create-server-portal.png)

2. İçinde **Analysis Services**, gerekli alanları doldurun ve ardından basın **oluşturma**.
   
    ![Sunucu oluşturma](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Sunucu adı**: sunucu başvurmak için kullanılan benzersiz bir ad yazın.
   * **Abonelik**: Bu sunucunun ilişkili aboneliği seçin.
   * **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya zaten seçin. Kaynak grupları, Azure kaynağı koleksiyonunu yönetmenize yardımcı olmak için tasarlanmıştır. Daha fazla bilgi için bkz: [kaynak grupları](../azure-resource-manager/resource-group-overview.md).
   * **Konum**: Bu Azure veri merkezi konum barındıran sunucu. En büyük kullanıcı tabanınızı en yakın bir konum seçin.
   * **Fiyatlandırma katmanı**: bir fiyatlandırma katmanı seçin. Test yapıyorsanız ve örnek modeli veritabanını yüklemek istiyorsanız, ücretsiz seçin **D1** katmanı. Daha fazla bilgi için bkz: [Azure Analysis Services fiyatlandırma](https://azure.microsoft.com/pricing/details/analysis-services/). 
    * **Yönetici**: varsayılan olarak, bu, oturum açarken hesap olacaktır. Azure Active Directory'den farklı bir hesap seçebilirsiniz.
    * **Depolama ayarı yedekleme**: isteğe bağlı. Zaten varsa bir [depolama hesabı](../storage/common/storage-introduction.md), model veritabanı yedeklemesi için varsayılan değer olarak belirtin. Ayrıca belirtebilirsiniz [yedekleme ve geri yükleme](analysis-services-backup.md) daha sonra ayarlar.
    * **Depolama anahtar sona erme**: isteğe bağlı. Bir depolama anahtar kullanım süresi belirtin.
3. **Oluştur**’a tıklayın.

Oluşturma genellikle altında bir dakika sürer. Seçtiyseniz **eklemek için Portal**, yeni sunucunuzu görmek için portalınıza gidin. Ya da gidin **tüm hizmetleri** > **Analysis Services** sunucunuz hazır olup olmadığını görmek için.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, sunucunuzun silin. Sunucunuzun içinde **genel bakış**, tıklatın **silmek**. 

 ![Temizleme](./media/analysis-services-create-server/aas-create-server-cleanup.png)


## <a name="next-steps"></a>Sonraki adımlar

[Örnek veri modeli ekleme](analysis-services-create-sample-model.md) sunucunuza.  
[Bir şirket içi veri ağ geçidi yükleme](analysis-services-gateway-install.md) şirket içi veri kaynakları için veri modelinizi bağlanırsa.  
[Tablo modeli projesini dağıtma](analysis-services-deploy.md) Visual Studio'dan.   


