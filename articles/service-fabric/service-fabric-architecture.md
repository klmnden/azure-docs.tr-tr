---
title: Azure Service fabric mimarisi | Microsoft Docs
description: Service Fabric, bulut için ölçeklenebilir, güvenilir ve kolayca yönetilen uygulamalar oluşturmak için kullanılan bir dağıtılmış sistemler platformudur. Bu makalede, Service Fabric mimarisi gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: chackdan
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/12/2017
ms.author: rsinha
ms.openlocfilehash: a1e68e2e39ea6f1c8cf8669e2e02d8dacaf0f284
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097853"
---
# <a name="service-fabric-architecture"></a>Service Fabric mimarisi
Service Fabric ile katmanlı alt sistemleri yerleşik olarak bulunur. Bu alt sistemleri olan uygulamaları yazmak etkinleştir:

* Yüksek oranda kullanılabilir
* Ölçeklenebilir
* Yönetilebilir
* Test edilebilir

Aşağıdaki diyagramda, Service Fabric, büyük alt sistemleri gösterir.

![Service Fabric mimarisi diyagramı](media/service-fabric-architecture/service-fabric-architecture.png)

Dağıtılmış bir sistemde, kümedeki düğümler arasında güvenli bir şekilde iletişim kurabilir büyük/küçük harf önemlidir. Yığın tabanına düğümler arasında güvenli iletişim sağlayan aktarım alt sistemi ' dir. Taşıma alt sistemi, Service Fabric hatalarını algılamak, öncü seçimi gerçekleştirmek ve tutarlı yönlendirmesi sağlayın (kümeleri adlı) tek bir varlığa farklı düğümlerde kümelerini Federasyon alt sistemi arasındadır. Federasyon alt sistemi üzerinde katmanlanmış güvenilirlik alt sistemi, Service Fabric Hizmetleri yük devretme çoğaltma ve kaynak yönetimi gibi bir yolla güvenilirliğini sorumludur. Federasyon alt sistemi ayrıca tek bir düğümde bir uygulamanın yaşam döngüsünü yöneten barındırma ve etkinleştirme alt sistemi vurgular. Uygulamalar ve hizmetler yaşam döngüsü yönetimi alt yönetir. Test Edilebilirlik alt sistemi, uygulama geliştiricilerin kendi sanal hataları hizmetleriyle önce ve sonra uygulamaları ve Hizmetleri üretim ortamlarına dağıtmadan test yardımcı olur. Service Fabric hizmet konumlar, iletişim alt sistemi aracılığıyla çözümleme olanağı sağlar. Geliştiriciler için kullanıma sunulan uygulama programlama modellerini, bu alt sistemlerin yanı sıra Araçları etkinleştirmek için uygulama modeli üzerinde katmanlıdır.

## <a name="transport-subsystem"></a>Aktarım alt sistemi
Aktarım alt sistemi, noktadan noktaya veri birimi iletişim kanalı uygular. Bu kanal, service fabric kümeleri içinde iletişim kurmak ve service fabric kümesi ve istemciler arasındaki iletişimi için kullanılır. Tek yönlü destekler ve istek-yanıt iletişim düzenleri yayın ve çok noktaya yayın Federasyon katmanda uygulamak için temel sağlar. Aktarım alt X509 kullanarak iletişimin güvenliğini sağlar sertifikaları veya Windows Güvenlik. Bu alt sistemi, Service Fabric tarafından dahili olarak kullanılır ve uygulama programlama için geliştiricilere doğrudan erişilebilir değil.

## <a name="federation-subsystem"></a>Federasyon alt sistemi
Dağıtılmış bir sistemde bir dizi düğümü hakkında nedeni için tutarlı bir görünüm sisteminin gerekir. Federasyon alt aktarım alt sistemi tarafından sağlanan iletişim temelleri kullanır ve hakkında nedeni tek bir birleştirilmiş kümesinde çeşitli düğümler bitiştirir. Bu, diğer alt sistemler tarafından - hata algılama, öncü seçimi ve tutarlı yönlendirmesi gereken dağıtılmış sistemler temelleri sağlar. Federasyon alt sistemi, 128 bit belirteci alana sahip dağıtılmış bir karma tablo üzerinde oluşturulmuştur. Alt düğümleri, her düğüm sahipliği için ayrılan bir alt simge alanı halka üzerinden halka topolojisi oluşturur. Hata algılama için katman beating Kalp ve Tahkim dayalı bir kiralama mekanizmasının kullanır. Federasyon alt sistemi, aynı zamanda karmaşık birleştirme ve herhangi bir zamanda yalnızca tek bir sahibi belirteci var. kalkış protokolleri üzerinden garanti eder. Bu, öncü seçimi ve tutarlı yönlendirme garantileri sağlar.

## <a name="reliability-subsystem"></a>Güvenilirlik alt sistemi
Güvenilirlik alt sistemi bir Service Fabric hizmetinin durumunu kullanımının yüksek oranda kullanılabilir hale getirmek için bir mekanizma sağlar *çoğaltıcı*, *Yük Devretme Yöneticisi*, ve *kaynak Dengeleyici*.

* Çoğaltma Hizmeti Çoğaltma kümesinde birincil ve ikincil çoğaltmalar arasında tutarlılığı sürdürmeye birincil hizmet çoğaltmasını durum değişiklikleri otomatik olarak ikincil çoğaltmalara çoğaltılır sağlar. Çoğaltma, çoğaltma yinelemeleri arasında çekirdek Yönetimi sorumludur. Çoğaltmak için işlemlerin listesini almak için yük devretme birimi ile etkileşim ve isteğe bağlı olarak yeniden yapılandırma aracısı çoğaltma kümesine yapılandırılmasını sağlar. Bu yapılandırma işlemleri çoğaltılacağını çoğaltmalar gösterir. Service Fabric programlama modeli API'si tarafından hizmet durumu, yüksek kullanılabilirliğe sahip ve güvenilir hale getirmek için kullanılabilecek Fabric Çoğaltıcısı adlı bir varsayılan yineleyici sağlar.
* Yük Devretme Yöneticisi düğümleri eklenir veya kümeden kaldırılmış, yükleme otomatik olarak kullanılabilir düğümlere dağıtılır sağlar. Kümedeki bir düğüm başarısız olursa, küme kullanılabilirliği sürdürmek için hizmeti çoğaltmaları otomatik olarak yeniden yapılandırır.
* Resource Manager, kümedeki hata etki alanı genelinde hizmet çoğaltmaları yerleştirir ve tüm yük devretme birimi işletimsel olmasını sağlar. Resource Manager, hizmet kaynakları ayrıca temel alınan paylaşılan havuzunda Tekdüzen en iyi yük dağılımı elde etmek için küme düğümlerinin dengeler.

## <a name="management-subsystem"></a>Yönetim alt sistemi
Yönetim alt sistemi, uçtan uca hizmet ve uygulama yaşam döngüsü yönetimi sağlar. PowerShell cmdlet'leri ve yönetim API'leri, sağlama, dağıtma, düzeltme eki uygulama, yükseltme ve kullanılabilirliği kaybı olmadan uygulamaları sağlamasını etkinleştirin. Yönetim alt sistemi, bu aşağıdaki hizmetleri aracılığıyla gerçekleştirir.

* **Küme Yöneticisi'ni**: İle yük devretme uygulamaları hizmet yerleştirme kısıtlamalarına göre düğümlerin yerleştirmek için Yöneticisi'nden güvenilirlik etkileşim birincil hizmetidir. Kaynak Yöneticisi'nde yük devretme alt kısıtlamaları hiçbir zaman bozuk olmasını sağlar. Küme Yöneticisi, uygulamaları yaşam döngüsü sağlamasını karşılık yönetir. Uygulama kullanılabilirliği yükseltmeler sırasında bir anlam sistem durumu açısından kaybı olmadığından emin olmak için sistem durumu Yöneticisi ile tümleştirilir.
* **Sistem Durumu Yöneticisi**: Bu hizmet, uygulamaları, hizmetleri ve küme varlık sistem durumu izleme sağlar. Merkezi Sistem deposuna toplanabilecek sistem durumu bilgileri, küme varlıkları (ör. düğüm, hizmet bölümleri ve çoğaltmalarını) bildirebilirsiniz. Bu sistem durumu bilgileri bir genel-belirli bir noktaya sistem durumu hizmetleri ve gerekli düzeltmeleri yapın olanak tanıyarak kümedeki birden fazla düğümde dağıtılmış düğümleri anlık görüntüsünü sağlar. Sistem durumu sorgusu API'leri, sistem durumu olayları'nı sorgulamak etkinleştirmek için sistem durumu alt bildirdi. Sistem durumu deposu veya toplu, depolanan ham sistem durumu verileri API'leri döndürüldüğü bir sistem durumu sorgu belirli küme varlık sistem durumu verilerini yorumlanır.
* **Görüntü Store**: Bu hizmet, depolama ve uygulama ikili dosyalarını dağıtımını sağlar. Bu hizmet, uygulamaları burada yüklenen ve indirilen basit dağıtılmış bir dosya deposu sağlar.

## <a name="hosting-subsystem"></a>Barındırma alt sistemi
Küme Yöneticisi'ni yönetmek için belirli bir düğüm için gereken hizmetleri (her düğümde çalışan) barındırma alt bildirir. Barındırma alt sistemi, ardından o düğümdeki uygulama yaşam döngüsü yönetir. Bu çoğaltmaların düzgün bir şekilde yerleştirildiğinden ve Sağlıklı olduğunu emin olmak için güvenilirlik ve sistem durumu bileşenlerle etkileşimi.

## <a name="communication-subsystem"></a>İletişim alt sistemi
Bu alt küme ve hizmet bulma adlandırma hizmeti aracılığıyla içinde güvenilir Mesajlaşma sağlar. Adlandırma hizmeti bir konumu kümedeki hizmet adları çözümlenir ve kullanıcıların hizmet adları ve özelliklerini yönetmenize olanak tanır. Adlandırma Hizmeti kullandığınızda, istemcileri güvenli bir şekilde bir hizmet adı çözümlenmeye ve hizmet meta verilerini almak için kümedeki herhangi bir düğüm ile iletişim kurabilir. Basit bir adlandırma İstemcisi API kullanarak, Service Fabric, kullanıcıların hizmetler ve düğüm dynamism rağmen geçerli ağ konumu veya kümesini yeniden boyutlandırma çözme özelliğine sahip istemciler geliştirebilirsiniz.

## <a name="testability-subsystem"></a>Test Edilebilirlik alt sistemi
Test Edilebilirlik bir Service Fabric üzerinde oluşturulmuş Hizmetleri test etmek için özel olarak tasarlanmış araçları paketidir. Kolayca anlamlı hataları anlamına ve çalışma ve çeşitli durumları ve hizmet ömrü boyunca, tüm kontrollü ve güvenli bir şekilde yaşar geçişleri doğrulamak için test senaryoları çalıştırın bir geliştirici araçları sağlar. Test Edilebilirlik ayrıca kullanılabilirlik kaybetmeden çeşitli olası hataları yineleyebilirsiniz uzun testleri çalıştırmak için bir mekanizma sağlar. Bu test, üretim ortamıyla sağlar.

