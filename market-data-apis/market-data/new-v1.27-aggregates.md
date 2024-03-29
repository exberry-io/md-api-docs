# (NEW v1.27) Aggregates

This API disseminates aggregated data on different time resolutions. It supports retrieving data for a given period or as a continuous stream. Updates are disseminated every 3 seconds (only in case of a change) or when the candle is closed.

Response description&#x20;

* When `from` was not sent:  only new streaming messages will be sent
* When `from` & `to` were sent: candles from that period will be returned, stream will be closed after last message.
  * In case there are no candles in range,  system will return 1 candle which is the last candle prior to range (just before the “from”)
* When only `from` was sent - candles from that point will be returned, stream will remain open for the new streamed messages to come

This API is non real-time but close to real-time.

{% hint style="info" %}
`qualifier: v1/exchange.marketdata/aggregates`
{% endhint %}

### **Request**

<table><thead><tr><th width="139.6710763680096">Parameter</th><th width="127">Type</th><th width="482.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Symbol to retrieve the candles</td></tr><tr><td>from<br><code>optional</code></td><td>Int or<br>Unix timestamp</td><td><p>Candles start from </p><p>format: if <code>timespan=DAY</code>: yyyyMMdd else epoch time in seconds</p></td></tr><tr><td>to<br><code>optional</code></td><td>Int or<br>Unix timestamp</td><td><p>Candles to</p><p>Same format as <code>from</code>, refer description of <code>from</code> for more details.</p></td></tr><tr><td>timespan</td><td>eNum</td><td><p>One of below values:</p><p>MINUTE -  flat min (from 00 seconds to 59.999 seconds) </p><p>DAY - Based on trade date (starts on calendar time zone midnight)</p></td></tr></tbody></table>

### **Response**

<table><thead><tr><th width="196.8239997035043">Parameter</th><th width="133">Type</th><th width="403.2">Description</th></tr></thead><tbody><tr><td>symbol</td><td>String</td><td>Instrument symbol </td></tr><tr><td>open</td><td>Decimal</td><td>First order book execution price of the interval</td></tr><tr><td>close</td><td>Decimal</td><td>Last order book execution price of the interval</td></tr><tr><td>high</td><td>Decimal</td><td>Highest order book execution price of the interval</td></tr><tr><td>low</td><td>Decimal</td><td>Lowest order book execution price of the interval</td></tr><tr><td>volume</td><td>Decimal</td><td>Total volume of order book execution price of the interval</td></tr><tr><td>vwa</td><td>Decimal</td><td>Volume weighted average price of order book executions of the interval</td></tr><tr><td>timeStamp</td><td>Int or<br>Unix timestamp</td><td><p>Starting timestamp of the interval </p><p>format: if <code>timespan=DAY</code>: yyyyMMdd else epoch time in sec</p></td></tr></tbody></table>

### **Error Codes**

<table><thead><tr><th width="93.27803690934905">Code</th><th width="554.4285714285713">Message</th></tr></thead><tbody><tr><td>1</td><td>System is unavailable</td></tr><tr><td>2</td><td>Missing or invalid parameter: [FieldName]</td></tr><tr><td>3</td><td>Wrong symbol [symbol]</td></tr></tbody></table>

### **Samples**

{% tabs %}
{% tab title="Subscription" %}
```json
{
  "q": "v1/exchange.marketdata/aggregates",
  "token": "eyJleGNoYW5nZUlkIjozMCwicHJvamVjdElkIjoyMDB9",
  "sid": 14,
  "d": {
    "symbol": "AAPL",
    "timespan": "MINUTE",
    "from": 1701159900
  }
}
```
{% endtab %}

{% tab title="Response (MINUTE)" %}
```json
{
  "q": "v1/exchange.marketdata/aggregates",
  "sid": 14,
  "d": {
    "symbol": "TEST2",
    "open": 110.5,
    "close": 100.5,
    "high": 120.5,
    "low": 90.5,
    "volume": 10000,
    "vwa": 100.5,
    "timeStamp": 1709536080000
  }
}
```
{% endtab %}

{% tab title="Response (DAY)" %}
```json
{
  "q": "v1/exchange.marketdata/aggregates",
  "sid": 16,
  "d": {
    "symbol": "TEST1",
    "open": 10.5,
    "close": 14.5,
    "high": 14.5,
    "low": 10.5,
    "volume": 450,
    "vwa": 11.922222222222222,
    "timeStamp": 20240304
  }
}
```
{% endtab %}

{% tab title="Error" %}
```json
{
  "sig": 2,
  "q": "v1/exchange.marketdata/aggregates",
  "errorType": "500",
  "sid": 13,
  "d": {
    "errorCode": 3,
    "errorMessage": "Wrong symbol 0Test"
  }
}
```
{% endtab %}
{% endtabs %}
