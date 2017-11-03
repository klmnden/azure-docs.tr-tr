---
title: ".NET 2.9 için Azure SDK sürüm notları"
description: ".NET 2.9 için Azure SDK sürüm notları"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 199f0906f73d693d7cd4b73c928f23ae83b99596
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a>.NET 2.9 için Azure SDK sürüm notları

Bu konu için .NET 2.9 ve Azure SDK'sının 2.9.6 sürümleri için sürüm notları içerir.

##<a name="azure-sdk-for-net-296-release-summary"></a>.NET 2.9.6 için Azure SDK sürüm özeti

Yayın Tarihi: 11/16/2016
 
Bu sürümde hiçbir önemli değişiklikler için Azure SDK 2.9 tanıtılmıştır. Bu SDK mevcut bulut hizmeti projeleri ile yararlanmak için gereken yükseltme hiçbir işlem yok.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Sürüm Adayı

- Visual Studio 2017 RC'de bu Azure SDK'sı sürüm .NET için Azure yükündeki yerleşik olarak bulunur. Azure geliştirme yapmak için ihtiyacınız olan araçları, Visual Studio 2017 RC ileriye dönük bir parçası olur. Visual Studio 2015 ve Visual Studio 2013 için SDK Webpı kullanılabilir olmaya devam eder. Visual Studio 2017 son bir ürün olarak bıraktığında biz Azure SDK'sı .NET sürümleri için Visual Studio 2013 için sona erdirme. Visual Studio 2017 RC indirmek için bu bağlantıyı izleyin: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Tanılama

- Yalnızca bulut Hizmetleri tanılama depolama bağlantı dizesi için bir belirteç değiştirilmiştir anahtarla bir kısmi bağlantı dizesi depolamak için kullanılacak davranışı değişti. Kendi erişim denetlenebilir için gerçek depolama anahtarı artık kullanıcı profili klasöründe depolanır. Visual Studio yerel hata ayıklama ve yayımlama işlemi için kullanıcı profili klasöründen depolama anahtarı okur. 
- Kullanıcıların Azure'da sürekli tümleştirme ve dağıtım için yayımlarken tanılama uzantısını ayarlamak için depolama anahtarı belirtebilirsiniz şekilde yukarıda açıklanan değişikliğe yanıt olarak, Visual Studio Online ekibi Azure Cloud Services Dağıtımı görev şablonu geliştirilmiştir.
- Bu güvenli bağlantı dizesini ve simgeleştirme için Azure tanılama (arasında environements yapılandırmasındaki sorunları gidermenize yardımcı olacak WAD), depolanmasını mümkün yaptık.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 sanal makineler

- Visual Studio artık işletim sistemi ailesi 5 (Windows Server 2016) sanal makineler için bulut Hizmetleri dağıtma destekler. Mevcut bulut hizmetlerini yeni işletim sistemi ailesi hedeflemek için ayarlarınızı değiştirebilirsiniz. .Net 4.6 ya da daha yüksek kullanarak hizmet oluşturmayı seçerseniz yeni bulut Hizmetleri, oluştururken, varsayılan olarak işletim sistemi ailesi 5 kullanmak için hizmet alır.  Daha fazla bilgi için gözden geçirebilirsiniz [konuk işletim sistemi ailesi destek tablo](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Bilinen sorunlar

- Azure .NET SDK 2.9.6 sunulan tüm işletim sistemi ailesi desteklenmeyen .NET Framework (örneğin, .NET 4.6) kullanarak projeleri dağıtılmasını engelleyen bir kısıtlama < 5. Geçici bir çözüm sağlanmaktadır [burada](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure rol içi önbellek 

- Azure rol içi önbelleği uçları 30 Kasım 2016 desteği. Daha fazla ayrıntı için tıklatın [burada](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure yığını için Azure Resource Manager şablonları

- Azure Resource Manager şablonlarınızı dağıtım hedefi olarak biz Azure yığın ekledik.


## <a name="azure-sdk-for-net-29-summary"></a>.NET 2.9 özeti için Azure SDK'sı

## <a name="overview"></a>Genel Bakış
Bu belge, .NET 2.9 yayın için Azure SDK'sı için sürüm notlarını içermektedir. 

Bu sürümde güncelleştirmeler hakkında ayrıntılı bilgi için bkz: [Azure SDK 2.9 duyuru post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Visual Studio 2015 güncelleştirme 2 ve Visual Studio "15" için Azure SDK 2.9 Önizleme
Bu güncelleştirme aşağıdaki hata düzeltmeleri içerir:

* REST API İstemci oluşturma ile ilgili sorun "Bilinmeyen türü" dize kod gen klasörün adını ve/veya ad alanı görüneceği oluşturulan koda bırakıldı.
* Hangi kimlik doğrulama bilgilerini sağlama işlemi Zamanlayıcı geçirilmesi başarısız Web işleri zamanlanmış ilgili bir sorun.

Bu güncelleştirme aşağıdaki yeni özellik içerir:

* Uygulama hizmeti sağlama iletişim "Hizmetler" sekmesinde ikincil uygulama hizmetleri için destek. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Visual Studio 2015 güncelleştirme 2 için Azure Data Lake araçları
Bu güncelleştirmeler şunları içerir:

* **Azure Data Lake Araçları** için Visual Studio şimdi .NET sürüm için Azure SDK'sı içine birleştirilir. Azure SDK'yı yüklediğinizde aracı otomatik olarak yüklenir. 
  
    Aracı sık sık güncelleştirilen, Git [burada](http://aka.ms/datalaketool) güncelleştirmeleri almak için.
* **Sunucu Gezgini** şimdi tüm görüntülemek ve bazı U-SQL meta veri varlıklarını oluşturmanıza olanak sağlar. Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.

## <a name="hdinsight-tools"></a>Hdınsight araçları
**Hdınsight Araçları** destekler Hdınsight sürüm 3.3 Tez grafiklerinde ve başka bir dilde gösteren dahil olmak üzere, düzeltmeleri şimdi Visual Studio için.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Bu sürüm ekler [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) için Resource Manager şablonları destekler.

## <a name="see-also"></a>Ayrıca bkz.
[Azure SDK 2.9 duyuru post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

