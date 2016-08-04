<properties 
   pageTitle="Azure Portal Etkileşimli öğreticilerini kullanarak Data Lake Analytics ve U-SQL öğrenme | Azure" 
   description="Data Lake Analytics ve U-SQL öğrenmeye hızlı bir başlangıç yapın. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="02/11/2016"
   ms.author="edmaca"/>


# Azure Data Lake Analytics etkileşimli öğreticilerini kullanma

Azure Portal, Data Lake Analytics ile çalışmaya başlamanız için etkileşimli bir öğretici sağlar. Bu makalede, web sitesi günlüklerini çözümlemeye yönelik öğreticinin nasıl tamamlanacağı gösterilmiştir.


>[AZURE.NOTE] Visual Studio'yu kullanarak aynı öğreticide ilerlemek istiyorsanız bkz. [Data Lake Analytics'i kullanarak web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
>Portala daha fazla etkinleşimli öğretici eklenecektir.


Diğer öğreticiler için bkz.

- [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
- [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
- [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md) 

**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Data Lake Analytics hesabı**.  Bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

##Data Lake Analytics hesabı oluşturma 

Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir.

Her Data Lake Analytics hesabı, bir [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) hesabı bağımlılığına sahiptir.  Bu hesap, varsayılan Data Lake Store hesabı olarak adlandırılır.  Data Lake Store hesabını önceden veya Data Lake Analytics hesabınızı oluşturduğunuzda oluşturabilirsiniz. Bu öğreticide, Data Lake Store hesabını Analytics hesabıyla oluşturacaksınız.

**Data Lake Analytics hesabı oluşturmak için**

1. [Azure Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)'da oturum açın.
2. Başlangıç Panosu'nu açmak için, sol üst köşedeki **Microsoft Azure**'a tıklayın.
3. **Market** kutucuğuna tıklayın.  
3. **Her Şey** dikey penceresinde arama kutusuna **Azure Data Lake Analytics** yazın ve **ENTER** tuşuna basın. Listede **Azure Data Lake Analytics**'i göreceksiniz.
4. Listeden **Azure Data Lake Analytics**'e tıklayın.
5. Dikey pencerenin alt kısmındaki **Oluştur**'a tıklayın.
6. Aşağıdakileri yazın veya seçin:

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Ad**: Analytics hesabına bir ad verin.
    - **Data Lake Store**: Her Data Lake Analytics hesabı, bağımlı bir Data Lake Store hesabına sahiptir. Data Lake Analytics hesabı ve bağımlı Data Lake Store hesabı aynı Azure veri merkezinde bulunmalıdır. Yeni bir Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin.
    - **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
    - **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun. Uygulamalar genellikle web uygulaması, veritabanı, veritabanı sunucusu, depolama alanı ve 3. taraf hizmetleri gibi birçok bileşenden oluşur. Azure Resource Manager (ARM), uygulamanızdaki kaynaklarla Azure Kaynak Grubu şeklinde adlandırılan bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir, izleyebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Tüm grup için aktarılmış maliyetleri görüntüleyerek kuruluşunuzun fatura işlemlerine açıklık getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](resource-group-overview.md). 
    - **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin. 
7. **Başlangıç Panosuna Sabitle**'yi seçin. Bu işlem, bu öğreticinin uygulanması için gereklidir.
8. **Oluştur**'a tıklayın. Bu işlem sizi Başlangıç Panosu'na götürür. Ana sayfaya, "Azure Data Lake Analytics'i dağıtma" etiketine sahip yeni bir kutucuk eklenir. Data Lake Analytics hesabının oluşturulması çok kısa sürer. Hesap oluşturulduğunda, portal, hesabı yeni bir dikey pencerede açar.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##Web Sitesi Günlüğü Analizi etkileşimli öğreticisini çalıştırma

**Web Sitesi Log Analytics etkileşimli öğreticisini açmak için**

1. Başlangıç Panosu'nu açmak üzere, Portal'da, soldaki menüden **Microsoft Azure**'a tıklayın.
2. Data Lake Analytics hesabınızla bağlantılı kutucuğa tıklayın.
3. **Temel Bileşenler** çubuğundan **Etkileşimli öğreticileri keşfedin** seçeneğine tıklayın.

    ![Azure Data Lake Analytics etkileşimli öğreticileri](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. "Örnekler ayarlanmadı, şu öğeye tıklayın:" ifadesini içeren turuncu bir uyarı görürseniz örnek verileri varsayılan Data Lake Store hesabına kopyalamak için **Örnek Verileri Kopyala**'ya tıklayın. Etkileşimli öğreticinin çalıştırılması için veri gereklidir.
5. **Etkileşimli Öğreticiler** dikey penceresinden **Web Sitesi Log Analytics**'e tıklayın. Portal, öğreticiyi yeni bir portal dikey penceresinde açar.
5. **1 Giriş** seçeneğine tıklayın ve ardından yönergeleri izleyin

##Ayrıca bkz.

- [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
- [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
- [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md)



<!----HONumber=Jun16_HO2-->


