---
title: "Azure Otomasyonu hesap yapılandırmasını doğrulama | Microsoft Docs"
description: "Bu makalede Otomasyon hesabınızın doğru şekilde ayarlandığını onaylama işlemi açıklanmaktadır."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 5bed2616e15a2e5f52e79d0c28159b568e7a1de3
ms.lasthandoff: 04/15/2017

---

# <a name="test-azure-automation-run-as-account-authentication"></a>Azure Otomasyonu Farklı Çalıştır hesabı kimlik doğrulamasını test etme
Bir Otomasyon hesabı başarıyla oluşturulduktan sonra, yeni oluşturduğunuz veya güncelleştirdiğiniz Azure Farklı Çalıştır hesabını kullanarak Azure Resource Manager veya Azure klasik dağıtımında kimlik doğrulamasını başarıyla yapabildiğinizi onaylamak üzere basit bir test gerçekleştirebilirsiniz.    

## <a name="automation-run-as-authentication"></a>Otomasyon Farklı Çalıştır kimlik doğrulaması

1. Azure portalında, daha önce oluşturduğunuz Otomasyon hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-verify-runas-authentication/job-summary-automationtutorialscript.png)<br>
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve kaynak grubunda mevcut olan tüm kaynakların bir listesini döndürdüğünü görmeniz gerekir.
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
10. **İş Özeti**’ni ve karşılık gelen **AzureAutomationTutorialScript** runbook dikey penceresini kapatın.

## <a name="classic-run-as-authentication"></a>Klasik Farklı Çalıştır kimlik doğrulaması
Kaynaklarınızı klasik dağıtım modelinde yönetecekseniz, yeni Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulamasını başarıyla yapabildiğinizi onaylamak üzere aşağıdaki adımları uygulayın.     

1. Azure portalında, daha önce oluşturduğunuz Otomasyon hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureClassicAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br><br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-verify-runas-authentication/job-summary-automationclassictutorialscript.png)<br>  
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve abonelikteki tüm klasik sanal makinelerin bir listesini döndürdüğünü görmeniz gerekir.
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
10. **İş Özeti**’ni ve karşılık gelen **AzureClassicAutomationTutorialScript** runbook dikey penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md).

