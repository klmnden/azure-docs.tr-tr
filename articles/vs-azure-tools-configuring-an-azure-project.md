---
title: "Bir Azure bulut hizmeti projesi ile Visual Studio'yu yapılandırma | Microsoft Docs"
description: "Bu proje için gereksinimlerinize bağlı olarak Visual Studio'da bir Azure bulut hizmeti projesi yapılandırmayı öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Bir Azure bulut hizmeti projesi ile Visual Studio'yu yapılandırma
Bu proje için gereksinimlerinize bağlı olarak, bir Azure bulut hizmeti projesi yapılandırabilirsiniz. Aşağıdaki kategorilerde ve proje özelliklerini ayarlayabilirsiniz:

- **Bir bulut hizmeti için Azure yayımlama** -Azure'a dağıtılan var olan bir bulut hizmetini yanlışlıkla silinmez emin olmak için bir özellik ayarlayabilirsiniz.
- **Çalıştırabilir veya yerel bilgisayarda bir bulut hizmetinde hata ayıklama** -kullanın ve Azure storage öykünücüsü başlatmak isteyip istemediğinizi belirtmek için bir hizmet yapılandırması seçebilirsiniz.
- **Bir bulut hizmet paketi oluşturulduğunda doğrulama** -bulut hizmet paketi sorunları dağıtır emin olmak için tüm uyarıları hata olarak değerlendirmek karar verebilirsiniz. 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a>Bir Azure bulut hizmeti projesi yapılandırma adımları
1. Visual Studio'da bir bulut hizmeti projesi oluşturun veya açın

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **özellikleri**.
   
1. Proje özellikleri sayfasını seçin **geliştirme** sekmesi.

    ![Proje Özellikleri menüsü](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Ayarlama **var olan bir dağıtıma silmeden önce sor** için **doğru**. Bu ayar Azure var olan bir dağıtıma yanlışlıkla silmeyin sağlamaya yardımcı olur.

1. İstenen seçin **hizmet yapılandırmasını** çalıştırdığınızda veya Bulut hizmeti yerel olarak hata ayıklama kullanmak istediğiniz hangi hizmet yapılandırmasını belirtmek için. Bir rol için bir hizmet yapılandırmasının nasıl değiştirileceği hakkında daha fazla bilgi için bkz: [Visual Studio ile bir Azure bulut hizmeti için rolleri yapılandırmak nasıl](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Ayarlama **Başlat Azure storage öykünücüsü** için **True** çalıştırdığınızda veya Bulut hizmeti yerel olarak hata ayıklama Azure storage öykünücüsü başlatmak için.

1. Ayarlama **uyarıları hata ele** için **True** paketi doğrulama hatası varsa yayımlayamıyor emin olmak için.

1. Ayarlama **web projesi bağlantı noktalarını kullanacak** için **True** web rolünüz aynı bağlantı noktasını kullandığından emin olmak için her zaman, yerel IIS Express olarak başlatır.

1. Visual Studio araç çubuğundan seçin **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
- [Birden çok hizmet yapılandırmalarını kullanarak bir Azure projesi yapılandırın](vs-azure-tools-multiple-services-project-configurations.md)

