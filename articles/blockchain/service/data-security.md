---
title: Azure Blockchain hizmet güvenliği
description: Azure Blockchain hizmeti veri erişimi ve güvenlik kavramları
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dd0a33364ed9395a85478798e47352c533bd47dc
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028208"
---
# <a name="azure-blockchain-service-security"></a>Azure Blockchain hizmet güvenliği

Azure Blockchain hizmeti verilerinizi güvenli ve kullanılabilir durumda tutmak için çeşitli Azure özelliklerini kullanır. Veriler, yalıtım, şifreleme ve kimlik doğrulaması kullanılarak korunmaktadır.

## <a name="isolation"></a>Yalıtım

Azure Blockchain hizmet kaynaklarını özel bir sanal ağ içinde yalıtılır. Her işlem ve doğrulama bir sanal makine (VM) düğümüdür. Bir sanal ağdaki VM'ler, farklı bir sanal ağdaki VM'ler için doğrudan iletişim kuramaz. Sanal ağ içinde özel iletişimin kalır yalıtım sağlar. Azure sanal ağ yalıtımı hakkında daha fazla bilgi için bkz. [Azure genel bulutta yalıtım](../../security/azure-isolation.md#networking-isolation).

![Sanal Ağ Diyagramı](./media/data-security/vnet.png)

## <a name="encryption"></a>Şifreleme

Kullanıcı verilerini Azure Depolama'da saklanır. Kullanıcı verileri, güvenlik ve gizlilik için bekleyen ve Hareket halindeki şifrelenir. Daha fazla bilgi için bkz. [Azure depolama Güvenlik Kılavuzu](../../storage/common/storage-security-guide.md).

## <a name="authentication"></a>Kimlik Doğrulaması

İşlem, blok zinciri düğümlere bir RPC uç nokta üzerinden gönderilebilir. İstemciler bu tanıtıcıları kullanıcı kimlik doğrulaması ters proxy sunucusu kullanarak bir işlem düğümü ile iletişim kurmak ve SSL üzerinden verileri şifreler.

![Kimlik doğrulama diyagramı](./media/data-security/authentication.png)

RPC erişim için kimlik doğrulamasının üç moddan vardır.

### <a name="basic-authentication"></a>Temel kimlik doğrulama

Temel kimlik doğrulaması, kullanıcı adını ve parolasını içeren bir HTTP kimlik doğrulama üst bilgisi kullanır. Kullanıcı adı, blok zinciri düğümünün adıdır. Parola, bir üye veya düğüm sağlama sırasında ayarlanır. Parola Azure portal veya CLI kullanılarak değiştirilebilir.

### <a name="access-keys"></a>Erişim tuşları

Erişim tuşları, uç nokta URL'si dahil rastgele oluşturulmuş bir dize kullanın. İki erişim anahtarlarını döndürme yardımcı oluyor. Anahtarları, Azure portalı ve CLI üretilebilir.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure AD kullanıcı kimlik bilgilerini kullanarak Azure AD tarafından kullanıcı kimliğinin doğrulandığı azure Active Directory (Azure AD) kullanır talep tabanlı kimlik doğrulama mekanizması. Azure AD bulut tabanlı kimlik yönetimi sağlar ve müşterilerin tüm kurumsal ve erişim uygulamaları bulutta tek bir kimlik kullanın olanak tanır. Azure Blockchain hizmeti Azure AD çoklu oturum açma ve çok faktörlü kimlik doğrulaması Kimlik Federasyonu etkinleştirme ile tümleştirilir. Blockchain üye ve düğüm erişimi kuruluşunuzdaki kullanıcılara, gruplara veya uygulama rolleri atayabilirsiniz.

Azure AD istemci proxy kullanılabilir [GitHub](https://github.com/Microsoft/azure-blockchain-connector/releases). İstemci proxy kullanıcıyı Azure AD oturum açma sayfasına yönlendirir ve başarılı kimlik doğrulamadan sonra bir taşıyıcı belirteci alır. Sonuç olarak, kullanıcı Ethereum istemci uygulamanın Geth veya Truffle gibi istemci proxy uç noktasına bağlanır. Son olarak, bir işlem gönderildiğinde, http üst bilgisinde taşıyıcı belirteci İstemci Proxy'i ekler ve ters proxy OAuth protokolünü kullanarak belirteci doğrular.

## <a name="keys-and-ethereum-accounts"></a>Anahtarlar ve Ethereum hesapları

Azure Blockchain hizmet üye sağlanırken Ethereum hesabınız ve ortak ve özel anahtar çifti oluşturulur. Özel anahtar, işlemler için blok zinciri göndermek için kullanılır. Son 20 bayt ortak anahtarın karma Ethereum hesaptır. Ethereum hesabı Cüzdan olarak da adlandırılır.

Genel ve özel anahtar çifti bir keyfile JSON biçiminde saklanır. Özel anahtar, blok zinciri kayıt defteri hizmeti oluşturulduğunda girilen parola kullanılarak şifrelenir.

Özel anahtarlar, işlem dijital olarak imzalamak için kullanılır. Özel blok zincirleri paylaşıldıkça içinde özel bir anahtarla imzalanmış akıllı bir sözleşme imzalayan kimlik temsil eder. İmza geçerliliğini doğrulamak için alıcı imzalayan ortak anahtar imzasının hesaplanan adresiyle karşılaştırabilirsiniz.

Constellation anahtarlar çekirdek düğümü benzersiz şekilde tanımlamak için kullanılır. Constellation anahtarları düğüm sağlama sırasında oluşturulan ve çekirdek özel bir işlemde privateFor parametresinin belirtilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain hizmeti işlem düğümleri yapılandırın](configure-transaction-nodes.md)
