---
layout: post
title: Architecture of PDF generator
categories: [pdf-service]
tags: [architecture,http-clients,document-path,input-data,micro-service]
---

# Diagram

![image](/assets/images/arch1.png)

---
## Node vs Nest vs Micro-Service
(#10) Nest.js is a wonderful backend library layered on top of Node. Nest.js is modeled after Angular's constructs and use of decorators but applied at the backend. It will greatly structure your code to be more understandable and neat. <br>

The entire PDF generator is a great example of creating a really sophisticated Micro Service which only job is taking a complex JSON tree, fusing it will a collection of pre-built templates, generating a final HTML and rendering it in PDF.
## HTTP Client & Headers
On the left side, it indicates a HTTP client, such as POSTMAN or a simple CURL command will contact the PDF generator via a RESTFUL API (e.g. CURL -x POST https://localhost/pdf). The caller needs to prepare the right HTTP Headers and a JSON tree encoded in the POST Body. A example of the headers are self-explanatory as follows: ![image](/assets/images/headers.png)

## Path of the document templates
The **path** header specifies a local directory accessible by Node and it can container multiple types of documents and multiple versions. These directories consist of **Handlebars Partial** in the form of *.hbs files. The directory can also include a **code/ext.ts** file that contains **Handlebars Helper Functions** specific to the document and not shareable elsewhere:<br>
<img src="/assets/images/document.png"  style="width:200px;"/>

## Data needed for rendering
The data need to render the PDF is supplied in the body of the HTTP POST. An example can be as follows;
```json
  {
    "data":{
        "company": {
            "address": "181 Suburban Road, San Luis OBsipo, CA 93401",
            "phone": "800-883-6647",
            "fax": "800-883-6648",
            "invoiceNum": "32321",
            "image2": "http://localhost:8000/rf2.png",
            "image":"http://localhost:8000/rf1.png"
        },
        "recipient":{
            "to":{
                "name": "John Donahoe",
                "companyName": "Nike Inc.",
                "streetAddress":"One Bowerman Dr",
                "city":"Beaverton",
                "state":"OR",
                "zip":"97005",
                "phone":"(503) 671-6453"

            },
            "shipTo":{
                "name": "Nike Company Store",
                "companyName": "Nike Inc.",
                "streetAddress":"3485 SW Knowlton Rd",
                "city":"Beaverton",
                "state":"OR",
                "zip":"97005",
                "phone":"(503) 671-1600"
            },
            "instruction":"Rush delivery, including weekends"
        },

        "purchased":{
            "items": [
                {
                    "quantity": 10,
                    "description": "Wilson Pro Staff RF 97 V13 Federer Autograph Tennis Racquet - Quality String (4-1/2) RF97",
                    "unitPrice": 279.00
                },
                {
                    "quantity": 20,
                    "description": "Roger's T12 Grey/Mint",
                    "unitPrice": 89.00
                }
                ,
                {
                    "quantity": 150,
                    "description": "Roger That Shirt - Funny Tennis T Shirt",
                    "unitPrice": 16.99
                },
                {
                    "quantity": 25,
                    "description": "ON The Roger Clubhouse Metal/Black Men's Shoe",
                    "unitPrice": 149.99
                },
                {
                    "quantity": 100,
                    "description": "Roger Federer Trucker Hat for Men Women Medium Profile Adjustable Classic Tennis",
                    "unitPrice": 15.99
                },
                {
                    "quantity": 75,
                    "description": "Roger Federer: The Biography Hardcover",
                    "unitPrice": 30.71
                },
                {
                    "quantity": 210,
                    "description": "Letter Embroidery 3D F Dad Hat Tennis Star Roger Federer Baseball Cap Black Adult Size",
                    "unitPrice": 16.00
                },
                {
                    "quantity": 320,
                    "description": "Welerony Home Coffee Mug Roger-federer-logo Interesting 330ml Mug Ceramic Coffee Mug Teacup",
                    "unitPrice": 19.88
                },
                {
                    "quantity": 1100,
                    "description": "Phone Case Roger Federer Compatible with iPhone 12/13 Pro Max 13 Mini 11 Pro max XR SE 2020/7/8 X/Xs 7 8 6/6S Plus Samsung S21+ Ultra",
                    "unitPrice": 21.00
                },
                {
                    "quantity": 37,
                    "description": "Wilson RF DNA 12 pack Tennis Bag - Black",
                    "unitPrice": 7.99
                },
                {
                    "quantity": 70,
                    "description": "Roger Federer: The Greatest",
                    "unitPrice": 199.00
                },
                {
                    "quantity": 430,
                    "description": "Roger Federer ArcDecals78601050 Set of Two (2X), Decal, Sticker, Laptop, Ipad, Car, Truck",
                    "unitPrice": 4.90
                }
                ,
                {
                    "quantity": 1100,
                    "description": "ROGER FEDERER RF Tennis Racket Vibration Dampeners (4 Pack), Decal, Sticker, Laptop, Ipad, Car, Truck",
                    "unitPrice": 14.99
                }
                ,
                {
                    "quantity": 35,
                    "description": "Uniqlo ROGER FEDERER DRY-EX POLO SHIRT MEN (S-XL) 2021 Qatar Dubai Open NWT NEW",
                    "unitPrice": 69.00
                }
                ,
                {
                    "quantity": 40,
                    "description": "UNIQLO 2021 Roger Federer French Open Dry-EX Shorts (WHITE) USA",
                    "unitPrice": 89.99
                }
                ,
                {
                    "quantity": 2,
                    "description": "New Wilson Pro Staff 97 Autograph Roger Federer RF97 4 3/8 Racket BLACK",
                    "unitPrice": 500
                }
                ,
                {
                    "quantity": 25,
                    "description": "Uniqlo Roger Federer Hat Cap 2021 French OPEN Brand New Tennis",
                    "unitPrice": 45
                },
                   {
                    "quantity": 24,
                    "description": "Uniqlo Roger Federer French Open 2021 White Red Tennis Shorts - New with tags RF",
                    "unitPrice": 85
                }
                ,
                {
                    "quantity": 89,
                    "description": "UNIQLO RF ROGER FEDERER WIMBLEDON TENNIS SOCKS 1 PAIR NWT",
                    "unitPrice": 24.99
                }
            ],
            "salesTax": 0.07,
            "shippingHandling": 1119
        }
    },
    "kind":"sales.invoice.v1"
}
```
## Next ...
In the next few postings, I will detail the sequence of actions, step #1 to step #9, once the HTTP enter into the PDF generator until a PDF is returned to the calling client.