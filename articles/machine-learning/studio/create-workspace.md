---
title: "Machine Learning çalışma alanı oluşturma | Microsoft Docs"
description: "Azure Machine Learning Studio'da bir çalışma alanı oluşturma"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 4e1fa0a9abd4721d15a94923263ff2f521bceee8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanı oluşturma ve paylaşma
Cortana analitik işlem (CAP) tarafından kullanılan çeşitli veri bilimi ortamları ayarlama nasıl açıklayan konulara bu menü bağlantılar.

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

Azure Machine Learning Studio kullanmak için bir Machine Learning çalışma alanınızın olması gerekir. Bu çalışma alanı oluşturmak, yönetmek ve denemeler yayımlamak için ihtiyacınız olan araçları içerir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a>Bir çalışma alanı oluşturmak için
1. Oturum [Azure portalı](https://portal.azure.com/)

    > [!NOTE]
    > Oturum açın ve bir çalışma alanı oluşturmak için Azure abonelik yöneticisi olmanız gerekir. 
    >
    > 

2. Tıklatın **+ yeni**

3. Seçin **Intelligence + analiz**, tıklatın **Machine Learning çalışma alanı**, ardından **oluştur**

4. Çalışma alanı bilgilerinizi girin

    - *Çalışma alanı adı* boşluk bitiş değil, en fazla 260 karakter olabilir. Adı şu karakterleri içeremez:`< > * % & : \ ? + /`
    - *Web hizmeti planı* , seçin (veya oluşturun), ilişkili birlikte *fiyatlandırma katmanı* seçin, web hizmetleri bu çalışma alanından dağıtırsanız kullanılır.

    ![Yeni bir çalışma alanı oluşturma](./media/create-workspace/create-new-workspace.png)

5. **Oluştur**'a tıklayın

Çalışma alanı dağıtıldığında, Machine Learning Studio'da açabilirsiniz.

1. Machine Learning Studio'ya için Gözat [https://studio.azureml.net/](https://studio.azureml.net/).

2. Çalışma alanınızı üst sağ taraftaki köşedeki seçin.

    ![Çalışma alanı seçin](./media/create-workspace/open-workspace.png)

3. Tıklatın **denemelerim**.

    ![Açık denemeleri](./media/create-workspace/my-experiments.png)

Çalışma alanınızı yönetme hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning çalışma alanını yönetme](manage-workspace.md).
Çalışma alanınızı oluşturulurken bir sorunla karşılaşırsanız, bkz: [sorun giderme kılavuzu: oluşturun ve Machine Learning çalışma alanına bağlayın](troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanı paylaşımı
Bir kez çalışma alanı oluşturulduğunda Machine Learning, kullanıcıları çalışma alanınızı ve tüm alt denemeler, veri kümeleri, dizüstü bilgisayarlar, vb. erişimi paylaşmak için çalışma alanına davet edebilirsiniz. Kullanıcılar iki rolleri ekleyebilirsiniz:

* **Kullanıcı** -çalışma kullanıcı oluşturmak, açın, değiştirmek ve silmek denemeler, veri kümeleri, vb. çalışma alanında.
* **Sahibi** - sahibi davet edebilir ve çalışma alanında, hangi kullanıcı yanı sıra Kaldır kullanıcılar yapabilir.

> [!NOTE]
> Çalışma alanı oluşturur yönetici hesabı, çalışma alanı sahibi olarak çalışma alanına otomatik olarak eklenir. Ancak, diğer Yöneticiler veya kullanıcılar bu abonelikte çalışma alanına erişim otomatik olarak verilmez - açıkça davet gerekir.
> 
> 

### <a name="to-share-a-workspace"></a>Bir çalışma alanı paylaşmak için

1. Machine Learning Studio'ya oturum [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. Sol panelinde tıklatın **ayarları**

3. Tıklatın **kullanıcılar** sekmesi

4. Tıklatın **daha fazla kullanıcı davet** sayfanın sonundaki

    ![Studio ayarları](./media/create-workspace/settings.png)

5. Bir veya daha fazla e-posta adresi girin. Kullanıcıların geçerli bir Microsoft hesabı veya kurumsal bir hesap (Azure Active Directory'den) gerekir.

6. Sahibi veya kullanıcı kullanıcılar eklemek istediğinizi seçin.

7. Tıklatın **Tamam** onay işareti düğmesine.

Eklediğiniz her kullanıcı için paylaşılan çalışma alanına oturum açmak yönergeler içeren bir e-posta alır.

> [!NOTE]
> Kullanıcıların dağıtmak veya bu çalışma alanında web hizmetleri yönetmek için bunların katkıda bulunan veya Azure Abonelik Yöneticisi olması gerekir. 



