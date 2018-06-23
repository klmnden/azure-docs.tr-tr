---
title: Bilişsel hizmetler Azure uygulamanızı - kaydetme | Microsoft Docs
description: Yeni bir uygulama Azure özel karar Hizmeti'ne kaydolmak nasıl bir adım adım kılavuzu
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo;alekh;marossi
ms.openlocfilehash: 2aa8fbe77c11df4434eefa4c92d8529d5ca1d885
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354515"
---
# <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanız için özel karar hizmetini kullanmak için portalda kaydedin. Bu makalede açıklanır nasıl.

1. Git [ön sayfa](https://ds.microsoft.com/) özel karar hizmet. Şerit'te tıklatın **My Portal**, görüntü vurgulu olarak:

    ![Portalımı](./media/portal.png)

    Zaten oturum portal, oturum ister, [Microsoft hesabı](https://account.microsoft.com/account). Oturum açtıktan sonra portal sayfasının sağ üst köşesinde Microsoft hesabınızı görüntüler.

2. Uygulamanızı kaydetmek için tıklatın **yeni uygulama** düğmesi.

3. İletişim kutusunda, uygulamanız için bir uygulama kimliği seçin. Özel karar hizmet her uygulama için benzersiz bir kimliği gerektirir. Bu kimliği başka birisi zaten olduysa, sistem, farklı bir çekme ister.

4. Bir eylem kümesi API belirtin. Bu ayar bir RSS ya da özel karar hizmetine, uygulamanız için kullanılabilir içeriği iletişim kuran Atom akışı. Akış için bir ad girin ve kendisinden sunulur URL'sini girin. Bu adımı daha sonra yapmak için **akışları** düğmesine tıklayın ve ardından **yeni akış** düğmesi. Bir RSS oluşturan bir örnek aşağıda açıklanmıştır.

5. Uygulamanızı kaydetmek için seçin **özel uygulama** sol alt köşesindeki onay kutusu. Girin bir [bağlantı dizesi](../../storage/common/storage-configure-connection-string.md) uygulama verilerinizi günlüğe nerede Azure depolama hesabı için. Bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [oluşturma, yönetme veya bir depolama hesabını silmek nasıl](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Sonraki adımlar

* En iyi duruma getirme başlamak [bir Web sayfası](custom-decision-service-get-started-browser.md) veya [bir akıllı telefon uygulaması](custom-decision-service-get-started-app.md).
* Aracılığıyla iş bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek için.
* Başvurun [API Başvurusu](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.