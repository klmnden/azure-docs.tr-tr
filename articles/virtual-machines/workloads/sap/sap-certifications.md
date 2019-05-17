---
title: SAP için Microsoft Azure sertifikaları | Microsoft Docs
description: Geçerli yapılandırmaları Azure platformunda SAP sertifikaları ve güncelleştirilmiş listesi.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: a38d5b44aaca0aa28e7ecd32543d2b0563964b3d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65797455"
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP sertifikaları ve Microsoft Azure üzerinde çalışan yapılandırmaları

SAP ve Microsoft müşterileri için karşılıklı avantaj sahip güçlü bir iş ortaklığı birlikte çalışan uzun bir geçmişe sahiptir. Microsoft Platformu sürekli olarak güncelleştiriliyor ve SAP iş yüklerinizi çalıştırmak en iyi platformu olan Microsoft Azure sağlamak için SAP yeni sertifika ayrıntıları gönderiliyor. Aşağıdaki tablolar anahat Azure desteklenen yapılandırmaları ve SAP sertifikaları büyüyen bir listesi. 

## <a name="sap-hana-certifications"></a>SAP HANA sertifikaları
Başvurular:

- [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) yerel Azure Vm'leri ve HANA büyük örnekler için SAP HANA desteği.

| SAP Ürünü | Desteklenen İşletim Sistemi | Azure Teklifleri |
| --- | --- | --- |
| SAP HANA Developer Edition (HANA istemci yazılımı da dahil olmak üzere anahtardan oluşan SQLODBC, ODBO-Windows yalnızca, ODBC, JDBC sürücüleri, HANA Stüdyo ve HANA veritabanından) | Red Hat Enterprise Linux, SUSE Linux Enterprise | D serisi VM ailesi |
| İş bir on HANA | SUSE Linux Enterprise | DS14_v2, M32ts, M32ls, M64ls, M64s <br /> [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) |
| SAP S/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 için denetlenen kullanılabilirlik. Tam destek M64s M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 <br /> SAP HANA (büyük örnekler) azure'da [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| Suite on HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, SAP HANA (büyük örnekler) azure'da <br /> [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| BW için HANA Enterprise, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> SAP HANA (büyük örnekler) azure'da [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> Azure’da SAP HANA (Büyük örnekler) <br /> [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |

SAP terimi 'Kümeleme' kullandığını unutmayın [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) için 'ölçeklendirme' ve 'Kümeleme' değil yüksek kullanılabilirlik için eş anlamlı olarak

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver sertifikaları
Microsoft Azure, aşağıdaki SAP ürünleri için Microsoft ve SAP'den tam destek alacak şekilde sertifikaya sahiptir.
Başvurular:

- [1928533 - azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533) tüm SAP NetWeaver tabanlı uygulamalar için SAP TREX LiveCache SAP ve SAP içerik sunucusu dahil olmak üzere. Ve SAP HANA hariç, tüm veritabanları.


| SAP Ürünü | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business Suite Yazılımı | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve Oracle Linux), DB2, SAP ASE |A5 ila A11, D11 ila D14, DS11 ila DS14, DS11_v2 DS15_v2 için GS1 ila GS5 D64s_v3 için D2s_v3 E64s_v3, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 E2s_v3 |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve Oracle Linux), DB2, SAP ASE |A5 ila A11, D11 ila D14, DS11 ila DS14, DS11_v2 DS15_v2 için GS1 ila GS5 D64s_v3 için D2s_v3 E64s_v3, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 E2s_v3 |
| SAP BusinessObjects BI | Windows |Yok |A5 ila A11, D11 ila D14, DS11 ila DS14, DS11_v2 DS15_v2 için GS1 ila GS5 D64s_v3 için D2s_v3 E64s_v3, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 E2s_v3 |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows ve Oracle Linux), DB2, SAP ASE |A5 ila A11, D11 ila D14, DS11 ila DS14, DS11_v2 DS15_v2 için GS1 ila GS5 D64s_v3 için D2s_v3 E64s_v3, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2 E2s_v3 |

## <a name="other-sap-workload-supported-on-azure"></a>Azure'da desteklenen diğer SAP iş yükü

| SAP Ürünü | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business bir üzerinde SQL sunucusu | Windows  | SQL Server | Tüm NetWeaver sertifikalı sanal makine türleri<br /> [#928839 SAP notuna göz atın](https://launchpad.support.sap.com/#/notes/928839) |
| SAP BPC 10.01 MS SP08 | Windows ve Linux | | Tüm NetWeaver sertifikalı VM türleri<br /> #2451795 SAP notuna göz atın |
| SAP Business nesneleri BI platformu | Windows ve Linux | | #2145537 SAP notuna göz atın |
| Veri Hizmetleri 4.2 SAP | | | #2288344 SAP notuna göz atın |
| SAP Hybris Commerce Platform 5.x ve 6.x | Windows | SQL Server, Oracle | Tüm NetWeaver sertifikalı sanal makine türleri<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
