---
title: Visual Studio'da Azure yığın şablonlarıyla dağıtma | Microsoft Docs
description: Visual Studio'da Azure yığın şablonlarıyla dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 6cd722fedc0483e37ce6ee491d74a7c985111353
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605132"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Visual Studio kullanarak Azure yığınındaki şablonlarını dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığınına dağıtmak için Visual Studio'yu kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. [Yüklemek ve bağlamak](azure-stack-install-visual-studio.md) Visual Studio ile Azure yığınına.
2. Visual Studio'yu açın.
3. Seçin **dosya**ve ardından **yeni**. İçinde **yeni proje**seçin **Azure kaynak grubu**.
4. Girin bir **adı** yeni proje ve ardından **Tamam**.
5. İçinde **Azure Şablonu Seç**, çekme **Azure yığın Quickstart** aşağı açılan listeden.
6. Seçin **101 oluşturma depolama hesabı**ve ardından **Tamam**.
7. Yeni projeniz genişletin **şablonları** düğümünde **Çözüm Gezgini** kullanılabilir şablonları görmek için.
8. İçinde **Çözüm Gezgini**projenizin adını seçin ve ardından **dağıtma**. Seçin **yeni dağıtım**.
9. İçinde **kaynak grubuna Dağıt**, kullanın **abonelik** Microsoft Azure yığın aboneliğinizi seçmek için aşağı açılan liste.
10. Gelen **kaynak grubu** listesinde, varolan bir kaynak grubu seçin veya yeni bir tane oluşturun.
11. Gelen **kaynak grubu konumu** listesinde, bir konum seçin ve ardından **dağıtma**.
12. İçinde **parametreleri Düzenle**, (Bu şablona göre değişir) parametreler için değerler sağlayın ve ardından **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları komut satırı ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
