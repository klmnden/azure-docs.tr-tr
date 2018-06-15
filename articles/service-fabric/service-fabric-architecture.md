---
title: Azure Service Fabric mimarisi | Microsoft Docs
description: Service Fabric, bulut için ölçeklenebilir, güvenilir ve kolay yönetilen uygulamalar oluşturmak için kullanılan bir dağıtılmış sistemler platformudur. Bu makalede, Service Fabric mimarisi gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: timlt
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/12/2017
ms.author: rsinha
ms.openlocfilehash: 5e69d4b09261c90fd3c33e60645fe484b816e369
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209979"
---
# <a name="service-fabric-architecture"></a>Service Fabric mimarisi
Service Fabric katmanlı alt sistemleri ile yerleşik olarak bulunur. Bu alt sistemleri, uygulamaları yazma olanak sağlar:

* Yüksek oranda kullanılabilir
* Ölçeklenebilir
* Yönetilebilir
* Test edilebilir

Aşağıdaki diyagramda, Service Fabric, ana alt sistemleri gösterir.

![Service Fabric mimarisi diyagramı](media/service-fabric-architecture/service-fabric-architecture.png)

Dağıtılmış bir sistemde, kümedeki düğümler arasında güvenli iletişim olanağı önemlidir. Yığın tabanına düğümler arasında güvenli iletişim sağlayan aktarım alt sistemidir. Aktarım subsystem Service Fabric hatalarını algılayacak, öncü seçim gerçekleştirmek ve tutarlı yönlendirme sağlayan farklı düğümler (kümeleri olarak adlandırılır) tek bir varlık kümeleri Federasyon alt sistemi arasındadır. Federasyon alt sistemi üzerinde katmanlanmış güvenilirlik alt sistemi, çoğaltma, kaynak yönetimi ve yük devretme gibi mekanizmaları Service Fabric hizmetleriyle güvenilirliğini sorumludur. Federasyon alt sistemi de tek bir düğümde bir uygulama yaşam döngüsü yönetir barındırma ve etkinleştirme alt sistemi vurgular. Uygulamalar ve hizmetler yaşam döngüsü yönetimi alt yönetir. Test Edilebilirlik alt sistemi uygulama geliştiricileri kendi sanal hataları hizmetleriyle önce ve sonra uygulamaları ve Hizmetleri üretim ortamlarına dağıtmadan test yardımcı olur. Service Fabric hizmet konumları, iletişim alt sistemi aracılığıyla çözümleme olanağı sağlar. Geliştiricilere sunulan uygulama programlama modelleri, araç etkinleştirmek için uygulama modeli ile birlikte bu alt sistemleri üzerinde katmanlanmış.

## <a name="transport-subsystem"></a>Taşıma alt sistemi
Taşıma alt sistemi bir noktadan noktaya veri birimi iletişim kanalı uygular. Bu kanal, service fabric kümeleri içinde iletişim ve service fabric kümesi ve istemciler arasında iletişim için kullanılır. Tek yönlü destekler ve yayın ve çok noktaya yayın Federasyon katmanda uygulamak üzere temelini istek-yanıt iletişim deseni. Taşıma alt sistemi X509 kullanarak iletişimi güvenli hale getirdiği sertifikalar veya Windows Güvenliği. Bu alt sistemi Service Fabric tarafından dahili olarak kullanılır ve geliştiricilere uygulama programlama için doğrudan erişilebilir değil.

## <a name="federation-subsystem"></a>Federasyon alt sistemi
Dağıtılmış bir sistemde düğümleri kümesi hakkında neden için tutarlı bir görünüm sisteminin gerekir. Federasyon alt sistemi hakkında neden tek bir birleşik kümesinde çeşitli düğümleri bitiştirir ve aktarım alt sistemi tarafından sağlanan iletişim temelleri kullanır. Diğer alt sistemleri tarafından - hata algılama, öncü seçim ve tutarlı yönlendirme gerekli dağıtılmış sistemlerin temelleri sağlar. Federasyon alt sistemi dağıtılmış karma tabloları 128-bit belirteci alanı ile en üstünde yerleşik olarak bulunur. Alt düğümleri, her bir alt simge alanı sahipliğini ayrılan halkası düğümünde üzerinden halka topolojisi oluşturur. Hata algılaması için katman vuruş Kalp ve yönetimini dayalı bir kiralama mekanizması kullanır. Federasyon alt sistemi, aynı zamanda karmaşık birleştirme ve herhangi bir anda yalnızca tek bir sahibi belirtecin var. ayrılma protokolleri aracılığıyla garanti eder. Bu kılavuz seçim ve tutarlı yönlendirme garantileri sağlar.

## <a name="reliability-subsystem"></a>Güvenilirlik alt sistemi
Güvenilirlik alt sistemi Service Fabric hizmetinin durumunu kullanılarak yüksek oranda kullanılabilir yapmak için mekanizma sağlar *çoğaltıcı*, *Yük Devretme Yöneticisi*, ve *kaynak Dengeleyici*.

* Çoğaltıcı birincil hizmet çoğaltma durumu değişiklikleri ikincil çoğaltmalar için otomatik olarak çoğaltılır hizmet çoğaltma kümesinde birincil ve ikincil çoğaltmalar arasında tutarlılığı koruma sağlar. Çoğaltıcı, çekirdek Yönetimi yineleme kümesindeki çoğaltmalar arasında sorumludur. Çoğaltmak için işlemlerin listesini almak için yük devretme birimi ile etkileşim kurar ve yeniden yapılandırma aracısı çoğaltma kümesi yapılandırması ile sağlar. Bu yapılandırma işlemleri çoğaltılması gereken çoğaltmalar gösterir. Service Fabric hizmet durumu yüksek oranda kullanılabilir ve güvenilir hale getirmek için programlama modelini API tarafından kullanılan Fabric çoğaltması olarak adlandırılan bir varsayılan çoğaltıcı sağlar.
* Yük Devretme Yöneticisi düğümleri eklenecek veya kümeden kaldırılmış olduğunda yük otomatik olarak kullanılabilir düğümleri arasında dağıtılır sağlar. Kümedeki bir düğüm başarısız olursa, küme kullanılabilirliğini sağlamak için hizmeti çoğaltmaları otomatik olarak yeniden yapılandırır.
* Resource Manager kümedeki hata etki alanı arasında hizmet çoğaltmaları yerleştirir ve tüm yük devretme birimi işletimsel olmasını sağlar. Kaynak Yöneticisi hizmet kaynakları en iyi Tekdüzen yük dağıtımı elde etmek için küme düğümleri arasında temel paylaşılan havuzu da dengeler.

## <a name="management-subsystem"></a>Yönetim alt sistemi
Yönetimi alt uçtan uca hizmet ve uygulama yaşam döngüsü yönetimi sağlar. PowerShell cmdlet'leri ve yönetim API'leri sağlamak, dağıtma, düzeltme eki, yükseltme ve kullanılabilirlik kaybı olmadan uygulamaları sağlanmasını olanak sağlar. Yönetim alt sistemi bu aşağıdaki hizmetler aracılığıyla gerçekleştirir.

* **Küme Yöneticisi'ni**: Bu ile yük devretme hizmet yerleştirme kısıtlamalarına göre düğümlerde uygulamaları yerleştirmek için Yöneticisi'nden güvenilirlik etkileşime giren, birincil hizmetidir. Kaynak Yöneticisi'nde yük devretme alt sistemi kısıtlamalarını hiçbir zaman bozuk olmasını sağlar. Küme Yöneticisi'ni sağlanmasını karşılık uygulamaları yaşam döngüsü yönetir. Uygulama kullanılabilirliği yükseltmeler sırasında bir anlam sistem durumu açısından kaybı olmadığından emin olmak için sistem durumu Yöneticisi ile entegre olur.
* **Sağlık Yöneticisi**: Bu hizmet uygulamaları, hizmetleri ve küme varlık sistem durumu izleme sağlar. Küme varlıklar (örneğin, düğümleri, hizmet bölümler ve çoğaltmalar) Merkezi durum deposuna toplanabilecek sistem durumu bilgilerini bildirebilirsiniz. Bu sistem durumu bilgileri Hizmetleri ve tüm gerekli düzeltici eylemleri olanak sağlayarak kümedeki birden çok düğüm arasında dağıtılmış düğümlere noktası zaman genel sistem durumu görüntüsünü sağlar. Sistem durumu sorgusu API'leri, sistem durumu olayları sorgulamak üzere etkinleştirmek için sistem durumu alt sisteminin bildirdiği. Sistem sağlığı deposunu veya toplanmış, depolanan ham sistem durumu verileri API'leri döndürüldüğü bir sistem durumu sorgu belirli küme varlık için sistem durumu veri yorumlanır.
* **Image Store**: Bu hizmet, depolama ve uygulama ikili dosyalarının dağıtımını sağlar. Bu hizmet, bir basit dağıtılmış dosya depolama burada uygulamaları için karşıya ve karşıdan sağlar.

## <a name="hosting-subsystem"></a>Barındırma alt sistemi
Küme Yöneticisi'ni yönetmek için belirli bir düğüm için gereken hizmetler (her düğüm üzerinde çalışan) barındırma alt bildirir. Barındırma alt sistemi, ardından bu düğümde uygulama yaşam döngüsü yönetir. Çoğaltmaları düzgün yerleştirilir ve iyi durumda emin olmak için güvenilirlik ve sistem durumu bileşenlerle etkileşimi.

## <a name="communication-subsystem"></a>İletişim alt sistemi
Bu alt sistemi adlandırma hizmeti aracılığıyla küme ve hizmet bulma içinde güvenilir Mesajlaşma sağlar. Adlandırma hizmeti kümedeki bir konuma hizmet adlarını çözer ve kullanıcıların hizmet adlarını ve özelliklerini yönetmenize olanak tanır. Adlandırma Hizmeti kullanıldığında, istemcileri güvenli bir şekilde bir hizmet adı giderin ve hizmeti meta verilerini almak için kümedeki herhangi bir düğümün ile iletişim kurabilir. Basit bir adlandırma İstemcisi API kullanarak, hizmetler ve istemcileri düğümü yeni bir dinamizm rağmen geçerli ağ konumu veya kümesini yeniden boyutlandırma çözme özellikli Service Fabric kullanıcılarının geliştirebilirsiniz.

## <a name="testability-subsystem"></a>Test Edilebilirlik alt sistemi
Test Edilebilirlik Service Fabric üzerinde oluşturulmuş Hizmetleri test etmek için özellikle tasarlanmış araçlar paketidir. Alıştırma ve çok sayıda durumları ve yaşam süresi, denetimli ve güvenli bir şekilde tümünde boyunca bir hizmet yaşar geçişleri doğrulamak için sınama senaryolarını kolayca anlamlı hataları anlamına ve geliştirici araçları sağlar. Test Edilebilirlik da kullanılabilirlik kaybetmeden çeşitli olası hataları yineleyebilirsiniz uzun testleri çalıştırmak için bir mekanizma sağlar. Bu olan bir üretim, test ortamı sağlar.

