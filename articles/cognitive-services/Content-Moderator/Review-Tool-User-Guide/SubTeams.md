---
title: Ekipleri ve içerik denetleyici API'sindeki subteams | Microsoft Docs
description: Ekipleri ve subteams içerik yönetici API'si Bilişsel hizmetler için nasıl kullanılacağını öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 161c7cd8bac07d5ffc138297d98a40317a8d88fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351539"
---
# <a name="team-and-subteams"></a>Takım ve Subteams #

İçerik denetleyici kullanmadan önce bir takım oluşturmanız gerekir. Oturum ayarlama ve ekibinizin ad, bu takım varsayılan takım haline gelir. Gözden geçirme aracında bir ekip yalnızca olabilir. Ancak, birden çok subteams oluşturabilirsiniz. Subteams yükseltme ekipler veya belirli kategorilerden içeriği gözden geçirme için ayrılmış gruplar oluşturmak için faydalıdır. Örneğin, daha fazla inceleme için farklı bir takım yetişkinlere yönelik içeriğe göndermek isteyebilirsiniz.

Bu konuda, subteams oluşturmak ve hızlı bir şekilde incelemeler kolay bir şekilde atamak açıklanmaktadır. Ancak, kullanabileceğiniz [iş akışları](workflows.md) belirli ölçütlere göre incelemeler atamak için.

## <a name="go-to-the-teams-setting"></a>Takımlar gidin ayarı ##

Bir alt oluşturmaya başlamak için seçin **takımlar** seçeneği ayarlar altında.

![Takım ayarları](images/0-teams-1.png)

## <a name="create-subteams"></a>Subteams oluşturma ##

Varsayılan takım tüm olası gözden geçirenler içerir; subteams varsayılan takım kümeleridir. Şimdi varsayılan ekibine tüm gözden geçirenler eklemeniz gerekir böylece zaten varsayılan takım üyeleri değillerse biri için bir alt atayamazsınız. Takım sayfasında Davet Et düğmesine tıklayın.

![Kullanıcıları davet](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Bir alt oluşturun.
Takım sayfanın alt kısmına kaydırın. Alt Ekle düğmesini tıklatın. 

![Alt ekleme](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Alt olarak adlandırın.
İletişim kutusunda alt adınızı girin ve Kaydet'e tıklayın.

![Alt adı](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Üyeleri, varsayılan ekibinden atayın.
Bir veya daha fazla subteams varsayılan ekibinizin üyeleri atamak Üye Ekle düğmesini tıklatın. Bu gibi durumlarda, mevcut kullanıcıları yalnızca bir alt ekleyebilirsiniz. Gözden geçirme aracı olmayan yeni kullanıcılar eklemek için bunları takım Ayarları sayfasında "Davet" düğmesini kullanarak davet edin.

![Alt üyeleri atayın](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Ata, subteams incelemeleri ##

Oluşturulan, subteams sahip bir kez ve ekip üyelerinin, atanan görüntü atama başlayabilir ve bu subteams metin inceler. Bu gözden geçirme penceresinden gerçekleştirilir.
Tek başına bir resmi bir alt atamak istiyorsanız, görüntü sağ üst köşesindeki üç nokta düğmesine, Git seçin ve bir alt seçin.

![Görüntü gözden subteam atayın](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Atanan içeriği gözden geçir subteams arasında geçiş ##

Bir veya daha fazla subteams üyesiyseniz bu subteams gözden geçirme araçları panosundan arasında geçiş yapabilirsiniz. Tüm geçerli bir alt ait incelemeler bekleyen görmek için görüntü sekmesinden seçin alt seçin.

![Subteams arasında geçiş](images/3-review-image-subteam-2.png)
