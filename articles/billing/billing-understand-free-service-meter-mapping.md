---
title: Ücretsiz Azure hesabı - eşleme ölçer hizmetine | Microsoft Docs
description: Ölçer eşleştirme ücretsiz hesapla dahil Hizmetleri için hizmet anlayın.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: amberb
ms.openlocfilehash: 683a94f25e94faf0eee7c6aa5fbae52132d58f34
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="understand-free-service-to-meter-mapping"></a>Ölçer eşleme için ücretsiz hizmeti anlama

Her Azure hizmeti kullanım kullanıcılar Hizmetleri için kaydedilecek Azure faturalama sistemi yararlanan ölçümler karşı yayar. Ücretsiz Hizmetler kullanımını daha iyi anlamak için bu hizmetler için ölçüm eşleme hizmete bakalım. Ücretsiz Hizmetler oluşturmayı öğrenmek için bkz: [Ücretsiz Hizmetler ile ücretsiz Azure hesabına oluşturma](billing-create-free-services-included-free-account.md).

## <a name="service-to-meter-mapping-for-free-account-eligible-services"></a>Ücretsiz hesap uygun hizmetlerin eşleme ölçer hizmetine 

|    Hizmet   | Azure Portal'da ölçüm adı | Kullanım dosya/API'si ölçüm adı | Ölçüm Kimliği |
| ------------ | -------------------------- | -------------------------| -------- |
| B1S Linux VM | Saatleri - Standard_B1 işlem VM | İşlem saatleri - ücretsiz | 8260cba2-4437-47d1-a31e-2561cd370f50
| B1S Windows VM | Saatleri - Standard_B1 işlem VM (Windows) | İşlem saatleri - ücretsiz | ff3e6fa5-EE46-478e-8d0e-b629f4f8a8ac
| B1S VM - genel IP adresleri  | IP adres saatleri - genel IP adresleri | IP adres saatleri - ücretsiz | ae56b367-2708-4454-a3d9-2be7b2364ea1
| CosmosDB | Depolama (GB) - Cosmos DB | Depolama (GB) - ücretsiz | 59c78b09-08e2-466a-9f3b-57a94c9e2f31
| CosmosDB | 100 istek birimleri (saatler) - Cosmos DB | 100 istek birimleri (saatler) - ücretsiz | 5d638a6f-e221-41cf-ae3f-0f81d368cef6 
| Dosya Depolama | Standart GÇ - dosya (GB) - yerel olarak yedekli | Standart GÇ - dosya (GB) - ücretsiz | a7f2aa67-b9a2-4593-a413-6ec86d6c8e5b
| Dosya Depolama | Standart GÇ - Dosya Okuma İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya okuma işlemi birimleri (10.000 s) - ücretsiz | 6207404d-3389-4d20-9087-cc078ddc3fd9
| Dosya Depolama | Standart GÇ - Dosya Yazma İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya yazma işlemi birimleri (10.000 s) - ücretsiz | 223d8004-d29a-46cf-b4f4-d2d34b12548b
| Dosya Depolama | Standart GÇ - Dosya Protokol İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya protokol işlemi birimleri (10.000 s) - ücretsiz | a347d8cc-51d1-4A0E-b9eb-76f67566c3f5
| Dosya Depolama | Standart GÇ - Dosya Listeleme İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - Dosya listeleme işlemi birimleri (10.000 s) - ücretsiz | e8ae79ad-c2ab-4D82-b226-dd3c33dfd40c
| Sık erişimli blok blobu depolama | Standart GÇ - sık erişimli blok blobu okuma işlemleri (10,000s içinde) | Standart GÇ - sık erişimli blok blobu okuma işlemleri (10.000 s) - ücretsiz |fd7cfa1e-026e-4be1-871b-1c2386e8902e
| Sık erişimli blok blobu depolama | Standart GÇ - sık erişimli blok blobu (GB) - yerel olarak yedekli | Standart GÇ - sık erişimli blok blobu (GB) - ücretsiz | 67a3a3fd-826f-42c1-8843-bffa14f0da13
| Sık erişimli blok blobu depolama | Standart GÇ - sık erişimli blok blobu yazma işlemleri (10.000 s) | Standart GÇ - sık erişimli blok blobu yazma işlemleri (10.000 s) - ücretsiz | b34bbb76-edce-4c2d-a288-81a2db1fea53
| Sık erişimli blok blobu depolama  | Standart GÇ - sık erişimli blok blobu yazma/listeleme işlemleri (10.000 s) | Standart GÇ - sık erişimli blok blobu yazma/listeleme işlemleri (10.000 s) - ücretsiz | 7e68cf36-1198-4d3b-baa7-86a74c5b3079
| Disk yönetilen *  | Standart yönetilen Disk/anlık görüntüler (GB) - yerel olarak yedekli | Standart yönetilen Disk/anlık görüntüler (GB) - ücretsiz | ad94c237-52a5-4804-ae65-38c5bf85ef42
| Disk yönetilen *  | Standart yönetilen Disk işlemleri (10.000 s) | Standart yönetilen Disk işlemleri (10.000 s) - ücretsiz | 82cc6ea4-0abd-43ac-ACC0-ec34edf0f14c
| Disk yönetilen *  | Premium depolama sayfa blobu/P6 (birim) - yerel olarak yedekli | Premium depolama sayfa blobu/P6 (birim) - ücretsiz | 2b98c168-27CA-4cc1-b509-e887dec87657
| SQL Database | Standart S0 veritabanı gün - SQL veritabanı | Standart S0 veritabanı gün - ücretsiz | dd6b69d3-9be0-4a91-abff-2c58bbcafd1d
| Paylaşılan - bant genişliği ** | Veri Aktarımı Gönderilen (GB) | Veri aktarımı çıkışı (GB) - ücretsiz | 0fc067a1-65d2-46da-b24b-7a9cbe2c69bd

\* Windows sanal makine oluşturursanız ve yönetilen disk seçin, sanal makinenin parçası olarak yönetilen disk ölçer tüketir.

\** Paylaşılan ölçümler birden çok Hizmetleri aracılığıyla tüketilebilir. Örneğin, sanal makineleri ve depolama kullanımı veri aktarım Out(GB) ölçer karşı yayma.





## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
