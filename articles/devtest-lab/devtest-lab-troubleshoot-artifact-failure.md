---
title: "Bir Azure DevTest Labs sanal makinede yapı hataları tanılamak | Microsoft Docs"
description: "Azure DevTest Labs de yapı hataları giderme öğrenin."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 9a79e50902e8e99e94148f8ef534e6745e31809a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Laboratuvara yapı hataları tanılama 
Bir yapı oluşturduktan sonra başarılı veya başarısız olup olmadığını görmek için kontrol edebilirsiniz. Azure DevTest Labs yapı günlükleri, bir yapı sorunu tanılamak için kullanabileceğiniz bilgileri sağlar. Yapı günlük bilgilerini bir Windows VM için görüntüleme seçeneklerini birkaç vardır:

* Azure portalında
* VM

> [!NOTE]
> Hataları doğru tanımlanan ve açıklanan emin olmak için yapıyı uygun yapısı olması önemlidir. Doğru bir yapı oluşturma hakkında daha fazla bilgi için bkz: [özel yapılar oluşturma](devtest-lab-artifact-author.md). Düzgün şekilde yapılandırılmış bir yapı örneği görmek için kullanıma [Test parametre türleri](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) yapı.

## <a name="troubleshoot-artifact-failures-by-using-the-azure-portal"></a>Azure portalını kullanarak yapı hatalarını giderme

1. Azure portalındaki kaynakları listesini Laboratuvarınızı seçin.
2. İncelemek istediğiniz yapıyı içeren Windows VM seçin.
3. Sol bölmede altında **genel**seçin **yapıları**. Bu VM ile ilişkili yapıları listesi görüntülenir. Adını yapı ve yapı durumu gösterilir.

   ![Yapı durumu](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Gösteren bir yapı seçin bir **başarısız** durumu. Yapı açılır. Hatanın yapısı hakkındaki ayrıntıları içeren bir uzantı iletisi görüntülenir.

   ![Yapı hata iletisi](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-virtual-machine"></a>Sanal makine içindeki yapı hatalarından sorun giderme

1. Tanılamak istediğiniz yapı içeren VM için oturum açın.
2. C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension gidin\\*1.9*\Status, burada *1.9* Azure özel betik uzantısı sürüm numarasıdır.

   ![Durum dosyası](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Açık **durum** dosya.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir VM mevcut bir Active Directory etki alanına katılma](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Laboratuvar için bir Git deposu ekleme](devtest-lab-add-artifact-repo.md).

