---
title: Ekipler ve alt ekipler, Content Moderator API'si - Content Moderator'ı yönetme
titlesuffix: Azure Cognitive Services
description: Ekipler ve alt ekipler Content Moderator API'si Bilişsel hizmetler için kullanmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e45e06bdcc69dc88d8c979bfbb5a0974f7284b22
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266238"
---
# <a name="manage-review-teams-and-subteams"></a>Gözden geçirme ekipler ve alt ekipler yönetme

Content Moderator'ı kullanmadan önce takım oluşturmanız gerekir. Eğer oturum yukarı ve takımınızın, bu takım varsayılan takım haline gelir. Yalnızca İnceleme aracında bir takım da olabilir. Ancak, birden çok alt ekipler oluşturabilirsiniz. Alt ekipler, yükseltme ekipler veya belirli kategorileri içeriğinin gözden geçirme için adanmış ekipler oluşturmak için kullanışlıdır. Örneğin, daha fazla inceleme için farklı bir takım yetişkinlere yönelik içeriğe göndermek isteyebilirsiniz.

Bu konuda, alt ekipler oluşturma ve hızlı bir şekilde gözden geçirmeleri hareket halindeyken atama açıklanmaktadır. Ancak, kullanabileceğiniz [iş akışları](workflows.md) belirli ölçütlere göre incelemeleri atamak için.

## <a name="go-to-the-teams-setting"></a>Takımlar Git ayarı

Bir alt oluşturmaya başlamak için seçin **takımlar** seçeneği ayarlar.

![Takım ayarları](images/0-teams-1.png)

## <a name="create-subteams"></a>Alt takımlar oluşturma

Varsayılan takımın tüm olası gözden geçirenler içerir. alt ekipler varsayılan takım kümeleridir. Tüm gözden geçirenler artık varsayılan takıma eklediğiniz gerek zaten varsayılan takım üyelerinin yoksa, birisi için bir alt atayamazsınız. Takım sayfasını Davet Et düğmesine tıklayın.

![Kullanıcıları davet](images/invite-users.png)

### <a name="create-a-subteam"></a>Bir alt oluşturma
Takım sayfasını alt bölümüne aşağı kaydırın. Alt Ekle düğmesine tıklayın. 

![Alt Ekle](images/1-teams-1.png)

### <a name="name-your-subteam"></a>Alt adı
İletişim kutusunda, alt adınızı girin ve Kaydet'e tıklayın.

![Alt ad](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>Üyeleri varsayılan takımınızdan atayın.
Bir veya daha fazla alt ekipler için varsayılan takımınızdan üyeleri atamak Üye Ekle düğmesine tıklayın. Bu gibi durumlarda, mevcut kullanıcıları yalnızca bir alt için ekleyebilirsiniz. Gözden geçirme aracı olmayan yeni kullanıcılar eklemek için onları takım Ayarları sayfasında "Davet et" düğmesini kullanarak davet edin.

![Alt üye atama](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Ata, alt takımlar için gözden geçirmeleri

Oluşturulan, alt takımlar olduğunda ve takım üyeleri, atanan görüntü atama başlayabilir ve söz konusu alt takımlar için metin inceler. Bu gözden geçirme penceresinden gerçekleştirilir.
Tek bir görüntü için bir alt atamak istiyorsanız, görüntü sağ üst köşesindeki üç nokta simgesine tıklayın, Git seçin ve bir alt seçin.

![Görüntü gözden subteam atayın](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Atanan içeriği gözden geçirmek için alt ekipler arasında geçiş yapın

Bir veya daha fazla alt ekipler üyesiyseniz, gözden geçirme aracı panosundan bu alt ekipler arasında geçiş yapabilirsiniz. Tüm incelemeleri için bir alt ait bekleyen geçerli görmek için görüntü sekmesinden alt seçin'i seçin.

![Alt ekipler arasında geçiş yapın](images/3-review-image-subteam-2.png)
