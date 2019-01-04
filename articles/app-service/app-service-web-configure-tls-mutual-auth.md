---
title: TLS karşılıklı kimlik doğrulama - Azure App Service'ı yapılandırma
description: TLS üzerinde istemci sertifikası kimlik doğrulaması kullanmak için uygulamanızı yapılandırmayı öğrenin.
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.custom: seodec18
ms.openlocfilehash: d441329bc3f279e95b2ee302db53d78f786c3470
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650406"
---
# <a name="how-to-configure-tls-mutual-authentication-for-azure-app-service"></a>Azure App Service için TLS karşılıklı kimlik doğrulamayı yapılandırma
## <a name="overview"></a>Genel Bakış
Kimlik doğrulaması için farklı türlerde etkinleştirerek Azure App Service uygulamanıza erişimi kısıtlayabilirsiniz. Bunu yapmak için bir TLS/SSL üzerinden istek olduğunda, bir istemci sertifikası kullanılarak kimlik doğrulaması yoludur. Bu mekanizma, TLS karşılıklı kimlik doğrulaması veya istemci sertifikası kimlik doğrulaması ve bu makalede istemci sertifikası kimlik doğrulaması kullanmak için uygulamanızı ayarlama konusunda ayrıntılarıyla çağrılır.

> **Not:** Sitenizi HTTP ve HTTPS değil üzerinden erişirseniz, herhangi bir istemci sertifikası almazsınız. Uygulamanızın istemci sertifikası gerektiriyorsa, bu nedenle, istekleri, uygulamanız için HTTP üzerinden izin vermemelidir.
> 
> 

## <a name="configure-app-service-for-client-certificate-authentication"></a>App Service için istemci sertifikası kimlik doğrulamasını yapılandırma
İstemci sertifikaları gerektirmek için uygulamanızı ayarlayın için uygulamanızı clientCertEnabled site ayarını ekleyin ve true olarak ayarlamanız gerekir. Bu ayar, ayrıca SSL sertifikaları dikey penceresinin altındaki Azure portalında yapılandırılmış olması mümkün olur.

Kullanabileceğiniz [ARMClient aracı](https://github.com/projectkudu/ARMClient) REST API çağrısı çalışıyorlardı kolaylaştırır. Aracıyla işaretinden sonra aşağıdaki komutu yürütün gerekir:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

her şeyi değiştirerek {} bilgilerle uygulamanızı ve aşağıdaki JSON ile enableclientcert.json içerik adlı bir dosya oluşturmak için:

    {
        "location": "My App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Her yerde uygulamanızı Örneğin, Kuzey Orta ABD veya Batı ABD vb. bulunduğu "Konum" değerini değiştirdiğinizden emin olun.

Ayrıca https://resources.azure.com çevrilecek `clientCertEnabled` özelliğini `true`.

> **Not:** Powershell'den ARMClient çalıştırırsanız, çıkış gerekecektir \@ sembol geri tık ile JSON dosyası için '.
> 
> 

## <a name="accessing-the-client-certificate-from-app-service"></a>App Service'ten istemci sertifikasına erişme
ASP.NET kullanıyorsanız ve istemci sertifikası kimlik doğrulaması kullanmak için uygulamanızı yapılandırma, sertifika aracılığıyla **HttpRequest.ClientCertificate** özelliği. Diğer uygulama yığınları için istemci sertifikası "X-ARR-ClientCert" istek üstbilgisindeki bir base64 olarak kodlanmış değer uygulamanızda kullanıma sunulacaktır. Uygulamanız, bu değeri bir sertifika oluşturmak ve uygulamanızdaki herhangi bir kimlik doğrulama ve yetkilendirme amacıyla kullanın.

## <a name="special-considerations-for-certificate-validation"></a>Sertifika doğrulama için özel hususlar
Uygulamaya gönderilen bir istemci sertifikası Azure App Service platformu tarafından herhangi bir doğrulama geçmez. Bu sertifika doğrulama uygulamanın sorumluluğundadır. Kimlik doğrulama amacıyla sertifika özellikleri doğrular örnek ASP.NET kodunu aşağıda verilmiştir.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
