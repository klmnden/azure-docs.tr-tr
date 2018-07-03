---
title: LUIS uygulamalarını azure'da başkalarıyla işbirliği yapma | Microsoft Docs
description: Bilgi Language Understanding (LUIS) uygulamalara başkalarıyla nasıl işbirliği yapılır.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: 55c50f5276e2cf97ac98d75b8fc2bd56f1c5d644
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340422"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Language Understanding (LUIS) apps'de başkalarıyla işbirliği yapın  

Başkalarıyla LUIS uygulamanızı birlikte işbirliği yapabilir. 

## <a name="owner-and-collaborators"></a>Sahibi ve ortak çalışanlar
Uygulama, tek bir sahibi yoktur ancak birçok ortak çalışanlar sahip olabilir. 

## <a name="add-collaborator"></a>Ortak çalışanı Ekle

LUIS uygulamanızı düzenlemek ortak çalışanlar izin vermek için **ayarları** sayfa LUIS uygulamanızı tıklatın ve ortak çalışan e-posta girin **Ekle ortak çalışanı**.

![Ortak çalışanı Ekle](./media/luis-how-to-collaborate/add-collaborator.png)

* Ortak Çalışanlar, oturum açın ve uygulama üzerinde çalıştığınız aynı anda LUIS uygulamanızı düzenleyin. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Ortak Çalışanlar, diğer ortak çalışanlar ekleyemezsiniz.

Bkz: [Azure Active Directory Kiracı Kullanıcı](luis-how-to-account-settings.md#azure-active-directory-tenant-user) Active Directory kullanıcı hesapları hakkında daha fazla bilgi edinmek için. 

## <a name="set-application-as-public"></a>Genel olarak kümesi uygulama
Bkz: [genel uygulama uç noktası erişimi](luis-concept-security.md#public-app-endpoint-access) daha fazla bilgi için.

### <a name="transfer-of-ownership"></a>Sahipliğin aktarılması
LUIS sahipliğin aktarılması şu anda desteklemiyor, ancak uygulamanızı dışa aktarabilir ve LUIS kullanıcı başka bir uygulamayı içeri aktarabilirsiniz. LUIS puanları iki uygulama arasındaki küçük farklılıklar olabilir. 
