---
title: "Amazon Web Hizmetleri ile Kimlik Doğrulamasını Yapılandırma | Microsoft Belgeleri"
description: "Bu makale, AWS kaynaklarını yöneten Azure Automation'daki runbook'lar için bir AWS kimlik bilgisinin nasıl oluşturulup doğrulandığı açıklanmaktadır."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "aws kimlik doğrulaması, aws yapılandırma"
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 68805a6b28fc9454262cb0503daa23af93c76a7e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Amazon Web Hizmetleri ile Kimlik Doğrulaması Runbook'ları
Amazon Web Hizmetleri’ndeki (AWS) kaynaklarla ortak görevlerin otomatikleştirilmesi Azure’ün Automation runbook'ları ile sonuçlandırılabilir.  Tıpkı Azure’deki kaynaklarla yapabildiğiniz gibi Automation runbook'ları kullanarak AWS’deki birçok görevi otomatik hale getirebilirsiniz.  Gerekenlerin tümü şu iki şeydir:

* Bir AWS aboneliği ve bir dizi kimlik bilgisi.  Özellikle AWS Erişim Anahtarınız ve Gizli Anahtarınız.  Daha fazla bilgi için [AWS Kimlik Bilgilerini Kullanma](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) makalesini gözden geçirin.
* Bir Azure aboneliği ve Automation hesabı.  Azure Otomasyonu hesabı oluşturma hakkında daha fazla bilgi için [Kimlik doğrulaması planlaması](automation-offering-get-started.md#authentication-planning) sayfasına bakın.  

AWS kullanarak kimlik doğrulamak için, Azure Automation’dan çalışan runbook’larınızın kimliklerini doğrulamak amacıyla bir dizi AWS kimlik bilgisi belirtmeniz gerekir. Zaten oluşturulmuş bir Automation hesabınız varsa ve AWS ile bunu kimlik doğrulamasını yapmak için kullanmak istiyorsanız aşağıdaki bölümde yer alan adımları uygulayabilirsiniz.  AWS kaynaklarını hedefleyen runbook hesabını atamak isterseniz, önce yeni bir [Otomasyon hesabı](automation-offering-get-started.md) oluşturup (hizmet sorumlusu oluşturmak için bu seçeneği atlayın) aşağıdaki adımları uygulamalısınız.

## <a name="configure-automation-account"></a>Automation hesabı yapılandırma
Azure Otomasyonu için, AWS ile iletişim kurmak amacıyla önce AWS kimlik bilgilerinizi almanız ve bunları Azure Otomasyonu’nda varlıklar olarak depolamanız gerekir.  Erişim Anahtarı oluşturmak için [AWS Hesabınız için Erişim Anahtarlarını Yönetme](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) AWS belgesinde belgelenen aşağıdaki adımları uygulayın ve **Erişim Anahtarı Kimliği** ve **Gizli Erişim Anahtarı**’nı kopyalayın (isteğe bağlı olarak, anahtar dosyanızı güvenli bir yerde saklamak için indirin).

AWS güvenlik anahtarlarınızı oluşturup kopyaladıktan sonra, bunları güvenli bir şekilde depolamak ve runbook’larınızla bunlara başvurmak için Azure Otomasyonu hesabıyla bir Kimlik Bilgisi hesabı oluşturmanız gerekir.  [Azure Automation’da kimlik bilgisi varlıkları](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) makalesinin **Yeni kimlik bilgileri oluşturmak için** bölümündeki adımları tamamlayın ve şu bilgileri girin:

1. **Ad** kutusuna **AWScred** veya adlandırma standartlarınıza uygun bir değer girin.  
2. **Kullanıcı adı** kutusuna kendinize ait **Erişim Kimliği**'ni yazın; **Gizli Erişim Anahtarı**'nı da **Parola** ve **Parolayı Onayla** kutusuna girin.   

## <a name="next-steps"></a>Sonraki adımlar
* AWS'de görevleri otomatikleştirmek için runbook'ların nasıl oluşturulacağını öğrenmek için [Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme](automation-scenario-aws-deployment.md) çözüm makalesini gözden geçirin.

