---
title: "Visual Studio ile bir Azure bulut hizmeti projesi oluşturma | Microsoft Docs"
description: "Şimdi Visual Studio ile bir Azure bulut hizmeti projesi oluşturmayı öğrenin"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 1f6ded87b551f660853ea4eb0548f3d942e28fa8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
