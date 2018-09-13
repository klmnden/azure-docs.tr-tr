---
title: Azure Stack portalını kullanarak | Microsoft Docs
description: Erişim ve kullanıcı portalını Azure Stack'te kullanmak hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 7ccf746cfd9645b63c2810bff83cfb0c4fa55d65
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713757"
---
# <a name="use-the-azure-stack-portal"></a>Azure Stack portalını kullanın

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Genel tekliflere abone olmak için Azure Stack portalını kullanın ve bu teklifleri sağlayan hizmetler kullanır. Zaten genel Azure portal'ı kullandıysanız, siteyi nasıl çalıştığı ile ilgili bilgi sahibi.

## <a name="access-the-portal"></a>Portala erişebilir

(Bir hizmet sağlayıcısı veya yöneticinin kuruluşunuzdaki), Azure Stack operatörünüze olanak tanır, portala erişmek için doğru URL'yi de biliyorsunuz demektir.

- Tümleşik bir sistem için URL, işlecin bölge ve dış etki alanı adına göre değişir ve biçimde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
- Azure Stack geliştirme Seti'ni kullanıyorsanız, portalı adresidir https://portal.local.azurestack.external.

![Azure Stack kullanıcı portalının ekran yakalama](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Panoyu özelleştirin

Pano kutucukları varsayılan kümesini içerir. Seçebileceğiniz **panoyu Düzenle** varsayılan pano değiştirmek veya **yeni Pano** özel bir Pano oluşturmak için. Ekleyerek veya kaldırarak kutucukları bir panoya kolayca özelleştirebilirsiniz. Örneğin, bir işlem kutucuk eklemek için seçin **+ kaynak Oluştur**. Sağ **işlem**ve ardından **panoya Sabitle**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonelik oluşturmak ve kullanılabilir kaynaklara göz atın

Zaten bir aboneliğiniz yoksa, yapmanız gereken ilk şey bir teklife abone olur. Bundan sonra kullanılabilir kaynaklara göz atabilirsiniz. Gözat ve kaynakları oluşturmak için aşağıdaki yaklaşımlardan birini kullanın:

- Seçin **Market** Panoda kutucuk.
- Üzerinde **tüm kaynakları** kutucuk seçin **kaynakları oluşturma**.
- Sol gezinti bölmesinde seçin **+ kaynak Oluştur**.

## <a name="learn-how-to-use-available-services"></a>Kullanılabilir hizmetler kullanmayı öğrenin

Kullanılabilir hizmetlerin nasıl kullanılacağı için rehberliğe ihtiyacınız varsa, farklı seçenekler, kullanılabilir olabilir.

- Kuruluşunuz veya hizmet sağlayıcısı özelleştirilmiş Hizmetleri veya uygulamaları teklifi sunuyorsanız olduğu genellikle kendi belgelerinin sağlayabilir.
- Üçüncü taraf uygulamalar, kendi belgelerinin sahiptir.
- Tutarlı Azure Hizmetleri için ilk Azure Stack belgeleri gözden geçirmenizi öneririz. Azure Stack kullanıcı belgeleri erişmek için Yardım simgesini seçin ve ardından **Yardım + Destek**.

    ![Yardım ve seçenek kullanıcı Arabiriminde destek](media/azure-stack-use-portal/HelpAndSupport.png)

    Özellikle, başlamak için aşağıdaki makaleleri gözden geçirmenizi öneririz:

    - [Anahtar dikkat edilmesi gerekenler: hizmetlerini kullanarak veya Azure Stack için uygulamalar oluşturma](azure-stack-considerations.md)
    - İçinde **kullanmanıza** bölüm belgelerine, her hizmet için bir konuları makalesi yoktur. Dikkat edilecek noktalar sayfanın Azure'da sunulan hizmet ve Azure Stack'te sunulan aynı hizmet arasındaki farklar açıklanmaktadır. Bir örnek için bkz. [VM konuları](azure-stack-vm-considerations.md). Diğer bilgiler olabilir **kullanmanıza** Azure Stack için benzersiz bir bölüm.

      Azure belgeleri bir hizmet için genel bir başvuru olarak kullanabilirsiniz, ancak şu farklılıklara olması gerekir. Belgeler üzerinde bağlantıları anlamak **hızlı başlangıç öğreticileri** kutucuğunda Azure belgelerine noktası.

## <a name="get-support"></a>Destek alın

Desteğe ihtiyacınız varsa, Yardım için kuruluşunuz veya hizmet sağlayıcınıza başvurun.

Azure Stack geliştirme Seti'ni kullanıyorsanız [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) desteği yalnızca kaynağıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar dikkat edilmesi gerekenler: hizmetlerini kullanarak veya Azure Stack için uygulamalar oluşturma](azure-stack-considerations.md)
