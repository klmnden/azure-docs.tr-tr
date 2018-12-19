---
title: Azure Resource Manager şablonu örnekleri - App Service | Microsoft Docs
description: App Service’in Web Apps özelliği için Azure Resource Manager şablon örnekleri
services: app-service
documentationcenter: app-service
author: tfitzmac
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 10/15/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 2c717d42f4461816fca2a48701d135c3126880f2
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53585098"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>App Service için Azure Resource Manager şablonları

Aşağıdaki tabloda, Azure App Service’in Web Apps özelliği için Azure Resource Manager şablonlarının bağlantıları yer alır. Web uygulaması şablonları oluştururken sık karşılaşılan hataların önlenmesiyle ilgili öneriler için bkz. [Azure Resource Manager şablonları ile web uygulaması dağıtma kılavuzu](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Web uygulamasını dağıtma**||
| [App Service planı ve basit Linux web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-linux) | Linux için yapılandırılmış bir Azure web uygulaması dağıtır. |
| [App Service planı ve basit Windows web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-windows) | Windows için yapılandırılmış bir Azure web uygulaması dağıtır. |
| [Bir GitHub deposuna bağlı web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Github'dan kod çeken bir Azure web uygulaması dağıtır. |
| [Özel dağıtım yuvaları içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Özel dağıtım yuvaları/ortamları içeren bir Azure web uygulaması dağıtır. |
|**Web uygulamasını yapılandırma**||
| [Key Vault’tan web uygulaması sertifikası](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Azure Key Vault gizli dizilerinden bir Azure web uygulaması sertifikası dağıtır ve SSL bağlaması için bunu kullanır. |
| [Özel etki alanı içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Özel bir ana bilgisayar adına sahip bir Azure web uygulaması dağıtır. |
| [Özel etki alanı ve SSL içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Özel bir ana bilgisayar adına sahip bir Azure web uygulaması dağıtır ve SSL bağlaması için Key Vault’tan web uygulaması sertifikası alır. |
| [GoLang uzantılı web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Golang site uzantısı ile bir Azure web uygulamasını dağıtır. Daha sonra Azure’da Golang üzerinde geliştirilmiş web uygulamalarını çalıştırabilirsiniz. |
| [Java 8 ve Tomcat 8 destekli web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Java 8 ve Tomcat 8 etkin bir Azure web uygulaması dağıtır. Daha sonra Azure'da Java uygulamaları çalıştırabilirsiniz. |
|**Bağlı kaynaklar içeren Linux web uygulaması**||
| [Linux üzerinde MySQL içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Linux üzerinde MySQL için Azure Veritabanı içeren bir Azure web uygulaması dağıtır. |
| [Linux üzerinde PostgreSQL içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Linux üzerinde PostgreSQL için Azure Veritabanı içeren bir Azure web uygulaması dağıtır. |
|**Bağlı kaynaklar içeren web uygulaması**||
| [MySQL ile web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Windows üzerinde MySQL için Azure Veritabanı içeren bir Azure web uygulaması dağıtır. |
| [PostgreSQL içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Windows üzerinde PostgreSQL için Azure Veritabanı içeren bir Azure web uygulaması dağıtır. |
| [SQL veritabanı içeren web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Temel hizmet düzeyinde bir Azure web uygulaması ve SQL veritabanı dağıtır. |
| [Blob depolama bağlantılı web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Azure Blob depolama bağlantı dizesi içeren bir Azure web uygulaması dağıtır. Daha sonra web uygulamasından Blob depolamayı kullanabilirsiniz. |
| [Web uygulaması ile bir Azure önbelleği için Redis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Bir Azure web uygulaması ile bir Azure önbelleği için Redis dağıtır. |
|**PowerApps için App Service Ortamı**||
| [App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Sanal ağınızda bir App Service ortamı v2 oluşturur. |
| [ILB adresli bir App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Sanal ağınızda özel bir iç yük dengeleyici adresine sahip App Service ortamı v2 oluşturur. |
| [Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırır. |
| | |
