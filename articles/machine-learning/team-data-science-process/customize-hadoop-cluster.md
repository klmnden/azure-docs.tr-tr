---
title: "Takım veri bilimi işlemi için Hadoop kümelerini özelleştirme | Microsoft Docs"
description: "Popüler Python modülleri özel Azure Hdınsight Hadoop kümeleri kullanılabilir."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 522e33b399f2648427464b439bc4405e9e8097cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Team Data Science Process için Azure HDInsight Hadoop kümelerini özelleştirme
Küme bir Hdınsight hizmeti olarak sağlandığında bu makalede bir Hdınsight Hadoop kümesi 64-bit Anaconda (Python 2.7) yükleyerek özelleştirmek her bir düğümde açıklar. Ayrıca, özel kümeye iş göndermek için headnode erişmek nasıl gösterir. Bu özelleştirme Anaconda, kümedeki Hive kayıtlarını işlemek üzere tasarlanmış kullanıcı tanımlı işlevler (UDF'ler) kullanmak için kolayca kullanılabilir içinde yer alan birçok popüler Python modüllerini sağlar. Bu senaryoda kullanılan yordamları hakkında yönergeler için bkz: [Hive sorguları göndermek nasıl](move-hive-tables.md#submit).

Tarafından kullanılan çeşitli veri bilimi ortamları ayarlama nasıl açıklayan konulara aşağıdaki menü bağlantılar [takım veri bilimi işlem (TDSP)](overview.md).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Azure Hdınsight Hadoop kümesi özelleştirme
Oturum açarak, özelleştirilmiş bir Hdınsight Hadoop kümesi oluşturmak için başlangıç [ **Klasik Azure portalı**](https://manage.windowsazure.com/), tıklatın **yeni** köşe ve ardından Veri Hizmetleri -> sol alt kısmındaki HDINSİGHT -> **özel Oluştur** ortaya çıkarmak için **küme ayrıntıları** penceresi. 

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Yapılandırma sayfasında 1 oluşturulacak kümenin adını girin ve diğer alanları varsayılan değerleri kabul edin. Sonraki yapılandırma sayfasına gitmek için oka tıklayın. 

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Yapılandırma sayfasında 2 sayısı giriş **veri DÜĞÜMLERİNİ**seçin **bölge/sanal ağ**ve boyutunu seçin **baş düğüm** ve **veri düğümü**. Sonraki yapılandırma sayfasına gitmek için oka tıklayın.

> [!NOTE]
> **Bölge/sanal ağ** Hdınsight Hadoop kümesi için kullanılacak depolama hesabı bölge ile aynı olması gerekir. Aksi takdirde, dördüncü yapılandırma sayfasında, depolama hesabı aşağı açılan listede görünmez **hesap adı**.
> 
> 

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/customize-cluster-img3.png)

Yapılandırma sayfasında 3, Hdınsight Hadoop kümesi için bir kullanıcı adı ve parola sağlayın. **Sağlamadığı** seçin *Hive/Oozie meta depo girin*. Ardından, sonraki yapılandırma sayfasına gitmek için oka tıklayın. 

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/customize-cluster-img4.png)

Yapılandırma sayfasında 4 varsayılan kapsayıcı Hdınsight Hadoop kümesi depolama hesabı adı belirtin. Seçerseniz *oluşturma varsayılan kapsayıcı* içinde **varsayılan KAPSAYICI** açılır listesinde, aynı ada sahip bir kapsayıcı küme oluşturulacak şekilde. Son yapılandırma sayfasına gitmek için oka tıklayın.

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/customize-cluster-img5.png)

En son üzerinde **betik eylemleri** yapılandırma sayfasında, tıklatın **betik eylemi eklemek** düğmesine tıklayın ve metin alanları şu değerlerle doldurun.

* **AD** -bu betik eylem adı olarak herhangi bir dize
* **DÜĞÜM türü** - seçin **tüm düğümler**
* **BETİK URI'si** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* depolama hesabındaki ortak bir kapsayıcı 
  * *getgoing* azure'da kullanıcıların çalışmalarını kolaylaştırmak için PowerShell komut dosyaları paylaşmak için kullanırız
* **PARAMETRELERİ** -(boş bırakın)

Son olarak, özelleştirilmiş Hdınsight Hadoop kümesi oluşturmayı başlatmak için onay işaretine tıklayın. 

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Hadoop küme baş düğümüne erişin
RDP Hadoop küme baş düğümüne erişmeden önce Azure Hadoop kümesine uzaktan erişimi etkinleştirmeniz gerekir. 

1. Oturum [ **Klasik Azure portalı**](https://manage.windowsazure.com/)seçin **Hdınsight** sol tarafta, Hadoop kümesi kümeleri listesinden seçin, **yapılandırma** sekmesini ve ardından **etkinleştirmek uzak** sayfanın simgesine tıklayın.
   
    ![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/enable-remote-access-1.png)
2. İçinde **Uzak Masaüstü'nü yapılandırmak** penceresinde, kullanıcı adı ve parola alanlarına girin ve Uzaktan erişim sona erme tarihini seçin. Ardından Hadoop kümenin baş düğüm için uzaktan erişimi etkinleştirmek için onay işaretine tıklayın.
   
    ![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> Kullanıcı adı ve parola uzaktan erişim için kullanıcı adı ve Hadoop küme oluştururken kullandığınız parolayı değildir. Bu kimlik bilgilerini ayrı bir kümesidir. Ayrıca, uzaktan erişim, sona erme tarihi geçerli tarihten itibaren 7 gün içinde olması gerekir.
> 
> 

Uzaktan erişim etkinleştirildikten sonra tıklatın **BAĞLAN** baş düğüm içine uzak sayfanın sonundaki. Hadoop kümesi baş düğüm için daha önce belirtilen uzaktan erişim kullanıcı için kimlik bilgileri girerek oturum açın.

![Çalışma alanı oluşturma](./media/customize-hadoop-cluster/enable-remote-access-3.png)

Sonraki adımlar gelişmiş analizler işlemindeki içinde eşlenmiş [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ve Hdınsight'a, veri taşıma sonra işleyen ve onu var. Azure Machine Learning ile verileri öğrenme hazırlığı örnek adımlar içerebilir.

Bkz: [Hive sorguları göndermek nasıl](move-hive-tables.md#submit) içinde Anaconda kümesinde depolanan Hive kayıtlarını işlemek için kullanılan kullanıcı tanımlı işlevler (UDF'ler) kümenin baş düğümünden dahil edilen Python modülleri erişmek yönergeler için.

