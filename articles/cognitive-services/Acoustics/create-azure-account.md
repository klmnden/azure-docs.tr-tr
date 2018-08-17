---
title: Akustik - Bilişsel hizmetler için Azure hesapları
description: Azure Batch ve Storage hesaplarını akustik ile çalışmak için gerekli ayarlamak için bu kılavuzu izleyin.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181764"
---
# <a name="create-an-azure-batch-account"></a>Azure Batch hesabı oluşturma
Azure Batch ve Storage hesaplarını akustik ile çalışmak için gerekli ayarlamak için bu kılavuzu izleyin. Proje akustik bir parçası olarak geliştirilen Unity eklenti hakkında daha fazla bilgi için bkz. [akustik nedir](what-is-acoustics.md). Akustik Unity projenize ekleyeceğiniz hakkında daha fazla bilgi için bkz: [Başlarken](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Azure aboneliği edinme
Bir [Azure abonelik](https://azure.microsoft.com/free/) Batch ve Storage hesaplarını ayarlama önce gereklidir. İlk kez oturum açtığınız, Azure birkaç süre sınırlı ücretsiz kaynakları ve 200 ABD Doları kredi sağlar.

## <a name="create-azure-batch-and-storage-accounts"></a>Azure Batch ve depolama hesapları oluşturun
Ardından, izleyin [bu yönergeleri](https://docs.microsoft.com/azure/batch/batch-account-create-portal) için Azure ayarlamak ve ilişkili Azure depolama hesapları.

Batch ve depolama hesapları için varsayılan seçenekleri seçin:
  
  ![Yeni Batch hesabı](media/NewBatchAccountCreate.png)

  ![Yeni depolama hesabı](media/BatchStorageAccountCreate.png)

Azure hesapları dağıtmak birkaç dakika sürer. Tamamlama bildirim portalında sağ üst köşedeki arayın.
  
  ![Dağıtılan bir hesapları](media/BatchAccountsDeployNotification.png)

Artık hesaplarınızı Panonuzda görünür olmalıdır.
  
  ![Portalı Panosu](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Azure kimlik bilgileriyle akustik hazırlama kullanıcı Arabirimi ayarlama
Panoda Batch hesabı bağlantısına tıklayın ve ardından tıklayarak **anahtarları** bağlantısı kimlik bilgilerinizi erişmek için Batch hesabı sayfası.
  
  ![Batch anahtarları bağlantı](media/BatchAccessKeys.png)

  ![Batch hesabı kimlik bilgileri](media/BatchKeysInfo.png)

Tıklayarak **depolama hesabı** sayfasında Azure depolama hesabı kimlik bilgilerinizi erişmek için bağlantı.
  
  ![Depolama Hesabı Kimlik Bilgileri](media/StorageKeysInfo.png)

Bu kimlik bilgileri bölümünde anlatıldığı gibi hazırlama sekmede girin [UI izlenecek yol hazırlama](bake-ui-walkthrough.md).

## <a name="node-types-and-region-support"></a>Düğüm türleri ve bölge desteği
Proje akustik F ve H serisi, tüm Azure bölgelerinde desteklenmeyebilir Azure VM düğümleri için iyileştirilmiş işlem gerektirir. Lütfen denetleyin [bu tabloda](https://azure.microsoft.com/global-infrastructure/services) Batch hesabınız için doğru konuma çekme emin olmak için. Bu süre H serisi sanal makineler, Doğu ABD, Kuzey Orta ABD, Güney Orta ABD, Batı ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa ve Japonya Batı desteklenir.

## <a name="upgrading-your-quota"></a>Kota yükseltme
Azure Batch hesapları oluşturma 20 sınırına sahip işlem çekirdek hesabında sağlanır. Akustik iş yükünüz, sahnedeki araştırma noktası sayısı kadar çok sayıda düğüm arasında getirebilen olduğundan daha hızlı hazırlama süreleri için bu sınırı artırmak isteyebilirsiniz. Tıklayarak bir kota artışı isteyebilir **kota** bağlantı, Azure Batch portalı sayfasında ve ardından tıklayarak **istek kotasını artırmak**:

![Azure kota artışı](media/azurequotas.png)

## <a name="next-steps"></a>Sonraki adımlar
* Başlama [akustik Unity projeniz tümleştirme](getting-started.md)
* Keşfedin [örnek Sahne](sample-walkthrough.md)

