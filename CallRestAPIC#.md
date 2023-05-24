
# How to call rest api in raw json payload in .net core in c#

Date: May 24, 2023
Tags: .net core, c#, chatGPT, json, notion

```
Show me the code to call rest api in post http method in .net core in c# , make sure  add the snippet in the code to pass in the header parameters     
and also show me how to pass the json payload as request, also show me how to check the http status and navigate through the response json
```

## How to call rest api in .net core in C#  - by notion AI.

Here is an example of how to call a REST API in C# using the `HttpClient` class:

```
using System;
using System.Net.Http;
using System.Threading.Tasks;

public class Program
{
    public static async Task Main()
    {
        using var client = new HttpClient();
        var response = await client.GetAsync("<https://jsonplaceholder.typicode.com/posts/1>");
        var content = await response.Content.ReadAsStringAsync();
        Console.WriteLine(content);
    }
}

```

In this example, we create a new instance of the `HttpClient` class and use its `GetAsync` method to make an HTTP GET request to the specified URL. We then use the `ReadAsStringAsync` method of the response's `Content` property to read the response body as a string.

Replace the URL with the one you need to call. You can also modify the HTTP method and add headers or request body as needed.

Note that this code requires the `System.Net.Http` namespace to be imported, and that it is using the C# 8.0 feature of using declarations to automatically dispose of the `HttpClient` instance when it goes out of scope.

```json
can you help to add the snippet in the above code to pass in the header parameters if there is any, also show me how to check the http status and navigate through the response json
```

Here's an example of how to make a POST request with headers and a JSON payload using `HttpClient` in .NET Core:

```
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

public class Program
{
    private static readonly HttpClient client = new HttpClient();

    public static async Task Main()
    {
        string url = "<https://example.com/api/endpoint>";
        string json = "{\\"key\\":\\"value\\"}";
        var content = new StringContent(json, Encoding.UTF8, "application/json");

        client.DefaultRequestHeaders.Add("Authorization", "Bearer <insert-token-here>");
        client.DefaultRequestHeaders.Add("User-Agent", "MyHttpClient");

        try
        {
            HttpResponseMessage response = await client.PostAsync(url, content);
            response.EnsureSuccessStatusCode(); // throws exception if not successful
            string responseBody = await response.Content.ReadAsStringAsync();
            Console.WriteLine(responseBody); // handle response here
        }
        catch(HttpRequestException e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
    }
}

```

In this example, we create a `HttpClient` instance and set the `Authorization` and `User-Agent` headers using the `DefaultRequestHeaders` property. We then create a JSON payload as a string and wrap it in a `StringContent` object, specifying the content type as `application/json`.

To make the request, we call the `PostAsync` method of the `HttpClient` instance, passing in the URL and content. We then call the `EnsureSuccessStatusCode` method of the response to ensure that the request was successful. If the request was successful, we read the response body as a string using the `ReadAsStringAsync` method of the response's `Content` property.

Replace the URL with the one you need to call, and modify the `Authorization` and `User-Agent` headers as needed. You can also modify the content type and payload as needed.

Note that this code requires the `System.Net.Http` and `System.Text` namespaces to be imported, and that it is using the C# 7.1 feature of async Main method.

To navigate through the response JSON, you can use a JSON parsing library such as `Newtonsoft.Json`. Here's an example of how to do this:

```
using Newtonsoft.Json.Linq;

// ...

string responseBody = await response.Content.ReadAsStringAsync();
JObject responseJson = JObject.Parse(responseBody);
string value = (string)responseJson["key"];

```

In this example, we read the response body as a string and parse it into a `JObject` using the `JObject.Parse` method from `Newtonsoft.Json`. We can then access values in the JSON using the indexing operator, casting the value to the appropriate type.

Replace `"key"` with the path to the value you want to extract from the JSON response.

Note that you will need to add a reference to the `Newtonsoft.Json` NuGet package to use this code.

## Answer by ChatGPT

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

public class Program
{
    public static async Task Main(string[] args)
    {
        // Create an HttpClient instance
        using (HttpClient httpClient = new HttpClient())
        {
            // Set the base URL of the API
            httpClient.BaseAddress = new Uri("https://api.example.com");

            // Set the headers (if required)
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer your_token");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            // Prepare the JSON payload
            var jsonPayload = "{\"name\": \"John\", \"age\": 30}";

            // Create the HttpContent from the JSON payload
            HttpContent content = new StringContent(jsonPayload);

            // Set the content type of the request
            content.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            // Send the POST request
            HttpResponseMessage response = await httpClient.PostAsync("/api/endpoint", content);

            // Check the HTTP status code
            if (response.IsSuccessStatusCode)
            {
                // Read the response as JSON
                string responseJson = await response.Content.ReadAsStringAsync();

                // Process the response JSON
                // Example: Get a specific property from the JSON
                var result = Newtonsoft.Json.JsonConvert.DeserializeObject<dynamic>(responseJson);
                string value = result.propertyName;

                Console.WriteLine("API call successful!");
                Console.WriteLine($"Response JSON: {responseJson}");
                Console.WriteLine($"Value: {value}");
            }
            else
            {
                Console.WriteLine("API call failed!");
                Console.WriteLine($"Status Code: {response.StatusCode}");
            }
        }
    }
}
```

Here's an example of how to call PayPal gateway API in C# using the PayPal SDK:

```csharp
using System;
using PayPalCheckoutSdk.Core;
using PayPalCheckoutSdk.Orders;
using PayPalHttp;

public class PayPalGatewayExample
{
    public async void CreateOrder()
    {
        // Set up the PayPal environment
        var environment = new SandboxEnvironment("client_id", "client_secret");
        var client = new PayPalHttpClient(environment);

        // Build the request body
        var orderReq = new OrderRequest
        {
            CheckoutPaymentIntent = "CAPTURE",
            PurchaseUnits = new List<PurchaseUnitRequest>
            {
                new PurchaseUnitRequest
                {
                    AmountWithBreakdown = new AmountWithBreakdown
                    {
                        CurrencyCode = "USD",
                        Value = "100.00"
                    }
                }
            }
        };

        try
        {
            // Call the PayPal API to create the order
            var request = new OrdersCreateRequest();
            request.RequestBody(orderReq);
            var response = await client.Execute(request);

            // Parse the response
            var statusCode = response.StatusCode;
            var responseBody = response.ResponseBody;
            var orderId = responseBody.Result<PayPalCheckoutSdk.Orders.Order>().Id;

            Console.WriteLine($"Order created with ID: {orderId}");
        }
        catch (HttpException ex)
        {
            Console.WriteLine($"Error calling PayPal API: {ex.Message}");
        }
    }
}

```

In this example, we first set up the PayPal environment using the client ID and secret for our PayPal app. We then build the request body for the order we want to create, specifying the payment intent and purchase units with their corresponding amounts.

To call the PayPal API, we use the `PayPalHttpClient` class and the `OrdersCreateRequest` class from the PayPal SDK. We pass the request body to the `RequestBody` method of the `OrdersCreateRequest` instance, and then call the `Execute` method of the `PayPalHttpClient` instance to send the request.

If the request is successful, we parse the response and extract the order ID from the response body.

Note that this code requires the `PayPalCheckoutSdk` and `PayPalHttp` namespaces to be imported, and that you will need to replace the `client_id` and `client_secret` parameters with your own PayPal app credentials.

For more information on how to use the PayPal SDK in C#, see [the official documentation](https://developer.paypal.com/docs/checkout/reference/server-integration/setup-sdk/#step-2-set-up-the-paypal-sdk).

```csharp
how to add reference to paypal sdk to .net core , 
```

To add a reference to the PayPal SDK in a .NET Core project, follow these steps:

1. In Visual Studio, open the Solution Explorer and right-click on the project you want to add the reference to.
2. Select "Manage NuGet Packages" from the context menu.
3. In the NuGet Package Manager window, search for "PayPal Checkout SDK".
4. Select the package from the search results and click "Install".
5. Wait for the package to be installed, and then close the NuGet Package Manager window.
6. You can now use the classes and methods from the PayPal SDK in your project.

Note that you may need to add a using statement for the `PayPalCheckoutSdk` namespace in your code to access the classes and methods from the SDK.
