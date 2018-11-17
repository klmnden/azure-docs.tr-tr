---
title: Ekipler ve alt ekipler, Content Moderator API'si | Microsoft Docs
description: Ekipler ve alt ekipler Content Moderator API'si Bilişsel hizmetler için kullanmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 6e1fc08af1062ae8962ba33c6df980182175264b
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852155"
---
# <a name="team-and-subteams"></a>Takım ve alt takımlar #

Content Moderator'ı kullanmadan önce takım oluşturmanız gerekir. Eğer oturum yukarı ve takımınızın, bu takım varsayılan takım haline gelir. Yalnızca İnceleme aracında bir takım da olabilir. Ancak, birden çok alt ekipler oluşturabilirsiniz. Alt ekipler, yükseltme ekipler veya belirli kategorileri içeriğinin gözden geçirme için adanmış ekipler oluşturmak için kullanışlıdır. Örneğin, daha fazla inceleme için farklı bir takım yetişkinlere yönelik içeriğe göndermek isteyebilirsiniz.

Bu konuda, alt ekipler oluşturma ve hızlı bir şekilde gözden geçirmeleri hareket halindeyken atama açıklanmaktadır. Ancak, kullanabileceğiniz [iş akışları](workflows.md) belirli ölçütlere göre incelemeleri atamak için.

## <a name="go-to-the-teams-setting"></a>Takımlar Git ayarı ##

Bir alt oluşturmaya başlamak için seçin **takımlar** seçeneği ayarlar.

![Takım ayarları](images/0-teams-1.png)

## <a name="create-subteams"></a>Alt takımlar oluşturma ##

Varsayılan takımın tüm olası gözden geçirenler içerir. alt ekipler varsayılan takım kümeleridir. Tüm gözden geçirenler artık varsayılan takıma eklediğiniz gerek zaten varsayılan takım üyelerinin yoksa, birisi için bir alt atayamazsınız. Takım sayfasını Davet Et düğmesine tıklayın.

![Kullanıcıları davet](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Bir alt oluşturun.
Takım sayfasını alt bölümüne aşağı kaydırın. Alt Ekle düğmesine tıklayın. 

![Alt Ekle](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Alt adlandırın.
İletişim kutusunda, alt adınızı girin ve Kaydet'e tıklayın.

![Alt ad](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Üyeleri varsayılan takımınızdan atayın.
Bir veya daha fazla alt ekipler için varsayılan takımınızdan üyeleri atamak Üye Ekle düğmesine tıklayın. Bu gibi durumlarda, mevcut kullanıcıları yalnızca bir alt için ekleyebilirsiniz. Gözden geçirme aracı olmayan yeni kullanıcılar eklemek için onları takım Ayarları sayfasında "Davet et" düğmesini kullanarak davet edin.

![Alt üye atama](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Ata, alt takımlar için gözden geçirmeleri ##

Oluşturulan, alt takımlar olduğunda ve takım üyeleri, atanan görüntü atama başlayabilir ve söz konusu alt takımlar için metin inceler. Bu gözden geçirme penceresinden gerçekleştirilir.
Tek bir görüntü için bir alt atamak istiyorsanız, görüntü sağ üst köşesindeki üç nokta simgesine tıklayın, Git seçin ve bir alt seçin.

![Görüntü gözden subteam atayın](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Atanan içeriği gözden geçirmek için alt ekipler arasında geçiş yapın ##

Bir veya daha fazla alt ekipler üyesiyseniz, gözden geçirme aracı panosundan bu alt ekipler arasında geçiş yapabilirsiniz. Tüm incelemeleri için bir alt ait bekleyen geçerli görmek için görüntü sekmesinden alt seçin'i seçin.

![Alt ekipler arasında geçiş yapın](images/3-review-image-subteam-2.png)
