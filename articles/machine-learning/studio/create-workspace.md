---
title: Machine Learning Studio çalışma alanı oluşturma
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da kullanmak için Machine Learning Studio çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 7aeee4f24f6c7133ad978bc0c6c7fb8853bc4c35
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62109357"
---
# <a name="create-and-share-an-azure-machine-learning-studio-workspace"></a>Oluşturma ve bir Azure Machine Learning Studio çalışma paylaşma

Azure Machine Learning Studio'da kullanmak için Machine Learning Studio çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.

## <a name="create-a-studio-workspace"></a>Studio çalışma alanı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açma

    > [!NOTE]
    > Studio çalışma alanı oluşturma ve oturum açmak için bir Azure aboneliğinin yöneticisi olmanız gerekir. 
    >
    > 

2. Tıklayın **+ yeni**

3. Arama kutusuna **Machine Learning Studio çalışma alanı** ve eşleşen bir öğeyi seçin. Seç'a tıklayarak **Oluştur** sayfanın alt kısmındaki.

4. Çalışma alanı bilgilerinizi girin:

   - *Çalışma alanı adı* bitiş boşluk olmayan en fazla 260 karakter olabilir. Ad şu karakterleri içeremez: `< > * % & : \ ? + /`
   - *Web hizmeti planı* siz seçin (veya oluşturma), ilişkili birlikte *fiyatlandırma katmanı* seçin, web hizmetleri bu çalışma alanından dağıtırsanız kullanılır.

     ![Yeni Studio çalışma alanı oluşturma](./media/create-workspace/create-new-workspace.png)

5. **Oluştur**’a tıklayın.

> [!NOTE]
> Machine Learning Studio iş akışını yürütürken Ara verileri kaydetmek için sağlayan bir Azure depolama hesabı kullanır. Depolama hesabı silinirse müşteri çalışma alanı oluşturulduktan sonra veya erişim anahtarları değiştirilirse çalışma alanı işlemeyi durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.
Depolama hesabını yanlışlıkla silerseniz, silinen depolama hesabıyla aynı bölgede aynı ada sahip bir depolama hesabını yeniden oluşturun ve erişim tuşunu yeniden eşitleyin. Depolama hesabının erişim anahtarlarını değiştirdiyseniz, Azure portalını kullanarak çalışma alanındaki erişim anahtarlarını yeniden eşitleyin.

Çalışma alanı dağıtıldıktan sonra Machine Learning Studio'da açabilirsiniz.

1. Machine Learning Studio'da, göz atma [ https://studio.azureml.net/ ](https://studio.azureml.net/).

2. Üst sağ taraftaki köşede çalışma alanınızı seçin.

    ![Çalışma alanını seçme](./media/create-workspace/open-workspace.png)

3. Tıklayın **denemelerim**.

    ![Açık denemeleri](./media/create-workspace/my-experiments.png)

Studio çalışma alanınıza yönetme hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning Studio çalışma alanını yönetme](manage-workspace.md).
Çalışma alanınızı oluşturulurken bir sorunla karşılaşırsanız bkz [sorun giderme kılavuzu: Oluşturma ve bir Machine Learning Studio çalışma alanına bağlanma](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-workspace"></a>Bir Azure Machine Learning Studio çalışma paylaşın
Bir kez çalışma alanı oluşturulduğunda Machine Learning Studio'da bir, kullanıcıların çalışma ve tüm alt denemeleri, veri kümeleri, not defterlerini vb. paylaşmanız için çalışma alanınıza davet edebilirsiniz. İki rol birinde kullanıcılar ekleyebilirsiniz:

* **Kullanıcı** -çalışma alanına kullanıcı oluşturma, açma, değiştirebilir ve silebilirsiniz denemeleri, veri kümeleri, çalışma alanındaki vb.
* **Sahibi** - bir sahip davet edebilir ve hangi kullanıcının ek olarak, çalışma alanınızdaki kullanıcıları kaldırma gerçekleştirebilirsiniz.

> [!NOTE]
> Çalışma alanını oluşturan yönetici hesabı, çalışma alanı sahibi olarak çalışma alanına otomatik olarak eklenir. Ancak, diğer Yöneticiler veya kullanıcılar bu abonelikte çalışma alanına erişim otomatik olarak verilmez - açıkça davet etmek gerekir.
> 
> 

### <a name="to-share-a-studio-workspace"></a>Studio çalışma alanına paylaşmak için

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



