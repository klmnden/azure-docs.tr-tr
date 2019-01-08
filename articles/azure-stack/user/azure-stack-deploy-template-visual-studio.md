---
title: Şablonları Azure Stack'te Visual Studio ile dağıtma | Microsoft Docs
description: Şablonları Visual Studio'da Azure Stack ile dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 9cbe39c018d5ba91599d8ec429430fceaaa567c5
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064636"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Şablonları Visual Studio kullanarak Azure Stack'te dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Visual Studio, Azure Stack için Azure Resource Manager şablonlarını dağıtmak için kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. [Yükleme ve bağlanma](azure-stack-install-visual-studio.md) Visual Studio ile Azure Stack için.
2. Visual Studio'yu açın.
3. Seçin **dosya**ve ardından **yeni**. İçinde **yeni proje**seçin **Azure kaynak grubu**.
4. Girin bir **adı** seçin ve yeni proje için **Tamam**.
5. İçinde **Azure şablonunu Seç**, çekme **Azure Stack hızlı** aşağı açılan listeden.
6. Seçin **101 oluşturduğunuz depolama hesabı**ve ardından **Tamam**.
7. Yeni projenizi genişletin **şablonları** düğümünde **Çözüm Gezgini** kullanılabilir şablonları görmek için.
8. İçinde **Çözüm Gezgini**projenizin adını seçin ve ardından **Dağıt**. Seçin **yeni dağıtım**.
9. İçinde **kaynak grubuna Dağıt**, kullanın **abonelik** Microsoft Azure Stack aboneliğinizi seçmek için aşağı açılan listesi.
10. Gelen **kaynak grubu** listesinde, var olan bir kaynak grubu seçin veya yeni bir tane oluşturun.
11. Gelen **kaynak grubu konumu** listesinde, bir konum seçin ve ardından **Dağıt**.
12. İçinde **parametreleri Düzenle**, (Bu şablona göre değişiklik gösterir) parametreleri için değerler sağlayın ve ardından **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları komut satırı ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
