---
title: "Microsoft Azure sertifikaları SAP için | Microsoft Docs"
description: "Geçerli yapılandırmaları ve SAP sertifikaları Azure platformunda güncelleştirilmiş listesi."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rclaus
ms.custom: 
ms.openlocfilehash: e4d5c78299903659a18aa9b8d04495e215bcca0d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP sertifikaları ve Microsoft Azure üzerinde çalışan yapılandırmaları

SAP ve Microsoft Karşılıklı avantajlıdır müşterileri için güçlü bir doğruladık birlikte çalışma uzun bir geçmişi vardır. Microsoft Platformu sürekli güncelleştiriyor ve Microsoft Azure emin olmak için yeni sertifika ayrıntılarını SAP için gönderme en iyi SAP iş yüklerini çalıştırmak için bir platformdur. Aşağıdaki tablolarda, desteklenen yapılandırmalar ve sertifikaları büyüyen bir listesi verilmiştir. 

## <a name="sap-hana-certifications"></a>SAP HANA sertifikaları

| SAP Ürünü | Desteklenen İşletim Sistemi | Azure Teklifleri |
| --- | --- | --- |
| SAP HANA Geliştirici sürümü (HANA istemci yazılımı da dahil olmak üzere SQLODBC, yalnızca ODBO Windows, ODBC, JDBC sürücüsü HANA oluşan studio ve HANA veritabanına) |Red Hat Enterprise Linux, SUSE Linux Enterprise | D-serisi VM ailesi |
| HANA One |Red Hat Enterprise Linux, SUSE Linux Enterprise |DS14_v2 (genel kullanıma sunulduktan sonra) |
| SAP HANA S/4 |Red Hat Enterprise Linux, SUSE Linux Enterprise |Denetimli kullanılabilirlik için GS5, SAP HANA azure'da (büyük örnekleri) |
| Suite on HANA, OLTP |Red Hat Enterprise Linux, SUSE Linux Enterprise |SAP HANA (büyük örnekler) azure'da GS5 üretim dışı senaryolarında, tek düğümlü dağıtımlar için |
| BW için HANA Enterprise, OLAP |Red Hat Enterprise Linux, SUSE Linux Enterprise |SAP HANA (büyük örnekler) azure'da GS5 tek düğümlü dağıtımlar için |
| SAP BW/4 HANA |Red Hat Enterprise Linux, SUSE Linux Enterprise |SAP HANA (büyük örnekler) azure'da GS5 tek düğümlü dağıtımlar için |

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver sertifikaları
Microsoft Azure, aşağıdaki SAP ürünleri için Microsoft ve SAP'den tam destek alacak şekilde sertifikaya sahiptir.

| SAP Ürünü | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business Suite Yazılımı |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5 GS1 DS11_v2 DS11 D11 a5 |
| SAP Business All-in-One |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5 GS1 DS11_v2 DS11 D11 a5 |
| SAP BusinessObjects BI |Windows |Yok |A11, D14, DS14, DS15_v2, GS5 GS1 DS11_v2 DS11 D11 a5 |
| SAP NetWeaver |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5 GS1 DS11_v2 DS11 D11 a5 |
