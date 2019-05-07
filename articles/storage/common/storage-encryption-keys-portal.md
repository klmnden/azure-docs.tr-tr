---
title: Müşteri tarafından yönetilen anahtarlar Azure portalından Azure depolama şifrelemesi için yapılandırma
description: Azure portalında Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarlar yapılandırmak için kullanmayı öğrenin. Müşteri tarafından yönetilen anahtarlar oluşturun, döndürme, devre dışı bırakın ve erişim denetimleri iptal olanak sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: baabc5a8e1d063cb51a3edea3a7218591e85aa1a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154164"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-the-azure-portal"></a>Müşteri tarafından yönetilen anahtarlar Azure portalından Azure depolama şifrelemesi için yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, bir anahtar kasası ile müşteri tarafından yönetilen anahtarlar kullanılarak yapılandırma işlemi gösterilmektedir [Azure portalında](https://portal.azure.com/). Azure portalını kullanarak bir anahtar kasası oluşturmayı öğrenmek için bkz: [hızlı başlangıç: Ayarlayın ve Azure portalını kullanarak Azure Key Vault gizli dizi alma](../../key-vault/quick-create-portal.md). 


> [!IMPORTANT]
> Azure depolama şifreleme ile müşteri tarafından yönetilen anahtarlar kullanılarak gerektirir anahtar kasanızın yapılandırılmış, iki gerekli özellikleri olmasını **geçici silme** ve **yapmak değil Temizleme**. Azure portalında yeni bir anahtar kasası oluşturduğunuzda, bu özellikler varsayılan olarak etkindir. Ancak, bu var olan bir anahtar kasasının özelliklerini etkinleştirmeniz gerekirse, PowerShell veya Azure CLI kullanmanız gerekir.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar etkinleştir

Azure Portalı'nda müşteri tarafından yönetilen anahtarlar etkinleştirmek için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin.
1. Üzerinde **ayarları** depolama hesabı dikey penceresine tıklayın **şifreleme**. Seçin **kendi anahtarınızı kullanın** seçeneği, aşağıdaki resimde gösterildiği gibi.

    ![Portal ekran görüntüsü şifreleme seçeneği](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>Bir anahtarı belirtin

Müşteri tarafından yönetilen anahtarlar etkinleştirdikten sonra depolama hesabıyla ilişkilendirmek için bir anahtar belirtmek için fırsatınız olacaktır.

### <a name="specify-a-key-as-a-uri"></a>Bir anahtar olarak bir URI belirtin.

Bir anahtar olarak bir URI belirtmek için bu adımları izleyin:

1. Anahtar URI'si Azure Portalı'nda bulmak için anahtar Kasası'na gidin ve seçin **anahtarları** ayarı. İstediğiniz anahtarı seçin ve ardından anahtar ayarlarını görüntülemek için tıklayın. Değerini kopyalayın **anahtar tanımlayıcısı** alanı URI sağlar.

    ![Ekran gösterme anahtar kasası anahtar URI'si](media/storage-encryption-keys-portal/key-uri-portal.png)

1. İçinde **şifreleme** depolama hesabınız için ayarları seçin **Enter tuşunu URI** seçeneği.
1. İçinde **anahtar URI'si** alan, URI belirtin.

   ![Anahtar URI'si girin gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>Bir anahtar, anahtar kasası belirtin

Bir key vault anahtarı belirtmek için önce bir anahtar içeren bir anahtar kasası sahip emin olun. Bir key vault anahtarı belirtmek için bu adımları izleyin:

1. Seçin **Key Vault'tan seçin** seçeneği.
2. Kullanmak istediğiniz anahtarı içeren anahtar Kasası'nı seçin.
3. Anahtar, anahtar kasasından seçin.

   ![Müşteri tarafından yönetilen anahtar seçeneğini gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>Anahtar sürümü güncelleştir

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabının güncelleştirilmesi gerekir. Şu adımları uygulayın:

1. Depolama hesabınıza gidin ve görüntüleme **şifreleme** ayarları.
1. Yeni anahtar sürümü URI'sini belirtin. Alternatif olarak, anahtar kasası ve anahtar sürümünü güncellemeyi tekrar seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure depolama şifrelemesi](storage-service-encryption.md)
- [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
