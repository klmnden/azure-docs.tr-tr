---
title: Microsoft Azure sertifikaları SAP için | Microsoft Docs
description: Geçerli yapılandırmaları ve SAP sertifikaları Azure platformunda güncelleştirilmiş listesi.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: 1038f0c8a967c05ebf07849619268f22b1eb0c4b
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP sertifikaları ve Microsoft Azure üzerinde çalışan yapılandırmaları

SAP ve Microsoft Karşılıklı avantajlıdır müşterileri için güçlü bir doğruladık birlikte çalışma uzun bir geçmişi vardır. Microsoft Platformu sürekli güncelleştiriyor ve Microsoft Azure emin olmak için yeni sertifika ayrıntılarını SAP için gönderme en iyi SAP iş yüklerini çalıştırmak için bir platformdur. Aşağıdaki tablolar anahat Azure desteklenen yapılandırmaları ve SAP sertifikalarından büyüyen bir listesi. 

## <a name="sap-hana-certifications"></a>SAP HANA sertifikaları
Başvurular:

- [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) yerel Azure Vm'leri ve HANA büyük örnekleri için SAP HANA desteği.

| SAP Ürünü | Desteklenen İşletim Sistemi | Azure Teklifleri |
| --- | --- | --- |
| SAP HANA Geliştirici sürümü (HANA istemci yazılımı da dahil olmak üzere SQLODBC, yalnızca ODBO Windows, ODBC, JDBC sürücüsü HANA oluşan studio ve HANA veritabanına) | Red Hat Enterprise Linux, SUSE Linux Enterprise | D-serisi VM ailesi |
| İş bir üzerinde HANA | SUSE Linux Enterprise | DS14_v2 |
| SAP HANA S/4 | Red Hat Enterprise Linux, SUSE Linux Enterprise | Denetimli kullanılabilirlik GS5, M64s, M64ms, M128s, M128ms, SAP HANA azure'da (büyük örnekleri) |
| Suite on HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 üretim dışı senaryolarında, M64s, M64ms, M128s, M128ms, SAP HANA azure'da (büyük örnekleri) |
| BW için HANA Enterprise, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA azure'da (büyük örnekleri) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA azure'da (büyük örnekleri) |

Tüm Azure VM'ler için SAP HANA ölçek büyütme kadarki sertifikalı.

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver sertifikaları
Microsoft Azure, aşağıdaki SAP ürünleri için Microsoft ve SAP'den tam destek alacak şekilde sertifikaya sahiptir.
Başvurular:

- [1928533 - Azure üzerinde SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533) SAP TREX, SAP LiveCache ve SAP içerik sunucusu tüm SAP NetWeaver tabanlı uygulamalar için de dahil olmak üzere. Ve SAP HANA hariç tüm veritabanları.


| SAP Ürünü | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business Suite Yazılımı | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5, D64s_v3, E64s_v3, M128ms M64s E2s_v3 D2s_v3 GS1 DS11_v2 DS11 D11 a5 |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5, D64s_v3, E64s_v3, M128ms M64s E2s_v3 D2s_v3 GS1 DS11_v2 DS11 D11 a5 |
| SAP BusinessObjects BI | Windows |Yok |A11, D14, DS14, DS15_v2, GS5, D64s_v3, E64s_v3, M128ms M64s E2s_v3 D2s_v3 GS1 DS11_v2 DS11 D11 a5 |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve yalnızca Oracle Linux), DB2, SAP ana |A11, D14, DS14, DS15_v2, GS5, D64s_v3, E64s_v3, M128ms M64s E2s_v3 D2s_v3 GS1 DS11_v2 DS11 D11 a5 |

## <a name="other-sap-workload-supported-on-azure"></a>Azure üzerinde desteklenen diğer SAP iş yükü

| SAP Ürünü | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business bir üzerinde SQL Server | Windows  | SQL Server | VM türler onaylanmış tüm NetWeaver |
| SAP BPC 10.01 MS SP08 | Windows ve Linux | | Tüm NetWeaver sertifikalı VM türleri<br /> #2451795 SAP notuna göz atın |
| SAP Business nesneleri BI platformu | Windows ve Linux | | #2145537 SAP notuna göz atın |
| Veri Hizmetleri 4.2 SAP | | | #2288344 SAP notuna göz atın |
| SAP Hybris Commerce Platform 5.x ve 6.x | Windows | SQL Server, Oracle | VM türler onaylanmış tüm NetWeaver<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
