---
title: Uç nokta koruma çözümleri bulma ve sistem durumu değerlendirmesi Azure Güvenlik Merkezi'nde | Microsoft Docs
description: Nasıl uç nokta koruma çözümleri bulunan ve Sağlıklı olarak tanımlanır.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: v-mohabe
ms.openlocfilehash: b17e5f16b988bfa562b00bc6f5b9dfd34be4ca43
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247972"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Uç nokta koruma değerlendirmesi ve Azure Güvenlik Merkezi'nde öneriler

Uç nokta koruma değerlendirmesi ve Azure Güvenlik Merkezi'nde öneriler algılar ve sistem durumu değerlendirmesi sağlar [desteklenen](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) uç nokta koruma çözümleri sürümleri. Bu konu, aşağıdaki iki uç nokta koruma çözümleri Azure Güvenlik Merkezi tarafından öneriler oluşturma senaryoları açıklar.

* **Uç nokta koruma çözümleri sanal makinenize yükleyin.**
* **Makinelerinizde Endpoint protection sistem durumu sorunları çözün**

## <a name="windows-defender"></a>Windows Defender

* **"Uç nokta koruma çözümleri sanal makineye Yükle"** öneri oluşturulur, [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştırır ve sonucunu **AMServiceEnabled: False**

* **"Endpoint protection sistem durumu sorunları makinelerinizde Çözümle"** öneri oluşturulur, [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) çalıştırır ve ya da aşağıdaki gerçekleşir:

  * Aşağıdaki özellikler en az biri yanlış:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **IoavProtectionEnabled**

     **OnAccessProtectionEnabled**

  * Bir veya iki aşağıdaki özellikleri ise, 7'ye eşit veya büyük.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center endpoint protection

* **"Uç nokta koruma çözümleri sanal makineye Yükle"** öneri içeri aktarılırken oluşturulan **SCEPMpModule ("$env: ProgramFiles\Microsoft güvenlik Client\MpProvider\MpProvider.psd1")** ve çalışan **Get-MProtComputerStatus** ile sonuçları **AMServiceEnabled = false**

* **"Endpoint protection sistem durumu sorunları makinelerinizde Çözümle"** öneri oluşturulur, **Get-MprotComputerStatus** çalıştırır ve ya da aşağıdaki gerçekleşir:

    * Aşağıdaki özellikler en az biri yanlış:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **IoavProtectionEnabled**
    
       **OnAccessProtectionEnabled**
          
    * Birini veya ikisini de aşağıdaki imza güncelleştirmeler olup olmadığını büyük veya buna eşit 7. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* **"Uç nokta koruma çözümleri sanal makineye Yükle"** aşağıdaki denetimleri birkaçını karşılanmadığı ya da bir öneri oluşturulur:
    * **HKLM:\SOFTWARE\TrendMicro\Deep güvenlik aracı** var.
    * **HKLM:\SOFTWARE\TrendMicro\Deep güvenlik Agent\InstallationFolder** var.
    * **Dsq_query.cmd** dosya yükleme klasöründe bulunamadı
    * Çalışan **dsa_query.cmd** ile sonuçları **Component.AM.mode: on - Trend Micro Deep Security Agent algılandı**

## <a name="symantec-endpoint-protection"></a>Symantec uç nokta koruması
**"Uç nokta koruma çözümleri sanal makineye Yükle"** herhangi birini aşağıdaki denetimleri karşılanmadığı takdirde öneri oluşturulur:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Or

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

**"Endpoint protection sistem durumu sorunları makinelerinizde Çözümle"** herhangi birini aşağıdaki denetimleri karşılanmadığı takdirde öneri oluşturulur:  

* Symantec sürümünü denetleyin > = 12:  Kayıt defteri konumu: **Uç nokta Protection\CurrentVersion HKLM:\Software\Symantec\Symantec"-"PRODUCTVERSION"değeri**

* Gerçek zamanlı koruma durumunu kontrol edin: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* İmza güncelleştirme durumunu kontrol edin: **HKLM\Software\Symantec\Symantec uç nokta Protection\CurrentVersion\public-opstate\LatestVirusDefsDate < = 7 gün**

* Tam tarama durumunu kontrol edin: **HKLM:\Software\Symantec\Symantec uç nokta Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime < = 7 gün**

* İmza sürümü imza sürüm numarasını yolu için Symantec 12 bulun: **Kayıt defteri yolları + "CurrentVersion\SharedDefs"-"SRTSP" değeri** 

* Symantec 14 sürümü imza yolu: **Kayıt defteri yolları + "CurrentVersion\SharedDefs\SDSDefs"-"SRTSP" değeri**

Kayıt defteri yolu:

**"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;** 
 **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>Windows için McAfee uç nokta koruması

**"Uç nokta koruma çözümleri sanal makineye Yükle"** aşağıdaki denetimleri sağlanmadıysa öneri oluşturulur:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** var.

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

**"Endpoint protection sistem durumu sorunları makinelerinizde Çözümle"** aşağıdaki denetimleri sağlanmadıysa öneri oluşturulur:

* McAfee sürümü: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* İmza sürümü bulun: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* İmza tarihi bulun: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 days**

* Tarama tarihi bulun: **HKLM:\Software\McAfee\Endpoint\AV\ODS-değeri "LastFullScanOdsRunTime" > = 7 gün**

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Microsoft Antimalware uzantı günlükleri şu konumda mevcuttur:  
**%SystemDrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware (PaaSAntimalware)\1.5.5.x (sürüm #) veya \CommandExecution.log**

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Veya bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
