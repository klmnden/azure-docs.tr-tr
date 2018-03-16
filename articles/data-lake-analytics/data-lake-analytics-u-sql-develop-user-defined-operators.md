---
title: "U-SQL kullanıcı tanımlı işleçler (Udo'lar) geliştirme | Microsoft Docs"
description: "Kullanıcı tanımlı işleçler kullanılan ve Data Lake Analytics işleri yeniden için geliştirmeyi öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: kfile
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: 7c0b9c193f8f1c3a3043824186e337f942ebfd56
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>U-SQL kullanıcı tanımlı işleçler (Udo'lar) geliştirin
Kullanıcı tanımlı işleçler bir U-SQL işi verileri işlemek için geliştirmeyi öğrenin.

U-SQL için genel amaçlı derlemeleri geliştirme ile ilgili yönergeler için bkz: [Azure Data Lake Analytics işleri için U-SQL geliştirmek derlemeleri](data-lake-analytics-u-sql-develop-assemblies.md)

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Tanımlama ve U-SQL kullanıcı tanımlı bir işleç kullanma
**Oluşturmak ve U-SQL işi göndermek için**

1. Visual Studio seçin gelen **Dosya > Yeni > Proje > U-SQL projesi**.
2. **Tamam**’a tıklayın. Visual Studio Script.usql dosyası ile çözüm oluşturur.
3. Gelen **Çözüm Gezgini**Script.usql genişletin ve ardından **Script.usql.cs**.
4. Dosyasına aşağıdaki kodu yapıştırın:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Açık **Script.usql**, aşağıdaki U-SQL betiğini yapıştırın:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Data Lake Analytics hesabını, Veritabanı'nı ve Şema'yı belirtin.
8. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın.
9. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın.
10. Azure aboneliğinize bağlanmadıysanız, Azure hesabı kimlik bilgilerinizi girmeniz istenir.
11. Tıklatın **gönderme**. Gönderim tamamlandığında Sonuçları penceresinde, gönderme işleminin sonuçları ve iş bağlantısı da kullanılabilir.
12. Tıklatın **yenileme** en son iş durumu ve ekranı yenilemek görmek için düğmesi.

**Çıkışı görmek için**

1. Gelen **Sunucu Gezgini**, genişletin **Azure**, genişletin **Data Lake Analytics**, Data Lake Analytics hesabınızı genişletin, **depolama hesapları**, varsayılan depolama sağ tıklayın ve ardından **Explorer**.
2. Örnekleri genişletin, çıkışları genişletin ve ardından **sürücüler.csv**.

## <a name="see-also"></a>Ayrıca bkz.
* [U-SQL kullanıcı kodu ifadelerle genişletme](https://msdn.microsoft.com/en-us/library/azure/mt621316.aspx)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanın](data-lake-analytics-data-lake-tools-get-started.md)
