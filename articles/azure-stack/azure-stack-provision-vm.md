---
title: "Azure yığınında test VM oluşturma | Microsoft Docs"
description: "Test VM bulut operatörü olarak Azure yığınındaki hazırlamayı öğrenin."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 41096f68e5e7d9e31098d1e8919101418abe4c03
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Test sanal makinesi Azure yığınında oluşturma

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Bir Azure yığın operatör olarak doğrulamak için test sanal makinesi oluşturabilirsiniz, [Azure yığın](azure-stack-poc.md) Geliştirme Seti dağıtım.

> [!NOTE]
> Sanal makineler sağlamak önce şunları yapmalısınız [Azure yığın Market Windows Server 2016 değerlendirmesi görüntüsü eklemek](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
1. Azure yığın Geliştirme Seti konaktaki [oturum](azure-stack-connect-azure-stack.md) Yönetici portalı'na (`https://adminportal.local.azurestack.external`) ve ardından **yeni** > **işlem**  >  **Windows Server 2016 Datacenter Eval** > **oluşturma**.  
2. İçinde **Temelleri** dikey penceresinde bir **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir tanesini seçin ve ardından **Tamam**.  
3. İçinde **bir boyutu seçin** dikey penceresinde tıklatın **A1 standart**ve ardından **seçin**.  
4. İçinde **ayarları** dikey penceresinde, Varsayılanları kabul edin ve tıklatın **Tamam**
5. Sanal makineyi oluşturmak için **Özet** dikey penceresinde **Tamam** seçeneğine tıklayın.  
6. Yeni bir sanal makine görmek için tıklatın **tüm kaynakları**, sanal makine için arama yapın ve adına tıklayın.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığınında yönetici ve kullanıcı portalı kullanma](azure-stack-manage-portals.md)
