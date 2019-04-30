---
title: include dosyası
description: include dosyası
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: 11280e1678f52ede928cb2a85ea83add222e15fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462598"
---
1. İçinde [Azure portalında](https://portal.azure.com), boş bir mantıksal uygulama oluşturun. 

2. Logic Apps Tasarımcısı'nda filtreniz olarak "github" girin. 

3. GitHub Bağlayıcısı'nı ve kullanmak istediğiniz tetikleyiciyi seçin.

   ![GitHub Bağlayıcısı'nı ve bir tetikleyici seçin](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Tüm mantıksal uygulama iş akışlarını, bir tetikleyici ile başlamalıdır. Yalnızca mantıksal iş akışınızı bir tetikleyiciyle zaten başladığında eylemleri seçebilirsiniz. 

4. Daha önce bir bağlantı oluşturmadıysanız, seçin **oturum** istendiğinde GitHub kimlik bilgilerinizi sağlayabilir.  

   ![GitHub kimlik bilgilerinizle oturum açın](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Mantıksal uygulamanızı, bağlanma ve GitHub hesabınız için verilere erişim yetkisi vermek için bu kimlik bilgilerini kullanır. 

5. GitHub kullanıcı adınızı ve parolanızı sağlayın ve ardından yetkinizi onaylayın.

   ![Kimlik ve yetkilendirme onaylayın](./media/connectors-create-api-github/github-connector-authorize.png)   

   Bağlantınız, artık Azure portalında oluşturulur ve kullanıma hazırdır.

6. Mantıksal uygulama iş akışınızı tanımlamaya devam edin.

   ![Mantıksal uygulama iş akışınızı için daha fazla eylem ekleme](./media/connectors-create-api-github/github-connector-logic-app.png)

