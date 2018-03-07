---
title: "Azure Resource Manager şablonu örnekleri - App Service | Microsoft Docs"
description: "Azure Resource Manager şablonu örnekleri - App Service"
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
editor: ggailey777
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 02/26/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: e28a27b9a00164fcbba6eb5d3e5435548486ffa4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="azure-resource-manager-templates-for-azure-web-apps"></a>Azure Web Apps için Azure Resource Manager şablonları

Aşağıdaki tablo, Azure Resource Manager şablonlarının bağlantılarını içerir. Web uygulaması şablonları oluştururken sık karşılaşılan hataların önlenmesiyle ilgili öneriler için bkz. [Azure Resource Manager şablonları ile web uygulaması dağıtma kılavuzu](web-sites-rm-template-guidance.md).

| | |
|-|-|
|**Web Uygulaması Dağıtma**||
| [Bir GitHub deposuna bağlı Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Github'dan kod çeken bir Azure web uygulaması dağıtır. |
| [Özel dağıtım yuvaları içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Özel dağıtım yuvaları/ortamları içeren bir Azure web uygulaması dağıtır. |
|**Web Uygulaması Yapılandırma**||
| [Key Vault’tan Web Uygulaması sertifikası](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Key Vault gizli dizilerinden bir Azure web uygulaması sertifikası dağıtır ve SSL bağlaması için bunu kullanır. |
| [Özel etki alanlı Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Özel bir ana bilgisayar adına sahip bir Azure web uygulaması dağıtır. |
| [Özel etki alanı ve SSL içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Özel bir ana bilgisayar adına sahip bir Azure web uygulaması dağıtır ve SSL bağlaması için Key Vault’tan web uygulaması sertifikasını alır. |
| [GoLang uzantılı Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Golang’de geliştirilmiş web uygulamalarını Azure’da çalıştırmanıza imkan tanıyan Golang site uzantılı bir Azure web uygulaması dağıtır. |
| [Java 8 ve Tomcat 8 destekli Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Java 8 ve Tomcat 8’in etkin olduğu, Azure'da Java uygulamaları çalıştırmanıza olanak sağlayan bir Azure web uygulaması dağıtır. |
|**Linux Web Uygulaması**||
| [Linux üzerinde MySQL içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Linux üzerinde MySQL için Azure veritabanı içeren bir Azure web uygulaması dağıtır. |
| [PostgreSQL ile Linux üzerinde Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Linux üzerinde PostgreSQL için Azure veritabanı içeren bir Azure web uygulaması dağıtır. |
|**Bağlı kaynaklar içeren Web Uygulaması**||
| [MySQL ile Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Windows üzerinde MySQL için Azure veritabanı içeren bir Azure web uygulaması dağıtır. |
| [PostgreSQL içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Windows üzerinde PostgreSQL için Azure veritabanı içeren bir Azure web uygulaması dağıtır. |
| [SQL Veritabanı içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| "Temel" hizmet düzeyinde bir Azure web uygulaması ve SQL Veritabanı dağıtır. |
| [Blob Depolama bağlantılı Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Web uygulamasından Azure Blob Depolama’yı kullanmanıza imkan sağlayan bir blob depolama bağlantı dizesi içeren bir Azure web uygulaması dağıtır. |
| [Redis Cache içeren Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Redis Cache içeren bir Azure web uygulaması dağıtır. |
|**App Service Ortamı**||
| [App Service Ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Sanal ağınızda bir App Service Ortamı v2 oluşturur. |
| [ILB Adresli bir App Service Ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Sanal ağınızda özel bir iç yük dengeleyici adresine sahip App Service Ortamı v2 oluşturur. |
| [ILB ASE veya ILB ASE v2 için Varsayılan SSL Sertifikasını Yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Bir ILB App Service Ortamı veya ILB App Service Ortamı v2 için varsayılan SSL sertifikasını yapılandırır |
| | |
