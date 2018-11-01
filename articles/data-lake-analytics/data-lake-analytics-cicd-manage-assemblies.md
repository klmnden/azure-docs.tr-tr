---
title: CI/CD işlem hattı için Azure Data Lake U-SQL derlemelerde yönetmenin en iyi uygulama
description: U-SQL yönetmenin en iyi bilgi C# derlemeleri CI/CD işlem hattı, Azure DevOps ile.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634608"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>U-SQL derlemeleri CI/CD işlem hattında yönetmenin en iyi uygulama

Bu makalede, U-SQL bütünleştirilmiş kod kaynak kodunu yeni sunulan U-SQL veritabanı projesi ile yönetmeyi öğrenin. Ayrıca sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı için Azure DevOps kullanarak derleme kaydı ayarlamak öğrenebilirsiniz.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>U-SQL veritabanı projesi derleme kaynak kodu yönetmek için kullanın

[U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md) geliştirmek, yönetmek ve U-SQL veritabanlarını hızla ve kolayca dağıtma olanağı tanır Visual Studio'da bir proje türü. (Kimlik) hariç tüm bir U-SQL veritabanı nesnesi, U-SQL veritabanı projesi ile yönetilebilir. 

Önerilen yöntem yönetme C# derleme kaynak kodu ve bütünleştirilmiş kod kaydı DDL U-SQL betikleri gelir:

* Kullanım **U-SQL veritabanı projesi** derleme kaydı U-SQL betikleri yönetme.
* Kullanım **sınıf kitaplığı (U-SQL uygulaması için)** yönetmek için C# kaynak kodu ve bağımlılıkları için kullanıcı tanımlı işleçler, İşlevler ve toplayıcılardan verileri (Udo'lar/UDF/UDAGs).
* Sınıf kitaplığı projesine başvurmak için U-SQL veritabanı projesini kullanın. 

U-SQL veritabanı projesi, bir sınıf kitaplığı (U-SQL uygulaması için) proje başvuruda bulunabilir. Başvurulan'nı kullanarak U-SQL veritabanı'nda kayıtlı derlemeleri oluşturulabilir C# kaynak kodunun bu sınıf kitaplığı (U-SQL uygulaması için) projesi.

Projeleri oluşturma ve başvuru eklerken aşağıdaki adımları izleyebilirsiniz:
1. Bir sınıf kitaplığı (U-SQL uygulaması için) projesi yoluyla oluşturma **Dosya > Yeni > Proje**. Proje altındadır **Azure Data Lake > U-SQL** düğümü.
   ![Data Lake araçları Visual Studio için--oluşturma C# sınıf kitaplığı projesi](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. Kullanıcı tarafından tanımlanan ekleme C# (U-SQL uygulaması için) sınıf kitaplığı projesinde kod. 
3. Aracılığıyla bir U-SQL projesi oluşturmak **Dosya > Yeni > Proje**. Proje altındadır **Azure Data Lake > U-SQL** düğümü.
   ![Visual Studio--oluşturma U-SQL veritabanı projesi için Data Lake araçları](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. Başvuru ekleme C# U-SQL veritabanı projesi için sınıf kitaplığı projesi.

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake araçları Visual Studio – U-SQL eklemek için veritabanı proje başvurusu](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. Bir bütünleştirilmiş kod ile U-SQL veritabanı projesi oluşturma **projeye sağ tıklayın > Yeni Öğe Ekle**.
   ![Data Lake araçları Visual Studio için--bütünleştirilmiş kod Ekle](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. Açık bütünleştirilmiş kod derleme Tasarım görünümünde, seçin başvurulan bütünleştirilmiş koddan **başvurudan derleme Oluştur** açılan menüsü.

    ![--Visual Studio için Data Lake araçları başvurudan derleme oluştur](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Ekleme **yönetilen bağımlılıklar** ve **ek dosyalar** varsa. Ek dosyalar eklediğinizde, araç göreli yol derlemeleri yerel makinenizde hem yapı makinesinde daha sonra bulabilmeniz emin olmak için kullanır. 

**@_DeployTempDirectory** düzenleyicide penceresinin altındaki araç için derleme çıktısı klasörü işaret eden önceden tanımlanmış bir değişkendir. Derleme çıktısı klasörü her derleme ile derleme adı adlı bir alt sahiptir. Tüm DLL'ler ve ek dosyalar bu alt klasörde var. 

## <a name="build-a-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

U-SQL veritabanı projesi sonekiyle adlı bir U-SQL veritabanı dağıtım paketi için çıkış derleme `.usqldbpack`. `.usqldbpack` Pakettir tüm DDL deyimleri içinde tek bir U-SQL betiği içeren bir .zip dosyası **DDL** klasörünü ve tüm yerleşik DLL'ler ve derlemeler için ek dosyalar **Temp** klasör.

## <a name="deploy-a-u-sql-database"></a>U-SQL veritabanı dağıtma

`.usqldbpack` Paket dağıtılabilir yerel bir hesap veya bir Azure Data Lake Analytics hesabı için Visual Studio veya dağıtım SDK'sını kullanarak. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

U-SQL veritabanı projesi ile bir U-SQL veritabanını dağıtabilirsiniz veya bir `.usqldbpack` Visual Studio'daki paket.

#### <a name="deploy-through-a-u-sql-database-project"></a>U-SQL veritabanı projesi dağıtma

1.  U-SQL veritabanı projesini sağ tıklayın ve ardından **Dağıt**.
2.  İçinde **U-SQL veritabanı Dağıtma Sihirbazı'nı**seçin **ADLA hesabı** veritabanını dağıtmak istediğiniz. Hem yerel hesaplar hem de ADLA hesapları desteklenir.
3.  **Kaynak veritabanı** otomatik olarak doldurulur ve projenin .usqldbpack paket noktalarına çıkış klasörü oluşturun.
4.  Bir ad girin **veritabanı adı** bir veritabanı oluşturmak için. Hedef Azure Data Lake Analytics hesabı, aynı ada sahip bir veritabanı zaten varsa veritabanını Yndn veritabanı projede tanımlanan tüm nesneleri oluşturulur.
5.  U-SQL veritabanı dağıtmak için seçebileceğiniz **Gönder**. Tüm kaynakları (derlemeler ve ek dosyaları) yüklenir ve tüm DDL deyimleri içeren bir U-SQL iş gönderilir.

    ![Visual Studio--dağıtma U-SQL veritabanı projesi için Data Lake araçları](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio--dağıtma U-SQL veritabanı projesi Sihirbazı için Data Lake araçları](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Azure DevOps, U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe` programlama ve U-SQL veritabanları dağıtma için komut satırı arabirimi sağlar. SDK'sı dahil [U-SQL SDK'sı Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)konumunda bulunan `build/runtime/PackageDeploymentTool.exe`.

Azure DevOps, U-SQL veritabanı yenileme için Otomasyon ardışık düzenini ayarlamak için komut satırını görev ve bu SDK'sını kullanabilirsiniz. [SDK ve U-SQL veritabanı dağıtımı için CI/CD işlem hattı ayarlama hakkında daha fazla bilgi](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
* [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
* [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
