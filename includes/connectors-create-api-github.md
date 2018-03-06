---
title: "include dosyası"
description: "include dosyası"
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: ec5b3ca9ccd139cbdf17768056eb1d835336e7a7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
1. İçinde [Azure portal](https://portal.azure.com), boş mantıksal uygulama oluşturma. 

2. Logic Apps Tasarımcısı'nda "github", filtre olarak girin. 

3. Kullanmak istediğiniz tetiği ve GitHub Bağlayıcısı'nı seçin.

   ![GitHub Bağlayıcısı'nı ve Tetikleyici seçin](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Tüm mantıksal uygulama iş akışları tetikleyici ile başlamalıdır. Yalnızca mantığı iş akışınızı bir tetikleyici ile zaten başladığında eylemleri seçebilirsiniz. 

4. Daha önce bir bağlantı oluşturmadıysanız, seçin **oturum** GitHub kimlik bilgileriniz istendiğinde sağlayabilir.  

   ![GitHub kimlik bilgilerinizle oturum](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Mantıksal uygulamanızı bağlanma ve GitHub hesabınız için verilere erişme yetkisi vermek için bu kimlik bilgilerini kullanır. 

5. GitHub kullanıcı adını ve parolasını girin, sonra yetkilendirme onaylayın.

   ![Kimlik bilgilerini sağlayın ve yetkilendirmeyi Onayla](./media/connectors-create-api-github/github-connector-authorize.png)   

   Bağlantınızı artık Azure portalında oluşturulur ve kullanıma hazırdır.

6. Mantıksal uygulama akışınızı tanımlamaya devam edin.

   ![Daha fazla eylem mantığı uygulama akışınıza ekleme](./media/connectors-create-api-github/github-connector-logic-app.png)

