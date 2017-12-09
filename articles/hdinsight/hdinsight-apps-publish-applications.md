---
title: "Azure Hdınsight uygulamalarını yayımlama | Microsoft Docs"
description: "Hdınsight uygulaması oluşturma ve Azure Marketi'nde yayımlama hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2017
ms.author: jgao
ms.openlocfilehash: 34550ed33cd81bcbf5b405a5e5c09d25adf5e6ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="publish-an-hdinsight-application-in-the-azure-marketplace"></a>Bir Hdınsight uygulamasının Azure Marketi'nde yayımlama
Linux tabanlı Hdınsight kümesinde bir Azure Hdınsight uygulamayı yükleyebilir. Bu makalede bir Hdınsight uygulamasının Azure Marketi'nde yayımlama öğrenin. Azure Marketi'nde yayımlama hakkında genel bilgi için bkz: [bir teklifi Azure Marketi'nde yayımlama](../marketplace-publishing/marketplace-publishing-getting-started.md).

Hdınsight uygulamaları *Getir bilgisayarınızı kendi lisans (KLG)* modeli. KLG senaryoda, bir uygulamanın uygulama kullanıcılara uygulama lisansı sağlamaktan sorumlu sağlayıcısıdır. Uygulama kullanıcıları, yalnızca, Hdınsight kümesi ve kümenin VM'ler ve düğümleri gibi oluşturdukları Azure kaynakları için ücretlendirilirsiniz. Şu anda, uygulama için fatura Azure'da oluşmaz.

Daha fazla bilgi için uygulama ile ilgili bu Hdınsight makalelere bakın:

* [Hdınsight uygulamaları yükleme](hdinsight-apps-install-applications.md). Hdınsight uygulaması yükleme, kümelerinde öğrenin.
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md). Yükleme ve özel Hdınsight uygulamaları test etme hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar
Market, ilk olarak, özel, uygulamanızda göndermek için [oluşturun ve özel uygulamanızı test](hdinsight-apps-install-custom-applications.md).

Ayrıca Geliştirici hesabınızı kaydetmeniz gerekir. Daha fazla bilgi için bkz: [bir teklifi Azure Marketi'nde yayımlama](../marketplace-publishing/marketplace-publishing-getting-started.md) ve [Microsoft Developer hesabı oluşturma](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-the-application"></a>Uygulama tanımlama
İki adımı Marketi'ndeki uygulamaları yayımlama ilgilidir. İlk olarak tanımlayan bir *createUiDef.json* dosya. CreateUiDef.json dosyası, hangi kümeleri, uygulamanızın ile uyumlu olduğunu gösterir. Ardından, Azure portal şablonu yayımlayın. Örnek bir createUiDef.json dosyası aşağıda verilmiştir:

```json
{
    "handler": "Microsoft.HDInsight",
    "version": "0.0.1-preview",
    "clusterFilters": {
        "types": ["Hadoop", "HBase", "Storm", "Spark"],
        "tiers": ["Standard", "Premium"],
        "versions": ["3.4"]
    }
}
```

| Alan | Açıklama | Olası değerler |
| --- | --- | --- |
| types |Uygulamanın uyumlu olduğu küme türleri. |Hadoop, HBase, Storm, Spark (veya bunların herhangi bir birleşimini) |
| tiers |Uygulamanın uyumlu olduğu küme katmanları. |Standart, Premium (veya her ikisi de) |
| versions |Uygulamanın uyumlu olduğu HDInsight küme türleri. |3.4 |

## <a name="application-installation-script"></a>Uygulama yükleme betiği
Bir kümede (ya da varolan bir kümeye veya yeni bir tane) bir uygulama yüklendiğinde bir kenar düğümüne oluşturulur. Uygulama yükleme betiği edge düğüm üzerinde çalışır.

  > [!IMPORTANT]
  > Uygulama yükleme komut dosyası adını belirli bir küme için benzersiz olmalıdır. Betik adı şu biçimde olmalıdır:
  > 
  > "name": "[concat (' ton yükleme v0 ','-', uniquestring('applicationName')]"
  > 
  > Betik adı üç bölümden oluşur:
  > 
  > * Uygulama adını veya uygulamayla ilgili adı içermesi gerekir bir betik adı ön eki.
  > * Okunabilirlik için bir tire.
  > * Parametre olarak uygulama adıyla benzersiz bir dize işlevi.
  > 
  > Kalıcı betik eylemi listesinde olarak önceki örnekte gösterilen **hue-Install-v0-4wkahss55hlas**. Bkz: bir [örnek JSON yükü](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 

Yükleme komut dosyası, aşağıdaki özelliklere sahip olması gerekir:
* Idempotent komut dosyasıdır. Komut dosyası için birden fazla çağrı aynı sonucu verir.
* Komut dosyası düzgün sürümlü ' dir. Yükseltme veya değişiklikleri test komut dosyası için farklı bir konum kullanın. Bu uygulamanın yüklenmesi müşteriler güncelleştirmelerinizi tarafından etkilenen ya da test sağlar. 
* Komut dosyası her noktada yeterli günlük kaydı var. Genellikle, komut dosyası günlükleri uygulama yükleme sorunlarını hata ayıklamak için tek yoludur.
* Dış hizmetler veya kaynakları çağrıları yeterli yeniden deneme olması, böylece yükleme geçici ağ sorunları etkilenmez.
* Kodunuzu düğümlerde Hizmetleri başlarsa, hizmetleri izlenen ve düğüm yeniden başlatma gerçekleşirse otomatik olarak başlatılacak şekilde yapılandırılmış.

## <a name="package-the-application"></a>Uygulama paketi
Hdınsight uygulaması yüklemek için gereken tüm dosyaları içeren bir .zip dosyası oluşturun. .zip dosyasını kullanmanız [uygulamayı yayımlamak](#publish-application). .Zip dosyasını aşağıdaki dosyaları içerir:

* [createUiDefinition.json](#define-application)
* mainTemplate.json (bir örnek için bkz: [özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).)
* Gerekli tüm betikler

> [!NOTE]
> Uygulama dosyaları (tüm web uygulama dosyaları dahil) üzerinde genel olarak erişilebilir olan herhangi bir uç nokta barındırabilir.
> 

## <a name="publish-the-application"></a>Uygulamayı yayımlama
Bir Hdınsight uygulamasını yayımlamak için:

1. Oturum [Azure yayımlama](https://publish.windowsazure.com/).
2. Soldaki menüde seçin **çözüm şablonları**.
3. Bir başlık girin ve ardından **yeni bir çözüm şablonu oluşturmak**.
4. Kuruluşunuz zaten kaydolmadıysanız seçin **Geliştirme Merkezi hesabı oluşturun ve Azure programına katılın**.  Daha fazla bilgi için bkz: [Microsoft Developer hesabı oluşturma](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Seçin **başlamak için bazı topolojiler tanımlayın**. Bir çözüm şablonu tüm Topolojileri için "üst" dir. Bir teklif ya da çözüm şablonunda birden fazla topoloji tanımlayabilirsiniz. Bir teklif hazırlama için gönderildiğinde tüm topolojileri ile gönderilir. 
6. Bir topoloji adı girin ve ardından  **+** .
7. Yeni bir sürüm girin ve ardından  **+** .
8. Ne zaman oluşturduğunuz .zip dosyasını karşıya yükleyin, [uygulama paketlenmiş](#package-application).  
9. Seçin **sertifika iste**. Microsoft Sertifika ekibi dosyaları gözden geçirir ve topolojiyi onaylar.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Hdınsight uygulamaları yükleme](hdinsight-apps-install-applications.md) , kümelerdeki.
* Bilgi edinmek için nasıl [özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md) ve yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a.
* Bilgi nasıl [Linux tabanlı Hdınsight kümelerini özelleştirme için betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md) ve daha fazla uygulama ekleyin. 
* Bilgi edinmek için nasıl [Azure Resource Manager şablonları kullanarak Hdınsight'ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
* Öğrenin nasıl [Hdınsight'ta boş kenar düğümünü kullanın](hdinsight-apps-use-edge-node.md) Hdınsight kümeleri erişmek için Hdınsight uygulamalarını test ve Hdınsight uygulamaları barındırır.

