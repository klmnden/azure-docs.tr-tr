---
title: HDInsight kümesi oluşturma - Azure sırasında Apache Hive kitaplıkları ekleme
description: Apache Hive kitaplıkları (jar dosyaları) eklemek bir HDInsight kümesine küme oluşturma sırasında öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: fe8f97368531ed572083834256d84cd1ed6dd8a1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687221"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>HDInsight kümenizi oluştururken özel Apache Hive kitaplıkları ekleme

Önceden yükleme öğrenin [Apache Hive](https://hive.apache.org/) HDInsight üzerinde kitaplıkları. Bu belge, betik eylemi kullanarak küme oluşturma sırasında kitaplıklarını önceden yükleme hakkında bilgi içerir. Bu belgedeki adımları kullanarak eklenen kitaplıkları vardır ve küresel olarak kullanılabilir kovanında - kullanmaya gerek yoktur [ekleme JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) bunları yüklenemedi.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir küme oluştururken, küme düğümleri oluşturuldukları sırada değiştirmek için betik eylemi kullanın. Bu belge betik kitaplıkları'nın konumudur tek bir parametre kabul eder. Bu konum, bir Azure depolama hesabında olmalıdır ve kitaplıkları jar dosyaları olarak depolanmış olması gerekir.

Küme oluşturma sırasında komut dosyaları listeler, kopyalar `/usr/lib/customhivelibs/` baş ve çalışan düğümleri üzerinde dizin ardından ekler kendisine `hive.aux.jars.path` özelliğinde `core-site.xml` dosya. Ayrıca Linux tabanlı kümelerde güncelleştirir `hive-env.sh` dosyasıyla dosyalarının konumu.

> [!NOTE]  
> Bu makalede betik eylemlerini kullanarak kitaplıkları aşağıdaki senaryolarda kullanılabilir hale getirir:
>
> * **Linux tabanlı HDInsight** - bir Hive istemcisi kullanılırken **WebHCat**, ve **HiveServer2**.
> * **Windows tabanlı HDInsight** - Hive istemcisi kullanılırken ve **WebHCat**.

## <a name="the-script"></a>Komut dosyası

**Betik konumu**

İçin **Linux tabanlı kümeler**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

İçin **Windows tabanlı kümeler**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Gereksinimler**

* Betikler her ikisi için de uygulanmalıdır **baş düğümlerine** ve **çalışan düğümleri**.

* Azure Blob depolama alanına yüklemek istediğiniz jar dosyaları dışındaki saklanmalıdır bir **tek kapsayıcı**.

* Kitaplık jar dosyaları içeren depolama hesabını **gerekir** , HDInsight küme oluşturma sırasında bağlı. Varsayılan depolama hesabını ya da olmalıdır veya bir hesap üzerinden eklenen __isteğe bağlı yapılandırma__.

* Betik eylemi bir parametre olarak kapsayıcı WASB yolu belirtilmelidir. Örneğin jar dosyaları dışındaki adlı bir kapsayıcıda depolanır, **libs** adlı bir depolama hesabında **depolamam**, parametre olacaktır **wasb://libs\@ mystorage.BLOB.Core.Windows.NET/**.

  > [!NOTE]  
  > Bu belge, zaten oluşturulan bir depolama hesabı, blob kapsayıcısını ve dosyaları karşıya olduğunu varsayar.
  >
  > Bir depolama hesabı oluşturmadıysanız, bu nedenle ile yapabileceğiniz [Azure portalında](https://portal.azure.com). Ardından bir hizmet gibi kullanabilir [Azure Depolama Gezgini](https://storageexplorer.com/) hesabında bir kapsayıcı oluşturun ve dosyalarını yükleyin.

## <a name="create-a-cluster-using-the-script"></a>Betik kullanarak küme oluşturma

> [!NOTE]  
> Aşağıdaki adımlar, Linux tabanlı HDInsight kümesi oluşturur. Windows tabanlı bir küme oluşturmak için Seç **Windows** kümesi (PowerShell) Windows komut dosyası yerine bash betiğini kullanın ve küme oluştururken işletim sistemi olarak.
>
> Bu komut dosyası kullanarak bir küme oluşturmak için Azure PowerShell veya HDInsight .NET SDK'sını kullanabilirsiniz. Bu yöntemleri kullanma hakkında daha fazla bilgi için bkz. [özelleştirme HDInsight kümeleri ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md).

1. Adımları kullanarak bir kümeyi sağlamaya başlayın [sağlama HDInsight kümelerinde Linux üzerinde](hdinsight-hadoop-provision-linux-clusters.md), ancak sağlama tamamlamayın.

2. Üzerinde **isteğe bağlı yapılandırma** bölümünden **betik eylemleri**ve aşağıdaki bilgileri sağlayın:

   * **AD**: Betik eylemi için bir kolay ad girin.

   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh.

   * **HEAD**: Bu seçeneği işaretleyin.

   * **ÇALIŞAN**: Bu seçeneği işaretleyin.

   * **ZOOKEEPER**: Bunu boş bırakın.

   * **PARAMETRELERİ**: WASB adresi için jar dosyaları dışındaki içeren kapsayıcı ve depolama hesabı girin. Örneğin, **wasb://libs\@mystorage.blob.core.windows.net/**.

3. Sayfanın alt kısmında **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğme.

4. Üzerinde **isteğe bağlı yapılandırma** bölümünden **bağlantılı depolama hesapları** seçip **depolama anahtarı Ekle** bağlantı. Jar dosyaları dışındaki içeren depolama hesabını seçin. Ardından **seçin** ayarları kaydedin ve döndürmek için düğmeler **isteğe bağlı yapılandırma**.

5. İsteğe bağlı yapılandırmayı kaydetmek için kullanın **seçin** düğme alttaki **isteğe bağlı yapılandırma** bölümü.

6. Açıklandığı gibi küme sağlama devam [sağlama HDInsight kümelerinde Linux üzerinde](hdinsight-hadoop-provision-linux-clusters.md).

Küme oluşturma işlemi tamamlandıktan sonra bu betik ile kullanmak zorunda kalmadan kovanından eklenen jar dosyaları dışındaki kullanabilmek için `ADD JAR` deyimi.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışma hakkında daha fazla bilgi için bkz. [Apache Hive ile HDInsight kullanma](hadoop/hdinsight-use-hive.md)
