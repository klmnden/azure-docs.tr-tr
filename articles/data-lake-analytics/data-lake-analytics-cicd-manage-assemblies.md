---
title: Bir CI/CD işlem hattı için Azure Data Lake U-SQL derlemelerde yönetmek için en iyi uygulamalar
description: U-SQL yönetmek için en iyi adımları öğrenin C# Azure DevOps ile CI/CD işlem hattında derlemeler.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 27a873fac8bf2b53ee06780b8a348eaaa5c94e97
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60334276"
---
# <a name="best-practices-for-managing-u-sql-assemblies-in-a-cicd-pipeline"></a>U-SQL derlemeleri CI/CD işlem hattında yönetmek için en iyi uygulamalar

Bu makalede, U-SQL bütünleştirilmiş kod kaynak kodunu yeni çıkan U-SQL veritabanı projesi ile yönetmeyi öğrenin. Ayrıca Azure DevOps kullanarak sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı derleme kayıt için ayarlama konusunda bilgi edinin.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>U-SQL veritabanı projesi derleme kaynak kodu yönetmek için kullanın

[U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md) geliştirmek, yönetmek ve U-SQL veritabanlarını hızla ve kolayca dağıtma olanağı tanır Visual Studio'da bir proje türü. Bütün U-SQL veritabanı nesnelerini (dışında kimlik bilgileri) U-SQL veritabanı projesi ile yönetebilirsiniz. 

Yönetilecek C# derleme kaynak kodu ve bütünleştirilmiş kod kaydı DDL U-SQL betikleri, kullanın:

* U-SQL veritabanı projesi derleme kaydı U-SQL betiklerini yönetme.
* Sınıf kitaplığı (U-SQL yönetmek için uygulama) C# kaynak kodu ve bağımlılıkları için kullanıcı tanımlı işleçler, İşlevler ve toplayıcılardan verileri (Udo'lar, UDF'ler ve UDAGs).
* Sınıf kitaplığı projesine başvuruda bulunmak için U-SQL veritabanı projesi. 

U-SQL veritabanı projesi, bir sınıf kitaplığı (U-SQL uygulaması için) proje başvuruda bulunabilir. Başvurulan'ı kullanarak U-SQL veritabanı'nda kayıtlı derlemeler oluşturabilir C# kaynak kodunun bu sınıf kitaplığı (U-SQL uygulaması için) projesi.

Projeler oluşturmak ve başvuruları eklemek için aşağıdaki adımları izleyin.
1. Seçerek bir sınıf kitaplığı (U-SQL uygulaması için) proje oluşturun **dosya** > **yeni** > **proje**. Proje altındadır **Azure Data Lake > U-SQL** düğümü.

   ![Data Lake araçları Visual Studio için--oluşturma C# sınıf kitaplığı projesi](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
1. Kullanıcı tarafından tanımlanan ekleme C# (U-SQL uygulaması için) sınıf kitaplığı projesinde kod.

1. Seçerek bir U-SQL projesi oluşturmak **dosya** > **yeni** > **proje**. Proje altındadır **Azure Data Lake** > **U-SQL** düğümü.

   ![Visual Studio--oluşturma U-SQL veritabanı projesi için Data Lake araçları](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
1. Bir başvuru ekleyin C# U-SQL veritabanı projesi için sınıf kitaplığı projesi.

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)

5. Projeye sağ tıklayarak ve seçerek bir bütünleştirilmiş kod U-SQL veritabanı projesi oluşturma **Yeni Öğe Ekle**.

   ![Data Lake araçları Visual Studio için--bütünleştirilmiş kod Ekle](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)

1. Bütünleştirilmiş kod derleme Tasarım Görünümü'nde açın. Başvurulan bütünleştirilmiş koddan seçin **başvurudan derleme Oluştur** açılan menüsü.

    ![--Visual Studio için Data Lake araçları başvurudan derleme oluştur](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Ekleme **yönetilen bağımlılıklar** ve **ek dosyalar**, varsa. Ek dosyalar eklediğinizde, araç göreli yol derlemeleri yerel makinenizde ve yapı makinesinde daha sonra bulabilmeniz emin olmak için kullanır.

**\@_DeployTempDirectory** düzenleyicide penceresinin altındaki araç için derleme çıktısı klasörü işaret eden bir önceden tanımlanmış değişkendir. Derleme çıktısı klasörü her derleme ile derleme adı adlı bir alt sahiptir. Tüm DLL'ler ve ek dosyalar bu alt klasörde var.

## <a name="build-a-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

U-SQL veritabanı projesi için derleme çıkışı bir U-SQL veritabanı dağıtım paketidir. Soneki ile adlandırılır `.usqldbpack`. `.usqldbpack` Tek bir U-SQL betiği DDL klasöründeki tüm DDL deyimleri içeren bir .zip dosyası bir pakettir. Tüm yerleşik .dll dosyaları ve derlemeleri için ek dosyalar Temp klasörde bulunur.

## <a name="deploy-a-u-sql-database"></a>U-SQL veritabanı dağıtma

`.usqldbpack` Yerel bir hesap veya bir Azure Data Lake Analytics hesabı için paket dağıtılabilir. Visual Studio veya dağıtım SDK'sını kullanın. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

U-SQL veritabanı projesi kullanarak U-SQL veritabanı dağıtabilirsiniz veya bir `.usqldbpack` Visual Studio'daki paket.

#### <a name="deploy-by-using-a-u-sql-database-project"></a>U-SQL veritabanı projesi kullanarak dağıtma

1.  U-SQL veritabanı projesini sağ tıklayın ve ardından **Dağıt**.
2.  İçinde **U-SQL veritabanı dağıtma** seçin **ADLA hesabı** veritabanını dağıtmak istediğiniz. Hem yerel hesaplar hem de ADLA hesapları desteklenir.
3.  **Kaynak veritabanı** otomatik olarak doldurulur. Projenin yapı çıkış klasöründe .usqldbpack pakete işaret eder.
4.  Bir ad girin **veritabanı adı** bir veritabanı oluşturmak için. Hedef Azure Data Lake Analytics hesabı, aynı ada sahip bir veritabanı zaten varsa, veritabanını yeniden oluşturmaya gerek kalmadan bir veritabanı projesinde tanımlanan tüm nesneleri oluşturulur.
5.  U-SQL veritabanı dağıtmak için seçebileceğiniz **Gönder**. Tüm kaynaklar, derlemeleri ve ek dosyaları gibi yüklenir. Tüm DDL deyimleri içeren bir U-SQL iş gönderilir.

    ![Visual Studio--dağıtma U-SQL veritabanı projesi için Data Lake araçları](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio--dağıtma U-SQL veritabanı projesi Sihirbazı için Data Lake araçları](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-a-u-sql-database-in-azure-devops"></a>Azure DevOps bir U-SQL veritabanı dağıtmak

`PackageDeploymentTool.exe` programlama ve U-SQL veritabanları dağıtma için komut satırı arabirimi sağlar. SDK'sı dahil [U-SQL SDK'sı Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)konumunda bulunan `build/runtime/PackageDeploymentTool.exe`.

Azure DevOps, U-SQL veritabanı yenileme için bir Otomasyon işlem hattı kurmak için bir komut satırı görevi ve bu SDK'sını kullanabilirsiniz. [SDK'sı ve U-SQL veritabanı dağıtımı için bir CI/CD işlem hattı ayarlama hakkında daha fazla bilgi edinin](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlayın](data-lake-analytics-cicd-overview.md)
* [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
* [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
