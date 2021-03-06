
[![Build Status](https://travis-ci.com/netpay-ecommerce/netpay_ecommerce_python.svg?branch=master)](https://travis-ci.com/netpay-ecommerce/netpay_ecommerce_python)
![README Cover Image](img/python.png)

# Netpay Ecommerce Python - SDK Python

Bienvenido a la documentación para usar los servicios de Netpay Ecommerce interactuando con scripts de python.

Instalacion
===============

```sh
pip install netpay-ecommerce-python
```

Uso
================

Para utilizar los servicios es indispensable tener el token de autorización, para ello se utiliza el objeto AuthJwt, la respuesta contiene un array con un hash de respuesta y el código de respuesta del servicio:

```python

import netpay_ecommerce

login_object = {
    'security': {
        "userName": "adrian@netpay.com",
        "password": "adm0n2"
    }
}

response = netpay_ecommerce.AuthJwt.create(login_object)

token = response["token"]
##eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ7XCJpZFwiOjI4NzAxLFwic3RvcmVJZFwiOjQwOTQ0LFwic3RvcmVJZEFjcVwiOlwiNTI5Mzk1XCIsXCJuYW1lXCI6XCJQT1NcIixcInVz...
```

Con el token ya es posible tener acceso a los demas servicios.

### Tokenizacion de la tarjeta

Para poder realizar transacciones, el primer paso es crear un token de la tarjeta con la cual se realizara la transacciones. Para esto haremos uso del objeto TokenCard.

```python
import netpay_ecommerce

jwt_token = "json web token"

token_card_object = {
    "username": "adrian@netpay.com",
    "storeApiKey": "6kQui=_ZJ4r15GRT5ix7Pot_sCObk1WG",
    "customerCard": {
        "cardNumber": "4000000000000004",
        "expirationMonth": "01",
        "expirationYear":  "24",
        "cvv": "123",
        "cardType": "001",
        "cardHolderName": "John Doe"
    }
}

response = netpay_ecommerce.TokenCard.create(data=token_card_object,token=jwt_token)

# Acceso a la tarjeta tokenizada
response["response"]["customerToken"]["token"]["publicToken"]
# GSlvaCAueF+Wl4zVg/ReUdWKeRKriNoSeDtISAqqdGPx4pEzy8t/3ckYPxJgfFdFIF9JNIm6sVHY3B+dbt8txg==
```

### Servicio de Risk Manager

El Risk Manager evaluara el riesgo de la trasaccion y nos entregara un status de la transaccion. Eso lo realizaremos con el objeto RiskManager

```python
import netpay_ecommerce

jwt_token = "json web token"

risk_manager_object = {  
    "storeApiKey":"6kQui=_ZJ4r15GRT5ix7Pot_sCObk1WG",
    "riskManager":{
        "promotion":"000000",
        "requestFraudService":{  
            "merchantReferenceCode":"14500049450000",
            "deviceFingerprintID":"086e46a7d7b0d22a8966feed39eefd1c881507939638",
            "bill":{  
            "city":"city",
            "country":"MX",
            "firstName":"mailto",
            "lastName":"mailto",
            "email":"accept@netpay.com.mx",
            "phoneNumber":"8110000011",
            "postalCode":"12345",
            "state":"state",
            "street1":"street 1",
            "street2":"street 2",
            "ipAddress":"10.0.0.1"
            },
            "ship":{  
            "city":"city ship",
            "country":"MX",
            "firstName":"mailto",
            "lastName":"mailto",
            "phoneNumber":"8110111111",
            "postalCode":"12345",
            "state":"state",
            "street1":"street 1",
            "street2":"street 2",
            "shippingMethod":"flatrate_flatrate"
            },
            "itemList":[{  
                "id":"421",
                "productSKU":"wbk012c-Royal Blue-S",
                "unitPrice":"1.0000",
                "productName":"Elizabeth Knit Top",
                "quantity":1,
                "productCode":"Tops & Blouses"
            }
            ],
            "card":{  
            "cardToken":"VW2FdHVU6BXGoiR99teVRKnuvIE897u7AMlh8MVHprBr4/LHIv6r7Nn0SwNEfsEfx8i7ngLOZyEL+eLsKoZCGg=="
            },
            "purchaseTotals":{  
            "grandTotalAmount":"6",
            "currency":"MXN"
            },
            "merchanDefinedDataList":[{  
                "id":2,
                "value":"Web"
            },{  
                "id":4,
                "value":"515"
            },{  
                "id":5,
                "value":"0"
            },{  
                "id":6,
                "value":"0"
            },{ 
                "id":7,
                "value":"0"
            },{  
                "id":9,
                "value":"Retail"
            },{  
                "id":10,
                "value":"3D"
            },{  
                "id":11,
                "value":"flatrate_flatrate"
            },{  
                "id":13,
                "value":"N"
            },{  
                "id":14,
                "value":"Domicilio"
            },{  
                "id":"16",
                "value":"50000"
            }]
        }
    }   
}

response = netpay_ecommerce.RiskManager.create(data=token_card_object,token=jwt_token)

response["transactionTokenId"] 
# 138f8b75-3d13-4fff-ac8b-29da3f99b851

# Estatus
response["status"]
# CHARGEABLE 
```

Endpoints Disponibles
=====================

```python
netpay_ecommerce.AuthJwt.create()
netpay_ecommerce.TokenCard.create()
netpay_ecommerce.CustomerTokens.create()
netpay_ecommerce.RiskManager.create()
netpay_ecommerce.WebAuthorizer.create()
netpay_ecommerce.Transaction.create()
netpay_ecommerce.TransactionRefund.create()
netpay_commerce.TokenCardDelete.create()
```







