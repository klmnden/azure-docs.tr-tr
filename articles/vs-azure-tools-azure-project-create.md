---
title: Visual Studio ile bir Azure bulut hizmeti projesi oluşturma | Microsoft Docs
description: Şimdi Visual Studio ile bir Azure bulut hizmeti projesi oluşturmayı öğrenin
services: visual-studio-online
author: ghogen
manager: douge
assetId: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 06213ecabf3669bf3b8cf2b8d73a4e8def359536
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791571"
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Visual Studio ile bir Azure bulut hizmeti projesi oluşturma
Visual Studio için Azure Araçları, bir Azure bulut hizmeti oluşturmanıza olanak sağlayan bir proje şablonu sağlar. Proje oluşturulduktan sonra yapılandırmak, hata ayıklama ve bulut hizmeti Azure'a dağıtmak Visual Studio sağlar.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Visual Studio'da bir Azure bulut hizmeti projesi oluşturma adımları
Bu bölümde bir veya daha fazla web rolleri ile Visual Studio'da bir Azure bulut hizmeti projesi oluşturmada size yol gösterir.  

1. Visual Studio'yu yönetici olarak başlatın.

1. Ana menüde seçin **dosya** > **yeni** > **proje**.

1. Seçin **bulut** Visual C# veya Visual Basic proje şablonu düğümleri ve seçin **Azure bulut hizmeti** şablonları listesinden.

    ![Yeni Azure bulut hizmeti](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Projenizi geliştirmek için kullanmak istediğiniz .NET Framework sürümünü belirtin.

1. Bir ad ve projenizin konumunu ve çözüm için bir ad girin. 

1. **Tamam**’ı seçin.

1. İçinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda, eklemek istediğiniz rolü seçin ve sağ ok düğmesine çözümünüze ekleyin.

    ![Yeni Azure bulut hizmeti rollerinizi seçin](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Eklediğiniz bir rolü yeniden adlandırmak için vurgulu rolünde üzerinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda ve bağlam menüsünden seçin **yeniden adlandırma**. Bir rol, çözümünüz içinde adlandırabilirsiniz (içinde **Çözüm Gezgini**) eklendikten sonra.

    ![Azure bulut hizmeti rolü yeniden adlandır](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

Visual Studio Azure project çözümdeki rol projelerine ilişkileri içerir. Proje ayrıca içeriyor *hizmet tanımı dosyası* ve *hizmet yapılandırma dosyası*:

- **Hizmet tanımı dosyası** -uygulama, hangi rollerin gerekli dahil olmak üzere, uç noktaları ve sanal makine boyutu için çalışma zamanı ayarları tanımlar. 
- **Hizmet yapılandırma dosyası** -kaç rol çalıştırın ve bir rol için tanımlanan ayarlarının değerleri örnekleridir yapılandırır. 

Bu dosyalar hakkında daha fazla bilgi için bkz: [rolleri bir Azure bulut hizmeti için Visual Studio ile yapılandırma](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Visual Studio ile Azure bulut hizmeti projelerinde rollerini yönetme](./vs-azure-tools-cloud-service-project-managing-roles.md)
