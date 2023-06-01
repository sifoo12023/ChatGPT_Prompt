# EPG project - Paypal Rest API - part 3

Date: June 1, 2023
Tags: AAA, Prompt, automation, c#, chatGPT, epg, json, rest api

# Summary:

This document explains how to generate C# data objects based on PayPal REST API's request/response JSON payloads. Initially, I tried to use Notion AI to generate the data objects by providing prompts to guide the object creation, based on a template data class from Swagger. However, I later discovered an [online tool](https://json2csharp.com/) that provides similar output.

My other reason for using Notion AI was to explore the ability of AI code generation. One benefit is that I hardly need to use Google or Stack Overflow for reference.

# Using online tool to generate the code

1. go to [https://json2csharp.com/](https://json2csharp.com/)
2. paste in the json file in the left: 
    
    ```json
    {
        "Class1":{
            "id":4,
            "user_id":"user_id_value",
            "awesomeobject":{
            "SomeProps1":1,
            "SomeProps2":"test"
        },
        "created_at":"2015-06-02 23:33:90",
        "updated_at":"2015-06-02 23:33:90",
        "users":[
            {
            "id":"6",
            "name":"Test Child 1",
            "created_at":"2015-06-02 23:33:90",
            "updated_at":"2015-06-02 23:33:90",
            "email":"test@gmail.com"
            },
            {
            "id":"6",
            "name":"Test Child 1",
            "created_at":"2015-06-02 23:33:90",
            "updated_at":"2015-06-02 23:33:90",
            "email":"test@gmail.com",
            "testanadditionalfield":"tet"
            } ]
        },
        "Class2":{
        "SomePropertyOfClass2":"SomeValueOfClass2"
        }
    }
    ```
    
3. Choose the following settings: 
    
    ![Untitled](EPG%20project%20-%20Paypal%20Rest%20API%20-%20part%203%2080175615ea464c1b88cf13f5e0384e33/Untitled.png)
    
4. Click convert to get the c# code. 
    
    ```csharp
    public class Awesomeobject
        {
            [JsonProperty("SomeProps1", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("SomeProps1")]
            public int? SomeProps1 { get; set; }
    
            [JsonProperty("SomeProps2", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("SomeProps2")]
            public string SomeProps2 { get; set; }
        }
    
        public class Class1
        {
            [JsonProperty("id", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("id")]
            public int? id { get; set; }
    
            [JsonProperty("user_id", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("user_id")]
            public string user_id { get; set; }
    
            [JsonProperty("awesomeobject", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("awesomeobject")]
            public Awesomeobject awesomeobject { get; set; }
    
            [JsonProperty("created_at", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("created_at")]
            public string created_at { get; set; }
    
            [JsonProperty("updated_at", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("updated_at")]
            public string updated_at { get; set; }
    
            [JsonProperty("users", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("users")]
            public List<User> users { get; set; }
        }
    
        public class Class2
        {
            [JsonProperty("SomePropertyOfClass2", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("SomePropertyOfClass2")]
            public string SomePropertyOfClass2 { get; set; }
        }
    
        public class Root
        {
            [JsonProperty("Class1", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("Class1")]
            public Class1 Class1 { get; set; }
    
            [JsonProperty("Class2", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("Class2")]
            public Class2 Class2 { get; set; }
        }
    
        public class User
        {
            [JsonProperty("id", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("id")]
            public string id { get; set; }
    
            [JsonProperty("name", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("name")]
            public string name { get; set; }
    
            [JsonProperty("created_at", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("created_at")]
            public string created_at { get; set; }
    
            [JsonProperty("updated_at", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("updated_at")]
            public string updated_at { get; set; }
    
            [JsonProperty("email", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("email")]
            public string email { get; set; }
    
            [JsonProperty("testanadditionalfield", NullValueHandling = NullValueHandling.Ignore)]
            [JsonPropertyName("testanadditionalfield")]
            public string testanadditionalfield { get; set; }
        }
    ```
    

# Notion AI code generation

## Step 1: Prepare the following prompt to guide the notion AI for code generation.

```
**Generate c# classe based on json structure below and follow the c# code template provided:** 
c# code: 
namespace IO.Swagger.Models
{
   
    [DataContract]
    public partial class ExpirationDate 
    { 
   
        [DataMember(Name="month")]
        public string Month { get; set; }

        [DataMember(Name="year")]
        public string Year { get; set; }
		}
}

Json sample: 
{
    "id": "2K2819263S889821R",
    "status": "VOIDED",
    "amount": {
        "currency_code": "USD",
        "value": "10.00"
    },
    "seller_protection": {
        "status": "ELIGIBLE",
        "dispute_categories": [
            "ITEM_NOT_RECEIVED",
            "UNAUTHORIZED_TRANSACTION"
        ]
    },
    "expiration_time": "2023-06-29T13:08:37-07:00",
    "create_time": "2023-05-31T13:08:37-07:00",
    "update_time": "2023-05-31T13:08:50-07:00",
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/2K2819263S889821R",
            "rel": "self",
            "method": "GET"
        }
    ]
}
```

In the prompt, you can define the namespace and additional requirement for Json property null value handling/ignore etc. 

## Step 2: Press space key to enable notion AI and copying in the above prompt text and wait

**Here is the generated C# code based on the provided JSON structure and code template:**

```
namespace IO.Swagger.Models
{
    [DataContract]
    public class Authorization
    {
        [DataMember(Name = "id")]
        public string Id { get; set; }

        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "amount")]
        public Amount Amount { get; set; }

        [DataMember(Name = "seller_protection")]
        public SellerProtection SellerProtection { get; set; }

        [DataMember(Name = "expiration_time")]
        public string ExpirationTime { get; set; }

        [DataMember(Name = "create_time")]
        public string CreateTime { get; set; }

        [DataMember(Name = "update_time")]
        public string UpdateTime { get; set; }

        [DataMember(Name = "links")]
        public List<Link> Links { get; set; }
    }

    [DataContract]
    public class Amount
    {
        [DataMember(Name = "currency_code")]
        public string CurrencyCode { get; set; }

        [DataMember(Name = "value")]
        public string Value { get; set; }
    }

    [DataContract]
    public class SellerProtection
    {
        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "dispute_categories")]
        public List<string> DisputeCategories { get; set; }
    }

    [DataContract]
    public class Link
    {
        [DataMember(Name = "href")]
        public string Href { get; set; }

        [DataMember(Name = "rel")]
        public string Rel { get; set; }

        [DataMember(Name = "method")]
        public string Method { get; set; }
    }
}

```

## Step 3: Ask notion AI to separate the classes into individual code block.

At this moment, notion AI doesn't have ability to generate zip file and put each class in individual files. 

```
Prompt: 

can you separate the following c# classes into individual code block :
namespace IO.Swagger.Models
{
    [DataContract]
    public class Authorization
    {
        [DataMember(Name = "id")]
        public string Id { get; set; }

        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "amount")]
        public Amount Amount { get; set; }

        [DataMember(Name = "seller_protection")]
        public SellerProtection SellerProtection { get; set; }

        [DataMember(Name = "expiration_time")]
        public string ExpirationTime { get; set; }

        [DataMember(Name = "create_time")]
        public string CreateTime { get; set; }

        [DataMember(Name = "update_time")]
        public string UpdateTime { get; set; }

        [DataMember(Name = "links")]
        public List<Link> Links { get; set; }
    }

    [DataContract]
    public class Amount
    {
        [DataMember(Name = "currency_code")]
        public string CurrencyCode { get; set; }

        [DataMember(Name = "value")]
        public string Value { get; set; }
    }

    [DataContract]
    public class SellerProtection
    {
        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "dispute_categories")]
        public List<string> DisputeCategories { get; set; }
    }

    [DataContract]
    public class Link
    {
        [DataMember(Name = "href")]
        public string Href { get; set; }

        [DataMember(Name = "rel")]
        public string Rel { get; set; }

        [DataMember(Name = "method")]
        public string Method { get; set; }
    }
}
```

AI generate code

```csharp
namespace IO.Swagger.Models
{
    [DataContract]
    public class Authorization
    {
        [DataMember(Name = "id")]
        public string Id { get; set; }

        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "amount")]
        public Amount Amount { get; set; }

        [DataMember(Name = "seller_protection")]
        public SellerProtection SellerProtection { get; set; }

        [DataMember(Name = "expiration_time")]
        public string ExpirationTime { get; set; }

        [DataMember(Name = "create_time")]
        public string CreateTime { get; set; }

        [DataMember(Name = "update_time")]
        public string UpdateTime { get; set; }

        [DataMember(Name = "links")]
        public List<Link> Links { get; set; }
    }
}

namespace IO.Swagger.Models
{
    [DataContract]
    public class Amount
    {
        [DataMember(Name = "currency_code")]
        public string CurrencyCode { get; set; }

        [DataMember(Name = "value")]
        public string Value { get; set; }
    }
}

namespace IO.Swagger.Models
{
    [DataContract]
    public class SellerProtection
    {
        [DataMember(Name = "status")]
        public string Status { get; set; }

        [DataMember(Name = "dispute_categories")]
        public List<string> DisputeCategories { get; set; }
    }
}

namespace IO.Swagger.Models
{
    [DataContract]
    public class Link
    {
        [DataMember(Name = "href")]
        public string Href { get; set; }

        [DataMember(Name = "rel")]
        public string Rel { get; set; }

        [DataMember(Name = "method")]
        public string Method { get; set; }
    }
}

```

## Step 4: Need to separate json objects into multiple pieces and use the prompt and piece together.

Some of the json payload is to big, notion AI seems to have token issue with it. 

```json
{
    "id": "8TC70130VH043512W",
    "intent": "AUTHORIZE",
    "status": "COMPLETED",
    "payment_source": {
        "paypal": {
            "attributes": {
                "vault": {
                    "id": "kyqbqf4h",
                    "status": "VAULTED",
                    "customer": {
                        "id": "aVHfQBQYdt"
                    },
                    "links": [
                        {
                            "href": "https://api.sandbox.paypal.com/v3/vault/payment-tokens/kyqbqf4h",
                            "rel": "self",
                            "method": "GET"
                        },
                        {
                            "href": "https://api.sandbox.paypal.com/v3/vault/payment-tokens/kyqbqf4h",
                            "rel": "delete",
                            "method": "DELETE"
                        },
                        {
                            "href": "https://api.sandbox.paypal.com/v2/checkout/orders/8TC70130VH043512W",
                            "rel": "up",
                            "method": "GET"
                        }
                    ]
                }
            }
        }
    },
    "purchase_units": [
        {
            "reference_id": "default",
            "amount": {
                "currency_code": "USD",
                "value": "1.00",
                "breakdown": {
                    "item_total": {
                        "currency_code": "USD",
                        "value": "1.00"
                    }
                }
            },
            "payee": {
                "email_address": "sb-473qmy26097592@business.example.com",
                "merchant_id": "6F3LXWP7K227Y"
            },
            "items": [
                {
                    "name": "AAA Membership",
                    "unit_amount": {
                        "currency_code": "USD",
                        "value": "1.00"
                    },
                    "quantity": "1",
                    "description": "Plus"
                }
            ],
            "shipping": {
                "name": {
                    "full_name": "John Doe"
                },
                "address": {
                    "address_line_1": "1 Main St",
                    "admin_area_2": "San Jose",
                    "admin_area_1": "CA",
                    "postal_code": "95131",
                    "country_code": "US"
                }
            },
            "payments": {
                "authorizations": [
                    {
                        "status": "CAPTURED",
                        "id": "83A77775M1190533M",
                        "amount": {
                            "currency_code": "USD",
                            "value": "1.00"
                        },
                        "seller_protection": {
                            "status": "ELIGIBLE",
                            "dispute_categories": [
                                "ITEM_NOT_RECEIVED",
                                "UNAUTHORIZED_TRANSACTION"
                            ]
                        },
                        "expiration_time": "2023-06-25T09:44:55Z",
                        "links": [
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/83A77775M1190533M",
                                "rel": "self",
                                "method": "GET"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/83A77775M1190533M/capture",
                                "rel": "capture",
                                "method": "POST"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/83A77775M1190533M/void",
                                "rel": "void",
                                "method": "POST"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/83A77775M1190533M/reauthorize",
                                "rel": "reauthorize",
                                "method": "POST"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/checkout/orders/8TC70130VH043512W",
                                "rel": "up",
                                "method": "GET"
                            }
                        ],
                        "create_time": "2023-05-27T09:44:55Z",
                        "update_time": "2023-05-27T09:46:11Z"
                    }
                ],
                "captures": [
                    {
                        "id": "08J9435870088674T",
                        "status": "REFUNDED",
                        "amount": {
                            "currency_code": "USD",
                            "value": "1.00"
                        },
                        "final_capture": true,
                        "disbursement_mode": "INSTANT",
                        "seller_protection": {
                            "status": "ELIGIBLE",
                            "dispute_categories": [
                                "ITEM_NOT_RECEIVED",
                                "UNAUTHORIZED_TRANSACTION"
                            ]
                        },
                        "seller_receivable_breakdown": {
                            "gross_amount": {
                                "currency_code": "USD",
                                "value": "1.00"
                            },
                            "paypal_fee": {
                                "currency_code": "USD",
                                "value": "0.52"
                            },
                            "net_amount": {
                                "currency_code": "USD",
                                "value": "0.48"
                            }
                        },
                        "invoice_id": "1685180770",
                        "links": [
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/captures/08J9435870088674T",
                                "rel": "self",
                                "method": "GET"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/captures/08J9435870088674T/refund",
                                "rel": "refund",
                                "method": "POST"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/authorizations/83A77775M1190533M",
                                "rel": "up",
                                "method": "GET"
                            }
                        ],
                        "create_time": "2023-05-27T09:46:11Z",
                        "update_time": "2023-05-27T11:17:26Z"
                    }
                ],
                "refunds": [
                    {
                        "id": "53J85355150636400",
                        "amount": {
                            "currency_code": "USD",
                            "value": "1.00"
                        },
                        "seller_payable_breakdown": {
                            "gross_amount": {
                                "currency_code": "USD",
                                "value": "1.00"
                            },
                            "paypal_fee": {
                                "currency_code": "USD",
                                "value": "0.03"
                            },
                            "platform_fees": [
                                {
                                    "amount": {
                                        "currency_code": "USD",
                                        "value": "0.00"
                                    }
                                }
                            ],
                            "net_amount": {
                                "currency_code": "USD",
                                "value": "0.97"
                            },
                            "total_refunded_amount": {
                                "currency_code": "USD",
                                "value": "1.00"
                            }
                        },
                        "invoice_id": "1685180770",
                        "status": "COMPLETED",
                        "links": [
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/refunds/53J85355150636400",
                                "rel": "self",
                                "method": "GET"
                            },
                            {
                                "href": "https://api.sandbox.paypal.com/v2/payments/captures/08J9435870088674T",
                                "rel": "up",
                                "method": "GET"
                            }
                        ],
                        "create_time": "2023-05-27T04:17:26-07:00",
                        "update_time": "2023-05-27T04:17:26-07:00"
                    }
                ]
            }
        }
    ],
    "create_time": "2023-05-27T09:40:56Z",
    "update_time": "2023-05-27T09:46:11Z",
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v2/checkout/orders/8TC70130VH043512W",
            "rel": "self",
            "method": "GET"
        }
    ]
}
```

We can remove the **Payments/Captures** and **Payments/refunds** section from the original payload and do step 1-3  for code generation