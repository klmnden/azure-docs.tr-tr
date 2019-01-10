---
title: Azure Stack depolama hizmet sorumlusu kimlik bilgileri anahtar Kasası'nda | Microsoft Docs
description: Key Vault hizmet sorumlusu kimlik bilgileri Azure Stack üzerinde nasıl depoladı öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2019
ms.author: sethm
ms.openlocfilehash: 570c1adc2f4615e78cbe5656c13b0e22b863baf7
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192332"
---
# <a name="store-service-principal-credentials-in-key-vault"></a>Anahtar Kasası'nda hizmet sorumlusu kimlik bilgileri Store

Azure Stack üzerinde uygulamalar genellikle geliştirme, hizmet sorumlusu oluşturma ve dağıtmadan önce kimliğini doğrulamak için bu kimlik bilgilerini kullanarak gerektirir. Ancak, genellikle hizmet sorumlusu için depolanan kimlik bilgileri yanlış yerleştirilmiş. Bu makalede, bir hizmet sorumlusu oluşturma ve değerleri daha sonra alma işlemi için Azure Key Vault'ta depolamak açıklar.

Key Vault hakkında daha fazla bilgi için bkz. [bu makalede](azure-stack-key-vault-intro.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure Key Vault hizmeti içeren bir teklif aboneliği.
- PowerShell, Azure Stack ile kullanmak için yapılandırılır.

## <a name="key-vault-in-azure-stack"></a>Azure Stack'te anahtar kasası

Şifreleme anahtarlarını korumak için Azure Stack Key Vault'ta yardımcı olur ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanabilirsiniz. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları şifreleyebilirsiniz.

Bir anahtar kasası oluşturmak için aşağıdaki adımları izleyin:

1. Azure Stack portalında oturum açın.

2. Panoda **+ kaynak Oluştur**, ardından **güvenlik + kimlik**, ardından **anahtar kasası.**

   ![Anahtar kasası oluşturma](media/azure-stack-key-vault-store-credentials/create-key-vault.png)

3. İçinde **Key Vault Oluştur** bölmesinde atamak bir **adı** kasanız için. Kasa adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir. Bir sayıyla başlamamalıdır.

4. Kullanılabilir abonelikler listesinden bir abonelik seçin.

5. Mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun.

6. Fiyatlandırma katmanını seçin.

7. Var olan erişim ilkelerinden birini seçin veya yeni bir tane oluşturun. Bir erişim ilkesi, bir kullanıcı, uygulama veya bir güvenlik grubu bu kasayla işlemleri gerçekleştirmek izinler sağlar.

8. İsteğe bağlı olarak, özelliklere erişimi etkinleştirmek için bir Gelişmiş erişim ilkesini seçin.

9. Ayarları yapılandırdıktan sonra seçin **Tamam**ve ardından **Oluştur**. Anahtar kasası dağıtım başlar.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

1. Azure portalı üzerinden Azure hesabınızda oturum açın.

2. Seçin **Azure Active Directory**, ardından **uygulama kayıtları**, ardından **Ekle**.

3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **Oluştur**.

4. Seçin **Active Directory**, ardından **uygulama kayıtları**ve uygulamanızı seçin.

5. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Örnek uygulamalar uygulamalarda kullanımı **istemci kimliği** söz konusu olduğunda **uygulama kimliği**.

6. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

7. Anahtar ve bir süre için bir açıklama sağlayın.

8. **Kaydet**’i seçin.

9. Kopyalama **anahtarı** , kullanılabilir tıkladığınız sonra **Kaydet**.

## <a name="store-the-service-principal-inside-key-vault"></a>Hizmet sorumlusu Key Vault içinde Store

1. Kullanıcı portalında oturum için Azure Stack, ardından daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **gizli** Döşe.

2. İçinde **gizli** bölmesinde **Oluştur/içeri aktarma**.

3. İçinde **gizli dizi oluşturma** bölmesinden seçenekleri seçin listesini **el ile**.

4. Girin **uygulama Kimliğini** anahtarınızı adı olarak hizmet sorumlusundan kopyalanır. Anahtar adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir.

5. Hizmet sorumlusu içine anahtarınızdan değerini yapıştırın **değer** sekmesi.

6. Seçin **hizmet sorumlusu** için **içerik türü**.

7. Ayarlama **etkinleştirme tarihi** ve **sona erme tarihi** anahtarınız için değerler.

8. Seçin **Oluştur** dağıtımı başlatmak için.

Gizli dizi başarıyla oluşturulduktan sonra hizmet sorumlusu bilgileri burada depolanır. Altında herhangi bir zamanda seçin **gizli dizileri**, görüntülemek ve özelliklerini değiştirin. Özellikler bölümü, dış uygulama bu gizli dizi erişmek için bir Tekdüzen Kaynak Tanımlayıcısı (URI) olan gizli tanımlayıcısını içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet sorumlularını kullanma](azure-stack-create-service-principals.md)
- [Anahtar Kasası'nda Azure Stack portal ile yönetme](azure-stack-key-vault-manage-portal.md)  
- [Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme](azure-stack-key-vault-manage-powershell.md)