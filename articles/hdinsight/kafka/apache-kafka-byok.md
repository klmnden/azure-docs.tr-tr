---
title: (Önizleme) Azure HDInsight üzerinde Apache Kafka için kendi anahtarını Getir
description: Bu makalede, Azure HDInsight üzerinde Apache Kafka'ya depolanan verileri şifrelemek için Azure Key vault'tan kendi anahtarınızı kullanmayı açıklar.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 46105ee92a5c98cb8180b2499d0ad295702aac43
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953386"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight-preview"></a>(Önizleme) Azure HDInsight üzerinde Apache Kafka için kendi anahtarını Getir

Azure HDInsight için Apache Kafka Getir bilgisayarınızı kendi anahtarını (BYOK) desteği içerir. Bu özellik, sahibi ve bekleyen verileri şifrelemek için kullanılan anahtarları yönetmenizi sağlar. 

Tüm yönetilen disklerin HDInsight, Azure depolama hizmeti şifrelemesi (SSE) ile korunmaktadır. Varsayılan olarak, bu disklerdeki verileri, Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. BYOK etkinleştirirseniz, HDInsight ve Azure anahtar Kasası'nı kullanarak yönetmek için şifreleme anahtarını sağlayın. 

BYOK şifreleme, ek ücret ödemeden küme oluşturma sırasında işlenen tek adımlı bir işlemdir. Tek yapmanız gereken olan HDInsight, Azure Key Vault ile bir yönetilen kimlik olarak kaydedin ve kümenize oluşturduğunuzda şifreleme anahtarını ekleyin.

Kafka kümesinin (Kafka tarafından saklanan çoğaltmaları dahil) için tüm iletiler bir simetrik veri şifreleme anahtarı (DEK ile) şifrelenir. DEK, anahtar Kasası'ndaki anahtar şifreleme anahtarı (KEK) kullanarak korunur. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir. 

Anahtarları key vault'ta güvenli bir şekilde döndürmek için Azure portal veya Azure CLI'yı kullanabilirsiniz. Bir anahtar döndürdüğünde, dakikalar içinde yeni bir anahtar kullanarak HDInsight Kafka kümesinin başlatır. Fidye yazılımı senaryoları ve yanlışlıkla silme durumlarına karşı korumak "Temizleme değil" ve "Geçici silme" anahtar koruma özelliklerini etkinleştirin. Anahtarları olmadan bu koruma özellikleri desteklenmez.

## <a name="get-started-with-byok"></a>BYOK ile çalışmaya başlama

1. Azure kaynakları için yönetilen kimlikler oluşturun.

   Anahtar Kasası'na kimliğini doğrulamak için bir kullanıcı tarafından atanan yönetilen kimlik kullanarak oluşturma [Azure portalı](../../active-directory/managed-service-identity/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-service-identity/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-service-identity/how-to-manage-ua-identity-arm.md), veya [ Azure CLI](../../active-directory/managed-service-identity/how-to-manage-ua-identity-cli.md). Azure Active directory yönetilen kimlikleri ve Kafka için BYOK için gerekli olsa da, Kurumsal güvenlik paketi (ESP) bir gereksinim değildir. Yönetilen kimlik kaynak kimliği için anahtar kasası erişim ilkesini eklediğinizde kaydettiğinizden emin olun.

   ![Azure Portalı'nda kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/apache-kafka-byok/user-managed-identity-portal.png)

2. Oluşturun veya Azure anahtar kasası içeri aktarın.

   HDInsight yalnızca Azure anahtar kasası destekler. Anahtar kasanız varsa, Azure Key Vault'a anahtarlarınızı içeri aktarabilirsiniz. Anahtarları "Geçici silme" ve "Yapmak değil etkin temizleme" olması gerektiğini unutmayın. "Geçici silme" ve "Temizleme değil" Özellikler .NET REST üzerinden kullanılabilir / C#, PowerShell ve Azure CLI arabirimleri.

   Yeni bir anahtar kasası oluşturmak için takip [Azure anahtar kasası](../../key-vault/key-vault-get-started.md) hızlı başlangıç. Mevcut anahtarları içeri aktarma hakkında daha fazla bilgi için ziyaret [anahtarlara, parolalara ve sertifikalara hakkında](../../key-vault/about-keys-secrets-and-certificates.md).

3. Yönetilen kimlik için anahtar kasası erişim ilkesi ekleyin.

   Yeni bir Azure anahtar kasası erişim ilkesi oluşturun.

   ![Yeni Azure anahtar kasası erişim ilkesi oluşturma](./media/apache-kafka-byok/add-key-vault-access-policy.png)

   Ayarlama **anahtar izinleri** için **alma**, **anahtarı kaydırma**, ve **anahtarı sarmalama**.

   ![Anahtar izinleri için Azure anahtar kasası erişim ilkesini ayarlama](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

   Ayarlama **gizli dizi izinleri** için **alma**, **ayarlamak**, ve **Sil**.

   ![Anahtar izinleri için Azure anahtar kasası erişim ilkesini ayarlama](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

   Altında **sorumlu Seç**, oluşturduğunuz kullanıcı tarafından atanan yönetilen kimlik seçin.

   ![Sorumlu Seç için Azure anahtar kasası erişim ilkesini ayarlama](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

4. HDInsight kümesi oluşturma

   Yeni bir HDInsight kümesi oluşturmak artık hazırsınız. BYOK küme oluşturma sırasında yalnızca yeni kümelerine uygulanabilir. Şifreleme BYOK kümelerdeki kaldırılamaz ve BYOK için var olan kümeleri eklenemez.

   ![Azure portalında Kafka disk şifrelemesi](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   Küme oluşturma sırasında tam sağlayan anahtar anahtar sürümü dahil olmak üzere URL. Örneğin, `myakv.azure.com/KEK1/v1`. Yönetilen kimlik kümeye atamak ve anahtar URI'si sağlamak gerekir.

## <a name="faq-for-byok-to-kafka"></a>Kafka için bYok ile ilgili SSS

**Kafka kümesinin, my anahtar kasasına nasıl erişebilir?**

   Yönetilen bir kimlik, küme oluşturma sırasında HDInsight Kafka kümesiyle ilişkilendirin. Bu yönetilen kimlik önce veya küme oluşturma sırasında oluşturulabilir. Yönetilen kimlik anahtarın depolandığı anahtar kasasına erişim gerekir.

**Bu özellik, HDInsight Kafka kümelerinde tüm kullanılabilir?**

   BYOK şifreleme, yalnızca Kafka 1.1 ve kümeleri üstündeki mümkündür.

**Farklı konulara/bölümler için farklı anahtarlara sahip olabilir miyim?**

   Hayır, kümedeki tüm yönetilen diskler, aynı anahtarla şifrelenir.

**Anahtarları silinirse nasıl küme kurtarma gerçekleştirebilir miyim?**

   Yalnızca "Yumuşak etkin Sil" anahtarları desteklendiğinden, anahtar kasasındaki anahtarlar geri yüklediyseniz, küme anahtarları erişebilmesi. Bir Azure Key Vault anahtarını geri yüklemek için bkz: [geri yükleme-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey).

**Üretici/tüketici uygulamaları BYOK küme ve BYOK küme ile aynı anda çalışma sahip olabilir miyim?**

   Evet. BYOK kullanımına üretici/tüketici uygulamaları için saydamdır. Şifreleme işletim sistemi katmanında'olmuyor. Herhangi bir değişiklik var olan üretici/tüketici Kafka uygulamaları yapılması gerekir.

**İşletim sistemi diskleri/kaynak diskler de şifrelenir?**

   Hayır. İşletim sistemi diskleri ve kaynak diskler şifrelenmez.

**Bir kümenin ölçeği genişletilirse, yeni aracılar BYOK sorunsuz bir şekilde destekliyor?**

   Evet. Küme, yedekleme sırasında ölçek anahtar kasasında anahtarı erişmesi gerekir. Kümedeki tüm yönetilen diskler şifrelemek için aynı anahtar kullanılır.

**BYOK konumunuzu içinde kullanılabilir mi?**

   Kafka BYOK tüm genel bulutlarda kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure anahtar kasası nedir](../../key-vault/key-vault-whatis.md)?
* Azure anahtar kasası ile çalışmaya başlamak için bkz. [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md).
