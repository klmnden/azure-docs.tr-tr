---
title: .NET 2.9 için Azure SDK sürüm notları
description: .NET 2.9 için Azure SDK sürüm notları
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 04ee2daaf7b06f8e7bdd8de144a039474551ea11
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227051"
---
# <a name="azure-sdk-for-net-29-release-notes"></a>.NET 2.9 için Azure SDK sürüm notları

Bu konu, .NET 2.9 ve Azure SDK'sının 2.9.6 sürümleri için sürüm notları içerir.

## <a name="azure-sdk-for-net-296-release-summary"></a>2.9.6 .NET için Azure SDK sürüm özeti

Yayın Tarihi: 16/11/2016
 
Azure SDK 2.9 sürümünü bozucu değişiklik yapmadan bu sürümünde tanıtılmıştır. Mevcut bulut hizmeti projeleri bu SDK'sı yararlanmak için gereken yükseltme işlem yoktur.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Sürüm Adayı

- Visual Studio 2017 RC'de .NET için Azure SDK'ın bu sürümüne Azure iş yükü için yerleşik olarak sunulmaktadır. Azure uygulamaları geliştirmek için ihtiyacınız olan tüm araçları, Visual Studio 2017 RC ileriye dönük bir parçası olur. Visual Studio 2015 ve Visual Studio 2013 için SDK Webpı kullanılabilir olacaktır. Visual Studio 2017 son bir ürün yayımlandığında biz Azure SDK'sı .NET sürümleri için Visual Studio 2013 için sona erdirme. Visual Studio 2017 RC indirmek için bu bağlantıyı izleyin: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Tanılama

- Yalnızca bir ağ geçidi tanılama depolama bağlantı dizesi için bir belirteç yerine anahtara sahip bir kısmi bağlantı dizesini depolamak için davranışı değişti. Erişimini denetlenebilir için gerçek depolama anahtarı artık kullanıcı profili klasöründe depolanır. Visual Studio yerel hata ayıklama ve yayımlama işlemi için kullanıcı profili klasöründen depolama anahtarını okur. 
- Yukarıda açıklanan değişikliğe yanıt olarak, kullanıcıların tanılama uzantısı, sürekli tümleştirme ile azure'a yayımlama sırasında ayarlamak için depolama anahtarı belirtebilirsiniz böylece Azure Cloud Services dağıtım görev şablonu Visual Studio Online takım Gelişmiş ve Dağıtım.
- Güvenli bağlantı dizesi ve simgeleştirme için Azure tanılama (ortamlar yapılandırma sorunlarını gidermenize yardımcı olacak WAD), depolanmasını kolaylaştırdık.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 sanal makinelerini

- Visual Studio artık, işletim sistemi ailesi 5 (Windows Server 2016) sanal makineler için bulut hizmetlerini dağıtma destekler. Var olan bulut Hizmetleri için yeni işletim sistemi ailesi hedeflemek için ayarlarını değiştirebilirsiniz. .Net 4.6 veya üzerini kullanan bir hizmet oluşturmak tercih ederseniz yeni bulut Hizmetleri oluştururken, varsayılan olarak işletim sistemi ailesi 5 kullanmak için hizmet olacaktır.  Daha fazla bilgi için gözden geçirebilirsiniz [konuk işletim sistemi ailesi destek tablo](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Bilinen sorunlar

- Azure .NET SDK'sı 2.9.6 sunulan herhangi bir işletim sistemi ailesi için projeleri (örneğin, .NET 4. 6'da) desteklenmeyen bir .NET Framework kullanılarak dağıtılmasını engelleyen bir kısıtlama < 5. Geçici bir çözüm sağlanır [burada](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure rol içi önbellek 

- Azure rol içi önbellek 30 Kasım 2016 tarihinde desteği. Daha fazla bilgi için tıklayın [burada](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure Stack için Azure Resource Manager şablonları

- Azure Resource Manager şablonlarınızı Azure Stack dağıtım hedefi olarak tanıttık.


## <a name="azure-sdk-for-net-29-summary"></a>.NET 2.9 özeti için Azure SDK'sı

## <a name="overview"></a>Genel Bakış
Bu belge, yayın .NET 2.9 için Azure SDK'sı sürüm notlarını içermektedir. 

Bu sürümdeki güncelleştirmelerin hakkında ayrıntılı bilgi için bkz: [Azure SDK 2.9 sürümünü duyuru post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Visual Studio 2015 güncelleştirme 2 ile Visual Studio "15" için Azure SDK 2.9 Önizleme
Bu güncelleştirme, aşağıdaki hata düzeltmeleri içerir:

* REST API İstemci, code-gen klasörünün adı ve/veya oluşturulan koda bırakılan ad alanının adı olarak "Bilinmeyen tür" dize görüneceği oluşturma ile ilgili sorun.
* Zamanlanan Web işleri, kimlik doğrulama bilgilerinin Zamanlayıcı sağlama işlemine geçirilecek başarısız ilgili bir sorun.

Bu güncelleştirme aşağıdaki yeni özellik içerir:

* Uygulama hizmeti sağlama iletişim kutusunun "Hizmetler" sekmesine ikincil uygulama hizmetleri için destek. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Visual Studio 2015 güncelleştirme 2 için Azure Data Lake araçları
Bu güncelleştirme şunları içerir:

* **Azure Data Lake Araçları** için Visual Studio artık .NET sürümü için Azure SDK'sı ile birleştirilir. Aracı, Azure SDK'yı yüklediğinizde otomatik olarak yüklenir. 
  
    Aracı sık sık güncelleştirilir, Git [burada](https://aka.ms/datalaketool) güncelleştirmeleri almak için.
* **Sunucu Gezgini** artık görmek ve bazı U-SQL meta veri varlıklarında oluşturmanıza olanak sağlar. Daha fazla bilgi için [bu](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.

## <a name="hdinsight-tools"></a>HDInsight araçları
**HDInsight Araçları** destekleyen HDInsight sürüm 3.3 Tez grafiklerinde ve başka bir dilde gösteren dahil olmak üzere, düzeltmeleri şimdi Visual Studio için.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Bu sürüm ekler [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) için Resource Manager şablonları destekler.

## <a name="see-also"></a>Ayrıca bkz.
[Azure SDK 2.9 sürümünü duyuru sonrası](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

