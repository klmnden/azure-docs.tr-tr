---
title: "1. adım: Machine Learning çalışma alanı oluşturma | Microsoft Docs"
description: "Adım 1 / geliştirme Tahmine dayalı bir çözüm izlenecek yol: yeni bir Azure Machine Learning Studio çalışma alanı ayarlayıp öğrenin."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: ff857e83a37b95bceb751539bb34e9fb7f202931
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Kılavuz Adımı 1: Bir Machine Learning çalışma alanı oluşturma
Bu izlenecek yol ilk adımdır [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md).

1. **Bir Machine Learning çalışma alanı oluşturma**
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişim](walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

Machine Learning Studio kullanmak için bir Microsoft Azure Machine Learning çalışma alanınızın olması gerekir. Bu çalışma alanı oluşturmak, yönetmek ve denemeler yayımlamak için ihtiyacınız olan araçları içerir.  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Azure aboneliğiniz için Yönetici çalışma alanı oluşturmak ve ardından sahibi veya katkıda eklemeniz gerekir. Ayrıntılar için bkz [oluşturma ve Paylaşım bir Azure Machine Learning çalışma alanı](create-workspace.md).

Çalışma alanınızı oluşturulduktan sonra Machine Learning Studio açın ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Birden fazla çalışma alanı varsa, pencerenin sağ üst köşesindeki araç çubuğundaki çalışma seçebilirsiniz.

![Studio çalışma alanı seçin][2]

> [!TIP]
> Çalışma alanının sahibi yapılmışsa başkaları için çalışma alanına davet üzerinde çalıştığınız denemeler paylaşabilirsiniz. Machine Learning Studio'da üzerinde bunu yapabilirsiniz **ayarları** sayfası. Microsoft hesabı veya Kurumsal hesap her kullanıcı için yeterlidir.
> 
> Üzerinde **ayarları** sayfasında, **kullanıcılar**, ardından **daha fazla kullanıcı davet** pencerenin altındaki.
> 
> 

- - -
**Sonraki: [var olan verileri yükleme](walkthrough-2-upload-data.md)**

[1]: ./media/walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/walkthrough-1-create-ml-workspace/open-workspace.png
