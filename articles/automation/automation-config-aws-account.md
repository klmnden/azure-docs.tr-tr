---
title: Amazon Web Hizmetleri ile kimlik doğrulamasını yapılandırma
description: Bu makale, AWS kaynaklarını yöneten Azure Automation'daki runbook'lar için bir AWS kimlik bilgisinin nasıl oluşturulup doğrulandığı açıklanmaktadır.
keywords: aws kimlik doğrulaması, aws yapılandırma
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 419d5643981083c8e06a68e780e95db8ba5ee2c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61074012"
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Amazon Web Hizmetleri ile Kimlik Doğrulaması Runbook'ları

Amazon Web Hizmetleri’ndeki (AWS) kaynaklarla ortak görevlerin otomatikleştirilmesi Azure’ün Automation runbook'ları ile sonuçlandırılabilir. Tıpkı Azure’deki kaynaklarla yapabildiğiniz gibi Automation runbook'ları kullanarak AWS’deki birçok görevi otomatik hale getirebilirsiniz. Gerekenlerin tümü şu iki şeydir:

* Bir AWS aboneliği ve bir dizi kimlik bilgisi. Özellikle AWS Erişim Anahtarınız ve Gizli Anahtarınız. Daha fazla bilgi için [AWS Kimlik Bilgilerini Kullanma](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) makalesini gözden geçirin.
* Bir Azure aboneliği ve Automation hesabı.

AWS kullanarak kimlik doğrulamak için, Azure Automation’dan çalışan runbook’larınızın kimliklerini doğrulamak amacıyla bir dizi AWS kimlik bilgisi belirtmeniz gerekir. Zaten sahip olduğunuz bir Otomasyon hesabı oluşturuldu ve, AWS ile kimlik doğrulaması için kullanmak istiyorsanız, aşağıdaki bölümdeki adımları izleyebilirsiniz: AWS kaynaklarını hedefleyen runbook için bir hesap ayırmak istiyorsanız, öncelikle yeni bir oluşturmanız [Otomasyon hesabı](automation-offering-get-started.md) (hizmet sorumlusu oluşturmak için bu seçeneği atlayın) ve aşağıdaki adımları kullanın:

## <a name="configure-automation-account"></a>Automation hesabı yapılandırma

Azure Otomasyonu için, AWS ile iletişim kurmak amacıyla önce AWS kimlik bilgilerinizi almanız ve bunları Azure Otomasyonu’nda varlıklar olarak depolamanız gerekir. Erişim Anahtarı oluşturmak için [AWS Hesabınız için Erişim Anahtarlarını Yönetme](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) AWS belgesinde belgelenen aşağıdaki adımları uygulayın ve **Erişim Anahtarı Kimliği** ve **Gizli Erişim Anahtarı**’nı kopyalayın (isteğe bağlı olarak, anahtar dosyanızı güvenli bir yerde saklamak için indirin).

AWS güvenlik anahtarlarınızı oluşturup kopyaladıktan sonra, bunları güvenli bir şekilde depolamak ve runbook’larınızla bunlara başvurmak için Azure Otomasyonu hesabıyla bir Kimlik Bilgisi hesabı oluşturmanız gerekir. Bölümündeki adımları izleyin: **Yeni bir kimlik bilgisi oluşturmak için** içinde [Azure automation'da kimlik bilgisi varlıkları](shared-resources/credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) makalesini inceleyin ve aşağıdaki bilgileri girin:

1. **Ad** kutusuna **AWScred** veya adlandırma standartlarınıza uygun bir değer girin.
2. **Kullanıcı adı** kutusuna kendinize ait **Erişim Kimliği**'ni yazın; **Gizli Erişim Anahtarı**'nı da **Parola** ve **Parolayı Onayla** kutusuna girin.

## <a name="next-steps"></a>Sonraki adımlar

* AWS'de görevleri otomatikleştirmek için runbook'ların nasıl oluşturulacağını öğrenmek için [Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme](automation-scenario-aws-deployment.md) çözüm makalesini gözden geçirin.
