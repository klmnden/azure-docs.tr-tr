---
title: Bir Machine Learning Studio çalışma alanı oluşturma | Microsoft Docs
description: Azure Machine Learning Studio'da bir çalışma alanı oluşturma
services: machine-learning
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 606ee124d8e7a59fc653f11f2ad7542fc84af4e4
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823444"
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanı oluşturma ve paylaşma

Azure Machine Learning Studio'da kullanmak için Machine Learning Studio çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a>Bir çalışma alanı oluşturmak için
1. [Azure portalda](https://portal.azure.com/) oturum açma

    > [!NOTE]
    > Oturum açın ve bir çalışma alanı oluşturmak için bir Azure aboneliğinin yöneticisi olmanız gerekir. 
    >
    > 

2. Tıklayın **+ yeni**

3. Arama kutusuna **Machine Learning Studio çalışma alanı** ve eşleşen bir öğeyi seçin. Seç'a tıklayarak **Oluştur** sayfanın alt kısmındaki.

4. Çalışma alanı bilgilerinizi girin:

    - *Çalışma alanı adı* bitiş boşluk olmayan en fazla 260 karakter olabilir. Ad şu karakterleri içeremez: `< > * % & : \ ? + /`
    - *Web hizmeti planı* siz seçin (veya oluşturma), ilişkili birlikte *fiyatlandırma katmanı* seçin, web hizmetleri bu çalışma alanından dağıtırsanız kullanılır.

    ![Yeni bir çalışma alanı oluşturma](./media/create-workspace/create-new-workspace.png)

5. **Oluştur**’a tıklayın.

Çalışma alanı dağıtıldıktan sonra Machine Learning Studio'da açabilirsiniz.

1. Machine Learning Studio'da, göz atma [ https://studio.azureml.net/ ](https://studio.azureml.net/).

2. Üst sağ taraftaki köşede çalışma alanınızı seçin.

    ![Çalışma alanını seçme](./media/create-workspace/open-workspace.png)

3. Tıklayın **denemelerim**.

    ![Açık denemeleri](./media/create-workspace/my-experiments.png)

Çalışma alanınızı yönetme hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning çalışma alanını yönetme](manage-workspace.md).
Çalışma alanınızı oluşturulurken bir sorunla karşılaşırsanız, bkz [sorun giderme kılavuzu: Machine Learning çalışma alanına bağlamak ve oluşturmak](troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanı paylaşımı
Bir kez çalışma alanı oluşturulduğunda Machine Learning, kullanıcıların çalışma ve tüm alt denemeleri, veri kümeleri, not defterlerini vb. paylaşmanız için çalışma alanınıza davet edebilirsiniz. İki rol birinde kullanıcılar ekleyebilirsiniz:

* **Kullanıcı** -çalışma alanına kullanıcı oluşturma, açma, değiştirebilir ve silebilirsiniz denemeleri, veri kümeleri, çalışma alanındaki vb.
* **Sahibi** - bir sahip davet edebilir ve hangi kullanıcının ek olarak, çalışma alanınızdaki kullanıcıları kaldırma gerçekleştirebilirsiniz.

> [!NOTE]
> Çalışma alanını oluşturan yönetici hesabı, çalışma alanı sahibi olarak çalışma alanına otomatik olarak eklenir. Ancak, diğer Yöneticiler veya kullanıcılar bu abonelikte çalışma alanına erişim otomatik olarak verilmez - açıkça davet etmek gerekir.
> 
> 

### <a name="to-share-a-workspace"></a>Bir çalışma alanı paylaşmak için

1. Machine Learning Studio'da, oturum açın [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. Sol bölmede bulunan tıklayın **ayarları**

3. Tıklayın **kullanıcılar** sekmesi

4. Tıklayın **daha fazla kullanıcı davet** sayfanın alt kısmındaki

    ![Studio ayarları](./media/create-workspace/settings.png)

5. Bir veya daha fazla e-posta adreslerini girin. Kullanıcıların geçerli bir Microsoft hesabı veya bir kuruluş hesabı (Azure Active Directory'den) gerekir.

6. Sahibi veya kullanıcı kullanıcılar eklemek isteyip istemediğinizi seçin.

7. Tıklayın **Tamam** onay işareti düğmesine.

Eklediğiniz her bir kullanıcı, paylaşılan çalışma alanına oturum açma yönergelerini içeren bir e-posta alırsınız.

> [!NOTE]
> Kullanıcıların ve bu çalışma alanında web Hizmetleri dağıtımı yapmak için katkıda bulunan veya Azure aboneliğinde yönetici olmaları gerekir. 



