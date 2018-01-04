---
title: "Görünüm erişim denetimi hizmeti (Java) tarafından döndürülen SAML"
description: "Azure üzerinde barındırılan Java uygulamalarını erişim denetim Hizmeti'nde tarafından döndürülen SAML görüntülemeyi öğrenin."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mtillman
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: d239145806be19d2199314fa351d1121f52203c8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Azure erişim denetimi hizmeti tarafından döndürülen SAML görüntüleme
Bu kılavuz temel güvenlik onaylama işlemi biçimlendirme dili (uygulamanızı Azure erişim denetimi Hizmeti'nden (ACS) tarafından döndürülen SAML) görüntülemek nasıl yapacağınızı gösterir. Kılavuzu derlemeler [kimlik doğrulaması Web kullanıcıları Azure erişim denetimi hizmeti kullanılarak Eclipse ile nasıl](active-directory-java-authenticate-users-access-control-eclipse.md) SAML bilgilerini görüntüler kod sağlayarak konu. Tamamlanan uygulama aşağıdakine benzer görünecektir.

![Örnek SAML çıktı][saml_output]

ACS hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next_steps) bölümü.

> [!NOTE]
> Azure Erişim Hizmetleri Denetim filtresi topluluk teknoloji önizlemesidir. Yayın öncesi yazılım olarak bunu resmi olarak Microsoft tarafından desteklenmiyor.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu Kılavuzu'ndaki görevleri tamamlamak için örnek: tamamlamak [kimlik doğrulaması Web kullanıcıları Azure erişim denetimi hizmeti kullanılarak Eclipse ile nasıl](active-directory-java-authenticate-users-access-control-eclipse.md) ve Bu öğretici için başlangıç noktası olarak kullanın.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Derleme yolu ve dağıtım derlemesi için JspWriter kitaplığı Ekle
İçeren kitaplığı eklemek **javax.servlet.jsp.JspWriter** derleme yolu ve dağıtım derleme sınıfı. Tomcat kullanıyorsanız, kitaplığıdır **jsp api.jar**, Apache bulunan **lib** klasör.

1. Eclipse'nın Proje Gezgini'nde sağ **MyACSHelloWorld**, tıklatın **yapı yolu**,'ı tıklatın **oluşturma yolunu Yapılandır**,'ı tıklatın **kitaplıkları**sekmesini ve ardından **dış Jar'lar Ekle**.
2. İçinde **JAR seçimi** iletişim kutusunda, gerekli JAR gidin, onu seçin ve ardından **açık**.
3. İle **MyACSHelloWorld özelliklerini** iletişim hala açık tıklatın **dağıtım derleme**.
4. İçinde **Web dağıtımı derleme** iletişim kutusunda, tıklatın **Ekle**.
5. İçinde **yeni derleme yönergesi** iletişim kutusunda, tıklatın **Java derleme yolu girişleri** ve ardından **sonraki**.
6. Uygun kitaplığı seçin ve tıklatın **son**.
7. Tıklatın **Tamam** kapatmak için **MyACSHelloWorld özelliklerini** iletişim.

## <a name="modify-the-jsp-file-to-display-saml"></a>SAML görüntülenecek JSP dosyası değiştirme
Değiştirme **index.jsp** aşağıdaki kodu kullanmak için.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-the-application"></a>Uygulamayı çalıştırma
1. Uygulamanızı bilgisayar öykünücüsünde çalıştırın veya adresinde belgelenen adımları kullanarak Azure dağıtmak [kimlik doğrulaması Web kullanıcıları Azure erişim denetimi hizmeti kullanılarak Eclipse ile nasıl](active-directory-java-authenticate-users-access-control-eclipse.md).
2. Bir tarayıcıyı başlatın ve web uygulamasını açın. Uygulamanıza oturum açtıktan sonra kimlik sağlayıcısı tarafından sağlanan güvenlik onaylama işlemi SAML bilgilerini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
Biraz daha ACS'ın işlevselliği inceleyin ve daha karmaşık senaryolarıyla denemeler yapmak için bkz: [erişim denetimi hizmeti 2.0][Access Control Service 2.0].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How to Authenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
