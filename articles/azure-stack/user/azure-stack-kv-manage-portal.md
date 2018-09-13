---
title: Anahtar Kasası'nda Azure Stack portalını kullanarak yönetme | Microsoft Docs
description: Anahtar Kasası'nda Azure Stack portalını kullanarak yönetmeyi öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.openlocfilehash: 51c04a567ff953c4e84930e3feae448f78627683
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713944"
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Anahtar Kasası'nda Azure Stack portalını kullanarak yönetme

Anahtar Kasası'nda Azure Stack Azure Stack portalını kullanarak yönetebilirsiniz. Bu makalede oluşturmak ve Azure Stack'te bir anahtar Kasası'nı yönetmek için çalışmaya başlamanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Azure Key Vault hizmetini içeren bir teklife abone olması gerekir.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external).

2. Panoda **+ kaynak Oluştur** > **güvenlik + kimlik** > **Key Vault**.

    ![Key Vault ekran](media/azure-stack-kv-manage-portal/image1.png)

3. İçinde **Key Vault Oluştur** bölmesinde atamak bir **adı** kasanız için. Kasa adı yalnızca alfasayısal karakterler ve özel karakter tire (-) içerebilir. Bunlar, bir sayı ile başlayamaz olmamalıdır.

4. Seçin bir **abonelik** kullanılabilir abonelikler listesinden. Key Vault hizmetinin sunduğu tüm abonelikler, aşağı açılan listede görüntülenir.

5. Yeni bir **Kaynak Grubu** seçin ya da yeni bir tane oluşturun.

6. Seçin **fiyatlandırma katmanı**.
    >[!NOTE]
    > Anahtar kasaları Azure Stack geliştirme Seti'ni Destek **standart** yalnızca SKU'ları.

7. Var olan birini seçin **erişim ilkeleri** veya yeni bir tane oluşturun. Bir erişim ilkesi, bir kullanıcı, uygulama veya bir güvenlik grubu bu kasayla işlemleri gerçekleştirmek izinler sağlar.

8. İsteğe bağlı olarak, bir **Gelişmiş erişim ilkesi** özelliklere erişimi etkinleştirmek için. Örneğin: dağıtım için Resource Manager şablon dağıtımı ve Azure Disk şifrelemesi birim şifreleme için erişim için sanal makineleri (VM'ler).

9. Ayarları yapılandırdıktan sonra seçin **Tamam**ve ardından **Oluştur**. Bu anahtar kasası dağıtım başlatır.

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli anahtarları yönetme

Bir kasayı oluşturduktan sonra anahtarları ve gizli dizileri kasa içinde oluşturmak ve yönetmek için aşağıdaki adımları kullanın.

### <a name="create-a-key"></a>Bir anahtar oluşturma

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external).

2. Panoda **tüm kaynakları**, daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **anahtarları** Döşe.

3. İçinde **anahtarları** bölmesinde **Ekle**.

4. İçinde **anahtar oluşturma** bölmesinden listesini **seçenekleri**, bir anahtar oluşturmak için kullanmak istediğiniz yöntemi seçin. Şunları yapabilirsiniz **Oluştur** yeni bir anahtar **karşıya** mevcut bir anahtar veya kullanın **geri yedekleme** anahtarının bir yedekleme seçin.

5. Girin bir **adı** anahtarınız için. Anahtar adı yalnızca alfasayısal karakterler ve özel karakter tire (-) içerebilir.

6. İsteğe bağlı olarak, yapılandırma **etkinleştirme tarihi ayarlamak** ve **sona erme tarihi ayarlayın** anahtarınız için değerler.

7. Seçin **Oluştur** dağıtımı başlatmak için.

Anahtar başarıyla oluşturulduktan sonra altında seçebilirsiniz **anahtarları** görüntüleyebilir veya özellikleri değiştirin. Özellikler bölümü içeren **anahtar tanımlayıcısı**, dış uygulama bu anahtara erişmek için bir Tekdüzen Kaynak Tanımlayıcısı (URI) olduğu. Bu anahtar işlemleri sınırlandırmak için ayarları yapılandırma **işlemlerine izin**.

![Anahtar URI'si](media/azure-stack-kv-manage-portal/image4.png)

### <a name="create-a-secret"></a>Gizli anahtar oluşturma

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external).
2. Panoda **tüm kaynakları**, daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **gizli dizileri** kutucuk.

3. Altında **gizli dizileri**seçin **Ekle**.

4. Altında **gizli dizi oluşturma**, listesinden **karşıya yükleme seçenekleri**, istediğiniz bir gizli dizi oluşturmak bir seçenek belirleyin. Gizli dizi oluşturabilirsiniz **el ile** gizli ya da karşıya yükleme için bir değer girerseniz bir **sertifika** yerel makinenizden.

5. Girin bir **adı** için gizli anahtarı. Gizli dizi adı yalnızca alfasayısal karakterler ve özel karakter tire (-) içerebilir.

6. İsteğe bağlı olarak, belirtin **içerik türü**ve değerlerini yapılandırmak **etkinleştirme tarihi ayarlamak** ve **sona erme tarihi ayarlayın** için gizli anahtarı.

7. Seçin **Oluştur** dağıtımı başlatmak için.

Gizli dizi başarıyla oluşturulduktan sonra altında seçebilirsiniz **gizli dizileri** görüntüleyebilir veya özellikleri değiştirin. **Gizli dizi tanımlayıcısı** dış uygulama bu gizli dizi erişmek için kullanabileceğiniz bir URI.

![URI gizli anahtarı](media/azure-stack-kv-manage-portal/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault'ta depolanan parola alarak bir VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)
* [Key Vault'ta depolanan bir sertifika ile VM dağıtma](azure-stack-kv-push-secret-into-vm.md)
