---
title: "HDInsight uygulamaları yayımlama | Microsoft Belgeleri"
description: "HDInsight uygulamaları oluşturma ve yayımlama hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/18/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ca18e2660d2e59f6dee12010abc9d1780f3a717a


---
# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>HDInsight uygulamalarını Azure Marketi’nde yayımlama
HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir. Bu makalede bir HDInsight uygulamasının Azure Marketi’nde nasıl yayımlanacağını öğreneceksiniz.  Azure Marketi’nde yayımlama hakkında genel bilgi için bkz. [bir teklifi Azure Marketinde yayımlama](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight uygulamaları, uygulama sağlayıcısının son kullanıcılara uygulama lisansı sağlamaktan sorumlu olduğu ve son kullanıcılardan yalnızca HDInsight kümesi ve VM/düğümleri gibi oluşturdukları kaynaklar için Azure tarafından ücret alındığı *Kendi Lisansını Getir (KLG)* modelini kullanır. Şu anda Azure üzerinden uygulama için faturalama yapılmamaktadır.

HDInsight uygulamasıyla ilgili diğer makale:

* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): Özel HDInsight uygulamalarını yükleyip test etme hakkında bilgi alın.

## <a name="prerequisites"></a>Ön koşullar
Özel uygulamanızı markete göndermek için özel uygulamanızı oluşturmuş ve test etmiş olmanız gerekir. Aşağıdaki makalelere bakın:

* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): Özel HDInsight uygulamalarını yükleyip test etme hakkında bilgi alın.

Ayrıca geliştirici hesabınızı kaydetmeniz gerekir. Bkz. [bir teklifi Azure Marketinde yayımlama](../marketplace-publishing/marketplace-publishing-getting-started.md) ve [Microsoft Developer hesabı oluşturma](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Uygulama tanımlama
Azure Marketi’nde uygulama yayımlama iki adımdan oluşur.  İlk olarak uygulamanızın hangi kümelerle uyumlu olduğunu göstermek üzere bir **createUiDef.json** dosyası tanımlarsınız, ardından şablonu Azure portalında yayımlarsınız. Örnek bir createUiDef.json dosyası aşağıda verilmiştir.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Alan | Açıklama | Olası değerler |
| --- | --- | --- |
| types |Uygulamanın uyumlu olduğu küme türleri. |Hadoop, HBase, Storm, Spark, (veya bunların herhangi bir birleşimi) |
| tiers |Uygulamanın uyumlu olduğu küme katmanları. |Standart, Premium (veya her ikisi) |
| versions |Uygulamanın uyumlu olduğu HDInsight küme türleri. |3.4 |

## <a name="package-application"></a>Uygulama paketleme
HDInsight uygulamalarınızı yüklemek için tüm gerekli dosyaları içeren bir zip dosyası oluşturun. Zip dosyasına [Uygulama yayımlama](#publish-application) işleminde ihtiyacınız olacaktır.

* [createUiDefinition.json](#define-application).
* mainTemplate.json. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md) konusunda örneğe bakın.
  
  > [!IMPORTANT]
  > Uygulama yükleme betik adları belirli bir küme için aşağıdaki biçimde benzersiz olmalıdır. Ayrıca yükleme ve kaldırma betik eylemleri bir kez etkili olmalı, diğer bir deyişle betikler tekrarlayarak çağrıldığında aynı sonucu vermelidir.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Betik adı üç bölümden oluşur:
  > 
  > 1. Uygulama adını veya uygulamayla ilgili adı içermesi gereken bir betik adı ön eki.
  > 2. Okunabilirlik için bir "-" işareti.
  > 3. Parametre olarak uygulama adı ile birlikte benzersiz bir dize işlevi.
  > 
  > Yukarıdakiler bir araya geldiğinde, kalıcı betik eylemi listesinde hue-install-v0-4wkahss55hlas gibi görünür. Örnek bir JSON yükü için bkz. [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
  > 
* Gerekli tüm betikler.

> [!NOTE]
> Uygulama dosyaları (varsa web uygulama dosyaları dahil) ortak olarak erişilebilen herhangi bir uç noktaya konumlandırılabilir.
> 
> 

## <a name="publish-application"></a>Uygulama yayımlama
Bir HDInsight uygulamasını yayımlamak için aşağıdaki adımları izleyin:

1. [Azure Yayımlama portalında](https://publish.windowsazure.com/) oturum açın.
2. Yeni bir çözüm şablonu oluşturmak için sol taraftaki **Çözüm şablonları**’na tıklayın.
3. Bir ad girin ve ardından **Yeni çözüm şablonu oluştur**’a tıklayın.
4. Henüz yapmadıysanız şirketinizi kaydetmek için **Geliştirme Merkezi hesabı oluşturun ve Azure programına katılın** seçeneğine tıklayın.  Bkz. [Microsoft Developer hesabı oluşturma](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. **Başlamak için bazı Topolojiler tanımlayın**’a tıklayın. Bir çözüm şablonu tüm topolojilerinin "üst öğesidir". Bir teklif/çözüm şablonunda birden fazla topoloji tanımlayabilirsiniz. Bir teklif hazırlama için gönderildiğinde tüm topolojileri ile birlikte iletilir. 
6. Topoloji adı girin ve ardından artı işaretine tıklayın.
7. Yeni bir sürüm girin ve ardından artı işaretine tıklayın.
8. [Uygulama paketleme](#package-application) adımında hazırlanan zip dosyasını karşıya yükleyin.  
9. **Sertifika İste**’ye tıklayın. Microsoft sertifika ekibi dosyaları gözden geçirir ve topolojiyi onaylar.

## <a name="next-steps"></a>Sonraki adımlar
* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir HDInsight uygulamasının HDInsight’a nasıl yükleneceğini öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Azure Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamaları test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.




<!--HONumber=Nov16_HO2-->


