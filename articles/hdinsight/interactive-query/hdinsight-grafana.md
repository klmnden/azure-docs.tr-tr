---
title: Azure HDInsight üzerinde Grafana kullanma
description: Azure HDInsight, Grafana erişmeyi öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 495c617f97b2a7f39182b7684c2c0e183c2002b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60484044"
---
# <a name="access-grafana-in-azure-hdinsight"></a>Azure HDInsight, erişim Grafana


[Grafana](https://grafana.com/) popüler, açık kaynaklı bir grafik ve Pano Oluşturucusu. Grafana zengin özelliğidir; yalnızca kullanıcılarının özelleştirilebilir oluşturmasına izin vermez ve paylaşılabilir panolar da şablonlu/Script panolar, LDAP tümleştirme, birden çok veri kaynağına ve daha fazlasını sunar.

Şu anda, Azure HDInsight Hbase ve etkileşimli sorgu kümesi türleri ile Grafana desteklenir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-apache-hadoop-cluster"></a>Bir Apache Hadoop kümesi oluşturma

Bu bölümde, bir Azure Resource Manager şablonu kullanarak HDInsight içinde bir etkileşimli sorgu kümesi oluşturun. Bu makaleyi izlemek için Kaynak Yöneticisi şablonuyla deneyim sahibi olmak gerekli değildir. 

1. Aşağıdaki **Azure’da dağıt** düğmesine tıklayarak Azure’da oturum açın ve Azure portalında Kaynak Yöneticisi şablonunu açın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-interactive-hive%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-grafana/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki ekran görüntüsünde önerilen değerleri girin veya seçin:

    > [!NOTE]  
    > Sağladığınız değerler benzersiz olmalı ve adlandırma yönergelerini izlemelidir. Şablon, doğrulama denetimleri gerçekleştirmez. Sağladığınız değerler zaten kullanılıyorsa veya yönergelere uygun değilse, şablonu gönderdikten sonra bir hata alırsınız.       
    > 
    >
    
    ![HDInsight Linux - Portalda Kaynak Yöneticisi şablonunu kullanmaya başlama](./media/hdinsight-grafana/hdinsight-linux-get-started-arm-template-on-portal.png "Azure portalını ve kaynak grup yöneticisi şablonunu kullanarak HDInsight’ta Hadoop kümesi dağıtma")

    Aşağıdaki değerleri yazın veya seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Abonelik**     |  Azure aboneliğinizi seçin. |
    |**Kaynak grubu**     | Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenleri için bir kapsayıcıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içermektedir. |
    |**Konum**     | Kümenizi oluşturmak istediğiniz bir Azure konumunu seçin.  Daha iyi performans için kendinize yakın bir konum seçin. |
    |**Küme Türü**     | **hadoop**’u seçin. |
    |**Küme Adı**     | Apache Hadoop kümesi için bir ad girin. HDInsight’taki tüm kümeler aynı DNS ad alanını paylaştığından, bu adın benzersiz olması gerekir. Ad, harf, rakam ve kısa çizgilerin dahil olduğu en fazla 59 karakterden oluşabilir. Adın ilk ve son karakterleri, kısa çizgi olamaz. |
    |**Küme oturum açma adı ve parolası**     | Varsayılan oturum açma adı **admin**’dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf, bir küçük harf, bir alfasayısal olmayan karakter (' " ` karakterleri hariç\) içermelidir. "Pass@word1" gibi genel parolalar **sağlamadığınızdan** emin olun.|
    |**SSH kullanıcı adı ve parolası**     | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adını yeniden adlandırabilirsiniz.  SSH kullanıcı parolasının gereksinimleri, küme oturum açma parolasıyla aynıdır.|
       
    Şablondaki bazı özellikler sabit kodlanmış olabilir.  Bu değerleri şablondan yapılandırabilirsiniz. Bu özellikler hakkında daha fazla açıklama için bkz: [Apache Hadoop kümeleri oluşturma HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**’u ve **Panoya sabitle**’yi seçip **Satın al**’a tıklayın. Portal panosunda **Dağıtım gönderiliyor** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer.

    ![Şablon dağıtımı ilerlemesi](./media/hdinsight-grafana/deployment-progress-tile.png "Azure Şablonu dağıtımı ilerlemesi")

4. Küme oluşturulduktan sonra, kutucuğun açıklamalı alt yazısı belirttiğiniz kaynak grubu adıyla değişir. Kutucukta, kaynak grubu içinde oluşturulan HDInsight kümesi de listelenir. 
   
    ![HDInsight Linux yeni başlayanlara yönelik kaynak grubu](./media/hdinsight-grafana/hdinsight-linux-get-started-resource-group.png "Azure HDInsight küme kaynağı grubu")
    
5. Kutucukta, kümeyle ilişkili varsayılan depolama da listelenir. Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu genellikle varsayılan depolama hesabı olarak ifade edilir. HDInsight kümesi ve kümenin varsayılan depolama hesabının aynı Azure bölgesinde birlikte gerekir. Kümeleri silmek depolama hesabını silmez.
    

> [!NOTE]  
> Diğer küme oluşturma yöntemleri ve bu öğreticide kullanılan özellikler hakkında bilgi edinmek için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="access-the-grafana-dashboard"></a>Grafana panosuna erişim

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **HDInsight kümeleri**ve ardından son bölümde oluşturduğunuz küme adını seçin.

3. Altında **hızlı bağlantılar**, tıklayın **küme Panosu**.

    ![HDInsight küme Panosu portalı](./media/hdinsight-grafana/hdinsight-portal-cluster-dashboard.png "portalında HDInsight küme Panosu")

4. Panodan tıklayın **Grafana** Döşe. Alternatif olarak, göz atın `/grafana/` küme URL'niz yolu. Örneğin, `https://<clustername>.azurehdinsight.net/grafana/`.

5. Hadoop kümesi kullanıcı kimlik bilgilerini girin.

6. Grafana panosunun görünür ve şu örnekteki gibi görünür:

    ![HDInsight panosunun](./media/hdinsight-grafana/hdinsight-grafana-dashboard.png "HDInsight Grafana Panosu")

   

## <a name="clean-up-resources"></a>Kaynakları temizleme
Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. 

> [!NOTE]  
> HDInsight üzerinde Hadoop kullanarak ETL işlemlerinin nasıl çalıştırılacağını öğrenmek için sonraki öğreticiye *hemen* ilerliyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Öğreticide tekrar Hadoop kümesi oluşturmanız gerektiğinden bu gereklidir. Ancak sonraki öğreticiye hemen geçmeyecekseniz şimdi kümeyi silmeniz gerekir.

**Küme ve/veya varsayılan depolama hesabını silmek için**

1. Azure portalın bulunduğu tarayıcı sekmesine dönün. Kümeye genel bakış sayfasında olmalısınız. Yalnızca kümeyi silmek, ancak varsayılan depolama hesabını korumak istiyorsanız **Sil**’i seçin.

    ![HDInsight küme silme](./media/hdinsight-grafana/hdinsight-delete-cluster.png "HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu sayfasını açmak için kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçin.

3. **Kaynak grubunu sil**’i seçerek, kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silin. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Resource Manager şablonu kullanarak Linux tabanlı HDInsight kümesi oluşturma ve temel Apache Hive sorguları gerçekleştirmeyi öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
>[Ayıklama, dönüştürme ve HDInsight üzerinde Apache Hive kullanarak verileri yükleme](../hdinsight-analyze-flight-delay-data-linux.md)

Kendi verilerinizle çalışmaya başlamaya hazırsanız ve HDInsight’ın verileri nasıl depoladı veya verileri HDInsight’a alma hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makalelere bakın:

* HDInsight’ın Azure Depolama’yı nasıl kullandığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Depolama kullanma](../hdinsight-hadoop-use-blob-storage.md).
* HDInsight’a veril yükleme hakkında daha fazla bilgi için bkz. [Verileri HDInsight’a yükleme](../hdinsight-upload-data.md).

HDInsight ile veri çözümleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Visual Studio'dan Hive sorguları gerçekleştirme dahil, HDInsight ile Hive kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma Apache](../hdinsight-use-hive.md).
* Verileri dönüştürmek için kullanılan bir dil olan pig hakkında bilgi için bkz: [HDInsight ile Apache Pig kullanma](../hdinsight-use-pig.md).
* Hadoop’ta verileri işleyen programları yazmanın bir yöntemi olan MapReduce hakkında bilgi edinmek için bkz. [HDInsight ile MapReduce kullanma](../hdinsight-use-mapreduce.md).
* HDInsight’taki verileri çözümlemek amacıyla Visual Studio için HDInsight Araçları kullanma hakkında bilgi edinmek için bkz. [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).



HDInsight kümesi oluşturma ve yönetme hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makalelere bakın:

* Linux tabanlı HDInsight kümenizi yönetme hakkında bilgi edinmek için bkz. [Ambari kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).
* HDInsight kümesi oluştururken tercih edebileceğiniz seçenekler hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak Linux’ta HDInsight oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).


[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
