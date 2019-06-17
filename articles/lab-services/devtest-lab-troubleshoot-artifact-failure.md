---
title: Azure DevTest Labs sanal makine'de yapıt hatalarını tanılama | Microsoft Docs
description: Azure DevTest labs'deki yapıt hatalarını giderme hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2019
ms.author: spelluru
ms.openlocfilehash: 29af70a2713e7b4aebf611d8f2b547e38c6c5d3d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60623138"
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Laboratuvardaki yapıt hatalarını tanılama 
Bir yapıt oluşturduktan sonra başarılı veya başarısız olduğunu görmek için kontrol edebilirsiniz. Azure DevTest labs'deki yapıt günlükleri, bir yapı hatası tanılamak için kullanabileceğiniz bilgileri sağlar. Birkaç bir Windows VM yapıt günlük bilgilerini görüntülemek için bir seçenek vardır:

* Azure portalında
* VM

> [!NOTE]
> Hataları doğru tanımlanan ve açıklanan emin olmak için yapıt uygun yapısı olması önemlidir. Doğru bir yapı oluşturmak nasıl hakkında daha fazla bilgi için bkz. [özel yapıtlar oluşturma](devtest-lab-artifact-author.md). Düzgün yapılandırılmış bir yapıt bir örneğini görmek için kullanıma [Test parametre türleri](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) yapıt.

## <a name="troubleshoot-artifact-failures-by-using-the-azure-portal"></a>Azure portalını kullanarak yapıt hatalarını giderme

1. Azure portalında kaynakları listesini Laboratuvarınızı seçin.
2. Araştırmak istediğiniz yapıt içeren Windows VM'yi seçin.
3. Sol bölmede altında **genel**seçin **Yapıtları**. Bu VM ile ilişkili yapıların listesi görüntülenir. Yapıt ve yapı durumu adı gösterilir.

   ![Yapı durumu](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Gösteren bir yapı seçin bir **başarısız** durumu. Yapıt açılır. Yapıt hata ayrıntılarını içeren bir uzantı iletisi görüntülenir.

   ![Yapıt hata iletisi](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-virtual-machine"></a>Sanal makine içinde yapıt hatalarını giderme

1. Tanılamak istediğiniz yapıt içeren VM için oturum açın.
2. Git C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\\*1.9*\Status, burada *1.9* Azure özel betik uzantısı sürüm numarasıdır.

   ![Durum dosyası](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Açık **durumu** dosya.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir sanal makine mevcut bir Active Directory etki alanına katılın](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir Git deposu bir laboratuvara ekleme](devtest-lab-add-artifact-repo.md).

