---
title: .NET 2.8 için Azure SDK sürüm notları
description: .NET 2.8 için Azure SDK sürüm notları
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 6aa2684a900dffecd481d51b8876b0e674c1a6ea
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235539"
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>.NET 2.8 ve 2.8.1 2.8.2 için Azure SDK
## <a name="overview"></a>Genel Bakış
.NET 2.8 2.8.1 ve 2.8.2 sürümleri için bu makale için Azure SDK'sı (bilinen sorunlar ve önemli değişiklikler dahil) yayın notları içerir. 

Yeni özellikler ve bu sürümde yapılan güncelleştirmelerin tam listesi için bkz: [Visual Studio 2013 ve Visual Studio 2015 için Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) duyuru. 

## <a name="azure-sdk-for-net-28"></a>.NET 2.8 için Azure SDK
### <a name="download-azure-sdk-for-net-28"></a>.NET 2.8 için Azure SDK'sını indirin
[Visual Studio 2015 için .NET 2.8 için Azure SDK](https://go.microsoft.com/fwlink/?LinkId=699285) 

[Visual Studio 2013 için .NET 2.8 için Azure SDK](https://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>.NET 4.5.2'nin desteği
#### <a name="known-issues"></a>Bilinen sorunlar
Azure .NET SDK 2.8 .NET 4.5.2'nin oluşturmanıza imkan tanır bulut hizmet paketleri. Ancak .NET 4.5.2'nin framework konuk işletim sistemi görüntüleri kadar Ocak 2016 konuk işletim sistemi sürüm varsayılan olarak yüklenmez. Bu, .NET 4.5.2'nin önce framework bir ayrı konuk işletim sistemi sürümü – Kasım 2015-02 kullanıma sunulacaktır. Bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](../cloud-services/cloud-services-guestos-update-matrix.md) görüntüsü yayımlandığında izlemek için sayfa.  Kasım 2015-02 görüntü sunulduktan sonra bulut hizmet yapılandırma dosyasının (.cscfg) dosyasını güncelleştirerek bu görüntüyü kullanmayı da tercih edebilirsiniz. Hizmet yapılandırmasında dosya osVersion özniteliği ServiceConfiguration öğesinin dize olarak ayarlanmasına "WA-GUEST-OS-4.26_201511-02". Ardından bu görüntüyü kullanma kabul etmek seçerseniz konuk işletim sistemi için Otomatik Güncelleştirmeler artık alırsınız. OsVersion otomatik güncelleştirmeler almak için ayarlanması gerekir "*" ve .NET 4.5.2'nin yalnızca Ocak 2016'daki Otomatik Güncelleştirmeler aracılığıyla kullanıma sunulacaktır.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Bilinen sorunlar
Sırasında bir **veri fabrikası şablonu** proje oluşturma ilgili örnek veriler, azure power shell betiği başarısız 0.9.8 sonra azure power Shell'i sürüm makinede yüklü olması durumunda olabilir.

Bu tür bir proje başarıyla oluşturmak için yüklemelisiniz [azure power Shell'i sürüm 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager araçları
#### <a name="breaking-changes"></a>Yeni değişiklikler
Azure kaynak grubu projesi tarafından sağlanan PowerShell betiğini bu sürümde, yeni Azure PowerShell cmdlet'leri sürüm 1.0 ile çalışacak şekilde güncelleştirildi.  Bu yeni betik alanından ile Visual Studio SDK 2.8 önceki bir sürümünü kullanırken çalışmaz.  

SDK'ın önceki sürümlerinde oluşturulmuş projeleri betiklerin gelen Visual Studio içinden 2.8 SDK'sı kullanırken çalışmaz.  Tüm betikler, Visual Studio'nun dışında Azure PowerShell cmdlet'lerini uygun sürümü ile çalışmaya devam eder.  

2.8 SDK, Azure PowerShell cmdlet'lerini 1.0 sürümünü gerektirir.  SDK'ın tüm sürümleri, Azure PowerShell cmdlet'lerini 0.9.8 sürümünü gerektirir.  Daha fazla bilgi için [bu](https://go.microsoft.com/fwlink/?LinkID=623011) blogu.

### <a name="web-tools-extensions"></a>Web Araçları uzantıları
#### <a name="known-issues"></a>Bilinen sorunlar
Aşağıdaki sürümünde aşağıdaki bilinen sorunlar ele alınacaktır.

* App Service, Bulut ve Sunucu Gezgini çalışmıyor (Azure Çin veya Azure Stack müşterileri gibi) üretim dışı ortamlar için hareket ilgili. Etkilenen bu alanlarda müşteriler, Azure portalından yayımlama profili indiriliyor Yayımlama özelliğini etkinleştirir. Gelecekteki sürümlerde, Azure Çin ve Stack müşterileri için "Hata ayıklayıcısı ekleme" ve "Akış günlüklerini görüntüle" gibi hareketler onarır. 
* Müşteriler, Doğu ABD dışında bir bölgedeyse, bunlar dağıtmakta için App Insights örneği oluşturma, App Service sırasında hataları görebilirsiniz. Bu senaryolarda, portalda bir App Service oluşturma ve yayımlama profili indiriliyor yayımlama senaryolarıyla etkinleştirir. 

### <a name="azure-hdinsight-tools"></a>Azure HDInsight araçları
#### <a name="new-updates"></a>Yeni güncelleştirmeler
* Hive sorgunuzu HiveServer2 aracılığıyla küme neredeyse hiçbir ek yükü ile yürütün ve gerçek zamanlı iş günlüklerine göz atın.
* Yeni Hive görev yürütme iş daha ayrıntılı incelemek görünümünü kullanarak, diğer ayrıntıları öğrenmek ve olası sorunları tanımlayın.

Bilgi için [Visual Studio 2013 ve Visual Studio 2015 için Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>.NET için Azure SDK 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 ve Visual Studio 2015 için bilinen sorunlar
1. Tetiklenen Web işi için yuvaları yayımlar ve hata Göster ve ayarlanmış bir zamanlama, ancak webjob'ı Azure'a gönderin olmaz. Geçirilmesi gereken bir zamanlanmış işi müşterileri ardından WebJob için bir zamanlama ayarlamak için Azure Portalı'nı kullanabilirsiniz. 
2. Python müşteriler, hata ayıklayıcı sorunlarla karşılaşabilirsiniz. Service ekibi bunun için bir düzeltme sunulmaktadır, ancak müşteri etkilenmeden, lütfen Microsoft forumları veya duyuru blogunu bildirin veya Sürüm Notları Açıklamalar bölümü sağlar. 
3. Belirli bir bölgede (örneğin, Güney Hindistan) müşteriler, uygulama hizmeti sağlama hataları karşılaşırsınız. Portal ile tutarlı budur ve bu sorunla karşılaşan müşteriler bu coğrafi bölgeler yayımlamak için erişim istemek için Azure portalını kullanabilirsiniz. Kullanarak bu bölgelere erişim isteyeceğini sonra Azure portalı sağlama çalışması gerekir. 

## <a name="azure-sdk-for-net-282"></a>2.8.2 .NET için Azure SDK
2.8.2 yüklenmesinden araçları, müşteriler, aşağıdaki sorunla karşılaşabilirsiniz.         

* Windows 10 kullanıyorsanız ve Internet Explorer yüklü olmayan, bir "Internet Explorer bulunamadı" hatası alabilirsiniz.
  Sorunu çözmek için Internet Explorer'ın Windows Bileşenlerini Ekle/Kaldır iletişim kutusunu kullanarak yükleyin.

Bu sorunu görüyorsanız, bu rapor için gülümseme Gönder özelliğini kullanın.

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) gönderin.

## <a name="other-updates"></a>Diğer güncelleştirmeler
Diğer güncelleştirmeler için bkz [Azure SDK 2.8 Duyurusu post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Ayrıca bkz:
[Azure SDK 2.8 Duyurusu sonrası](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Destek ve kullanımdan kaldırma Azure SDK'sı ve bilgi için .NET API'leri](https://msdn.microsoft.com/library/azure/dn479282.aspx)

