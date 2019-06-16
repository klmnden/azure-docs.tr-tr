---
title: Windows yapılandırma Java uygulamaları - Azure uygulama hizmeti | Microsoft Docs
description: Java uygulamalarını Azure App Service varsayılan Windows örneklerini çalıştırmak için yapılandırmayı öğrenin.
keywords: Azure app service, web uygulaması, windows, oss, java
services: app-service
author: jasonfreeberg
manager: jeconnock
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe;cephalin
ms.custom: seodec18
ms.openlocfilehash: 82e8936a888cbc99088ab18423e55dd57a3c2e77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65604172"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Bir Windows yapılandırma Azure App Service için Java uygulaması

Azure App Service Java geliştiricilerinin kolayca oluşturmanızı, dağıtmanızı ve bunların Tomcat ölçeklendirme sağlar veya Windows tabanlı tam olarak yönetilen bir hizmet üzerinde web uygulamaları Java Standard Edition (SE) paketlenir. Komut satırından veya Intellij, Eclipse veya Visual Studio Code gibi düzenleyicilerde uygulamalarını Maven eklentileri ile dağıtın.

Bu kılavuzu temel kavramları ve App Service içinde Java geliştiricileri için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız okumanız gereken [Java Hızlı Başlangıç](app-service-web-get-started-java.md) ilk. Java geliştirme belirli olmayan App Service'ı kullanma hakkında genel soruları [App Service Windows SSS](faq-configuration-and-management.md).

> [!NOTE]
> Aradığınızı bulamadınız mı? Lütfen [Windows OSS SSS](faq-configuration-and-management.md) veya [Java Linux Yapılandırma Kılavuzu](containers/configure-language-java.md) dağıtma ve Java uygulamanızı güvenli hale getirme hakkında bilgi.

## <a name="configuring-tomcat"></a>Tomcat yapılandırma

Tomcat'in düzenlenecek `server.xml` veya diğer yapılandırma dosyalarını önce portalda, Tomcat ana sürüm not alın.

1. Tomcat giriş dizini çalıştırarak sürümünüz için bulma `env` komutu. İle başlayan ortam değişkenini arayın `AZURE_TOMCAT`ve ana sürümünüzle eşleşen. Örneğin, `AZURE_TOMCAT85_HOME` Tomcat 8.5 için Tomcat dizine işaret eder.
1. Sürümünüz için Tomcat giriş dizini belirledikten sonra yapılandırma dizinine kopyalayın `D:\home`. Örneğin, varsa `AZURE_TOMCAT85_HOME` değerine sahip `D:\Program Files (x86)\apache-tomcat-8.5.37`, kopyalanan yapılandırma dizinin tam yolu olmalıdır `D:\home\tomcat\conf`.

Son olarak, App service'inizi yeniden başlatın. Dağıtımlarınızı gitmesi gereken `D:\home\site\wwwroot\webapps` önceki yalnızca gibi.

## <a name="java-runtime-statement-of-support"></a>Java Çalışma zamanı destek bildirimi

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve Bakım

Azure'nın desteklenen Java Development Kit (JDK) olan [Zulu](https://www.azul.com/downloads/azure-only/zulu/) aracılığıyla sağlanan [Azul Systems](https://www.azul.com/).

Ana sürüm güncelleştirmeleri ile Windows için Azure App service'taki yeni çalışma zamanı seçenekleri sağlanır. Müşteriler, App Service dağıtımı yapılandırarak Java daha yeni sürümleri için güncelleştirme ve test etmeden sorumlu ve ihtiyaçlarını karşılayan önemli güncelleştirme sağlama.

Desteklenen JDK otomatik olarak üç aylık olarak Ocak, Nisan, Temmuz ve Ekim her yıl düzeltme eki.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azul sistemlerden kullanılabilir olduklarında hemen sonra yamaları ve düzeltmeler önemli güvenlik açıkları için kullanıma sunulacaktır. "Büyük" bir güvenlik açığı 9.0 temel puanı ile tanımlanan ya da üzerinde daha büyük [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve devre dışı bırakma

Desteklenen Java Çalışma zamanı kullanımdan kaldırılacak, çalışma zamanı kullanımdan önce en az altı ay etkilenen çalışma zamanı kullanan Azure geliştiricileri, kullanımdan kaldırma bildirimi verilir.

### <a name="local-development"></a>Yerel geliştirme

Geliştiriciler, üretim sürümü, Azul Zulu Enterprise JDK yerel geliştirme için indirebileceği [Azul'ın indirme sitesinde](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Geliştirme desteği

Ürün desteği [Azure tarafından desteklenen Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) Microsoft Azure için geliştirmeye olduğunda kullanılabilir veya [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ile bir [Azure destek planı koşullu](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler şunları yapabilir [bir sorun açın](/azure/azure-supportability/how-to-create-azure-support-request) oluşturulduysa Azure desteği aracılığıyla Azul Zulu JDK ile bir [tam destek planı](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Bu konu, Windows üzerinde desteği Azure App Service için Java Çalışma zamanı deyiminin sağlar.

- Bkz. Azure App Service ile web uygulamalarını barındırma hakkında daha fazla bilgi edinmek için [App Service'e genel bakış](overview.md).
- Azure geliştirme Java hakkında bilgi için bkz. [Azure Java Geliştirici Merkezi için](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
