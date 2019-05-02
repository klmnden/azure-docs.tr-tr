---
title: (Önizleme) Azure HDInsight üzerinde Apache Kafka için kendi anahtarını Getir
description: Bu makalede, Azure HDInsight üzerinde Apache Kafka'ya depolanan verileri şifrelemek için Azure Key vault'tan kendi anahtarınızı kullanmayı açıklar.
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: ce9df58e9640cab2e6ba50fce772f1e30739dc5a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714844"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight"></a>Azure HDInsight üzerinde Apache Kafka için kendi anahtarını Getir

Azure HDInsight için Apache Kafka Getir bilgisayarınızı kendi anahtarını (BYOK) desteği içerir. Bu özellik, sahibi ve bekleyen verileri şifrelemek için kullanılan anahtarları yönetmenizi sağlar. 

Tüm yönetilen disklerin HDInsight, Azure depolama hizmeti şifrelemesi (SSE) ile korunmaktadır. Varsayılan olarak, bu disklerdeki verileri, Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. BYOK etkinleştirirseniz, HDInsight ve Azure anahtar Kasası'nı kullanarak yönetmek için şifreleme anahtarını sağlayın. 

BYOK şifreleme, ek ücret ödemeden küme oluşturma sırasında işlenen tek adımlı bir işlemdir. Tek yapmanız gereken olan HDInsight, Azure Key Vault ile bir yönetilen kimlik olarak kaydedin ve kümenize oluşturduğunuzda şifreleme anahtarını ekleyin.

Kafka kümesinin (Kafka tarafından saklanan çoğaltmaları dahil) için tüm iletiler bir simetrik veri şifreleme anahtarı (DEK ile) şifrelenir. DEK, anahtar Kasası'ndaki anahtar şifreleme anahtarı (KEK) kullanarak korunur. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir. 

Anahtarları key vault'ta güvenli bir şekilde döndürmek için Azure portal veya Azure CLI'yı kullanabilirsiniz. Bir anahtar döndürdüğünde, dakikalar içinde yeni bir anahtar kullanarak HDInsight Kafka kümesinin başlatır. Fidye yazılımı senaryoları ve yanlışlıkla silme durumlarına karşı korumak "Geçici silme" anahtar koruma özelliklerini etkinleştirin. Bu koruma özelliği desteklenmez olmadan anahtar kasaları.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="get-started-with-byok"></a>BYOK ile çalışmaya başlama
Oluşturmak için BYOK bir Kafka kümesi etkin, aşağıdaki adımları ele alacağız:
1. Azure kaynakları için yönetilen kimlikler oluşturun
2. Azure Key Vault ve anahtarlar ayarlama
3. Etkin BYOK ile HDInsight Kafka kümesi oluşturma
4. Şifreleme anahtarını döndürme

## <a name="create-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler oluşturun

   Anahtar Kasası'na kimliğini doğrulamak için bir kullanıcı tarafından atanan yönetilen kimlik kullanarak oluşturma [Azure portalında](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md), veya [ Azure CLI](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Azure HDInsight yönetilen kimlikleri çalışması hakkında daha fazla bilgi için bkz. [yönetilen Azure HDInsight kimliklerini](../hdinsight-managed-identities.md). Azure Active directory yönetilen kimlikleri ve Kafka için BYOK için gerekli olsa da, Kurumsal güvenlik paketi (ESP) bir gereksinim değildir. Yönetilen kimlik kaynak kimliği için anahtar kasası erişim ilkesini eklediğinizde kaydettiğinizden emin olun.

   ![Azure Portalı'nda kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/apache-kafka-byok/user-managed-identity-portal.png)

## <a name="setup-the-key-vault-and-keys"></a>Key Vault ve anahtarlar ayarlama

   HDInsight yalnızca Azure anahtar kasası destekler. Anahtar kasanız varsa, Azure Key Vault'a anahtarlarınızı içeri aktarabilirsiniz. Anahtarları "Geçici silme" olması gerektiğini unutmayın. "Geçici silme" Bu özellik .NET REST üzerinden kullanılabilir /C#, PowerShell ve Azure CLI'dan arabirimleri.

   1. Yeni bir anahtar kasası oluşturmak için takip [Azure anahtar kasası](../../key-vault/key-vault-overview.md) hızlı başlangıç. Mevcut anahtarları içeri aktarma hakkında daha fazla bilgi için ziyaret [anahtarlara, parolalara ve sertifikalara hakkında](../../key-vault/about-keys-secrets-and-certificates.md).

   2. "Geçici silme" kullanarak anahtar kasası üzerinde etkinleştirmek [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) CLI komutu.
        '''Azure CLI az keyvault update--ad <Key Vault Name> --enable geçici silme
        ```

   3. Create keys

        a. To create a new key, select **Generate/Import** from the **Keys** menu under **Settings**.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

        b. Set **Options** to **Generate** and give the key a name.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

        c. Select the key you created from the list of keys.

        ![Azure Key Vault key list](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

        d. When you use your own key for Kafka cluster encryption, you need to provide the key URI. Copy the **Key identifier** and save it somewhere until you're ready to create your cluster.

        ![Copy key identifier](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
    4. Add managed identity to the key vault access policy.

        a. Create a new Azure Key Vault access policy.

        ![Create new Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy.png)

        b. Under **Select Principal**, choose the user-assigned managed identity you created.

        ![Set Select Principal for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

        c. Set **Key Permissions** to **Get**, **Unwrap Key**, and **Wrap Key**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

        d. Set **Secret Permissions** to **Get**, **Set**, and **Delete**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

        e. Click on **Save**. 

        ![Save Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-save.png)

## Create HDInsight cluster

   You're now ready to create a new HDInsight cluster. BYOK can only be applied to new clusters during cluster creation. Encryption can't be removed from BYOK clusters, and BYOK can't be added to existing clusters.

   ![Kafka disk encryption in Azure portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   During cluster creation, provide the full key URL, including the key version. For example, `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. You also need to assign the managed identity to the cluster and provide the key URI.

## Rotating the Encryption key
   There might be scenarios where you might want to change the encryption keys used by the Kafka cluster after it has been created. This can be easily via the portal. For this operation, the cluster must have access to both the current key and the intended new key, otherwise the rotate key operation will fail.

   To rotate the key, you must have the full url of the new key (See Step 3 of [Setup the Key Vault and Keys](#setup-the-key-vault-and-keys)). Once you have that, go to the Kafka cluster properties section in the portal and click on **Change Key** under **Disk Encryption Key URL**. Enter in the new key url and submit to rotate the key.

   ![Kafka rotate disk encryption key](./media/apache-kafka-byok/kafka-change-key.png)

## FAQ for BYOK to Apache Kafka

**How does the Kafka cluster access my key vault?**

   Associate a managed identity with the HDInsight Kafka cluster during cluster creation. This managed identity can be created before or during cluster creation. You also need to grant the managed identity access to the key vault where the key is stored.

**Is this feature available for all Kafka clusters on HDInsight?**

   BYOK encryption is only possible for Kafka 1.1 and above clusters.

**Can I have different keys for different topics/partitions?**

   No, all managed disks in the cluster are encrypted by the same key.

**What happens if the cluster loses access to the key vault or the key?**
   If the cluster loses access to the key, warnings will be shown in the Ambari portal. In this state, the **Change Key** operation will fail. Once key access is restored, ambari warnings will go away and operations such as key rotation can be successfully performed.

   ![Kafka key access ambari alert](./media/apache-kafka-byok/kafka-byok-ambari-alert.png)

**How can I recover the cluster if the keys are deleted?**

   Since only “Soft Delete” enabled keys are supported, if the keys are recovered in the key vault, the cluster should regain access to the keys. To recover an Azure Key Vault key, see [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) or [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Can I have producer/consumer applications working with a BYOK cluster and a non-BYOK cluster simultaneously?**

   Yes. The use of BYOK is transparent to producer/consumer applications. Encryption happens at the OS layer. No changes need to be made to existing producer/consumer Kafka applications.

**Are OS disks/Resource disks also encrypted?**

   No. OS disks and Resource disks are not encrypted.

**If a cluster is scaled up, will the new brokers support BYOK seamlessly?**

   Yes. The cluster needs access to the key in the key vault during scale up. The same key is used to encrypt all managed disks in the cluster.

**Is BYOK available in my location?**

   Kafka BYOK is available in all public clouds.

## Next steps

* For more information about Azure Key Vault, see [What is Azure Key Vault](../../key-vault/key-vault-whatis.md)?
* To get started with Azure Key Vault, see [Getting Started with Azure Key Vault](../../key-vault/key-vault-overview.md).
