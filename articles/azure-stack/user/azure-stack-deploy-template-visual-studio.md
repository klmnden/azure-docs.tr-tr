---
title: "Visual Studio'da Azure yığın şablonlarıyla dağıtma | Microsoft Docs"
description: "Visual Studio'da Azure yığın şablonlarıyla dağıtmayı öğrenin."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 8fc32dc50d96d202dfc982cbdc52d8e479c3a3eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Visual Studio kullanarak Azure yığınındaki şablonlarını dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığın Geliştirme Seti dağıtmak için Visual Studio kullanın.

1. [Yüklemek ve bağlamak](azure-stack-install-visual-studio.md) Visual Studio ile Azure yığınına.
2. Visual Studio'yu açın.
3. Tıklatın **dosya**, tıklatın **yeni**hem de **yeni proje** iletişim kutusunda **Azure kaynak grubu**.
4. Girin bir **adı** yeni proje ve ardından **Tamam**.
5. İçinde **Azure Şablonu Seç** iletişim kutusu, değişiklik *Göster şablonlar bu konumdan* aşağı açılan **Azure yığın hızlı başlangıç**
6. Tıklatın **101 oluşturma depolama hesabı**ve ardından **Tamam**.  
7. Yeni projeniz genişleterek kullanılabilir şablonların listesini görebilirsiniz **şablonları** düğümünde **Çözüm Gezgini** bölmesi.
8. İçinde **Çözüm Gezgini** bölmesinde, projenizin adına sağ tıklayın, **dağıtma**, ardından **yeni dağıtım**.
9. İçinde **kaynak grubuna Dağıt** iletişim kutusunda **abonelik** açılan listesinde, Microsoft Azure yığın aboneliğinizi seçin.
10. İçinde **kaynak grubu** listesinde, varolan bir kaynak grubu seçin veya yeni bir tane oluşturun.
11. İçinde **kaynak grubu konumu** listesinde, bir konum seçin ve ardından **dağıtma**.
12. İçinde **parametreleri Düzenle** iletişim kutusunda, (Bu şablona göre değişir) parametreler için değer girin ve ardından **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
[Şablonları komut satırı ile dağıtma](azure-stack-deploy-template-command-line.md)

[Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)

