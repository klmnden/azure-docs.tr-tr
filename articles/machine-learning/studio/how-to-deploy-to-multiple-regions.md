---
title: Studio web hizmeti için birden çok bölgede dağıtma
titleSuffix: Azure Machine Learning Studio
description: (Kopya) yeni bir Web hizmetini diğer bölgeler dağıtma adımları. Birden çok abonelik veya çalışma alanları gerek kalmadan kolayca bir web hizmetini birden fazla bölgeye dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: 536a4ae0b740eae7f6072cbd23d96e199e1598e7
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487090"
---
# <a name="deploy-an-azure-machine-learning-studio-web-service-to-multiple-regions"></a>Bir Azure Machine Learning Studio web hizmetini birden fazla bölgeye dağıtma

Yeni Azure Web Hizmetleri, bir Azure Machine Learning Studio web hizmeti birden çok abonelik veya çalışma alanları gerek kalmadan kolayca birden fazla bölgeye dağıtın olanak tanır. 

Fiyatlandırma bölgeye özeldir, bu nedenle web hizmeti dağıtacağınız her bölge için bir faturalama planını tanımlayın var.

## <a name="to-create-a-plan-in-another-region"></a>Başka bir bölgede bir plan oluşturmak için
1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/).
2. Tıklayın **planları** menü seçeneği.
3. Görünüm sayfası üzerinden planlarında tıklayın **yeni**.
4. Gelen **abonelik** açılır listesinde, yeni plan bulunacağı aboneliği seçin.
5. Gelen **bölge** açılır listesinde, yeni plan için bölge seçin. Seçilen bölge için Plan seçenekleri görüntüler **planlama seçenekleri** sayfasının bölümünde.
6. Gelen **kaynak grubu** bir kaynak grubu için plan açılır penceresinde seçin. Kaynak grupları hakkında daha fazla bilgi [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).
7. İçinde **planı adı** planın adını yazın.
8. Altında **planı seçenekleri**, fatura yeni plan için tıklayın.
9. **Oluştur**’a tıklayın.

## <a name="deploying-the-web-service-to-another-region"></a>Başka bir bölgeye web Hizmeti'ni dağıtma
1. Tıklayın **Web Hizmetleri** menü seçeneği.
2. Yeni bir bölgeye dağıttığınız Web hizmeti seçin.
3. Tıklayın **kopyalama**.
4. İçinde **Web hizmeti adı**, web hizmeti için yeni bir ad yazın.
5. İçinde **Web hizmeti açıklaması**, web hizmeti için bir açıklama yazın.
6. Gelen **abonelik** açılır listesinde, yeni bir web hizmeti bulunacağı aboneliği seçin.
7. Gelen **kaynak grubu** bir kaynak grubu web hizmeti için açılır penceresinde seçin. Kaynak grupları hakkında daha fazla bilgi [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).
8. Gelen **bölge** açılır listesinde, web hizmeti dağıtacağınız bölgeyi seçin.
9. Gelen **depolama hesabı** açılır listesinde, bir depolama hesabı web hizmeti depolamak için.
10. Gelen **fiyat planı** açılır listesinde, 8. adımda seçtiğiniz bölgede planı seçin.
11. Tıklayın **kopyalama**.

