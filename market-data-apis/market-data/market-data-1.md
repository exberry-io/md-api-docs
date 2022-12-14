# Light Tickers

## lightTickers

This API allows easy refresh of the instruments market data.

{% hint style="info" %}
`qualifier:` v1/exchange.marketdata/lightTickers
{% endhint %}

### **Request**

| Parameter | Type      | Description                                                      |
| --------- | --------- | ---------------------------------------------------------------- |
| symbols   | \[]String | List of symbols to retrieve the light tickers for                |
| interval  | eNum      | Response interval in milliseconds, allowed values: 100,1000,2000 |

### **Response**

| Parameter                    | Type           | Description                                                                                             |
| ---------------------------- | -------------- | ------------------------------------------------------------------------------------------------------- |
| symbol                       | String         | Instrument symbol                                                                                       |
| lastPrice                    | Decimal        | last execution price                                                                                    |
| bidPrice                     | Decimal        | Highest bid price                                                                                       |
| bidQuantity                  | Decimal        | Sum of quantity of all orders with `bidPrice`                                                           |
| askPrice                     | Decimal        | Lowest ask price                                                                                        |
| askQuantity                  | Decimal        | Sum of quantity of all orders with the `askPrice`                                                       |
| timeStamp                    | Unix timestamp | Latest timestamp where one of the above values was changed                                              |
| openingPrice                 | Decimal        | First order book trade on the day, will be empty until first execution is happening                     |
| low                          | Decimal        | Lowest order book executed price of the day                                                             |
| high `new`                   | Decimal        | Highest order book executed price of the day                                                            |
| volume`new`                  | Decimal        | Total trade volume (in base asset)                                                                      |
| quoteVolume `new`            | Decimal        | <p>Total trade volume in quote asset<br><span class="math"> \xi (Trade Amount * Trade Price)</span></p> |
| closingPrice `new`           | Decimal        | Last day closing price (=last order book trade on the day)                                              |
| closingPriceTimestamp  `new` | Unix timestamp | The time where closing price was determined                                                             |

### **Error Codes**

| Code | Message                                          |
| ---- | ------------------------------------------------ |
| 1    | System is unavailable                            |
| 2    | Missing fields: \[Fieldname]                     |
| 3    | <p>Wrong interval |<br>Wrong symbol [symbol]</p> |

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```javascript
{
  "q": "v1/exchange.marketdata/lightTickers",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 10,
  "d": {
    "symbols": ["AMZN","INS1"],
    "interval": 2000
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 11,
  "d": {
    "symbol": "Test1Feb23",
    "lastPrice": 4.99,
    "bidPrice": 4.01,
    "bidQuantity": 3.91,
    "askPrice": 4.99,
    "askQuantity": 1.33,
    "timeStamp": 1670511325058,
    "openingPrice": 4.99,
    "low": 4.89,
    "high": 5.99,
    "volume": 1.33,
    "quoteVolume": 6.6367,
    "closingPrice": 4.01,
    "closingPriceTimestamp": 1670511325058
  }
}
```
{% endtab %}

{% tab title="Empty Response" %}
```javascript
{
  "q": "v1/exchange.marketdata/lightTickers",
  "sid": 10,
  "d": {
    "symbol": "INS10",
    "bidQuantity": 0,
    "askQuantity": 0,
    "timeStamp": 1646661691612
  }
}
```
{% endtab %}
{% endtabs %}
