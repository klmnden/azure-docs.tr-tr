---
title: Uygulamanızı - Custom Decision Service kaydetme
titlesuffix: Azure Cognitive Services
description: Bir adım adım nasıl yeni bir uygulama ile Azure Custom Decision Service kaydedileceği yol.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo
ms.openlocfilehash: c56aef180f5e16d2ec1931caabe04295ef288ca9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60509784"
---
# <a name="register-your-application"></a>Uygulamanızı kaydetme

Custom Decision Service, uygulamanız için kullanmak için portalda kaydedin. Bu makalede açıklanır nasıl.

1. Git [ön sayfa](https://portal.ds.microsoft.com/) Custom Decision Service. Şerit üzerinde tıklayın **My portalı**, görüntüde vurgulanan:

    ![My portalı](./media/portal.png)

    Portalda oturum açmak için zaten oturum değilse, ister kendi [Microsoft hesabı](https://account.microsoft.com/account). Açtıktan sonra portal Microsoft hesabınızı sayfanın sağ üst köşedeki görüntüler.

2. Uygulamanızı kaydetmek için tıklatın **yeni uygulama** düğmesi.

3. İletişim kutusunda, uygulamanız için bir uygulama kimliği'ni seçin. Özel karar alma hizmeti, her uygulama için benzersiz bir kimlik gerektirir. Bu kimliği başkası zaten olduysa, sistem Farkl ister.

4. Bir eylem kümesi API belirtin. Bu ayar bir RSS ya da uygulamanız Custom Decision Service için kullanılabilir içerik iletişim kuran Atom akışı. Akış için bir ad girin ve kendisinden hizmet URL'sini girin. Bu adım daha sonra yapmak için tıklatın **akışları** düğmesine ve ardından **yeni akış** düğmesi. Bir RSS akışı oluşturan bir örnek daha sonra açıklanmıştır.

5. Uygulamanızı kaydetmek için seçin **özel uygulama** sol alt köşesindeki onay kutusu. Girin bir [bağlantı dizesi](../../storage/common/storage-configure-connection-string.md) , uygulama verileri günlüğe nerede Azure depolama hesabı için. Bir depolama hesabının nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [oluşturmak, yönetmek veya bir depolama hesabını silmek nasıl](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Sonraki adımlar

* [Bir web sayfasını](custom-decision-service-get-started-browser.md) veya [akıllı telefon uygulamasını](custom-decision-service-get-started-app.md) iyileştirmeye başlayın.
* Çalışmak bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek.
* Sağlanan işlevler hakkında daha fazla bilgi edinmek için [API başvurusu](custom-decision-service-api-reference.md)’na bakın.
