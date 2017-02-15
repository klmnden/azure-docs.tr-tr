---
title: "Azure portalı Etkileşimli öğreticilerini kullanarak Data Lake Analytics ve U-SQL öğrenme | Microsoft Belgeleri"
description: "Data Lake Analytics ve U-SQL öğrenmeye hızlı bir başlangıç yapın. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 31399d47-e937-4c43-ab0a-6aea8875378d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 194b5d79505afbfd0208f63dd182a0e03227ba69
ms.openlocfilehash: 36677be6bc5599f55f1f15bc145c59033ad20e0a


---
# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Azure Data Lake Analytics etkileşimli öğreticilerini kullanma
Azure portalı, Data Lake Analytics ile çalışmaya başlamanız için etkileşimli bir öğretici sağlar. Bu makalede, web sitesi günlüklerini çözümlemeye yönelik öğreticinin nasıl tamamlanacağı gösterilmiştir.

> [!NOTE]
> Visual Studio'yu kullanarak aynı öğreticide ilerlemek istiyorsanız bkz. [Data Lake Analytics'i kullanarak web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
> Portala daha fazla etkinleşimli öğretici eklenecektir.
> 
> 

Diğer öğreticiler için bkz.

* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md) 

**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Data Lake Analytics hesabı**.  Bkz. [Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma
Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir.

Her Data Lake Analytics hesabı, varsayılan Data Lake Store hesabı olan [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) hesabı bağımlılığına sahiptir.  Bu öğreticide, varsayılan Data Lake Store hesabını Analytics hesabıyla oluşturacaksınız ancak bunu önceden de oluşturabilirsiniz.

**Data Lake Analytics hesabı oluşturmak için**

1. [Azure portalı](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute) üzerinde oturum açın.
2. Başlangıç Panosu'nu açmak için, sol üst köşedeki **Microsoft Azure**'a tıklayın.
3. **Market** kutucuğuna tıklayın.  
4. **Her Şey** dikey penceresinde arama kutusuna **Azure Data Lake Analytics** yazın ve **ENTER** tuşuna basın. Listede **Azure Data Lake Analytics**'i göreceksiniz.
5. Listeden **Azure Data Lake Analytics**'e tıklayın.
6. Dikey pencerenin alt kısmındaki **Oluştur**'a tıklayın.
7. Yazın veya seçin:
   
    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)
   
   * **Ad**: Analytics hesabına bir ad verin.
   * **Data Lake Store**: Her Data Lake Analytics hesabı, bağımlı bir Data Lake Store hesabına sahiptir. Data Lake Analytics hesabı ve bağımlı Data Lake Store hesabı aynı Azure veri merkezinde bulunmalıdır. Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin.
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun. Uygulamalar genellikle web uygulaması, veritabanı, veritabanı sunucusu, depolama alanı ve üçüncü taraf hizmetleri gibi birçok bileşenden oluşur. Azure Resource Manager (ARM), uygulamanızdaki kaynaklarla Azure Kaynak Grubu şeklinde adlandırılan bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza yönelik kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir, izleyebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Tüm grup için aktarılmış maliyetleri görüntüleyerek kuruluşunuzun fatura işlemlerine açıklık getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](../azure-resource-manager/resource-group-overview.md). 
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin. 
8. **Başlangıç Panosuna Sabitle**'yi seçin. Bu işlem, bu öğreticinin uygulanması için gereklidir.
9. **Oluştur**’a tıklayın. Bu işlem sizi Başlangıç Panosu'na götürür. Ana sayfaya, "Azure Data Lake Analytics'i dağıtma" etiketine sahip yeni bir kutucuk eklenir. Data Lake Analytics hesabının oluşturulması çok kısa süren bir işlemdir. Hesap oluşturulduğunda, portal, hesabı yeni bir dikey pencerede açar.
   
    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

## <a name="run-website-log-analysis-interactive-tutorial"></a>Web Sitesi Günlüğü Analizi etkileşimli öğreticisini çalıştırma
**Web Sitesi Log Analytics etkileşimli öğreticisini açmak için**

1. Başlangıç Panosu'nu açmak üzere, Portal'da, soldaki menüden **Microsoft Azure**'a tıklayın.
2. Data Lake Analytics hesabınızla bağlantılı kutucuğa tıklayın.
3. **Temel Bileşenler** çubuğundan **Etkileşimli öğreticileri keşfedin** seçeneğine tıklayın.
   
    ![Azure Data Lake Analytics etkileşimli öğreticileri](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)
4. "Örnekler ayarlanmadı, şu öğeye tıklayın:" ifadesini içeren turuncu bir uyarı görürseniz örnek verileri varsayılan Data Lake Store hesabına kopyalamak için **Örnek Verileri Kopyala**'ya tıklayın. Etkileşimli öğreticinin çalıştırılması için veri gereklidir.
5. **Etkileşimli Öğreticiler** dikey penceresinden **Web Sitesi Log Analytics**'e tıklayın. Portal, öğreticiyi yeni bir portal dikey penceresinde açar.
6. **Giriş** seçeneğine tıklayın ve ardından yönergeleri izleyin

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md)




<!--HONumber=Dec16_HO2-->


