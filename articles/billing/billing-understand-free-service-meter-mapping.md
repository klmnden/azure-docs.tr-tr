---
title: Azure ücretsiz hesabı için ölçüm eşleme hizmetine | Microsoft Docs
description: Hizmete ücretsiz hesaba dahil hizmetler için ölçer eşlemesini anlama.
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
ms.author: banders
ms.openlocfilehash: 2468f61c187d9b10ed9fe55ccf76e5d2561d0505
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370739"
---
# <a name="understand-free-service-to-meter-mapping"></a>Ücretsiz hizmet ölçer eşlemesini anlama

Her bir Azure hizmetinde Azure faturalandırma sistem hizmetleri için kullanıcıların kaydedilecek yararlanan ölçümleri karşı kullanım yayar. Ücretsiz hizmetlerin kullanımını daha iyi anlamak için bu hizmetler için ölçüm eşleme hizmete bakalım. Ücretsiz Hizmetler oluşturmayı öğrenmek için bkz: [Ücretsiz Hizmetler ile ücretsiz Azure hesabı oluşturma](billing-create-free-services-included-free-account.md).

## <a name="service-to-meter-mapping-for-free-account-eligible-services"></a>Ücretsiz hesap uygun hizmetlerin eşleme ölçümüne hizmeti 

|    Hizmet   | Azure Portal'daki ölçüm adı | Ölçüm adlarında kullanım dosyası/API'si | Ölçüm Kimliği |
| ------------ | -------------------------- | -------------------------| -------- |
| B1S Linux VM | Standart_b1 saatleri - işlem VM | İşlem saatleri - ücretsiz | 8260cba2-4437-47d1-a31e-2561cd370f50
| B1S Windows VM | Standart_b1 saatleri - işlem VM (Windows) | İşlem saatleri - ücretsiz | ff3e6fa5-ee46-478e-8d0e-b629f4f8a8ac
| B1S VM - genel IP adresleri  | IP adresi saatleri - genel IP adresleri | IP adresi saatleri - ücretsiz | ae56b367-2708-4454-a3d9-2be7b2364ea1
| CosmosDB | Depolama (GB) - Cosmos DB | Depolama (GB) - ücretsiz | 59c78b09-08e2-466a-9f3b-57a94c9e2f31
| CosmosDB | 100 istek birimi (saatler) - Cosmos DB | 100 istek birimi (saatler) - ücretsiz | 5d638a6f-e221-41cf-ae3f-0f81d368cef6 
| Dosya Depolama | Yerel olarak yedekli standart GÇ - dosyalar (GB): | Standart GÇ - dosyalar (GB) - ücretsiz | a7f2aa67-b9a2-4593-a413-6ec86d6c8e5b
| Dosya Depolama | Standart GÇ - Dosya Okuma İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya okuma işlemi birimleri (10,000s) - ücretsiz | 6207404d-3389-4d20-9087-cc078ddc3fd9
| Dosya Depolama | Standart GÇ - Dosya Yazma İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya yazma işlemi birimleri (içinde 10,000s) - ücretsiz | 223d8004-d29a-46cf-b4f4-d2d34b12548b
| Dosya Depolama | Standart GÇ - Dosya Protokol İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - dosya protokol işlemi birimleri (içinde 10,000s) - ücretsiz | a347d8cc-51d1-4A0E-b9eb-76f67566c3f5
| Dosya Depolama | Standart GÇ - Dosya Listeleme İşlemi Birimleri (10.000'lik bloklar) | Standart GÇ - Dosya listeleme işlemi birimleri (içinde 10,000s) - ücretsiz | e8ae79ad-c2ab-4D82-b226-dd3c33dfd40c
| Sık erişimli blok blobu depolama | Standart GÇ - sık erişimli blok blobu okuma işlemleri (10,000s içinde) | Standart GÇ - sık erişimli blok blobu okuma işlemleri (10,000s) - ücretsiz |fd7cfa1e-026e-4be1-871b-1c2386e8902e
| Sık erişimli blok blobu depolama | Yerel olarak yedekli standart GÇ - sık erişimli blok blobu (GB)- | Standart GÇ - sık erişimli blok blobu (GB) - ücretsiz | 67a3a3fd-826f-42c1-8843-bffa14f0da13
| Sık erişimli blok blobu depolama | Standart GÇ - sık erişimli blok blobu yazma işlemleri (10,000s) | Standart GÇ - sık erişimli blok blobu yazma işlemleri (10,000s) - ücretsiz | b34bbb76-edce-4c2d-a288-81a2db1fea53
| Sık erişimli blok blobu depolama  | Standart GÇ - sık erişimli blok blobu yazma/listeleme işlemleri (içinde 10,000s) | Standart GÇ - sık erişimli blok blobu yazma/listeleme işlemleri (içinde 10,000s) - ücretsiz | 7e68cf36-1198-4d3b-baa7-86a74c5b3079
| Yönetilen disk *  | Standart yönetilen Disk/anlık görüntüler (GB) - yerel olarak yedekli | Standart yönetilen Disk/anlık görüntüler (GB) - ücretsiz | ad94c237-52a5-4804-ae65-38c5bf85ef42
| Yönetilen disk *  | Standart yönetilen Disk işlemleri (10,000s) | Standart yönetilen Disk işlemleri (10,000s) - ücretsiz | 82cc6ea4-0abd-43ac-acc0-ec34edf0f14c
| Yönetilen disk *  | Premium depolama sayfa blobu/P6 (birimler) - yerel olarak yedekli | Premium depolama sayfa blobu/P6 (birimler) - ücretsiz | 2b98c168-27ca-4cc1-b509-e887dec87657
| SQL Veritabanı | Standart S0 veritabanı günleri - SQL veritabanı | Standart S0 veritabanı günleri - ücretsiz | dd6b69d3-9be0-4a91-abff-2c58bbcafd1d
| Paylaşılan - bant genişliği ** | Veri Aktarımı Gönderilen (GB) | Veri aktarımı gönderilen (GB) - ücretsiz | 0fc067a1-65d2-46da-b24b-7a9cbe2c69bd

\* Bir Windows sanal makine oluşturun ve yönetilen disk seçin, sanal makinenin bir parçası olarak yönetilen disk ölçüm kullanacaktır.

\** Paylaşılan ölçümleri birden çok hizmet tarafından kullanılabilir. Örneğin, hem sanal makineleri hem de depolama kullanım veri aktarımı Out(GB) ölçüm karşı gösterin.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
