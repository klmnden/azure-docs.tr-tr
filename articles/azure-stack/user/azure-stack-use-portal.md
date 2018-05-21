---
title: Azure yığın Portalı'nı kullanarak | Microsoft Docs
description: Erişim ve kullanıcı portalı Azure yığınında nasıl kullanılacağını öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 2ebafc4249f8455bdbe45a07a5bf88aa8984d67a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Ortak teklifleri abone olmak için Azure yığın Portalı'nı kullanın ve bu teklifleri sağladığı hizmetler kullanın. Genel Azure Portalı'nı kullandıysanız, zaten site nasıl çalıştığı ile biliyorsunuzdur.

## <a name="access-the-portal"></a>Erişim portalı

(Bir hizmet sağlayıcısı ya da bir yönetici, kuruluşunuzda), Azure yığın operatörünüze olanak tanır portalına erişmek için doğru URL'yi bildirin.

- Tümleşik bir sistem için URL operatörünüze'nın bölge ve dış etki alanı adına göre değişir ve şu biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
- Azure yığın Geliştirme Seti kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

![Azure yığın Kullanıcı Portalı ekran görüntüsü](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Pano özelleştirme

Pano kutucukları varsayılan kümesini içerir. Tıklayabilirsiniz **düzenleme Pano** tıklayın veya varsayılan pano değiştirme **yeni Pano** özel bir Pano oluşturmak için. Bir Pano ekleyerek veya kaldırarak döşeme kolayca özelleştirebilirsiniz. Örneğin, bir işlem kutucuk eklemek için tıklatın **yeni**. Sağ **işlem**ve ardından **panoya Sabitle**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonelik oluşturma ve kullanılabilir kaynakları bulun
 
Zaten bir aboneliğiniz yoksa, yapmanız gereken ilk şey teklife abone olur. Bundan sonra kullanılabilir kaynaklara göz atabilirsiniz. Gözat ve kaynakları oluşturmak için aşağıdaki yaklaşımlardan birini kullanın:

- Tıklatın **Market** döşeme Panoda.
- Üzerinde **tüm kaynakları** döşeme, tıklatın **kaynakları oluşturmak**.
- Sol gezinti bölmesinde tıklatın **yeni**.

## <a name="learn-how-to-use-available-services"></a>Kullanılabilir hizmetler kullanmayı öğrenin

Kullanılabilir hizmetler kullanma için yönergeler gerekiyorsa, farklı seçenekler, kullanılabilir olabilir.

- Kuruluşunuz ya da hizmet sağlayıcısı özelleştirilmiş Hizmetleri veya uygulamaları sağlamıyorsa genellikle söz konusu olduğu kendi belgelerinin sağlayabilir.
- Üçüncü taraf uygulamaların kendi belgelerine vardır.
- Azure tutarlı hizmetler için ilk Azure yığın belgeleri gözden geçirmenizi öneririz. Azure yığın kullanıcı belgelerine erişmek için Yardım simgesini tıklatın ve ardından **Yardım + Destek**.
 
    ![Kullanıcı Arabirimi Yardım ve Destek seçeneğinin ekran görüntüsü](media/azure-stack-use-portal/HelpAndSupport.png)

    Özellikle, başlamak için aşağıdaki makaleleri gözden geçirmenizi öneririz:

    - [Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](azure-stack-considerations.md)
    - İçinde **hizmetlerini kullanmak** bölüm belgeleri, her hizmet için dikkat edilecek noktalar makale yoktur. Dikkat edilecek noktalar sayfanın Azure'da sunulan hizmet ve Azure yığınında sunulan aynı hizmeti arasındaki farklar açıklanmaktadır. Bir örnek için bkz: [VM konuları](azure-stack-vm-considerations.md). Diğer bilgiler olabilir **hizmetlerini kullanmak** Azure yığınına benzersizdir bölümü.
     
      Azure belgelerine hizmeti için genel başvuru olarak kullanabilirsiniz, ancak bu farkların bilincinde olmanız gerekir. Belge üzerinde bağlantılar anlamak **hızlı başlangıç öğreticileri** döşeme Azure belgelerine noktası.

## <a name="get-support"></a>Destek alın

Destek gerekirse, Yardım için kuruluş veya hizmet sağlayıcınıza başvurun.

Azure yığın Geliştirme Seti kullanıyorsanız [Azure yığın Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) destek yalnızca bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar dikkat edilecek noktalar: hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](azure-stack-considerations.md)
