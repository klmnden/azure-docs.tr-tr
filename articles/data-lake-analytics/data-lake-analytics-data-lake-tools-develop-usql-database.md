---
title: Azure Data Lake için U-SQL veritabanı geliştirme için U-SQL veritabanı proje kullanmak | Microsoft Docs
description: Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL veritabanı geliştirmeyi öğrenin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a8099a8c9bcec8ed5a7bb480dae58d2c077b8fe0
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889684"
---
# <a name="use-u-sql-database-project-to-develop-u-sql-database-for-azure-data-lake"></a>Azure Data Lake için U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın

U-SQL veritabanı, yapılandırılmamış veriler üzerinde yapılandırılmış görünümler sağlar, yapılandırılmış verileri tablolarda yönetmek ve yapılandırılmış veri ve özel kod düzenlemek için genel bir meta veri Kataloğu sistemi sağlar. Bu ilgili nesneler gruplandıran kavramı veritabanıdır.

Daha fazla bilgi edinin [U-SQL veritabanı ve veri tanımlama dili (DDL)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

U-SQL veritabanı projesi geliştirmek, yönetmek ve hızlı ve kolay bir şekilde, U-SQL veritabanları dağıtma geliştiricilerin yardımcı olan Visual Studio'da bir proje türüdür.

## <a name="create-a-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

Visual Studio için Azure Data Lake araçları, U-SQL veritabanı projesi sürümü 2.3.3000.0 adlı yeni bir proje şablonu eklendi. U-SQL projesi oluşturmak için Git **Dosya > Yeni > Proje**, U-SQL veritabanı projesi altında bulunabilir **Azure Data Lake > U-SQL düğümü**.

![Visual Studio için Data Lake araçları, u-sql veritabanı projesi oluşturun](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-using-database-project"></a>Veritabanı projesi kullanarak U-SQL veritabanı nesnelerini geliştirin

U-SQL veritabanı projesini sağ tıklayın, **Ekle > Yeni öğe**, desteklenen tüm nesne türlerini Yeni Öğe Ekle Sihirbazı'nda bulunabilir. 

1.  Derleme olmayan nesne için örneğin, tablo değerli işlev, yeni bir U-SQL betiği yeni öğe ekledikten sonra oluşturulur. DDL deyimi Düzenleyicisi'nde bu nesne için geliştirmeye başlayabilirsiniz.
2.  Derleme nesne için derlemeyi kayıt ve .dll ve ek dosyaları dağıtma yardımcı olan kullanıcı dostu bir UI Düzenleyicisi araç sağlar. U-SQL veritabanı projeye bir bütünleştirilmiş kod nesnesinin tanımı eklemek için aşağıdaki adımları izleyin:

1.  UDO/UDAG/UDF için U-SQL veritabanı projesini içeren C# projesi başvuruları ekleyin.

    ![Visual Studio için Data Lake araçları, u-sql veritabanı proje başvurusu Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Visual Studio için Data Lake araçları, u-sql veritabanı proje başvurusu Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Başvurulan derlemenin derleme Tasarım görünümünde, seçin **başvurudan derleme Oluştur** açılır.

    ![Visual Studio için Data Lake araçları başvurudan derleme oluştur](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Ekleme **yönetilen bağımlılıklar** ve **ek dosyalar** varsa. Ek dosyaları eklerken, araç hem de yerel makinenize ve derleme makinesi derlemeleri daha sonra bulabilmeniz emin olmak için göreli yol kullanır. @_DeployTempDirectory araç, yapı çıktı klasörüne işaret önceden tanımlanmış bir değişkendir. Derleme çıkışını her derleme derleme adı, tüm DLL'ler ve ek dosyaları var. alt klasöründe bulunabilir gibi adlı bir alt klasör sahiptir. 
 
## <a name="build-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

U-SQL veritabanı projesi sonekiyle adlı bir U-SQL veritabanı dağıtım paketi için çıkış derleme `.usqldbpack`. `.usqldbpack` Pakettir bir zip dosyası içinde tek bir U-SQL betiği tüm DDL deyimleri içeren **DDL** klasöründe ve tüm DLL'ler ve içindeki derlemeler için ek dosyaları **Temp** klasör.

Daha fazla bilgi edinin [MSBuild komut satırını ve Visual Studio Team Service derleme görevi ile U-SQL veritabanı projesi oluşturmak nasıl](data-lake-analytics-cicd-overview.md#build-u-sql-database-project).

## <a name="deploy-u-sql-database"></a>U-SQL veritabanı dağıtma

Yerel hesap veya Visual Studio veya dağıtım SDK kullanarak Azure Data Lake Analytics hesabı .usqldbpack paket dağıtılabilir. 

### <a name="deploy-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

Bir U-SQL veritabanı bir U-SQL veritabanı projesi veya Visual Studio'da .usqldbpack paketi ile dağıtabilirsiniz.

#### <a name="deploy-through-u-sql-database-project"></a>U-SQL veritabanı projesi dağıtma

1.  U-SQL veritabanı projesini sağ tıklatın ve seçin **Dağıt**.
2.  U-SQL veritabanı Dağıtma Sihirbazı'nda seçin **ADLA hesabı** veritabanını dağıtmak istiyor. (Yerel) hesabının hem ADLA hesabı desteklenir.
3.  **Kaynak veritabanı** projesinin yapı çıkış klasöründe .usqldbpack paketi otomatik olarak işaret içinde doldurulur
4.  Girin **veritabanı adı** bir veritabanı oluşturmak için. Aynı hedef Azure Data Lake Analytics hesabı var olan bir veritabanı varsa, bir veritabanı projesinde tanımlanan tüm nesneler veritabanını Yndn oluşturulur.
5.  Tıklayın **Gönder** U-SQL veritabanı dağıtmak için. Tüm kaynakları (derlemeler ve ek dosyaları) yüklenir ve bir U-SQL işi tümünü içeren DDL deyimleri gönderilir.

    ![Visual Studio için Data Lake araçları, u-sql veritabanı projesi dağıtma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio için Data Lake araçları, u-sql veritabanı projesi Sihirbazı'nı dağıtma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-u-sql-database-deployment-package"></a>U-SQL veritabanı dağıtım paketi dağıtma

1.  Açık **Sunucu Gezgini**, genişletin **Azure Data Lake Analytics hesabı** veritabanını dağıtmak istiyor.
2.  U-SQL veritabanlarını sağ tıklayın ve seçin **veritabanını dağıtma**.
3.  Ayarlama **veritabanı kaynağı** U-SQL veritabanı dağıtım paketi (.usqldbpack dosyası) yolu.
4.  Girin **veritabanı adı** bir veritabanı oluşturmak için. Aynı hedef Azure Data Lake Analytics hesabı var olan bir veritabanı varsa, bir veritabanı projesinde tanımlanan tüm nesneleri veritabanını Yndn oluşturulur.

    ![Visual Studio için Data Lake araçları, u-sql veritabanı paketi dağıtma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Visual Studio için Data Lake araçları, u-sql veritabanı paket Sihirbazı'nı dağıtma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-using-sdk"></a>SDK'sını kullanarak U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe` programlama ve U-SQL veritabanları dağıtma için komut satırı arabirimi sağlar. SDK'sı dahil [U-SQL SDK'sı Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), adresindeki bulmayla `build/runtime/PackageDeploymentTool.exe`.

[SDK ve U-SQL veritabanı dağıtımı için CI/CD işlem hattı ayarlama hakkında daha fazla bilgi](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>U-SQL veritabanı projesi başvurusu

U-SQL projesi U-SQL veritabanı projesi başvurabilirsiniz. Başvuru iki iş yükünden etkiler:

- Proje derleme: U-SQL betikleri oluşturmadan önce başvurulan veritabanı ortamları ayarlanır. 
- Yerel (yerel proje) hesaplarında: başvurulan veritabanı ortamları U-SQL betiği yürütme önce hesap (yerel proje) dağıtılır. [Yerel çalıştırma ve (yerel makine) arasındaki fark hakkında daha fazla bilgi edinin ve burada (yerel proje) hesabı](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-u-sql-database-reference"></a>U-SQL veritabanı başvurusu ekleme

1. U-SQL projeye sağ **Çözüm Gezgini**ve **U-SQL veritabanı Başvurusu Ekle...** .

    ![Visual Studio için Data Lake araçları, veritabanı proje başvurusu Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. U-SQL veritabanı projesi veritabanı başvurusu geçerli çözümü veya bir U-SQL veritabanı paket dosyası yapılandırın.
3. Veritabanı için bir ad sağlayın.

    ![Visual Studio için Data Lake araçları, veritabanı proje başvurusu Sihirbazı Ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Data Lake Analytics için CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
