---
title: Azure HDInsight yönetilen kimlikleri
description: Azure HDInsight yönetilen kimliklerini uygulamasının genel bir bakış sağlar.
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: hrasheed
ms.openlocfilehash: 5012b669b7460a44cb2732d7db7bf76fd1f567cf
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715771"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight yönetilen kimlikleri

Yönetilen bir kimlik, Azure Active Directory'de kimlik bilgileri, Azure tarafından yönetilir (Azure AD) kayıtlı bir kimliktir. Yönetilen kimliklerle hizmet sorumluları, Azure AD'ye kaydetme ve sertifikalar gibi kimlik bilgilerini güncelleştirmek gerekmez.

Yönetilen kimlik Azure HDInsight kümelerinizi Azure AD Etki Alanı Hizmetleri'ne erişebilmek, Azure Key Vault'a erişmek veya Azure Data Lake depolama Gen2, dosyalara erişmek izin vermek için kullanılabilir.

Yönetilen kimlik iki tür vardır: kullanıcı tarafından atanan ve sistem tarafından atanan. Azure HDInsight, kullanıcı tarafından atanan yönetilen kimlikleri kullanır. Kullanıcı tarafından atanan bir yönetilen kimlik, bir tek başına bir veya daha fazla Azure hizmeti örnekleri ardından atayabilirsiniz Azure kaynağı olarak oluşturulur. Buna karşılık, sistem tarafından atanan bir yönetilen kimlik Azure AD'de oluşturulur ve doğrudan belirli bir Azure hizmet örneğinde'ı otomatik olarak etkinleştirilir. Ardından, sistem tarafından atanan bir yönetilen kimlik ömrünü üzerindeki etkin hizmet örneği ömrünü bağlıdır.

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight yönetilen kimlik uygulaması

Azure HDInsight'ın içinde her küme düğümünde yönetilen kimlikleri sağlanır. Bu kimlik, ancak yalnızca HDInsight hizmeti tarafından kullanılabilir bileşenleridir. Şu anda desteklenen yöntemi yok, HDInsight küme düğümlerinde yüklü yönetilen kimliklerle erişim belirteçleri oluşturun. Bazı Azure Hizmetleri için kendi diğer Azure hizmetleriyle etkileşim kurmaya yönelik erişim belirteçlerini almak için kullanabileceğiniz bir uç nokta ile yönetilen kimlikleri uygulanır.

## <a name="create-a-managed-identity"></a>Yönetilen bir kimlik oluşturun

Yönetilen kimlik aşağıdaki yöntemlerden birini oluşturulabilir:

* [Azure portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Yönetilen kimlik yapılandırma kalan adımlarını burada kullanılacak bir senaryoya bağlıdır.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight senaryolarda yönetilen kimlik

Yönetilen kimlikleri, Azure HDInsight birden çok senaryolarda kullanılır. Ayrıntılı Kurulum ve yapılandırma yönergeleri için ilgili belgelere bakın:

* [Azure Data Lake depolama 2. nesil](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-managed-identity)
* [Kurumsal güvenlik paketi](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Kafka kendi anahtarını getir (BYOK)](kafka/apache-kafka-byok.md#get-started-with-byok)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)
