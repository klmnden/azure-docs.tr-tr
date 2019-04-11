---
title: Bir Azure Data Lake için U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın
description: Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL veritabanı geliştirmeyi öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 47235fa5676acd8de8a7cc0d969b813837faf0af
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469718"
---
# <a name="use-a-u-sql-database-project-to-develop-a-u-sql-database-for-azure-data-lake"></a>Bir Azure Data Lake için U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın

U-SQL veritabanı, yapılandırılmamış verileri ve tablolarda yönetilen yapılandırılmış veriler üzerinde yapılandırılmış görünümler sağlar. Ayrıca, yapılandırılmış veri ve özel kod düzenlemek için genel bir meta veri Kataloğu sistemi da sağlar. Bu ilgili nesneler gruplandıran kavramı veritabanıdır.

Daha fazla bilgi edinin [U-SQL veritabanı ve veri tanımlama dili (DDL)](/u-sql/data-definition-language-ddl-statements). 

U-SQL veritabanı projesi geliştirmek, yönetmek ve U-SQL veritabanlarını hızla ve kolayca dağıtma olanağı tanır Visual Studio'da bir proje türüdür.

## <a name="create-a-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

Visual Studio için Azure Data Lake araçları, U-SQL veritabanı projesi sürümü 2.3.3000.0 adlı yeni bir proje şablonu eklendi. U-SQL projesi oluşturmak için Seç **Dosya > Yeni > Proje**. U-SQL veritabanı projesi altında bulunabilir **Azure Data Lake > U-SQL düğümü**.

![--Visual Studio için Data Lake araçları, U-SQL veritabanı projesi oluşturma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-by-using-a-database-project"></a>Veritabanı projesi'nı kullanarak U-SQL veritabanı nesnelerini geliştirme

U-SQL veritabanı projesini sağ tıklayın. Select **Ekle > Yeni öğe**. Tüm yeni desteklenen nesne türleri bulabilirsiniz **Yeni Öğe Ekle** Sihirbazı. 

Yeni bir öğe eklendikten sonra bir derleme olmayan nesne (örneğin, bir tablo değerli işlev) için yeni bir U-SQL betiği oluşturuldu. DDL deyimi Düzenleyicisi'nde bu nesne için geliştirmeye başlayabilirsiniz.

Bir derleme nesne için derlemeyi kayıt ve DLL dosyaları ve diğer ek dosyaları dağıtma yardımcı olan kullanıcı dostu bir UI Düzenleyicisi araç sağlar. Aşağıdaki adımlar bir derleme nesne tanımı için U-SQL veritabanı projesi ekleme işlemini gösterir:

1.  UDO/UDAG/UDF için U-SQL veritabanı projesini içeren C# projesi başvuruları ekleyin.

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Başvurulan derlemenin derleme Tasarım görünümünde, seçin **başvurudan derleme Oluştur** açılan menüsü.

    ![--Visual Studio için Data Lake araçları başvurudan derleme oluştur](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Ekleme **yönetilen bağımlılıklar** ve **ek dosyalar** varsa. Ek dosyalar eklediğinizde, araç göreli yol derlemeleri yerel makinenizde hem yapı makinesinde daha sonra bulabilmeniz emin olmak için kullanır. 

@_DeployTempDirectory araç, yapı çıktı klasörüne işaret önceden tanımlanmış bir değişkendir. Derleme çıktısı klasörü her derleme ile derleme adı adlı bir alt sahiptir. Tüm DLL'ler ve ek dosyalar bu alt klasörde var. 
 
## <a name="build-a-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

U-SQL veritabanı projesi sonekiyle adlı bir U-SQL veritabanı dağıtım paketi için çıkış derleme `.usqldbpack`. `.usqldbpack` Pakettir tüm DDL deyimleri içinde tek bir U-SQL betiği içeren bir .zip dosyası **DDL** klasöründe ve tüm DLL'ler ve içindeki derlemeler için ek dosyaları **Temp** klasör.

Daha fazla bilgi edinin [MSBuild ile U-SQL veritabanı projesi oluşturmak için görev komut satırı ve bir Azure DevOps Hizmetleri oluşturmasına nasıl](data-lake-analytics-cicd-overview.md).

## <a name="deploy-a-u-sql-database"></a>U-SQL veritabanı dağıtma

.Usqldbpack paketini yerel bir hesap veya bir Azure Data Lake Analytics hesabı için Visual Studio veya dağıtım SDK kullanılarak dağıtılabilir. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

Bir U-SQL veritabanı bir U-SQL veritabanı projesi veya Visual Studio'da .usqldbpack paketi ile dağıtabilirsiniz.

#### <a name="deploy-through-a-u-sql-database-project"></a>U-SQL veritabanı projesi dağıtma

1.  U-SQL veritabanı projesini sağ tıklayın ve ardından **Dağıt**.
2.  İçinde **U-SQL veritabanı Dağıtma Sihirbazı'nı**seçin **ADLA hesabı** veritabanını dağıtmak istediğiniz. Hem yerel hesaplar hem de ADLA hesapları desteklenir.
3.  **Kaynak veritabanı** otomatik olarak doldurulur ve projenin .usqldbpack paket noktalarına çıkış klasörü oluşturun.
4.  Bir ad girin **veritabanı adı** bir veritabanı oluşturmak için. Hedef Azure Data Lake Analytics hesabı, aynı ada sahip bir veritabanı zaten varsa veritabanını Yndn veritabanı projede tanımlanan tüm nesneleri oluşturulur.
5.  U-SQL veritabanı dağıtmak için seçebileceğiniz **Gönder**. Tüm kaynakları (derlemeler ve ek dosyaları) yüklenir ve tüm DDL deyimleri içeren bir U-SQL iş gönderilir.

    ![Visual Studio--dağıtma U-SQL veritabanı projesi için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio--dağıtma U-SQL veritabanı projesi Sihirbazı için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-a-u-sql-database-deployment-package"></a>U-SQL veritabanı dağıtım paketi dağıtma

1.  Açık **Sunucu Gezgini**. Ardından **Azure Data Lake Analytics hesabı** veritabanını dağıtmak istediğiniz.
2.  Sağ tıklayın **U-SQL veritabanlarını**ve ardından **veritabanını dağıtma**.
3.  Ayarlama **veritabanı kaynağı** U-SQL veritabanı dağıtım paketi (.usqldbpack dosyası) yolu.
4.  Girin **veritabanı adı** bir veritabanı oluşturmak için. Azure Data Lake Analytics hesabı hedefte zaten aynı ada sahip bir veritabanı varsa, bir veritabanı projesinde tanımlanan tüm nesneleri veritabanını Yndn oluşturulur.

    ![Visual Studio--dağıtma U-SQL veritabanı paket için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Visual Studio--dağıtma U-SQL veritabanı paket Sihirbazı için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-by-using-the-sdk"></a>SDK'sını kullanarak U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe` programlama ve U-SQL veritabanları dağıtma için komut satırı arabirimi sağlar. SDK'sı dahil [U-SQL SDK'sı Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)konumunda bulunan `build/runtime/PackageDeploymentTool.exe`.

[SDK ve U-SQL veritabanı dağıtımı için CI/CD işlem hattı ayarlama hakkında daha fazla bilgi](data-lake-analytics-cicd-overview.md).

## <a name="reference-a-u-sql-database-project"></a>U-SQL veritabanı projesi başvurusu

U-SQL projesi U-SQL veritabanı projesi başvurabilirsiniz. Başvuru iki iş yükünden etkiler:

- *Proje derleme*: U-SQL betikleri oluşturmadan önce başvurulan veritabanı ortamları ayarlama. 
- *Yerel çalıştırma (yerel-projesine karşı) hesabı*: Başvurulan veritabanı ortamları (yerel-projeye) dağıtılan önce U-SQL betiği yürütme hesabı. [Yerel çalıştırma ve (yerel makine) arasındaki fark hakkında daha fazla bilgi edinin ve (bir yerel proje) burada hesap](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-a-u-sql-database-reference"></a>U-SQL veritabanı başvurusu ekleme

1. U-SQL projeye sağ **Çözüm Gezgini**ve ardından **U-SQL veritabanı Başvurusu Ekle...** .

    ![--Visual Studio için Data Lake araçları, veritabanı proje başvurusu Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. U-SQL Veritabanı projesinden bir veritabanı başvurusu geçerli çözümde veya bir U-SQL veritabanı paket dosyası yapılandırın.
3. Veritabanı için bir ad sağlayın.

    ![Visual Studio için Data Lake araçları, veritabanı proje başvurusu Sihirbazı Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
