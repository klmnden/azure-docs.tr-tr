---
title: "Visual Studio - Azure Logic Apps mantığı uygulamaları yönetme | Microsoft Docs"
description: "Mantıksal uygulamalar ve diğer Azure varlıkları Visual Studio bulut Gezgini ile yönetme"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: a5bf24de1a7a2b6d4c1ae6416c95d83ef7506da3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Mantıksal uygulamalarınızı Visual Studio bulut Gezgini ile yönetme

Ancak [Azure portal](https://portal.azure.com/) tasarlama ve Azure mantıksal uygulamaları yönetmek harika bir yol sunar, Visual Studio Cloud Explorer mantıksal uygulamalar dahil olmak üzere birçok Azure varlıklar yönetmek için kullanabilirsiniz. Visual Studio bulut Gezgini, Gözat, yönetme, düzenleme sağlar ve logic apps indirme yayımlanır. Yönetim görevleri etkinleştir, devre dışı bırakma ve geçmişini görüntüleme çalıştırmak içerir. 

Erişebilir ve logic apps Visual Studio içinde yönetebilirsiniz önce yükleyin ve bu Visual Studio Araçları Azure Logic Apps için yapılandırın. 

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2015 veya Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [En son Azure SDK'sı](https://azure.microsoft.com/downloads/) (2.9.1 veya üzeri)
* [Visual Studio bulut Gezgini](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Katıştırılmış Tasarımcısı'nı kullanırken web erişimi

## <a name="install-visual-studio-tools-for-logic-apps"></a>Mantıksal uygulamalar için Visual Studio araçlarını yükleme

Önkoşulları yüklendikten sonra indirin ve Visual Studio için Azure Logic Apps Araçları'nı yükleme.

1. Visual Studio'yu açın. Üzerinde **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**.
2. Genişletme **çevrimiçi** Visual Studio Galerisi çevrimiçi arama yapabilmeniz için kategori.
3. Göz atın veya arayın **Logic Apps** bulana kadar **Visual Studio için Azure Logic Apps Araçları**.
4. Karşıdan yükle ve uzantıyı yüklemek için tıklatın **karşıdan**.
5. Yüklendikten sonra Visual Studio'yu yeniden başlatın.

> [!NOTE]
> Azure Logic Apps araçları Visual Studio için doğrudan indirmek için Git [Visual Studio Market'te](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Logic apps Cloud Explorer'da gözatın.

1.  Cloud Explorer açmak için **Görünüm** menüsünde seçin **Cloud Explorer**.
2.  Mantıksal uygulamanız için kaynak grubu veya kaynak türüne göre göz atın. 

    * Kaynak türüne göre göz atarsanız, Azure aboneliğinizi seçin, genişletin **Logic Apps** bölümünde ve mantıksal uygulamanızı seçin. 
    * Kaynak grubu tarafından göz atarsanız, mantıksal uygulamanızı sahip olan kaynak grubunu genişletin ve mantıksal uygulamanızı seçin.

    Mantıksal uygulamanız için komutları görüntülemek için mantıksal uygulamanızı sağ tıklayın ya da bulut Explorer alt kısmındaki arasından **Eylemler** menüsü.

    ![Mantıksal uygulamanızı gözatın.](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Mantıksal uygulamanızı Logic Apps Tasarımcısı ile Düzenle

Bulut Gezgini'nden Azure portalında kullandığınız aynı Tasarımcısı'nda şu anda dağıtılmış mantıksal uygulama açabilirsiniz. 

* Cloud Explorer'da mantıksal uygulamanızı düzenlemek için mantıksal uygulamanızı sağ tıklatın ve seçin **mantığı uygulama Düzenleyicisi ile açık**. 

* Güncelleştirmelerinizi buluta yayımlamak için tercih **Yayımla**. 

* Yeni bir çalışma başlatmayı seçin **tetikleyici çalıştırmak**.

![Logic Apps Tasarımcısı](./media/logic-apps-manage-from-vs/designer.png)

Tasarımcısı'ndan şunları da yapabilirsiniz **karşıdan** bir mantıksal uygulama. Bu eylem otomatik olarak mantıksal uygulama tanımını parameterizes ve tanım bir Azure Resource Manager dağıtım şablonu olarak kaydeder. Bu dağıtım şablonu Azure kaynak grubu projenizi ekleyebilirsiniz.

## <a name="browse-your-logic-app-run-history"></a>Mantıksal uygulamanızı çalıştırma geçmişi göz atın

Mantıksal uygulamanızı çalıştırma geçmişini görüntülemek için mantıksal uygulamanızı sağ tıklatın ve seçin **açık çalıştırma geçmişi**. Gösterilen özellikleri hiçbirinde göre çalışma geçmişinizi yeniden sıralamak için sütun başlığını seçin.

![çalıştırma geçmişi](media/logic-apps-manage-from-vs/runs.png)

Girişleri ve çıkışları her adımdan dahil olmak üzere çalışma sonuçlarını inceleyebilirsiniz bir örneği için çalıştırma geçmişi görüntüleyecek şekilde çalışma örneklerden birini çift tıklatın.

![Geçmiş sonuçlarını, giriş ve çıkış adımları çalıştırın](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Sonraki adımlar

* [İlk mantıksal uygulamanızı oluşturma](logic-apps-create-a-logic-app.md)
* [Tasarım, derleme ve logic apps Visual Studio içinde dağıtma](logic-apps-deploy-from-vs.md)
* [Sık rastlanan örnekleri ve senaryoları inceleyin](logic-apps-examples-and-scenarios.md)
* [Video: Azure Logic Apps ile iş süreçlerini otomatikleştirmek](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: sistemlerinizi Azure Logic Apps ile tümleştirme](http://channel9.msdn.com/Events/Build/2016/P462)
