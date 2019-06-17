---
title: Tehdit modelleme Aracı sürümleri - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme aracı için sürüm notları belgeleme
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2019
ms.author: jegeib
ms.openlocfilehash: bdf8b701567aaa3a0d9006333557bcec4f312723
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586459"
---
# <a name="threat-modeling-tool-ga-release-71509112---9122018"></a>Tehdit modelleme aracı GA 7.1.50911.2 - 12/9/2018 sürüm

Microsoft tehdit modelleme aracı, desteklenen genel kullanıma (GA) sürümü indirmek kullanılabilir kullanıma sunulacağını açıklamaktan heyecan duyuyoruz. Bu sürüm, önemli gizlilik ve güvenlik güncelleştirmeleri, kararlılık geliştirmeleri yanı sıra hata düzeltmeleri ve özellik güncelleştirmeleri içerir. Mevcut kullanıcıları 2017 Önizleme sürümü, istemci açma sırasında ClickOnce teknolojisi aracılığıyla en son sürümüne güncelleştirmek için istenir. Aracı'nın yeni kullanıcıların [istemci yüklemek için burayı tıklatın](https://aka.ms/threatmodelingtool).

Bu sürümle birlikte, 2017 önizlemesi desteği sona erecektir ve tüm kullanıcıların Önizleme güncelleştirmesi GA sürümü için önerilir. Şirket veya 15 Ekim 2018'den sonra tehdit modelleme aracı için gereken en düşük ClickOnce sürüm ayarlayacağız ve tüm önizleme istemcileri yükseltmek için gerekli.

Microsoft tehdit modelleme aracı edinebileceğiniz 2016 [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=49168), 1 Ekim 2019'kadar yalnızca kritik güvenlik düzeltmeleri için desteklenen kalır.

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="azure-stencil-updates"></a>Azure şablonu güncelleştirmeleri

Ek Azure kalıpları ve kendi ilişkili tehditleri ve risk azaltma işlemleri bu sürümle birlikte sevk ayarlamak şablona eklenmiştir. "Azure uygulama hizmetleri", "Azure veritabanı tekliflerine" ve "Azure depolama." odak alanından önemli değişiklikler yapıldı

![Azure şablonu güncelleştirmeleri](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_azure_stencil_update-300x70.png)

### <a name="onedrive-integration-feature-removed"></a>OneDrive tümleştirme özelliği kaldırıldı

Önizleme "için Onedrive'a Kaydet", "OneDrive açık gelen" ve "Bir bağlantıyı Paylaş" özellikleri kaldırıldı. Kullanıcıların OneDrive'nın Microsoft'un kullanılacak geçebilirler [OneDrive için Windows](https://onedrive.live.com/about/en-us/download/) istemcinin standart dosya aracılığıyla Onedrive'da depolanan dosyalarına erişmek ve iletişim kutuları açın.

## <a name="notable-fixed-bugs-reported-by-customers"></a>Düzeltilen dikkat çeken nokta müşteriler tarafından bildirilen

### <a name="in-tmt-preview-the-tool-crashes-when-using-the-standard-template"></a>Standart şablon kullanırken TMT Önizleme'de, araç kilitleniyor

- Genel bir şablon (örneğin "genel veri akışı") için çizim yüzeyi eklenir ve tehditler oluşturur, aracın kilitlenebilir. Bu sorun düzeltilmiştir.

### <a name="in-tmt-preview-when-i-save-a-report-or-copy-the-threats-the-risk-levels-are-incorrect"></a>Rapor kaydetme veya tehditleri kopyalama TMT önizlemede risk düzeyleri yanlış

- Bir kullanıcı belirli tehditlerin Risk düzeyi değiştirir ve bir raporu kaydeder veya riskleri kopyalar, risk düzeyi "Yüksek" olarak döndürülebilir. Bu sorun düzeltilmiştir.

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

### <a name="users-of-high-resolution-screens-may-experience-small-text-in-the-threat-properties"></a>Kullanıcılar yüksek çözünürlüklü ekranlar, küçük metin tehdit özelliklerinde karşılaşabilir

#### <a name="issue"></a>Sorun

Kullanıcının varsayılan olarak, Windows, okunabilirlik için büyütmek için ayarlanmış yüksek çözünürlüklü bir ekran varsa aracının analiz görünümünde ile küçük metin tehdit "Olası Mitigation(s)" bölümünü görünebilir.

![Yüksek çözünürlüklü ekranlar ile ilgili bilinen sorun](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_screen_resolution-300x153.png)

#### <a name="workaround"></a>Geçici Çözüm

Kullanıcı risk azaltma metnine tıklayın ve bu bölümün büyütmeyi için standart Windows yakınlaştırma denetimi (Crtl fare tekerleği) kullanın.

### <a name="files-in-the-recently-opened-models-section-of-the-main-window-may-fail-to-open"></a>Ana pencereyi "Son açılan modelleri" bölümündeki dosyaları açmak başarısız olabilir

#### <a name="issue"></a>Sorun

Önizleme sürümü "OneDrive açık gelen" özelliği kaldırıldı. "Son açılan Onedrive'a kaydedildiğinde modelleri" olan kullanıcılar, aşağıdaki hatayı alırsınız.

![OneDrive özelliği kaldırıldı](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_save_error-300x131.png)

#### <a name="workaround"></a>Geçici Çözüm

Kullanıcıların OneDrive'nın Microsoft'un kullanılacak geçebilirler [OneDrive için Windows](https://onedrive.live.com/about/en-us/download/) standart ve "bir model Aç" iletişim kutusu aracılığıyla Onedrive'da depolanan dosyalarına erişmek için istemci.

![OneDrive özelliği kaldırıldı](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_save_onedrive-300x149.png)

### <a name="my-organization-uses-the-2016-version-of-the-tool-can-i-use-the-azure-stencil-set"></a>Kuruluşum aracı 2016 sürümünü kullanır, Azure şablon kümesi kullanabilir miyim?

Evet, kullanabilirsiniz! [Azure şablon kümesi github'da kullanılabilir](https://github.com/Microsoft/threat-modeling-templates/)ve Aracı'nın 2016 sürümü yüklenebilir. Azure şablon kümesi ile yeni bir modeli oluşturmak için ana menü ekranda "Şablonu için yeni modeller" iletişim kutusunu kullanın. Azure şablonu kümesinin "Olası risk azaltmaları" alanları bulunan bağlantılar TMT 2016 işlenemiyor, bu nedenle, HTML etiketlerini stil olarak görüntülenen bağlantılar görebilirsiniz.

![Azure şablonu 2016 istemci güncelleştirmeleri](./media/azure-security-threat-modeling-tool-releases-71509112/tmt_azure_stencils-300x212.png)

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - Microsoft Windows 10
- Gerekli .NET sürümü
  - .NET 3.5.2
- Ek Gereksinimler
  - Şablonları yanı sıra aracı güncelleştirmeleri almak için Internet bağlantısı gerekir.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Tehdit modelleme aracı belgelerine yer alan [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)ve bilgileri [aracını kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Sonraki adımlar

En son sürümünü indirin [Microsoft tehdit modelleme aracı](https://aka.ms/threatmodelingtool).
