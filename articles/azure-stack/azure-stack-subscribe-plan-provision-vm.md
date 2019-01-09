---
title: Azure Stack'te bir teklife abone olma | Microsoft Docs
description: Azure Stack'te teklifler için abonelikleri oluşturma
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.openlocfilehash: 7de32573ac6c0d084be3fdd6ff2c3641559fc31f
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118838"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Azure Stack'te teklifleri abonelikleri oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Çalıştırdıktan sonra [teklif oluşturma](azure-stack-create-offer.md), kullanıcıların, kullanmadan önce bu teklif için bir abonelik gerekir. Kullanıcıların abone iki yolla bir teklif için:

- Bulut operatörü olarak, bir kullanıcının yönetici portalından bir aboneliği oluşturabilirsiniz. Oluşturduğunuz abonelik için genel ve özel teklifler olabilir.
- Bir kiracı kullanıcı olarak, Kullanıcı Portalı'nı kullandığınızda ortak bir teklife abone olabilirsiniz.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Bulut işleci olarak bir abonelik oluşturun

Bulut operatörleri, bir kullanıcı için bir teklif için bir abonelik oluşturmak için Yönetici portalını kullanabilir. Kendi dizin Kiracı üyeleri için abonelikleri oluşturabilirsiniz. Zaman [çok kiracılı](azure-stack-enable-multitenancy.md) olan etkin kullanıcılar için abonelikler ek dizin Kiracı oluşturabilirsiniz.

Kiracılarınızın kendi abonelikleri oluşturabilir, tekliflerinizi özelleştirin ve ardından, kiracılar için abonelikler oluşturmak istemiyorum. Bu yaklaşım, Azure Stack dış faturalama veya hizmet Kataloğu sistemleri ile tümleştirdiğinizde yaygındır.

Bir kullanıcı için bir abonelik oluşturduktan sonra kullanıcı portalında oturum açın ve teklife abone olduğunuz bakın.  

### <a name="to-create-a-subscription-for-a-user"></a>Bir kullanıcı için bir abonelik oluşturmak için

1. Yönetim Portalı'nda Git **kullanıcı aboneliklerini.**
2. **Add (Ekle)** seçeneğini belirleyin. Altında **yeni kullanıcı aboneliği**, aşağıdaki bilgileri girin:  

   - **Görünen ad** – olarak görünür aboneliği tanımlamak için bir kolay ad *kullanıcı abonelik adı*.
   - **Kullanıcı** – Bu abonelik için bir kullanılabilir dizin kiracıda bir kullanıcı belirtin. Kullanıcı adı olarak görünür *sahibi*.  Kullanıcı adının biçimi, kimlik çözümünüzde bağlıdır. Örneğin:

     - **Azure AD:** `<user1>@<contoso.onmicrosoft.com>`

     - **AD FS:** `<user1>@<azurestack.local>`

   - **Dizin kiracısı** – kullanıcı hesabına ait olduğu dizin kiracısı seçin. Çok kiracılılık etkinleştirmediyseniz, yalnızca yerel dizin kiracınız kullanılabilir.

3. Seçin **teklif**. Altında **sunar**, seçin bir **teklif** Bu abonelik için. Bir kullanıcı için abonelik oluşturduğumuzdan seçin **özel** erişilebilirlik durumu.

4. Seçin **Oluştur** aboneliği oluşturmak için. Yeni Abonelik altında görünür **kullanıcı aboneliği**. Kullanıcının kullanıcı portalında oturum açtığında, abonelik ayrıntıları görebilir.

### <a name="to-make-an-add-on-plan-available"></a>Eklenti planı kullanılabilir hale getirmek için

Bir bulut işleci bir plan daha önce oluşturulan bir aboneliği dilediğiniz zaman ekleyebilirsiniz:

1. Yönetim Portalı'nda seçin **tüm hizmetleri** altındaki **yönetim kaynakları** kategorisi, select **kullanıcı aboneliklerini**. Değiştirmek istediğiniz aboneliği seçin.

2. Seçin **eklentileri** seçip **+ Ekle**.  

3. Altında **planı Ekle**, planı eklenti olarak seçin.

## <a name="create-a-subscription-as-a-user"></a>Bir kullanıcı olarak bir abonelik oluşturun

Bir kullanıcı olarak, bulup directory kiracınız için (kuruluş), genel teklifler ve eklenti planları için abone kullanıcı portalında oturum açabilirsiniz.

>[!NOTE]
>Azure Stack ortamınıza destekliyorsa [çok kiracılı](azure-stack-enable-multitenancy.md), ayrıca bir Uzak dizin kiracısı teklifler için abone olabilirsiniz.

### <a name="to-subscribe-to-an-offer"></a>Bir teklife abone olma

1. [Oturum](azure-stack-connect-azure-stack.md) için [Azure Stack kullanıcı portalı](https://portal.local.azurestack.external) seçip **bir abonelik edinmeniz**.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. Altında **bir abonelik edinmeniz**, abonelikte kolay adını girin **görünen ad**. Seçin **teklif** altında **bir teklif seçin**, bir teklif seçin. Seçin **Oluştur** aboneliği oluşturmak için.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Bir teklife abone olduktan sonra yeni aboneliğe hangi hizmetlerin kapsamındaki görmek için portalı yenileyin.

4. Oluşturduğunuz aboneliği görmek için seçin **tüm hizmetleri** altındaki **genel** kategorisi seçin **abonelikleri**. Abonelik Ayrıntıları görmek için bir abonelik seçin.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Bir eklenti plana abone olma

Eklenti planı bir teklif varsa aboneliğinizi istediğiniz zaman bu planı ekleyebilirsiniz.  

1. Kullanıcı Portalı'nda seçin **tüm hizmetleri**. Sonraki altında **genel** kategorisi, select **abonelikleri**ve ardından değişiklik istediğiniz aboneliği seçin. Kullanılabilir Eklenti planları varsa **+ Ekle-planı** etkin olduğu ve bir kutucuk için **eklenti planları**. 

   Varsa **+ Ekle-planı** etkin değil, eğer bu abonelikle ilişkili teklif için eklenti planı yok.

1. Seçin **+ Ekle-planı** veya **eklenti planları** Döşe. Altında **eklenti planları**, eklemek istediğiniz planı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine sağlama](azure-stack-provision-vm.md)
