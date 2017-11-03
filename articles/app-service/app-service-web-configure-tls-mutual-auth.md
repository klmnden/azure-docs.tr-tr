---
title: "Web App için TLS Karşılıklı Kimlik Doğrulamayı Yapılandırma"
description: "Web uygulamanız TLS istemci sertifikası kimlik doğrulamasını kullanmak için yapılandırmayı öğrenin."
services: app-service
documentationcenter: 
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
ms.openlocfilehash: db69852cffd1ff331ac4a640b04ea4360d00bf75
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Web App için TLS Karşılıklı Kimlik Doğrulamayı Yapılandırma
## <a name="overview"></a>Genel Bakış
Kimlik doğrulaması için farklı türlerde etkinleştirerek Azure web uygulamanıza erişimi kısıtlayabilirsiniz. Bunu yapmak için bir istek TLS/SSL üzerinden olduğunda istemci sertifikası kullanılarak kimlik doğrulaması için yoludur. Bu mekanizma TLS karşılıklı kimlik doğrulaması veya istemci sertifikası kimlik doğrulaması ve bu makalede istemci sertifikası kimlik doğrulamasını kullanmak için web uygulamanızı nasıl ayrıntılarıyla açıklar çağrılır.

> **Not:** sitenizi HTTP ve HTTPS değil üzerinden erişirseniz, herhangi bir istemci sertifikası almaz. Uygulamanızın istemci sertifikası gerektiriyorsa, bu nedenle, istekleri uygulamanıza HTTP üzerinden izin vermemelisiniz.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Web uygulaması için istemci sertifikası kimlik doğrulamasını yapılandırma
İstemci sertifikaları gerektirmek için web uygulamanızı için web uygulamanız için clientCertEnabled site ayarı ekleyin ve true olarak ayarlamanız gerekir. Bu ayar Portalı'nda yönetim deneyimi aracılığıyla şu anda kullanılabilir değil ve REST API bunu gerçekleştirmek için kullanılması gerekir.

Kullanabileceğiniz [ARMClient aracı](https://github.com/projectkudu/ARMClient) REST API çağrısı oluşturabilir kolaylaştırmak için. Aracın oturum oturum sonra aşağıdaki komutu yürütün gerekir:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

{} her şeyi web uygulamanız için bilgi ile değiştirerek ve bir dosya oluşturulurken aşağıdaki JSON ile enableclientcert.json içerik çağrılır:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Yerlerde, web uygulamanızın bulunduğu için "Konum" değerini değiştirdiğinizden emin olun ör Kuzey Orta ABD veya Batı ABD vs.

Çevrilecek https://resources.azure.com kullanabilirsiniz `clientCertEnabled` özelliğine `true`.

> **Not:** Powershell'den ARMClient çalıştırırsanız, kaçış gerekir @ simgesinden geri değer çizgisi olan JSON dosyası için '.
> 
> 

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Web uygulamanızdan istemci sertifikasına erişme
ASP.NET kullanıyorsanız ve istemci sertifikası kimlik doğrulamasını kullanmak için uygulamanızı yapılandırmak için sertifika ile de kullanılabilir olması **HttpRequest.ClientCertificate** özelliği. Diğer uygulama yığınları için istemci sertifikası uygulamanızda "X-ARR-ClientCert" istek üst bilgisinde base64 ile kodlanmış değeri ile kullanılabilir. Uygulamanız bu değerinden bir sertifika oluşturur ve uygulamanızda herhangi bir kimlik doğrulama ve yetkilendirme amacıyla kullanın.

## <a name="special-considerations-for-certificate-validation"></a>Sertifika doğrulama için özel hususlar
Uygulamaya gönderilen istemci sertifikası Azure Web Apps platformu tarafından herhangi bir doğrulama geçmez. Bu sertifika doğrulama web uygulaması sorumluluğundadır. Burada, kimlik doğrulama amacıyla sertifika özellikleri doğrular örnek ASP.NET kodu verilmiştir.

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
