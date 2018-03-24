---
title: Machine Learning web hizmetleri için günlüğe kaydetme | Microsoft Docs
description: Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirmek öğrenin. Günlük API'leri gidermenize yardımcı olacak ek bilgiler sağlar.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.openlocfilehash: 345c1b1bd72d3df19f8b087d4cbffd4e25e092ab
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="enable-logging-for-machine-learning-web-services"></a>Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirme
Bu belgede, Machine Learning web hizmetleri günlüğe kaydetme özelliği bilgiler sağlanmaktadır. Günlük yalnızca bir hata numarası ve Machine Learning API'ları aramalarınız gidermenize yardımcı olabilecek bir ileti ötesinde ek bilgi sağlar.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Bir Web hizmeti için günlük kaydını etkinleştirme

Gelen günlük kaydını etkinleştir [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portal. 

1. Oturum açtığınızda Azure Machine Learning Web Hizmetleri portalında [ https://services.azureml.net ](https://services.azureml.net). Klasik web hizmeti, ayrıca Portalı'na tıklayarak alabilirsiniz **yeni Web Hizmetleri deneyiminizin** Machine Learning Studio'da Machine Learning Web Hizmetleri sayfasında.

   ![Yeni Web Hizmetleri deneyiminizin bağlantısı](./media/web-services-logging/new-web-services-experience-link.png)

2. Üst menü çubuğunda **Web Hizmetleri** için yeni web hizmeti veya tıklatın **Klasik Web Hizmetleri** için Klasik web hizmeti.

   ![Yeni veya Klasik web hizmetleri seçin](./media/web-services-logging/select-web-service.png)

3. İçin yeni bir web hizmeti, web hizmeti adına tıklayın. Klasik web hizmeti, web hizmeti adına tıklayın ve uygun endpoint sonraki sayfada'ı tıklatın.

4. Üst menü çubuğunda **yapılandırma**.

5. Ayarlama **Enable Logging** için seçenek *hata* (yalnızca hataları günlüğe kaydetmek için) veya *tüm* (için tam günlük kaydı).

   ![Günlük düzeyini seçin](./media/web-services-logging/enable-logging.png)

6. **Kaydet**’e tıklayın.

7. Klasik web hizmetleri için oluşturmanız **ml tanılama** kapsayıcı.

   Tüm web hizmeti günlükleri adlı blob kapsayıcısında tutulur **ml tanılama** web hizmeti ile ilişkili depolama hesabındaki. Yeni web hizmetleri için bu kapsayıcı web hizmetine erişim ilk kez oluşturulur. Klasik web hizmetleri için zaten yoksa kapsayıcı oluşturmanız gerekir. 

   1. İçinde [Azure portal](https://portal.azure.com), web hizmeti ile ilişkilendirilmiş depolama hesabına gidin.

   2. Altında **Blob hizmeti**, tıklatın **kapsayıcıları**.

   3. Varsa kapsayıcı **ml tanılama** mevcut değil, tıklatın **+ kapsayıcı**, kapsayıcı "ml-Tanılama" ad verin ve seçin **erişim türüne** "Blob" olarak. **Tamam**’a tıklayın.

      ![Günlük düzeyini seçin](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Klasik web hizmeti, Machine Learning Studio Web Hizmetleri panosunda da günlüğe kaydetmeyi etkinleştirmek için bir anahtar vardır. Ancak, günlük kaydı Web Hizmetleri Portalı aracılığıyla şimdi yönetildiğinden, bu makalede anlatıldığı gibi günlüğe kaydetme portalı üzerinden etkinleştirmeniz gerekir. Studio'da oturum zaten etkinleştirilmişse Web Hizmetleri Portalı'nda günlüğü devre dışı bırakın ve yeniden etkinleştirin.


## <a name="the-effects-of-enabling-logging"></a>Günlüğü etkinleştirme etkileri
Günlük kaydı etkinleştirildiğinde tanılama ve web hizmeti uç noktası hatalarından oturum **ml tanılama** Azure depolama hesabı blob kapsayıcısında kullanıcının çalışma alanıyla bağlı. Bu kapsayıcıdaki tüm web hizmeti uç noktaları için bu depolama hesabıyla ilişkili tüm çalışma alanları için tüm tanılama bilgileri tutar.

Günlükleri, herhangi bir Azure depolama hesabı keşfetmek kullanılabilen çeşitli araçlar kullanılarak görüntülenebilir. Daha kolay olabilir Azure Portal'da depolama hesabı gitmek için **kapsayıcıları**ve kapsayıcı ardından **ml tanılama**.  

## <a name="log-blob-detail-information"></a>Günlük blob ayrıntı bilgileri
Her bir blob kapsayıcısında tanılama bilgileri için aşağıdaki eylemleri tam olarak birine tutar:

* Bir toplu iş yürütme yönteminin yürütülmesi  
* İstek-yanıt yönteminin bir yürütme  
* İstek-yanıt kapsayıcı başlatma

Her bir blob adı ön eki aşağıdaki biçime sahiptir: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Burada _oturum türü_ aşağıdaki değerlerden biri:  

* toplu iş  
* puan/istekleri  
* puan/başlatma  

