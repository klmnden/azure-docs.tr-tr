---
title: Proje akustik Azure Batch hesabı kurulumu
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılır proje akustik Unity ve Unreal engine tümleştirmeleri ile kullanmak için Azure Batch hesabı ayarlama açıklanır.
services: cognitive-services
author: ashtat
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: db4f96ff7c355f3582966e4daa945f54a6e5b847
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616541"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Proje akustik Azure Batch hesabı kurulumu
Bu nasıl yapılır proje akustik Unity ve Unreal engine tümleştirmeleri ile kullanmak için Azure Batch hesabı ayarlama açıklanır.

## <a name="get-an-azure-subscription"></a>Azure aboneliği edinme
Bir [Azure abonelik](https://azure.microsoft.com/free/) Batch ve Storage hesaplarını ayarlama önce gereklidir. İlk kez oturum açtığınız, Azure birkaç süre sınırlı ücretsiz kaynakları ve 200 ABD Doları kredi sağlar.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch ve depolama hesapları oluşturun
Ardından, izleyin [bu yönergeleri](https://docs.microsoft.com/azure/batch/batch-account-create-portal) için Azure ayarlamak ve ilişkili Azure depolama hesapları.

Batch ve depolama hesapları için varsayılan seçenekleri seçin:
  
  ![Seçenekleri varsayılan ayarları gösteren ekran görüntüsü, Azure Batch yeni hesapları](media/new-batch-account-create.png)

  ![Seçenekleri varsayılan ayarları gösteren ekran görüntüsü, Azure depolama yeni hesapları](media/batch-storage-account-create.png)

Azure hesapları dağıtmak birkaç dakika sürer. Tamamlama bildirim portalında sağ üst köşedeki arayın.
  
  ![Bildirim, ekran Azure hesapları dağıtılan](media/batch-accounts-deploy-notification.png)

Artık hesaplarınızı Panonuzda görünür olmalıdır.
  
  ![Batch ve depolama hesabını gösteren ekran görüntüsü, Azure portalı Panosu](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Azure kimlik bilgileriyle akustik hazırlama kullanıcı Arabirimi ayarlama
Panoda Batch hesabı bağlantısına tıklayın ve ardından tıklayarak **anahtarları** bağlantısı kimlik bilgilerinizi erişmek için Batch hesabı sayfası.
  
  ![Anahtarları sayfasına vurgulanmış ekran görüntüsü, Azure Batch hesabıyla](media/batch-access-keys.png)

  ![Ekran görüntüsü, Azure Batch hesabı anahtarları erişim anahtarlarını sayfayla](media/batch-keys-info.png)

Tıklayarak **depolama hesabı** sayfasında Azure depolama hesabı kimlik bilgilerinizi erişmek için bağlantı.
  
  ![Ekran görüntüsü, Azure depolama hesabı anahtarları erişim anahtarlarını sayfayla](media/storage-keys-info.png)

Bu kimlik bilgilerini girin [Unity hazırlama eklentisi](unity-baking.md) veya [Unreal hazırlama eklentisi](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Düğüm türleri ve bölge desteği
Proje akustik Fsv2 ve H serisi, tüm Azure bölgelerinde desteklenmeyebilir Azure VM düğümleri için iyileştirilmiş işlem gerektirir. Lütfen denetleyin [bu tabloda](https://azure.microsoft.com/global-infrastructure/services) Batch hesabınız için doğru konuma çekme emin olmak için.
![Bölgeye göre Azure sanal makineler gösteren ekran görüntüsü](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Kota yükseltme
Azure Batch hesapları oluşturma 20 sınırına sahip işlem çekirdek hesabında sağlanır. Akustik iş yükünüz, sahnedeki araştırma noktası sayısı kadar çok sayıda düğüm arasında getirebilen çünkü biz daha hızlı hazırlama süreleri için bu sınırı artırmak isteyebilirsiniz. Tıklayarak bir kota artışı isteyebilir **kota** bağlantı, Azure Batch portalı sayfasında ve ardından tıklayarak **istek kotasını artırmak**:

![Azure kotası ekran sayfası](media/azure-quotas.png)

## <a name="next-steps"></a>Sonraki adımlar
* Proje akustik eklentisi ile tümleştirmek, [Unity](unity-integration.md) veya [Unreal](unreal-integration.md) proje

