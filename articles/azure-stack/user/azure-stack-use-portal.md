---
title: "Azure yığın Portalı'nı kullanarak | Microsoft Docs"
description: "Erişim ve kullanıcı portalı Azure yığınında nasıl kullanılacağını öğrenin."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 63c270affca31d3db7e03116f5e287d8569b0dae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın Hizmetleri tüketici olarak ortak teklifleri abone olmak için Azure yığın Portalı'nı kullanın ve bu teklifleri kullanılabilir hale getirilir Hizmetleri kullanın. Önce Azure Portalı'nı kullandıysanız, zaten kullanıcı arabirimiyle biliyorsunuzdur.

## <a name="access-the-portal"></a>Erişim portalı

(Bir hizmet sağlayıcısı ya da bir yönetici, kuruluşunuzda), Azure yığın operatörünüze olanak tanır portalına erişmek için doğru URL'yi bildirin. 

- Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve biçim https://portal olacaktır. &lt; *bölge*&gt;.&lt; *FQDN*&gt;.
- Azure yığın Geliştirme Seti kullanıyorsanız, portalı https://portal.local.azurestack.external adresidir.

![Azure yığın Kullanıcı Portalı ekran görüntüsü](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Pano özelleştirme

Pano, bir dizi varsayılan kutucukla içerir. Tıklayabilirsiniz **düzenleme Pano** tıklayın veya varsayılan pano değiştirme **yeni Pano** özel panolar eklemek için. Pano için kutucuk kolayca ekleyebilirsiniz. Örneğin, tıklayabilirsiniz **yeni**, sağ **işlem**ve ardından **panoya Sabitle**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonelik oluşturma ve kullanılabilir kaynakları bulun
 
Zaten bir aboneliğiniz yoksa, yapmanız gereken ilk şey teklife abone olur. Bundan sonra hangi kaynaklara kullanılabilen göz atabilirsiniz. Gözat ve kaynakları oluşturmak için aşağıdakilerden birini yapın:

- Tıklatın **Market** döşeme Panoda. 
- Üzerinde **tüm kaynakları** döşeme, tıklatın **kaynakları oluşturmak**.
- Sol gezinti bölmesinde tıklatın **yeni**.

## <a name="learn-how-to-use-available-services"></a>Kullanılabilir hizmetler kullanmayı öğrenin

Kullanılabilir hizmetler kullanma için yönergeler gerekiyorsa, farklı seçenekler, kullanılabilir olabilir.

- Kuruluşunuz ya da hizmet sağlayıcısı kendi belgelerinin sağlayabilir. Özelleştirilmiş Hizmetleri veya uygulamaları sağlamıyorsa bu özellikle doğrudur.
- Üçüncü taraf uygulamaların kendi belgelerine vardır.
- Azure tutarlı hizmetler için ilk Azure yığın belgeleri gözden geçirmenizi öneririz. Azure yığın kullanıcı belgelerine erişmek için Yardım simgesini tıklatın ve ardından **Yardım + Destek**.
 
    ![Kullanıcı Arabirimi Yardım ve Destek seçeneğinin ekran görüntüsü](media/azure-stack-use-portal/HelpAndSupport.png)

    Özellikle, başlamak için aşağıdaki makaleleri gözden geçirmenizi öneririz:

    - [Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](azure-stack-considerations.md)
    - Belgeleri "Hizmetleri kullan" bölümünde listelenen her Azure tutarlı hizmet görürsünüz. Azure içinde sunulan hizmet ve Azure yığınında sunulan aynı hizmeti arasındaki farkları açıklar her hizmet için bir "hususlar" konu yoktur. Bir örnek için bkz: [VM konuları](azure-stack-vm-considerations.md). Azure yığınına benzersizdir "Hizmetleri kullan" bölümündeki diğer bilgileri olabilir. 
     
      Azure belgelerine hizmeti için genel başvuru olarak kullanabilirsiniz, ancak bu farkların bilincinde olmanız gerekir. Belge üzerinde bağlantılar anlamak **hızlı başlangıç öğreticileri** döşeme Azure belgelerine noktası.

## <a name="get-support"></a>Destek alın

Lütfen ek destek gerekirse, Yardım için kuruluş veya hizmet sağlayıcınıza başvurun. 

Azure yığın Geliştirme Seti kullanıyorsanız [Azure yığın Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) destek tek yöntemdir.

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](azure-stack-considerations.md)
