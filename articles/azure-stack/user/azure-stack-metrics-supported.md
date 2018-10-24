---
title: Azure Stack'te ölçümleri Azure İzleyici ile desteklenen | Microsoft Docs
description: Azure Stack'te Azure İzleyici için desteklenen ölçümler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: a9849b5c96b38fbfe6fa8ef4a69a1a2d4d6e6f2f
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958083"
---
# <a name="supported-metrics-with-azure-monitor-on-azure-stack"></a>Azure Stack'te Azure İzleyici ile desteklenen ölçümler

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure'dan ölçümlerinizi alabilirsiniz izleyicisinde Azure Stack'te genel Azure ile aynı. Portalda, ölçü oluşturma, REST API'SİNDEN alın veya PowerShell veya CLI ile sorgulama.

Aşağıdaki tablolarda Azure İzleyicisi'nin Azure Stack'te ölçüm işlem hattı ile mevcut olan ölçümler listelenmektedir. Sorgulamak ve bu ölçümlere erişmek için için ihtiyacınız olacak **2018-01-01** API profili api sürümü sürümü. API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| CPU yüzdesi | CPU yüzdesi | Yüzde | Ortalama | Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi | Boyut yok |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Kullanılan kapasite | Bayt | Ortalama | Hesabın kullanılan kapasitesi | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob Kapasitesi | Bayt | Toplam | Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı. | BlobType |
| BLOB sayısı | Blob Sayısı | Sayı | Toplam | Depolama hesabının Blob hizmetindeki Blob sayısı. | BlobType |
| ContainerCount | Blob Kapsayıcı Sayısı | Sayı | Ortalama | Depolama hesabının Blob hizmetindeki kapsayıcı sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Tablo Kapasitesi | Bayt | Ortalama | Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| TableCount | Tablo Sayısı | Sayı | Ortalama | Depolama hesabının Tablo hizmetindeki tablo sayısı. | Boyut yok |
| TableEntityCount | Tablo Varlık Sayısı | Sayı | Ortalama | Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Kuyruk Kapasitesi | Bayt | Ortalama | Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| QueueCount | Kuyruk Sayısı | Sayı | Ortalama | Depolama hesabının Kuyruk hizmetindeki sıra sayısı. | Boyut yok |
| QueueMessageCount | Kuyruk İleti Sayısı | Sayı | Ortalama | Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
