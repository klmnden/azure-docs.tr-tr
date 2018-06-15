---
title: Oluşturma ve Azure veritabanı PostgreSQL VNet hizmet uç noktaları ve Azure CLI kullanarak kurallar için yönetme | Microsoft Docs
description: Bu makalede, oluşturma ve Azure veritabanı PostgreSQL VNet hizmet uç noktaları ve Azure CLI komut satırını kullanarak kurallar için yönetme açıklar.
services: postgresql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 7312000d1f22af3eb0091b46caac2c9607231513
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736635"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-using-azure-cli"></a>Oluşturma ve Azure CLI kullanarak PostgreSQL VNet hizmet uç noktaları için Azure veritabanını yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kurallar için Azure veritabanınızda PostgreSQL sunucu özel adres alanı sanal ağ genişletir. Uygun Azure komut satırı arabirimi (CLI) komutlarını kullanarak, oluşturabilir, güncelleştirme, silme, liste ve VNet hizmet uç noktaları ve sunucunuzu yönetmek için kuralları gösterir. Azure veritabanı sınırlamalar da dahil olmak üzere PostgreSQL VNet hizmet uç noktaları için bir genel bakış için bkz: [Azure veritabanı PostgreSQL sunucu VNet hizmet uç noktaları için](concepts-data-access-and-security-vnet.md). VNet Hizmeti uç noktalarını genel Önizleme'de tüm desteklenen bölgeler Azure veritabanı için PostgreSQL için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure bulut Kabuğu'nu kullanın.
- Bir [PostgreSQL sunucu ve veritabanı için Azure veritabanı](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için VNet hizmet uç noktaları desteğidir.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Vnet Hizmeti uç Azure veritabanı için PostgreSQL için yapılandırın.
[Az ağ vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) komutları sanal ağları yapılandırmak için kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

CLI’yi yerel olarak çalıştırıyorsanız, [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutunu kullanarak hesabınızda oturum açmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.
```azurecli-interactive
az login
```
CLI uzantıyı kullanarak PostgreSQL VNet hizmet uç noktaları için Azure veritabanı için yüklemek `az extension add --name rdbms-vnet` komutu. 
```azurecli-interactive
az extension add --name rdbms-vnet
```

Çalıştırma `az extension list` CLI uzantısı'nın yüklendiğini doğrulamak için komutu.
```azurecli-interactive
az extension list
```
Komutunun çıktısını tüm yüklü uzantılarını listeler. Azure veritabanı PostgreSQL CLI uzantısı için şöyledir:

 {"extensionType": "whl", "ad": "rdbms-vnet", "Sürüm": "10.0.0"}

> [!NOTE]
> Çalıştırma CLI uzantıyı kaldırmak için `az extension remove -n rdbms-vnet` komutu. 

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account#az_account_set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Aboneliğinizin **az login** çıktısındaki **id** özelliğini abonelik kimliği yer tutucusuyla değiştirin.

- Hesap bir sanal ağ ve hizmet uç noktası oluşturmak için gerekli izinlere sahip olmalıdır.

Hizmet uç noktaları sanal ağlarda bağımsız olarak, sanal ağa yazma erişimi olan bir kullanıcı tarafından yapılandırılabilir.

Azure hizmet kaynaklarını bir sanal ağ ile sınırlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/JoinServicetoaSubnet" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.

[Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı Aboneliklerde varsa, kaynaklar Bu önizleme aynı anda aynı Active Directory (AD) Kiracı altında olmalıdır.

> [!IMPORTANT]
> Aşağıdaki örnek komut dosyasını çalıştırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okumak için önerilir veya Hizmeti uç noktalarını yapılandırma. **Sanal Ağ Hizmeti uç noktası:** A [sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla resmi Azure hizmeti tür adları bir alt ağ. Sanal Ağ Hizmetleri uç noktaları kullanma hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL Database, Azure hizmetine başvurduğu. Bu hizmet etiketi de Azure SQL Database, Azure veritabanı PostgreSQL ve MySQL Hizmetleri için geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi VNet Hizmeti uç noktası için hizmet uç noktası trafiği Azure SQL Database, Azure veritabanı PostgreSQL için de dahil olmak üzere tüm Azure veritabanı hizmetleri için yapılandırır ve Azure veritabanı alt ağdaki MySQL sunucuları için. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Azure veritabanı PostgreSQL veritabanı için oluşturmak, bir VNet VNet Hizmeti uç noktası oluşturun ve alt ağ bir sanal ağ kuralı ile sunucuya güvenli komut dosyası örneği
Bu örnek betikte, vurgulanan satırları değiştirerek yönetici kullanıcı adını ve parolasını özelleştirin. Kullanılan Subscriptionıd yerine `az account set --subscription` kendi abonelik tanımlayıcısı ile komutu.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
