---
title: "Etkileşimli olmayan kimlik doğrulama .NET uygulamalarını Azure Hdınsight'ta oluşturun. | Microsoft Docs"
description: "Etkileşimli olmayan kimlik doğrulaması Azure Hdınsight'ta Microsoft .NET uygulamaları oluşturmayı öğrenin."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: b2b24747ce4ea8499c999c693f00fb09178d52b0
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Etkileşimli olmayan kimlik doğrulama .NET Hdınsight uygulaması oluşturma
Microsoft .NET Azure Hdınsight uygulamanızı ya da uygulamanın kendi kimliğini (etkileşimli olmayan) (etkileşimli) uygulamasının oturum açmış kullanıcının kimliğini altında ya da çalıştırabilirsiniz. Bu makalede etkileşimli olmayan kimlik doğrulaması için Azure bağlanmak ve Hdınsight yönetmek için .NET uygulaması oluşturulacağını gösterir. Etkileşimli bir uygulamanın bir örnek için bkz: [Azure Hdınsight Bağlan](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). 

Etkileşimli olmayan .NET uygulamanızdan, aşağıdakiler gerekir:

* Azure aboneliği Kiracı Kimliğinizi (olarak da adlandırılan bir *dizin kimliği*). Bkz: [alma Kiracı kimliği](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* Azure Active Directory (Azure AD) uygulama istemci kimliği. Bkz: [Azure Active Directory Uygulama oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) ve [bir uygulama kimliği alma](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
* Azure AD uygulama gizli anahtarı. Bkz: [Get uygulama kimlik doğrulama anahtarı](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

## <a name="prerequisites"></a>Önkoşullar
* Hdınsight kümesi. Bkz: [başlama Öğreticisi](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Azure AD uygulaması rol atama
Azure AD uygulama atama bir [rol](../active-directory/role-based-access-built-in-roles.md), eylemleri gerçekleştirmek için izinleri vermek için. Abonelik, kaynak grubu ya da kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. (Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme uygulama kaynak grubunu ve tüm kaynaklarında okuyabileceği anlamına gelir.) Bu öğreticide kaynak grubu düzeyinde kapsamı ayarlayın. Daha fazla bilgi için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../active-directory/role-based-access-control-configure.md).

**Azure AD uygulama sahip rolünü eklemek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde seçin **kaynak grubu**.
3. Daha sonra Bu öğreticide, Hive sorgu çalıştıracaksınız Hdınsight kümesine sahip olan kaynak grubunu seçin. Çok sayıda kaynak grupları varsa, istediğinizi bulmak için filtre kullanabilirsiniz.
4. Kaynak grubu menüsünden seçin **erişim denetimi (IAM)**.
5. Altında **kullanıcılar**seçin **Ekle**.
6. Sahip rolü, Azure AD uygulama eklemek için yönergeleri izleyin. Rolü başarıyla ekledikten sonra uygulamanın altında listelendiğini **kullanıcılar**, sahibi rolüne sahip. 

## <a name="develop-an-hdinsight-client-application"></a>Hdınsight istemci uygulaması geliştirme

1. Bir C# konsol uygulaması oluşturun.
2. Aşağıdaki NuGet paketleri ekleyin:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Aşağıdaki kodu çalıştırın:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```


## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure Active Directory, Azure portalında uygulama ve hizmet sorumlusu oluşturmak](../azure-resource-manager/resource-group-create-service-principal-portal.md).
* Bilgi edinmek için nasıl [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md).
* Hakkında bilgi edinin [Azure rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md).
