---
title: Azure Blockchain hizmeti işlem düğümleri yapılandırın
description: Azure Blockchain hizmeti işlem düğümleri yapılandırma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dffeb81ae1eb244c38639a1241c0581e6fcdf94a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65027968"
---
# <a name="configure-azure-blockchain-service-transaction-nodes"></a>Azure Blockchain hizmeti işlem düğümleri yapılandırın

Azure Blockchain hizmetiyle etkileşim kurmak için blok zinciri üyelik bir veya daha fazla işlem düğümlerine bağlanma aracılığıyla bunu.  İşlem düğümleri ile etkileşim kurmak için düğümlerinize yönelik erişimi yapılandırmanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

* [Azure Blockchain üye oluştur](create-member.md)

## <a name="transaction-node-overview"></a>İşlem düğümü'ne genel bakış

İşlem düğümleri, blok zinciri işlemleri Azure blok zinciri hizmetine genel bir uç nokta göndermek için kullanılır. Varsayılan işlem düğümü üzerinde blok zinciri kayıtlı Ethereum hesabının özel anahtarı içerir ve bu nedenle silinemiyor.

Varsayılan işlem düğümü ayrıntılarını görüntülemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain Service üyelik için gidin. Seçin **işlem düğümleri**.

    ![Varsayılan işlem düğümünü seçin](./media/configure-transaction-nodes/nodes.png)

    Genel Bakış ayrıntıları, genel bir uç nokta adresleri ve ortak anahtarı içerir.

## <a name="create-transaction-node"></a>İşlem düğümü oluşturma

Blok zinciri üyelik, on işlem düğümlerinin toplam en çok dokuz ek işlem düğümleri ekleyebilirsiniz. İşlem düğümleri ekleyerek ölçeklenebilirliği artırmak veya yük dağıtın. Örneğin, farklı istemci uygulamaları için bir işlem düğümü uç noktası olabilir.

İşlem düğümü eklemek için:

1. Azure portalı, Azure Blockchain hizmet üyesine bulun ve seçin **işlem düğümleri > Ekle**.
1. Yeni işlem düğümü ayarlarını tamamlayın.

    ![İşlem düğümü ekleme](./media/configure-transaction-nodes/add-node.png)

    | Ayar | Açıklama |
    |---------|-------------|
    | Ad | İşlem düğümü adı. Adı, işlem düğümü uç nokta için DNS adresi oluşturmak için kullanılır. Örneğin, `newnode-myblockchainmember.blockchain.azure.com`. Düğüm adı, oluşturulduktan sonra değiştirilemez. |
    | Parola | Güçlü bir parola ayarlayın. İşlem düğümü uç noktasına temel kimlik doğrulaması ile erişmek için parolayı kullanın.

1. **Oluştur**’u seçin.

    Yeni bir işlem düğümü sağlama yaklaşık 10 dakika sürer. Ek işlem düğümleri ücret. Maliyetler hakkında daha fazla bilgi için bkz. [Azure fiyatlandırması](https://aka.ms/ABSPricing).

## <a name="endpoints"></a>Uç Noktalar

İşlem düğümleri, benzersiz bir DNS adı ve ortak Uç noktalara sahip.

Bir işlem düğümünün uç noktası ayrıntıları görüntülemek için:

1. Azure portalında Azure blok zinciri hizmet üye işlem düğümlerinizi birine gidin ve seçin **genel bakış**.

    ![Uç Noktalar](./media/configure-transaction-nodes/endpoints.png)

İşlem düğümü uç noktaları, güvenli ve kimlik doğrulaması gerektirir. Azure AD kimlik doğrulaması, HTTPS temel kimlik doğrulaması ve SSL üzerinden HTTPS veya Websocket üzerinden bir erişim anahtarı kullanarak bir işlem uç noktasına bağlanabilir.

### <a name="azure-active-directory-access-control"></a>Azure Active Directory erişim denetimi

Azure Blockchain hizmet işlemi düğüm uç noktaları, Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Uç noktanız için Azure AD kullanıcı, Grup ve hizmet sorumlusu erişimi verebilirsiniz.

AD erişim Azure vermek için uç noktanıza denetler:

1. Azure portalında, Azure Blockchain hizmet üyesine bulun ve seçin **işlem düğümleri > erişim denetimi (IAM) > Ekle > Rol ataması Ekle**.
1. Bir kullanıcı, Grup veya hizmet sorumlusu (uygulama rolleri) için yeni bir rol ataması oluşturun.

    ![IAM rolünü ekleyin](./media/configure-transaction-nodes/add-role.png)

    | Ayar | Eylem |
    |---------|-------------|
    | Rol | Seçin **sahibi**, **katkıda bulunan**, veya **okuyucu**.
    | Erişim ata: | Seçin **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
    | Seçim | Kullanıcı, Grup veya eklemek istediğiniz hizmet sorumlusu arayın.

1. Seçin **Kaydet** rol ataması ekleme.

Azure AD erişim denetimi hakkında daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-portal.md)

Azure AD kimlik doğrulamasını kullanarak bağlanma hakkında daha fazla bilgi için bkz: [düğümünüzü için AAD kimlik doğrulamasını kullanarak bağlanma](configure-aad.md).

### <a name="basic-authentication"></a>Temel kimlik doğrulaması

HTTPS temel kimlik doğrulaması için kullanıcı adı ve parola kimlik bilgilerini ve uç noktasına HTTPS istek üst bilgisinde geçirilir.

Azure portalında bir işlem düğümünün temel kimlik doğrulama uç noktası ayrıntıları görüntüleyebilirsiniz. Azure Blockchain hizmet üye işlem düğümlerinizi, birine gidin ve seçin **temel kimlik doğrulaması** ayarları.

![Temel kimlik doğrulaması](./media/configure-transaction-nodes/basic.png)

Kullanıcı adı, düğümün adı olduğundan değiştirilemez.

URL'sini kullanmak üzere değiştirin \<parola\> parolayla düğümü sağlanırken ayarlayın. Parola belirleyerek güncelleştirebilirsiniz **parolayı Sıfırla**.

### <a name="access-keys"></a>Erişim anahtarları

Erişim anahtarı kimlik doğrulaması için erişim anahtarı uç nokta URL'si dahil edilir. İşlem düğümü sağlandığında iki erişim tuşu oluşturulur. İki erişim anahtarı kimlik doğrulaması için kullanılabilir. İki anahtar değiştirme ve anahtarları döndürme etkinleştirin.

Bir işlem düğümünün erişim önemli ayrıntıları görüntülemek ve erişim anahtarlarını içeren uç nokta adresleri kopyalayın. Azure Blockchain hizmet üye işlem düğümlerinizi, birine gidin ve seçin **erişim anahtarlarını** ayarları.

### <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Güvenlik duvarı kuralları, işlem düğümü için kimlik doğrulama girişiminde bulunabilirsiniz IP adreslerini sınırlamak etkinleştirin.  Güvenlik duvarı kuralları olmadığından, işlem düğümü için yapılandırıldıysa, herhangi bir şirket tarafından erişilemez.  

Bir işlem düğümünün güvenlik duvarı kurallarını görüntülemek için Azure blok zinciri hizmet üye işlem düğümlerinizi birine gidin ve seçin **güvenlik duvarı kuralları** ayarları.

Kural adı girerek, güvenlik duvarı kuralları ekleyebilirsiniz, başlangıç IP adresi ve bitiş IP içinde adres **güvenlik duvarı kuralları** kılavuz.

![Güvenlik duvarı kuralları](./media/configure-transaction-nodes/firewall-rules.png)

Etkinleştirmek için:

* **Tek IP adresi:** Başlangıç ve bitiş IP adreslerinin aynı IP adresini yapılandırın.
* **IP adres aralığı:** Başlangıç ve bitiş IP adresi aralığı yapılandırın. Örneğin, aralığı sırasında 10.221.34.0 başlayıp 10.221.34.255 tüm 10.221.34.xxx alt etkinleştirebilirsiniz.
* **Tüm IP adreslerine izin ver:** Başlangıç IP adresi 0.0.0.0 için ve bitiş IP adresi 255.255.255.255 için yapılandırın.

## <a name="connection-strings"></a>Bağlantı dizeleri

Bağlantı dizesi söz dizimi işlem düğümü için temel için sağlanan kimlik doğrulama veya erişim tuşlarını kullanarak. HTTPS ve WebSockets üzerinden erişim anahtarları da dahil olmak üzere bağlantı dizelerini sağlanır.

Bir işlem düğümünün bağlantı dizelerini görüntüleyin ve uç nokta adresleri kopyalayın. Azure Blockchain hizmet üye işlem düğümlerinizi, birine gidin ve seçin **bağlantı dizeleri** ayarları.

![Bağlantı dizeleri](./media/configure-transaction-nodes/connection-strings.png)

## <a name="sample-code"></a>Örnek kod

Örnek kod, hızlı bir şekilde işlem düğümünüzü Web3, Nethereum Web3js ve Truffle aracılığıyla bağlanmayı etkinleştirmek için sağlanır.

Bir işlem düğümünün örnek bağlantı kod görüntüleyebilir ve popüler geliştirici araçları ile kullanmak üzere kopyalayın. Azure Blockchain hizmet üye işlem düğümlerinizi, birine gidin ve seçin **örnek kodu** ayarları.

Kullanmak istediğiniz kod örneği görmek için Web3 veya Nethereum sekmesini seçin.

![Örnek kod](./media/configure-transaction-nodes/sample-code.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak işlem düğümleri yapılandırın](manage-cli.md)
