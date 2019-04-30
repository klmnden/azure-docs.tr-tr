---
title: Azure HDInsight uygulamaları yayımlama
description: Bir HDInsight uygulaması oluşturun ve ardından Azure Marketi'nde yayımlama konusunda bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: e64bf253a73df3a2f8170109dc1dfb9a59613733
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097418"
---
# <a name="publish-an-hdinsight-application-in-the-azure-marketplace"></a>Bir HDInsight uygulaması Azure Market'te yayımlama
Bir Linux tabanlı HDInsight kümesi üzerinde bir Azure HDInsight uygulama yükleyebilirsiniz. Bu makalede, HDInsight uygulama Azure Marketi'nde yayımlama konusunda bilgi edinin. Azure Marketi'nde yayımlama hakkında genel bilgi için bkz. [Azure Marketi'nde teklif yayımlamak](../marketplace/marketplace-publishers-guide.md).

HDInsight uygulamaları *kendi lisansını getir (KLG)* modeli. Bir BYOL senaryosunda uygulama sağlayıcısı uygulama kullanıcılara uygulama lisansı sağlamaktan sorumlu. Uygulama kullanıcıları, yalnızca, HDInsight kümesi ve kümenin sanal makinelerini ve düğümleri gibi oluşturdukları Azure kaynakları için ücretlendirilirsiniz. Şu anda, uygulamanın kendisi için faturalandırma, Azure'da oluşmaz.

Daha fazla bilgi için uygulamayla ilgili bu HDInsight makalelere bakın:

* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md). Kümeleriniz üzerinde bir HDInsight uygulamasını yüklemeyi öğrenin.
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md). Yükleme ve özel HDInsight uygulamaları test etme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar
Özel Market, ilk olarak, uygulamanızda göndermek için [oluşturabilir ve özel uygulamanızı test](hdinsight-apps-install-custom-applications.md).

Ayrıca Geliştirici hesabınızı kaydetmeniz gerekir. Daha fazla bilgi için [Azure Marketi'nde teklif yayımlamak](../marketplace/marketplace-publishers-guide.md) ve [Microsoft Developer hesabı oluşturma](../marketplace/marketplace-publishers-guide.md).

## <a name="define-the-application"></a>Uygulama tanımlama
İki adım, uygulamaları Market'te yayımlama faydalanırsınız. İlk olarak tanımlayan bir *createUiDef.json* dosya. CreateUiDef.json dosyası, hangi kümeleri uygulamanız ile uyumlu olduğunu gösterir. Ardından şablonu Azure portalında yayımlayın. Örnek bir createUiDef.json dosyası aşağıda verilmiştir:

```json
{
    "handler": "Microsoft.HDInsight",
    "version": "0.0.1-preview",
    "clusterFilters": {
        "types": ["Hadoop", "HBase", "Storm", "Spark"],
        "versions": ["3.6"]
    }
}
```

| Alan | Açıklama | Olası değerler |
| --- | --- | --- |
| types |Uygulamanın uyumlu olduğu küme türleri. |Hadoop, HBase, Storm, Spark (veya bunların herhangi bir birleşimi) |
| versions |Uygulamanın uyumlu olduğu HDInsight küme türleri. |3.4 |

## <a name="application-installation-script"></a>Uygulama yükleme betiği
Bir küme (ya da mevcut bir kümeye ya da yeni bir tane) bir uygulama yüklendiğinde, kenar düğümüne oluşturulur. Uygulama yükleme betiği, kenar düğümünde çalışır.

  > [!IMPORTANT]  
  > Uygulama yükleme betiğinin adını belirli bir küme için benzersiz olmalıdır. Betik adı şu biçimde olmalıdır:
  > 
  > "name": "[concat ('hue yükleme v0 ','-', uniquestring('applicationName')]"
  > 
  > Betik adı üç bölümden oluşur:
  > 
  > * Uygulama adı ya da uygulamayla ilgili adı içermelidir bir betik adı ön eki.
  > * Okunabilirlik için bir tire.
  > * Uygulama adı parametre olarak ile benzersiz bir dize işlevi.
  > 
  > Kalıcı betik eylemi listesinde, önceki örnek olarak görüntülenen **hue-yükle-v0-4wkahss55hlas**. Bkz: bir [örnek JSON yükü](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 

Yükleme betiğini aşağıdaki özelliklere sahip olmanız gerekir:
* Betik etkilidir. Komut dosyası birden çok çağrı aynı sonucu üretir.
* Doğru tutulan betiğidir. Güncelleştirirken veya değişiklikleri test etmek için komut dosyası farklı bir konum kullanın. Bu, uygulama yüklenirken müşteriler güncelleştirmelerinizi tarafından etkilenen veya test olmamasını sağlar. 
* Betik her noktada yeterli günlük kaydı var. Genellikle, uygulama yükleme sorunları hata ayıklamak için tek yolu betik günlüklerdir.
* Dış hizmetlere ya da kaynaklara çağrıları yeterli deneme olması yükleme geçici ağ sorunları etkilenmez.
* Betiğinizi düğümlerinde Hizmetleri başlarsa, hizmetleri izlenir ve düğümü yeniden başlatma oluşması durumunda otomatik olarak başlatılacak şekilde yapılandırılmış.

## <a name="package-the-application"></a>Uygulama paketi
HDInsight uygulamanızı yüklemek için gereken tüm dosyaları içeren bir .zip dosyası oluşturun. Uygulamayı yayımlamak için .zip dosyasını kullanın. .Zip dosyasını aşağıdaki dosyaları içerir:

* createUiDefinition.json
* mainTemplate.json (bir örnek için bkz. [özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).)
* Gerekli tüm betikler

> [!NOTE]  
> Uygulama dosyaları (tüm web uygulama dosyaları dahil), herhangi genel olarak erişilebilen bir uç noktaya barındırabilirsiniz.

## <a name="publish-the-application"></a>Uygulamayı yayımlama
Bir HDInsight uygulamasını yayımlamak için:

1. Oturum [Azure yayımlama](https://publish.windowsazure.com/).
2. Sol menüde **çözüm şablonları**.
3. Bir başlık girin ve ardından **yeni bir çözüm şablonu oluşturmak**.
4. Kuruluşunuz henüz kaydolmadıysanız, seçin **Geliştirme Merkezi hesabı oluşturun ve Azure programına katılın**.  Daha fazla bilgi için [Microsoft Developer hesabı oluşturma](../marketplace/marketplace-publishers-guide.md).
5. Seçin **kullanmaya başlamak için bazı topolojiler tanımlayın**. Bir çözüm şablonu tüm topolojileri "üst" dir. Bir teklif veya çözüm şablonunda birden fazla topoloji tanımlayabilirsiniz. Bir teklif hazırlama için gönderildiğinde tüm topolojileri ile gönderildi. 
6. Topoloji adı girin ve ardından **+**.
7. Yeni bir sürüm girin ve ardından **+**.
8. Uygulama paketlendi sırada oluşturulan .zip dosyasını karşıya yükleyin.  
9. Seçin **sertifika iste**. Microsoft Sertifika ekibi dosyaları gözden geçirir ve topolojiyi onaylar.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md) kümeleri içinde.
* Bilgi edinmek için nasıl [özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md) HDInsight yayımlanmamış bir HDInsight uygulaması ve dağıtma.
* Bilgi nasıl [Linux tabanlı HDInsight kümeleri özelleştirmek için betik eylemi kullanmanız](hdinsight-hadoop-customize-cluster-linux.md) ve daha fazla uygulama ekleyin. 
* Bilgi edinmek için nasıl [Azure Resource Manager şablonlarını kullanarak HDInsight içinde Linux tabanlı Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
* Bilgi edinmek için nasıl [HDInsight içinde boş bir kenar düğümünü kullanın](hdinsight-apps-use-edge-node.md) HDInsight kümeleri erişmek için HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak.

