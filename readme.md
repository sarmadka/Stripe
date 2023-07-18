# Stripe
[[عربي]](readme.ar.md)

An API client library for Stripe payment gateway.

## Adding to the Project

We can install this library using the following statements:

```
import "Apm";
Apm.importFile("Alusus/Stripe");
```

## Example

```
import "Apm";
Apm.importFile("Alusus/Stripe");
use Stripe;

// Inistantiate the client.
def client: Stripe.Client(String("my_test_key"));

// Variable to hold all the balance of stripe account. 
def balanceArray : Array[SrdRef[Balance]];
// Get the balance. 
balanceArray = client.getBalance();

// Print the balance array.
def j: Int;
for j = 0, j < balanceArray.getLength(), j++ {
    Console.print(
        "type: %s\ncurrency: %s\namount: %f\ndetailed source:\n",
        balanceArray(j).balanceType.buf, balanceArray(j).currency.buf, balanceArray(j).amount
    );
    def i: Int;
    for i = 0, i < balanceArray(j).sources.getLength(), i++ {
        Console.print(
            "\tsource type: %s\n\tsource amount: %f\n",
            balanceArray(j).sources(i).sourceType.buf, balanceArray(j).sources(i).amount
        )
    }
    Console.print("\n");
}
```

## Classes

### Customer 

This class holds the properties of a customer from Stripe API.

```
class Customer {
    def id: String;
    def address: SrdRef[Address];
    def balance: Float = 0;
    def defaultSourceId: String;
    def delinquent: Bool;
    def created: Int[64];
    def email: String;
    def invoicePrefix: String;
    def preferredLocales: Array[String];
    def name: String;
    def phone: String;
    def shipping: String;
    def taxExempt: String;
    def testClock: String;
    def nextInvoiceSequence: Int;
    def currency: String;
    def description: String;


    handler this~init();

    handler this~init(id: String, email: String, name: String, preferredLocales: Array[String]);

    handler this~init(
        id: String, address: SrdRef[Address], balance: Float, created: Int[64],
        currency: String, defaultSourceId: String, delinquent: Bool,
        description: String, email: String, invoicePrefix: String, name: String,
        nextInvoiceSequence: Int, phone: String, preferredLocales: Array[String],
        shipping: String, taxExempt: String, testClock: String
    );
}
```

### Address

Holds physical address information.

```
class Address {
    def city: String;
    def country: String;
    def line1: String;
    def line2: String;
    def postalCode: String;
    def state: String;

    handler this~init();

    handler this~init(city: String, country: String, line1: String, line2: String, postalCode: String, state: String);
}
```

### Balance

This class holds the details of a balance object from Stripe API.

```
class Balance {
    def amount: Float = 0;
    def currency: String = "usd";
    def sources: Array[SrdRef[Source]];
    def balanceType: String;

    handler this~init() {
    }

    handler this~init(amount: Float, currency: String, sources: Array[SrdRef[Source]], balanceType: String);
}
```

`sources` The sources of the balance (bank_account, card).

`balanceType` The type of the balance (available, pending).

### BalanceTransaction

This class holds the properties of a balance transaction object from Stripe API.

```
class BalanceTransaction {
    def id: String;
    def amount: Float = 0;
    def availableOn: Int[64];
    def created: Int[64];
    def currency: String = "usd";
    def description: String;
    def exchangeRate: Float;
    def fee: Float;
    def feeDetails: Array[String];
    def net: Float = 0;
    def reportingCategory: String;
    def source: String;
    def status: String;
    def sourceType: String;


    handler this~init();

    handler this~init(
        id: String, amount: Float, availableOn: Int[64], created: Int[64],
        currency: String, description: String, exchangeRate: Float, fee: Float,
        feeDetails: Array[String], net: Float, reportingCategory: String,
        source: String, status: String, sourceType: String
    );
}
```

`id` The id of the BalanceTransaction object from Stripe.

`amount` The amount of the transaction.

`availableOn` The time this amount will be available.

`fee` The fee on this transaction.

`exchangeRate` The exchange rate of the transaction.

### CheckoutSession

This class holds the properties of a checkout object from Stripe API.

```
class CheckoutSession {
    def id: String;
    def cancelUrl: String;
    def uniqeId: String;
    def currency: String = "usd";
    def customerId: String;
    def lineItems: String;
    def mode: String;
    def paymentStatus: String;
    def status: String;
    def successUrl: String;
    def url: String;
    def amountTotal: String;


    handler this~init();

    handler this~init(
        id: String, uniqeId: String, cancelUrl: String, successUrl: String, url: String,
        currency: String, customerId: String, lineItems: String,
        mode: String, paymentStatus: String, status: String, amountTotal: String
    );
}
```

`id` The ID of the checkout object from Stripe.

`amountTotal` The total amount of the transaction.

`url` The checkout url.

`successUrl` The url to redirect to if the checkout is successful.

`cancelUrl` The url to redirect to if the checkout is canceled.

### Source

Holds the info of a single source contributing to a balance.

```
class Source {
    def amount: Float = 0;
    def sourceType: String = "card"; // "card" or "bank_account"

    handler this~init();

    handler this~init(amount: Float, sourceType: String);
}
```

### Client

This class has methods for all available calls to Stripe. It can be initialized with a Stripe API key:

```
handler this~init(k: String);
```

It has the following methods:

#### getCustomers

```
handler this.getCustomers(): Array[SrdRef[Customer]]
```

Returns all customers.

#### getCustomer

```
handler this.getCustomer(id: String): SrdRef[Customer]
```

Returns a specific customer by its ID.

#### createCustomer

```
handler this.createCustomer(parameters: String): String
```

Creates a customer entry.

`parameters` the parametes of customer object in the form "email=super211@gmail.com&name=sonicmrah".

Returns the customer's id.

#### getBalance

```
handler this.getBalance(): Array[SrdRef[Balance]]
```

Returns the balance of all the accounts owned by the API key.

#### getBalanceTranasactions

```
handler this.getBalanceTranasactions(): Array[SrdRef[BalanceTranasaction]]
```

Returns a list of transactions that have contributed to the Stripe account balance (e.g., charges,
transfers, and so forth).

#### getBalanceTranasaction

```
handler this.getBalanceTranasaction(id: String): SrdRef[BalanceTranasaction]
```

Retrieves the balance transaction with the given ID.

#### getCheckoutSessions

```
handler this.getCheckoutSessions(): Array[SrdRef[CheckoutSession]] 
```

Returns a list of Checkout Sessions.

#### getCheckoutSession

```
handler this.getCheckoutSession(sessionId: String): SrdRef[CheckoutSession]
```

Gets a specific checkout session by its ID.

#### createCheckoutSession

```
handler this.createCheckoutSession(parameters: String): String
```

Creates a checkout session.

`parameters` The parametes of the checkout session in the form: "customer=customerID&line_items=planID".

Returns CheckoutSession id.

