---
title: Hizmet günlüğü - Azure Machine Learning Studio web | Microsoft Docs
description: Machine Learning Studio web hizmetleri için günlüğe kaydetmeyi etkinleştirme hakkında bilgi edinin. Günlük API'leri gidermenize yardımcı olacak ek bilgiler sağlar.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.subservice: studio
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.openlocfilehash: 727379edb60756ca8cb3e5ebdc29cd38858945e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345660"
---
# <a name="enable-logging-for-azure-machine-learning-studio-web-services"></a>Azure Machine Learning Studio web hizmetleri için günlüğe kaydetmeyi etkinleştirme
Bu belge, bilgi Machine Learning Studio web hizmetleri üzerinde günlüğe kaydetme olanağı sağlar. Günlük, yalnızca bir hata numarası ve Machine Learning Studio API'leri aramalarınız gidermenize yardımcı olabilecek bir ileti ötesinde ek bilgi sağlar.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Bir Web hizmeti için günlük kaydını etkinleştirme

Günlük etkinleştirme [Azure Machine Learning Studio Web Hizmetleri](https://services.azureml.net) portalı. 

1. Azure Machine Learning Studio Web Hizmetleri portalında oturum açın [ https://services.azureml.net ](https://services.azureml.net). Klasik web hizmeti, ayrıca Portalı'na tıklayarak alabilirsiniz **yeni Web Hizmetleri deneyiminizin** Machine Learning Studio Web Hizmetleri sayfasında Machine Learning Studio'da.

   ![Yeni Web Hizmetleri deneyiminizin bağlantı](./media/web-services-logging/new-web-services-experience-link.png)

2. Üst menü çubuğunda **Web Hizmetleri** için bir yeni web hizmeti veya tıklayın **Klasik Web Hizmetleri** için bir Klasik web hizmeti.

   ![Yeni veya Klasik web hizmetleri seçin](./media/web-services-logging/select-web-service.png)

3. Yeni web hizmeti, web hizmeti adına tıklayın. Klasik web hizmeti, web hizmeti adına tıklayın ve ardından sonraki sayfada uygun uç noktaya tıklayın.

4. Üst menü çubuğunda **yapılandırma**.

5. Ayarlama **günlüğü etkinleştir** seçeneğini *hata* (yalnızca hataları günlüğe kaydetmek için) veya *tüm* (için tam günlük kaydı).

   ![Günlüğe kaydetme düzeyini seçin](./media/web-services-logging/enable-logging.png)

6. **Kaydet**’e tıklayın.

7. Klasik web hizmetleri için oluşturma **ml tanılama** kapsayıcı.

   Tüm web hizmeti günlükleri adlı bir blob kapsayıcısını tutulur **ml tanılama** web hizmeti ile ilişkili depolama hesabında. Yeni web hizmetleri için bu kapsayıcı web hizmetine erişim ilk kez oluşturulur. Klasik web hizmetleri için henüz yoksa kapsayıcıyı oluşturun gerekir. 

   1. İçinde [Azure portalında](https://portal.azure.com)web hizmeti ile ilişkilendirilmiş depolama hesabına gidin.

   2. **Blob Hizmeti**’nin altında, **Kapsayıcılar**’a tıklayın.

   3. Kapsayıcı **ml tanılama** mevcut olmayan'a tıklayın **+ kapsayıcı**, kapsayıcı "ml-diagnostics" adını verin ve seçin **erişim türü** "BLOB". **Tamam**'ı tıklatın.

      ![Tanılama günlüklerinizi depolamak için yeni bir kapsayıcı oluşturma](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Klasik web hizmeti, Machine Learning Studio Web Hizmetleri Panoda ayrıca günlük kaydını etkinleştirmek için bir anahtar var. Ancak, günlük kaydı Web Hizmetleri portalından artık yönetildiğinden, bu makalede anlatıldığı gibi portal üzerinden günlük etkinleştirmeniz gerekir. Zaten Studio'da günlük etkinleştirilirse, Web Hizmetleri portalında günlük kaydını devre dışı bırakın ve yeniden etkinleştirin.


## <a name="the-effects-of-enabling-logging"></a>Etkilerini günlüğünü etkinleştirme
Günlüğe kaydetme etkinleştirildiğinde tanılama ve web hizmeti uç noktası hatalarından oturum açtınız **ml tanılama** Azure depolama hesabındaki blob kapsayıcısına kullanıcının çalışma ile bağlı. Bu kapsayıcıdaki tüm web hizmeti uç noktaları için bu depolama hesabıyla ilişkili tüm çalışma alanları için tüm tanılama bilgileri tutar.

Herhangi bir Azure depolama hesabını keşfetmek kullanılabilen çeşitli araçları kullanarak günlükleri görüntülenebilir. Daha kolay olabilir Azure portalında depolama hesabına gidin, tıklatın **kapsayıcıları**ve kapsayıcı ardından **ml tanılama**.  

## <a name="log-blob-detail-information"></a>Günlük blob ayrıntı bilgileri
Kapsayıcıdaki her blob, aşağıdaki eylemleri tam olarak birine ait tanılama bilgilerini tutar:

* Bir toplu iş yürütme yönteminin yürütülmesi  
* Bir istek-yanıt yönteminin yürütülmesi  
* İstek-yanıt kapsayıcı başlatma

Her blob adı bir önek aşağıdaki biçime sahiptir: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Burada _günlük türü_ aşağıdaki değerlerden biri:  

* toplu iş  
* puan/istekleri  
* puan/başlatma  

