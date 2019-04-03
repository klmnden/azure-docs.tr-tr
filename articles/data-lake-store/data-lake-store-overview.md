---
title: Azure Data Lake depolama Gen1 genel bakış | Microsoft Docs
description: (Daha önce Azure Data Lake Store bilinir) hangi Data Lake depolama Gen1 ve diğer veri depolarına kıyasla sağladığı değeri anlama
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 438eab091fac103b66f0789beca0098b87ee44cd
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885664"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 genel bakış

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake depolama Gen1 bir büyük veri analizi iş yükleri için kuruluş çapında hiper ölçekli depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

> [!TIP]
> Kullanım [Data Lake depolama Gen1 öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) Data Lake depolama Gen1 hizmetini keşfetmeye başlamak için.
> 
> 

Data Lake depolama Gen1 hadoop'tan (HDInsight kümesiyle kullanılabilir) erişilebilir WebHDFS ile uyumlu REST API'lerini kullanarak. Bu, depolanan veriler üzerinde analizi olanaklı kılmak için özel olarak tasarlanmış ve veri analizi senaryolarına yönelik performans için ayarlanmıştır. Sağlandığı şekilde, gerçek kurumsal kullanım vakaları için temel önem taşıyan güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik gibi tüm kurumsal düzeydeki özellikleri içerir.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Data Lake depolama Gen1 anahtar özelliklerinin bazıları aşağıda verilmiştir.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir
Data Lake depolama Gen1, bir Apache Hadoop dosya sistemi Hadoop ekosistemiyle çalışan ve Hadoop dağıtılmış dosya sistemi (HDFS) ile uyumlu değil.  Var olan HDInsight uygulamaları veya WebHDFS API'sini kullanan hizmetler, Data Lake depolama Gen1 ile kolayca tümleştirilebilir. Data Lake depolama Gen1 WebHDFS ile uyumlu REST arabirimi uygulamalar için de sunar.

Data Lake depolama Gen1 içinde depolanan veriler, MapReduce veya Hive gibi Hadoop analitik çerçeveler kullanılarak kolayca çözümlenebilir. Microsoft Azure HDInsight kümeleri sağlanabilir ve Data Lake depolama Gen1 içinde depolanan verileri doğrudan erişim sağlamak için yapılandırılmış.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar
Data Lake depolama Gen1 sınırsız depolama sağlar ve veri analizi için çeşitli depolamak için uygundur. Hesap boyutları, dosya boyutları veya bir veri gölü içinde depolanabilen veri miktarı için herhangi bir sınırlama uygulamaz. Ayrı dosyalar kilobayttan petabayta kadar uzanan boyutlarda olabilir ve bu da herhangi bir tür verinin depolanması için harika bir seçim olmasını sağlar. Birden çok kopya oluşturularak veriler sağlam bir şekilde depolanır ve verilerin data lake içinde depolanmasına yönelik bir süre sınırı mevcut değildir.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır
Data Lake depolama Gen1, büyük ölçekli sorgulanması ve büyük miktarlarda verinin çözümlenmesi için devasa bir işlem hacmi gerektiren analiz sistemlerini çalıştırmak üzere tasarlanmıştır. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal kullanıma hazır: Yüksek oranda kullanılabilir ve güvenli
Data Lake depolama Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır. Kuruluşlar, kendi çözümlerinde, mevcut veri platformu önemli bir parçası Data Lake depolama Gen1 kullanabilirsiniz.

Data Lake depolama Gen1 ayrıca depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tüm Veriler
Data Lake depolama Gen1 depolayabileceğiniz tüm verileri yerel biçiminde, olduğu gibi herhangi bir önceki dönüştürme gerektirmeden. Data Lake depolama Gen1, veriler yüklenmeden önce tanımlanacak bir şema kadar verilerin yorumlanmasını ve şema analiz zamanında tanımlamak için ayrı analitik çerçeveye bırakır gerektirmez. İsteğe bağlı boyut ve biçimlerdeki dosyaların depolamak, yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemek Data Lake depolama Gen1 için mümkün kılar.

Veriler için Data Lake depolama Gen1 kapsayıcılar olan temelde klasörleri ve dosyaları. SDK'ları, Azure Portal'ı ve Azure PowerShell'i kullanarak depolanan veriler üzerinde işlem yaparsınız. Verilerinizi bu arabirimleri kullanarak depoya yerleştirdiğiniz ve ilgili kapsayıcıları kullandığınız sürece, istediğiniz türde veriyi depolayabilirsiniz. Data Lake depolama Gen1, depoladığı verilerin türüne göre veri herhangi bir özel işlem gerçekleştirmez.

## <a name="DataLakeStoreSecurity"></a>Data Lake depolama Gen1 veri güvenliğini sağlama
Data Lake depolama Gen1 kullanan Azure Active Directory kimlik doğrulaması ve erişim için verilerinizi listeleri (ACL) erişimi yönetmek üzere denetler.

| Özellik | Açıklama |
| --- | --- |
| Authentication |Data Lake depolama Gen1, Data Lake depolama Gen1 içinde depolanan tüm veriler için kimlik ve erişim yönetimi için Azure Active Directory (AAD) ile tümleşir. Tümleştirme sonucunda, Data Lake depolama Gen1 çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımını izleme, güvenlik izleme ve uyarı verme, vb. dahil olmak üzere tüm AAD özelliklerinden faydalanır. Data Lake depolama Gen1, REST arabirimi içinde kimlik doğrulaması için OAuth 2.0 protokolünü destekler. Bkz: [Data Lake depolama Gen1 kimlik doğrulaması](data-lakes-store-authentication-using-azure-active-directory.md)|
| Erişim denetimi |Data Lake depolama Gen1 WebHDFS protokolünün kullanıma sunulan POSIX stili izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. ACL'ler Data Lake depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1'deki erişim denetimi](data-lake-store-access-control.md). |
| Şifreleme |Data Lake depolama Gen1 ayrıca hesapta depolanan veriler için şifreleme sağlar. Bir Data Lake depolama Gen1 hesabı oluşturulurken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Daha fazla bilgi için [şifreleme Data Lake depolama Gen1](data-lake-store-encryption.md). Şifreleme tabanlı yapılandırma sağlama konusunda yönergeler için bkz. [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md). |

Data Lake depolama Gen1 veri güvenliğini sağlama hakkında daha fazla öğrenmek ister misiniz? Aşağıdaki bağlantıları izleyin.

* Data Lake depolama Gen1 güvenli verilerde konusunda yönergeler için bkz: [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](data-lake-store-secure-data.md).
* Videoyu mu tercih ediyorsunuz? [Bu videoyu](https://mix.office.com/watch/1q2mgzh9nn5lx) Data Lake depolama Gen1 içinde depolanan verilerin güvenliğini sağlama konusunda.

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ile uyumlu uygulamalar
Data Lake depolama Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir. Bu Data Lake depolama Gen1 veri depolama ihtiyaçlarınız için ideal bir seçenek sağlar. Data Lake depolama Gen1 hem açık kaynak bileşenlerini ve bunun yanı sıra diğer Azure Hizmetleri ile kullanma hakkında hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [uygulamaların ve hizmetlerin Azure Data Lake depolama Gen1 ile uyumlu](data-lake-store-compatible-oss-other-applications.md) Data Lake depolama Gen1 ile birlikte çalışabilen açık kaynak uygulamaların listesi.
* Bkz: [diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md) nasıl Data Lake depolama Gen1 diğer Azure hizmetleriyle geniş bir senaryoları etkinleştirmek için kullanılabileceğini anlamak için.
* Bkz: [Data Lake depolama Gen1 kullanma senaryoları](data-lake-store-data-scenarios.md) veri alma, veri işleme, veri indirme ve veri görselleştirme gibi senaryolarda Data Lake depolama Gen1 kullanmayı öğrenin.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Data Lake depolama Gen1 dosya sistemi nedir (adl: / /)?
Data Lake depolama Gen1 erişilebilir yeni dosya olan AzureDataLakeFilesystem (adl: / /), Hadoop ortamlarında (HDInsight kümesiyle kullanılabilir). adl:// kullanan uygulamalar ve hizmetler, geçerli olarak WebHDFS için kullanılabilir olmayan ek performans iyileştirmelerinden faydalanabilir. Sonuç olarak, Data Lake depolama Gen1, ya da önerilen seçeneğiyle adl kullanmanın en iyi performans esnekliği sağlar: / / veya doğrudan WebHDFS API'yi kullanmaya devam ederek var olan kod güncelleştirin. Azure HDInsight, Data Lake depolama Gen1 üzerinde en iyi performansı sağlamak üzere AzureDataLakeFilesystem tam olarak yararlanır.

Data Lake depolama Gen1 kullanarak verilerinize erişebilirsiniz `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Data Lake depolama Gen1 verilere erişim hakkında daha fazla bilgi için bkz: [depolanan verilerin özelliklerini görüntüleme](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Sonraki adımlar

* [Get başlangıç Azure portalını kullanarak Data Lake depolama Gen1](data-lake-store-get-started-portal.md)
* [Azure Data Lake depolama Gen1 ile çalışmaya başlama .NET SDK'sını kullanma](data-lake-store-get-started-net-sdk.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)