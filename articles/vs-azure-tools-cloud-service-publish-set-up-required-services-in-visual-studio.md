---
title: Yayımlama veya Visual Studio'dan bir bulut hizmeti dağıtmak hazırlama | Microsoft Docs
description: Bulut ve depolama hesabı Services'i ayarlamak ve Azure uygulamanızı yapılandırmak için yordamlar hakkında bilgi edinin.
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: 8a7d6f114bfa10170cdfe7126e01a35b02affd20
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="prepare-to-publish-or-deploy-a-cloud-service-from-visual-studio"></a>Yayımlama veya Visual Studio'dan bir bulut hizmeti dağıtmak hazırlama

Bir bulut hizmeti projesini yayımlamak için bu makalede açıklanan aşağıdaki hizmetleri ayarlamanız gerekir:

* A **bulut hizmeti** rollerinizi Azure ortamında çalıştırmak için ve 
* A **depolama hesabı** Blob, kuyruk ve tablo hizmetlerine erişim sağlar.

## <a name="create-a-cloud-service"></a>Bulut hizmeti oluşturun

Bir bulut hizmeti rollerinizi Azure ortamda çalışır. Bir bulut hizmeti Visual Studio veya aracılığıyla oluşturabilirsiniz [Azure portal](https://portal.azure.com/) izleyen bölümlerde açıklandığı gibi.

### <a name="create-a-cloud-service-from-visual-studio"></a>Visual Studio'dan bir bulut hizmeti oluştur

1. Önceden oluşturulmuş bir bulut hizmeti projesi ile proje seçme sağ **Yayımla**.
1. Gerekirse, Microsoft Azure aboneliğinizle ilişkili veya kuruluş hesabı oturum açın ve ardından **sonraki** ilerletmek için **ayarları** sayfası.
1. A **bulut hizmeti oluşturma ve depolama hesabı** iletişim kutusu görüntülenir (değilse, seçin **Yeni Oluştur** gelen **bulut hizmeti** listesi).
1. URL'niz parçası oluşturur ve benzersiz olması gerekir, bulut hizmeti için büyük küçük harf duyarsız bir ad girin. Ayrıca bir bölge veya benzeşim grubu seçin ve çoğaltma seçeneğini seçin.

### <a name="create-a-cloud-service-through-the-azure-portal"></a>Azure Portalı aracılığıyla bulut hizmeti oluştur

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Seçin **bulut Hizmetleri (Klasik)** sayfanın sol tarafında.
1. Seçin **+ Ekle**, (DNS adı, abonelik, kaynak grubunu ve konumu) gerekli bilgileri sağlayın. Bu noktada, daha sonra Visual Studio'da olmadığından bir paketini karşıya yüklemek gerekli değildir.
1. Seçin **oluşturma** işlemini tamamlayın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir depolama hesabı Blob, kuyruk ve tablo hizmetlerine erişim sağlar. Visual Studio üzerinden depolama hesabı oluşturabilirsiniz veya [Azure portal](https://portal.azure.com/).

### <a name="create-a-storage-account-from-visual-studio"></a>Visual Studio'dan bir depolama hesabı oluşturma

1. İçinde **Çözüm Gezgini** önceden oluşturulmuş bir bulut hizmeti projesi ile bulun **bağlantılı Hizmetler** rolü projesi, sağ tıklatın ve seçin düğümde **bağlı hizmet Ekle**. (Visual Studio 2015'te sağ **depolama** düğümü ve select **depolama hesabı oluştur**.)
1. İçinde **bağlantılı Hizmetler** görünür, select listesi **Azure Storage ile bulut depolama**.
1. Görüntülenen Azure Storage iletişim kutusunda seçin **+ oluştur yeni depolama hesabı**, aboneliğiniz, bir ad belirtin iletişim kutusunu getirir fo hesabı, bir fiyatlandırma katmanı, kaynak grubunu ve konumu.
1. Seçin **oluşturma** , bitirdiğinizde. Yeni depolama hesabı, aboneliğinizdeki kullanılabilir depolama hesaplarının listesi görüntülenir.
1. Hesap ve seçin seçin **Ekle**.

### <a name="create-a-storage-account-through-the-azure-portal"></a>Azure portal üzerinden depolama hesabı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Seçin **+ yeni** sol üst kısmında.
1. Seçin **depolama** "Azure Market," ardından altında **depolama hesabı - blob, dosya, tablo, kuyruk** sağdan.
1. (Adı, dağıtım modeli ve benzeri. gerekli bilgileri sağlayın
1. Seçin **oluşturma** işlemini tamamlayın.

## <a name="configure-your-app-to-use-the-storage-account"></a>Depolama hesabı kullanmak için uygulamanızı yapılandırma

Bir depolama hesabı oluşturduktan sonra Visual Studio'dan otomatik olarak bağlanmak URL'leri ve erişim anahtarları dahil olmak üzere bu proje için hizmet yapılandırması güncelleştirir.

Bir bulut hizmeti Visual Studio kullanarak oluşturduysanız **bağlı hizmet Ekle**, açarak bağlantıları kontrol edebilirsiniz `ServiceConfiguration.Cloud.cscfg` ve `ServiceConfiguration.Local.cscfg`.

Bir bulut hizmeti Azure portalı üzerinden oluşturduysanız, aynı adımları [Visual Studio'dan bir depolama hesabı oluşturma](#create-a-storage-account-from-visual-studio) ancak var olan bir hesap seçin yerine yeni bir oluşturma. Visual Studio, daha sonra yapılandırmasını güncelleştirir.

Yapılandırma ayarları el ile kullanım özellik sayfaları Visual Studio için bulut hizmeti projenizi geçerli rolde (role sağ tıklayıp seçin **özellikleri**). Daha fazla bilgi için bkz: [bir depolama hesabı bağlantı dizesi yapılandırma](https://docs.microsoft.com/azure/vs-azure-tools-multiple-services-project-configurations#configuring-a-connection-string-to-a-storage-account).

### <a name="about-access-keys"></a>Erişim anahtarları hakkında

Azure portalı, her Azure storage Hizmetleri ve hesabınız için birincil ve ikincil erişim anahtarları kaynaklara erişmek için kullanabileceğiniz URL'leri gösterir. Depolama Hizmetleri karşı yapılan isteklerin kimliğini doğrulamak için bu tuşlarını kullanın.

İkincil erişim anahtarını depolama hesabınız için birincil erişim anahtarı ile aynı erişim sağlar ve bir yedek olarak birincil erişim anahtarınız riske oluşturulur. Ayrıca, erişim anahtarlarınızı düzenli olarak yeniden önerilir. Birincil anahtarı yeniden oluşturmak ikincil anahtar yeniden oluşturuldu birincil anahtarı kullanım için değiştirebileceğiniz sonra ikincil anahtarı kullanmak için bir bağlantı dizesi ayarı değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio'dan için Azure yayımlama uygulamalar hakkında daha fazla bilgi için bkz [yayımlama Azure araçlarını kullanarak bir bulut hizmeti](vs-azure-tools-publishing-a-cloud-service.md).
