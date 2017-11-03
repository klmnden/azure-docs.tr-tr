---
title: "Sprint 0 Ekim 2017 Azure ML çalışma ekranı sürüm notları"
description: "Bu belgede Azure ML sprint 0 sürümü için güncelleştirmeler ayrıntıları"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 10/12/2017
ms.openlocfilehash: 37e0a4461e8a0de631a6194a1eb8cc16b610954f
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="sprint-0---october-2017"></a>Sprint 0 - Ekim 2017 

**Sürüm numarası: 0.1.1710.04013**

Bizim ilk genel Önizleme Microsoft Ignite 2017 konferansında aşağıdaki Azure Machine Learning ekranının ilk güncelleştirme Hoş Geldiniz. Ana bu sürümde güvenilirlik ve sabitlemeyi güncelleştirmelerin giderir.  Biz ele kritik sorunlardan bazıları şunlardır:

## <a name="new-features"></a>Yeni Özellikler
- macOS yüksek Sierra artık desteklenmiyor

## <a name="bug-fixes"></a>Hata düzeltmeleri
### <a name="workbench-experience"></a>Çalışma ekranı deneyimi
- Sürükle ve bırak çalışma ekranı dosyasına çalışma ekranı çökmesine neden olur.
- VS code'da çalışma ekranı tanımaz için bir IDE yapılandırılan terminal penceresi _az ml_ komutları.

### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
Biz yapılan bildirilen çeşitli oturum açma ve kimlik doğrulama sorunları artırmak için güncelleştirmeleri sayısı.
- Özellikle Internet bağlantısı kararlı olmadığında kimlik doğrulama penceresi pencerelerinin-yukarı, tutar.
- Geliştirilmiş güvenilirlik sorunlarını çözmek kimlik doğrulama belirteci süre sonu.
- Bazı durumlarda, iki kez kimlik doğrulaması penceresi görüntülenir.
- Çalışma ekranı ana penceresi hala zaten kapatıldığında açılır iletişim kutusunu ve kimlik doğrulama işlemi gerçekte bittiğinde "kimlik doğrulaması" iletisi görüntüler.
- Internet bağlantısı yoksa bu kimlik doğrulama iletişim boş bir ekran ile açılır.

### <a name="data-preparation"></a>Veri hazırlama 
- Belirli bir değere filtre uygulandığında, hataları ve eksik değerleri de filtrelenir.
- Bir örnekleme stratejiyi değiştirmeden sonraki varolan birleştirme işlemleri kaldırır.
- Eksik değerini değiştirerek dönüştürme NaN dikkate almaz.
- Null değer karşılaştığında tarih tür çıkarımı özel durum oluşturur.

### <a name="job-execution"></a>İş yürütme
- Boyut sınırını aştığı için proje klasörünü karşıya yüklemek iş yürütme başarısız olduğunda Temizle hata iletisi yok.
- Kullanıcının Python komut çalışma dizini değişirse çıktıları klasörlere yazılmış dosyaları izlenmez. 
- Etkin Azure aboneliği geçerli projeye ait olandan farklı ise, iş gönderme 403 hatası oluşur.
- Docker mevcut olmadığında, kullanıcı bir yürütme hedef olarak Docker kullanmaya çalışırsa Temizle hata iletisi döndürülür.
- .runconfig dosyası kaydedilmedi otomatik olarak kullanıcı tıklattığında _çalıştırmak_ düğmesi.

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Belirli bir oturum açma türleri ile kullanıcı kullanıyorsa, Not Defteri sunucusu başlatılamıyor.
- Not Defteri sunucu hata iletileri, kullanıcıya görünür günlüklerindeki yüzey değil.

### <a name="azure-portal"></a>Azure portalına
- Azure portal'ın koyu tema seçilmesi siyah bir kutu görüntülemek Model yönetim dikey neden olur.

### <a name="operationalization"></a>Operationalization
- Bir web hizmeti güncelleştirmek için bir bildirim yeniden rastgele bir ad ile oluşturulmuş yeni bir Docker görüntüsü neden olur.
- Web hizmeti günlükleri Kubernetes kümeden alınamıyor.
- Yanıltıcı hata iletisi kullanıcı bir Model yönetim hesabı veya bir ML işlem hesabı oluşturma girişiminde bulunduğunda yazdırılabilir ve izinleri sorunları karşılaşır.
