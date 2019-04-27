---
title: Azure Resource Manager şablonu örnekleri - App Service | Microsoft Docs
description: App Service için Azure Resource Manager şablonu örnekleri
services: app-service
documentationcenter: app-service
author: tfitzmac
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 01/04/2019
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 842ec98245522095334b9f17e8c12292b7c1dda8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835439"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>App Service için Azure Resource Manager şablonları

Aşağıdaki tabloda, Azure App Service için Azure Resource Manager şablonlarının bağlantılarını içerir. Uygulaması şablonları oluştururken sık karşılaşılan hataların önlenmesiyle ilgili öneriler için bkz. [Azure Resource Manager şablonları ile uygulaması Dağıtma Kılavuzu](deploy-resource-manager-template.md).

Uygulama Hizmetleri kaynakların özellikleri ve JSON söz dizimi hakkında bilgi edinmek için bkz. [Microsoft.Web kaynak türleri](/azure/templates/microsoft.web/allversions).

| | |
|-|-|
|**Bir uygulamayı dağıtma**||
| [App Service planı ve temel Linux uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-linux) | Linux için yapılandırılmış bir App Service uygulaması dağıtır. |
| [App Service planı ve temel bir Windows uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-windows) | Windows için yapılandırılmış bir App Service uygulaması dağıtır. |
| [Bir GitHub deposuna bağlı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Kodu Github'dan çeken bir App Service uygulaması dağıtır. |
| [Özel dağıtım yuvaları içeren uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Özel dağıtım yuvaları/ortamları içeren bir App Service uygulaması dağıtır. |
|**Uygulama yapılandırma**||
| [Anahtar Kasası'ndaki uygulama sertifikası](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Bir Azure Key Vault gizli dizilerinden bir App Service uygulaması sertifikası dağıtır ve SSL bağlaması için bunu kullanır. |
| [Özel bir etki alanı ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Özel ana bilgisayar adı ile bir App Service uygulaması dağıtır. |
| [Özel etki alanı ve SSL uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Özel ana bilgisayar adı ile bir App Service uygulaması dağıtır ve SSL bağlaması için Key Vault'tan bir uygulaması sertifikası alır. |
| [GoLang uzantılı uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Golang site uzantılı bir App Service uygulaması dağıtır. Daha sonra Azure’da Golang üzerinde geliştirilmiş web uygulamalarını çalıştırabilirsiniz. |
| [Java 8 ve tomcat 8 destekli uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Java 8 ve Tomcat 8 etkin bir App Service uygulaması dağıtır. Daha sonra Azure'da Java uygulamaları çalıştırabilirsiniz. |
|**Bağlı kaynaklar içeren Linux uygulaması**||
| [Linux üzerinde MySQL içeren uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | MySQL için Azure veritabanı ile Linux'ta App Service uygulaması dağıtır. |
| [Linux üzerinde PostgreSQL içeren uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | PostgreSQL için Azure veritabanı ile Linux'ta App Service uygulaması dağıtır. |
|**Bağlı kaynaklar içeren uygulama**||
| [MySQL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| MySQL için Azure veritabanı ile Windows üzerindeki bir App Service uygulaması dağıtır. |
| [PostgreSQL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| PostgreSQL için Azure veritabanı ile Windows üzerindeki bir App Service uygulaması dağıtır. |
| [SQL veritabanı içeren uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Bir App Service uygulaması ve temel hizmet düzeyinde bir SQL veritabanı dağıtır. |
| [Blob Depolama bağlantılı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Bir Azure Blob Depolama bağlantı dizesi içeren bir App Service uygulaması dağıtır. Ardından, uygulama Blob depolamadan kullanabilirsiniz. |
| [Uygulama ile bir Azure önbelleği için Redis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Bir App Service uygulaması ile bir Azure önbelleği için Redis dağıtır. |
|**PowerApps için App Service Ortamı**||
| [App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Sanal ağınızda bir App Service ortamı v2 oluşturur. |
| [ILB adresli bir App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Sanal ağınızda özel bir iç yük dengeleyici adresine sahip App Service ortamı v2 oluşturur. |
| [Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırır. |
| | |
