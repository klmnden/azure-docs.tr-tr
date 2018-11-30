---
title: Birden fazla bölgeye - Azure Machine Learning Studio Web hizmeti dağıtma | Microsoft Docs
description: (Kopya) yeni bir Web hizmetini diğer bölgeler dağıtma adımları.
services: machine-learning
documentationcenter: ''
author: ericlicoding
manager: hjerez
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.custom: (previous ms.author=aashishb, author=aashishb)
ms.author: amlstudiodocs
ms.openlocfilehash: ab28cce0f973c4798bfd6995cc275c4724b7bcc9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308028"
---
# <a name="azure-machine-learning-studio-deploy-a-web-service-to-multiple-regions"></a>Azure Machine Learning Studio: bir web hizmetini birden fazla bölgeye dağıtın.
Yeni Azure Web Hizmetleri, bir web hizmetini birden çok abonelik veya çalışma alanları gerek kalmadan kolayca birden fazla bölgeye dağıtma olanak tanır. 

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

