---
title: "Hdınsight küme oluşturma işlemi sırasında - Azure Hıve kitaplıkları ekleme | Microsoft Docs"
description: "Hive kitaplıkları (jar dosyaları) eklemek bir Hdınsight kümesine küme oluşturma sırasında öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 90a1ea99cbba82b49a0ff6712bcaaa5dc814810e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Özel Hıve kitaplıkları, Hdınsight kümesi oluştururken ekleme

Hdınsight'ta Hive kitaplık önceden yükleme hakkında bilgi edinin. Bu belge kitaplıkları küme oluşturma sırasında önceden yüklemek için bir betik eylemi kullanarak bilgi içerir. Bu belgede yer alan adımları kullanarak eklenen kitaplıkları genel olarak kullanılabilir kovanında - kullanmaya gerek yoktur [eklemek JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) bunları yüklemek için.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir küme oluştururken, oluşturuldukları sırada küme düğümleri değiştirmek için bir betik eylemini kullanabilirsiniz. Bu belgedeki betik kitaplıkları konumudur tek bir parametre kabul eder. Bu konum bir Azure depolama hesabında olmalıdır ve kitaplıkları jar dosyaları olarak depolanmalıdır.

Küme oluşturma sırasında komut dosyaları sıralar, kopyalar `/usr/lib/customhivelibs/` head ve çalışan düğümleri üzerinde dizin sonra ekler onlara `hive.aux.jars.path` özelliğinde `core-site.xml` dosya. Ayrıca Linux tabanlı kümelerde güncelleştirir `hive-env.sh` dosyalarının konumunu dosyasıyla.

> [!NOTE]
> Bu makalede betik eylemleri kullanılarak kitaplıkları aşağıdaki senaryolarda kullanılabilir hale getirir:
>
> * **Linux tabanlı Hdınsight** - Hive istemci kullanırken **WebHCat**, ve **HiveServer2**.
> * **Windows tabanlı Hdınsight** - Hive istemci kullanırken ve **WebHCat**.

## <a name="the-script"></a>Komut dosyası

**Komut dosyası konumu**

İçin **Linux tabanlı kümelerde**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

İçin **Windows tabanlı kümeler**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Gereksinimleri**

* Betikler her ikisini de uygulanması gereken **baş düğümler** ve **çalışan düğümleri**.

* Azure Blob depolama alanına yüklemek istediğiniz Kavanoz saklanmalıdır bir **tek kapsayıcısı**.

* Kitaplığın adı jar dosyalarını içeren depolama hesabı **gerekir** oluşturma sırasında bağlı Hdınsight kümesi. Ya da varsayılan depolama hesabı olması gerekir ya da bir hesap üzerinden eklenen __isteğe bağlı yapılandırma__.

* Kapsayıcı WASB yoluna betik eylemi parametresi olarak belirtilmelidir. Örneğin Kavanoz adlı bir kapsayıcıda depolanır, **kitaplıklar** bir depolama hesabında adlı **mystorage**, parametre olacaktır  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Bu belgede zaten oluşturulmuş bir depolama hesabı, blob kapsayıcısı ve kendisine karşıya yüklenen dosyaların olduğunu varsayar.
  >
  > Bir depolama hesabı oluşturmadıysanız, bu nedenle aracılığıyla yapabileceğiniz [Azure portal](https://portal.azure.com). Ardından bir programı gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/) bir kapsayıcı hesabı oluşturun ve dosyaları yükleyin.

## <a name="create-a-cluster-using-the-script"></a>Komut dosyası kullanarak bir küme oluşturun

> [!NOTE]
> Aşağıdaki adımlar Linux tabanlı Hdınsight kümesi oluşturur. Windows tabanlı bir küme oluşturmak için seçin **Windows** (PowerShell) Windows komut dosyası yerine bash betik kullanımını ve küme oluştururken, işletim sistemi kümesi olarak.
>
> Bu komut dosyası kullanarak bir küme oluşturmak için Azure PowerShell veya Hdınsight .NET SDK'sını kullanabilirsiniz. Bu yöntemleri kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight betik eylemleri ile kümeleri](hdinsight-hadoop-customize-cluster-linux.md).

1. ' Ndaki adımları kullanarak bir küme hazırlama Başlat [Hdınsight kümeleri hazırlama Linux'ta](hdinsight-hadoop-provision-linux-clusters.md), ancak sağlama tamamlamayın.

2. Üzerinde **isteğe bağlı yapılandırma** bölümünde, select **betik eylemleri**ve aşağıdaki bilgileri sağlayın:

   * **AD**: betik eylemi için kolay bir ad girin.

   * **BETİK URI'si**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: Bu seçeneği işaretleyin.

   * **ÇALIŞAN**: Bu seçeneği işaretleyin.

   * **ZOOKEEPER**: Bu alanı boş bırakın.

   * **PARAMETRELERİ**: Kavanoz içeren kapsayıcı ve depolama hesabına WASB adresini girin. Örneğin,  **wasb://libs@mystorage.blob.core.windows.net/** .

3. Ekranın alt kısmındaki **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğmesi.

4. Üzerinde **isteğe bağlı yapılandırma** bölümünde, select **bağlantılı depolama hesapları** seçip **depolama anahtarı eklemek** bağlantı. Kavanoz içeren depolama hesabı seçin. Ardından **seçin** ayarları kaydedin ve dönüş düğmeleri **isteğe bağlı yapılandırma**.

5. İsteğe bağlı yapılandırma kaydetmek için kullanın **seçin** alt kısmındaki düğmesi **isteğe bağlı yapılandırma** bölümü.

6. Bölümünde açıklandığı gibi küme hazırlama devam [Hdınsight kümeleri hazırlama Linux'ta](hdinsight-hadoop-provision-linux-clusters.md).

Küme oluşturma tamamlandıktan sonra bu komut dosyası aracılığıyla kullanmak zorunda kalmadan kovanından eklenen Kavanoz kullanabilmek için `ADD JAR` deyimi.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışma hakkında daha fazla bilgi için bkz: [Hdınsight ile Hive kullanma](hdinsight-use-hive.md)
