---
title: "2.8 .NET için Azure SDK sürüm notları"
description: "2.8 .NET için Azure SDK sürüm notları"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 0b9f55d69c824e86245738a082f95fc529583f58
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>2.8, 2.8.1 ile ve 2.8.2 .NET için Azure SDK
## <a name="overview"></a>Genel Bakış
.NET 2.8, 2.8.1 ile ve 2.8.2 yayımları için bu makale (, bilinen sorunlar ve önemli değişiklikler içeren) sürüm notları için Azure SDK'sı içerir. 

Yeni özellikler ve bu sürümde yapılan güncelleştirmeler tam listesi için bkz: [Visual Studio 2013 ve Visual Studio 2015 için Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) duyuru. 

## <a name="azure-sdk-for-net-28"></a>.NET 2.8 için Azure SDK
### <a name="download-azure-sdk-for-net-28"></a>2.8 .NET için Azure SDK'sını indirin
[Visual Studio 2015 için 2.8 .NET için Azure SDK](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Visual Studio 2013 için 2.8 .NET için Azure SDK](http://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>.NET 4.5.2 desteği
#### <a name="known-issues"></a>Bilinen sorunlar
Azure .NET SDK 2.8 .NET 4.5.2 oluşturmanıza imkan tanır bulut hizmet paketleri. Ancak 4.5.2 .NET framework konuk işletim sistemi görüntüleri kadar Ocak 2016 konuk işletim sistemi sürüm varsayılan olarak yüklenmez. Bu, .NET 4.5.2 önce framework bir ayrı konuk işletim sistemi sürümü – Kasım 2015-02 kullanılabilir olacak. Bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](../cloud-services/cloud-services-guestos-update-matrix.md) görüntü yayımlandığında izlemek için sayfa.  Kasım 2015-02 görüntü sunulduktan sonra bulut hizmeti yapılandırma dosyasının (.cscfg) dosyanızı güncelleştirerek, görüntü kullanmayı seçebilirsiniz. Hizmet yapılandırmasında dosya ServiceConfiguration öğesinin osVersion özniteliği dizeye ayarlayın "WA-GUEST-OS-4.26_201511-02". Bu görüntüyü kullanmak için kabul seçerseniz konuk işletim sistemi için Otomatik Güncelleştirmeler artık alırsınız. OsVersion otomatik güncelleştirmeleri almak üzere ayarlanması gerekir "*" ve .NET 4.5.2 yalnızca Ocak 2016 Otomatik Güncelleştirmeler aracılığıyla kullanılabilir olacaktır.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Bilinen sorunlar
Sırasında bir **veri fabrikası şablonu** proje oluşturma ilgili örnek veriler, azure power shell betiği başarısız 0.9.8 sonra azure power shell sürüm makinede yüklü olması durumunda olabilir.

Başarılı bir şekilde bu tür bir proje oluşturmak için yüklemelisiniz [azure power shell sürüm 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Azure Kaynak Yöneticisi Araçları
#### <a name="breaking-changes"></a>Yeni değişiklikler
Azure kaynak grubu projesi tarafından sağlanan PowerShell komut dosyasını yeni Azure PowerShell cmdlet'leri sürüm 1.0 ile çalışmak için bu sürümde güncelleştirildi.  Bu yeni betik gelen Visual Studio ile 2.8 önce SDK sürümünü kullanırken çalışmaz.  

SDK önceki sürümlerinde oluşturulan projeleri betiklerinden gelen Visual Studio içinde 2.8 SDK kullanırken çalışmaz.  Tüm betikler dışında Visual Studio Azure PowerShell cmdlet'lerini uygun sürümü ile çalışmaya devam eder.  

2.8 SDK'sı Azure PowerShell cmdlet'lerini 1.0 sürümünü gerektirir.  Tüm SDK sürümleri Azure PowerShell cmdlet'lerini 0.9.8 sürümünü gerektirir.  Daha fazla bilgi için bkz: [bu](http://go.microsoft.com/fwlink/?LinkID=623011) blogu.

### <a name="web-tools-extensions"></a>Web uzantılarının araçları
#### <a name="known-issues"></a>Bilinen sorunlar
Aşağıdaki sürümünde aşağıdaki bilinen sorunlar ele alınacaktır.

* Uygulama hizmeti ile ilgili Bulut ve Sunucu Gezgini hareketi (gibi Azure Çin ya da Azure yığın Müşteriler) üretim dışı ortamlar için çalışmaz. Etkilenen bu alanlarda müşteriler için Azure portalından yayımlama profili indiriliyor Yayımlama özelliğini etkinleştirir. Gelecek sürümlerinden hareketleri "Hata ayıklayıcı Ekle" ve "Akış günlüklerini görüntüle" gibi Azure Çin ve yığın müşteriler için onarın. 
* Müşteriler, uygulama hizmet Doğu ABD dışındaki bir bölgede, bunlar dağıtımına App Insights örneği olduğunda oluşturma olduğundan sırasında hatalar görebilirsiniz. Bu senaryolarda, bir uygulama hizmeti Portalı'nda oluşturma ve yayımlama profili indiriliyor yayımlama senaryoları etkinleştirir. 

### <a name="azure-hdinsight-tools"></a>Azure Hdınsight araçları
#### <a name="new-updates"></a>Yeni güncelleştirmeler
* Neredeyse hiçbir ek yükü ile HiveServer2 aracılığıyla kümedeki Hive sorgunuzu yürütün ve gerçek zamanlı iş günlükleri bakın.
* Yeni Hive görev yürütme daha derin işinizi derinliklerine görünümünü kullanarak, daha fazla ayrıntı ve olası sorunları.

Bilgi için bkz: [Visual Studio 2013 ve Visual Studio 2015 için Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>.NET için Azure SDK 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 ve Visual Studio 2015 için bilinen sorunlar
1. Tetiklenen WebJob yayımlar yuvaları Göster ve hata ve kümesi bir zamanlama, ancak Azure Web işi gönderir olmaz. Zamanlanmış işi gerekli olan müşteriler sonra Web işi zamanlaması ayarlamak için Azure Portalı'nı kullanabilirsiniz. 
2. Python müşterileri hata ayıklayıcı sorunlarla karşılaşabilirsiniz. Hizmet takım bunun için bir düzeltme sunmak ancak müşteriler etkilenir, lütfen Microsoft forumlarında veya duyuru blogunda bilmek veya Sürüm Notları Açıklamalar bölümüne sağlar. 
3. Uygulama hataları sağlama hizmeti (örneğin, Güney Hindistan) belirli bölgelerdeki müşterilere karşılaşırsınız. Bu portalı ile tutarlı ve bu sorunla karşılaşan müşteriler bu coğrafi bölgeler yayımlamak için erişim istemek için Azure portalını kullanabilirsiniz. Kullanarak bu bölge için erişim isteği sonra Azure portal sağlama çalışması gerekir. 

## <a name="azure-sdk-for-net-282"></a>.NET 2.8.2 için Azure SDK
2.8.2 yüklemesini aşağıdaki araçlar, müşteriler, aşağıdaki sorunla karşılaşabilirsiniz.         

* Windows 10 kullanıyorsanız ve Internet Explorer yüklü olmayan bir "Internet Explorer bulunamadı" hatası alabilirsiniz.
  Sorunu çözmek için Internet Explorer'ın Windows Bileşenlerini Ekle/Kaldır iletişim kutusunu kullanarak yükleyin.

Bu sorunu gözlemlerseniz, bu rapor için gönderme bir gülümseme gönderme özelliğini kullanın.

Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) gönderin.

## <a name="other-updates"></a>Diğer güncelleştirmeler
Diğer güncelleştirmeler için bkz: [Azure SDK 2.8 duyuru post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Ayrıca bkz.
[Azure SDK 2.8 duyuru post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Destek ve .NET ve API'ler için devre dışı bırakma bilgi Azure SDK'sı](https://msdn.microsoft.com/library/azure/dn479282.aspx)

