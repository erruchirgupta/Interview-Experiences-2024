# ![Addepar logo](https://logowik.com/content/uploads/images/addepar-inc-corporate2439.logowik.com.webp) <br>Addepar | Sr. Software Engineer-Alternatives Data Management (June-2024)

## [Round 1] HackerRank on-site coding Round
### [Coding] Problem Statement - Recon

Reconciliation is a term Addepar uses for a set of correctness and consistency measures applied to the data we receive and use in financial calculations.
 <br>
One of the most common reconciliation checks is called unit reconciliation, which answers the question, "does the transaction history add up to the number of shares the bank says I have?". For example, if the bank said I had 100 shares of Apple at the end of yesterday, and I bought 20 shares of Apple today, then we expect the bank to report 120 shares at the end of today. This surprisingly isn't always the case! The bank may send incomplete data, we may be parsing it incorrectly, or there may be events like corporate actions or trade settlement lag that cause an inconsistency.
 <br>
Unit reconciliation is very important, because numbers that don't add up shouldn't be trusted for any metrics.
<br>

### The ask
Write a function that takes three lists of strings as input:

- D0-POS
  - Describes the positions in the account at the end of Day 0
  - Each record is a space-separated pair of Symbol and Shares
  - For example, "AAPL 10" means 10 shares of AAPL were held at the end of Day 0, and "Cash 122.16" means we had $122.16 in the account at the end of Day 0
- D1-TRN
  - Describes the positions in the account at the end of Day 0
  - Each record is a space-separated pair of Symbol and Shares
  - For example, "AAPL 10" means 10 shares of AAPL were held at the end of Day 0, and "Cash 122.16" means we had $122.16 in the account at the end of Day 0
- D1-POS
  - Describes the positions in the account at the end of Day 1, and has the same format as D0-POS

And outputs a list of positions with unit reconciliation errors:
- Each record is a space-separated pair of Symbol and difference between the actual quantities and the expected ones
- Only positions with unit reconciliation problems should appear in the list
- The list may be empty if no reconciliation issues are found
- For example: "GOOG 10" indicates we ended up the day with 10 extra shares of GOOG that were not explained by the intra-day transactions, while "Cash -10" shows that we ended the day with 10 fewer units of Cash than expected
 
To start us off, let's support two basic transaction types and a single Cash currency (assume USD):
- Buy [BY]
  - Besides the first two generic fields, two additional ones: Units, and Total Value
  - For example, "AAPL BY 10 6123.21" means 10 shares of AAPL were bought for a total cost of $6123.21
- Sell [SL]
  - Same as Buy case, but sells shares for dollars

| input |   output    |
| :-:   |:-:|
| <p align="left">["AAPL 100", "GOOG 200", "Cash 10"] # D0-POS<br>["AAPL SL 50 30000", "GOOG BY 10 10000"] # D1-TRN<br>["AAPL 50", "GOOG 220", "Cash 20000"] # D1-POS</p>|<p align="left">["GOOG 10", "Cash -10"]</p>|

### Solution
<br>Here's how we can implement this in Java:

```java
import java.util.*;
import java.util.stream.Collectors;

public class ReconCodePair {

    private static Map<String, Integer> parsePos(List<String> pos) {
        return pos.stream()
                .map(s -> s.split(" "))
                .collect(Collectors
                        .toMap(parts -> parts[0], parts -> Integer.parseInt(parts[1]))
                );
    }

    private static void applyTxn(String txn, Map<String, Integer> ledger) {
        String[] entityTxn = txn.split(" ");
        if("SL".equalsIgnoreCase(entityTxn[1])) {
            ledger.put("Cash", ledger.getOrDefault("Cash", 0) +  Integer.parseInt(entityTxn[3]));
            ledger.put(entityTxn[0], ledger.get(entityTxn[0]) - Integer.parseInt(entityTxn[2]));
        }
        else if("BY".equalsIgnoreCase(entityTxn[1])) {
            ledger.put("Cash", ledger.getOrDefault("Cash", 0) - Integer.parseInt(entityTxn[3]));
            ledger.put(entityTxn[0], ledger.get(entityTxn[0]) + Integer.parseInt(entityTxn[2]));
        }
        else
            System.out.println("Invalid transaction : " + txn);
    }

    private static List<String> reconcile(List<String> pos0, List<String> sampleTxn1, List<String> pos1) {
        Map<String, Integer> ledger = parsePos(pos0);
        Map<String, Integer> finalLedger = parsePos(pos1);
        sampleTxn1.forEach(txn -> applyTxn(txn, ledger));

        List<String> reconResult = new ArrayList<>();
        finalLedger.forEach((key, value) -> {
            if(!Objects.equals(ledger.getOrDefault(key, 0), value))
                reconResult.add(key + " " + (value - ledger.get(key)));
        });
        return reconResult;
    }

    public static void main(String[] args) {
        List<String> samplePos0 = List.of("AAPL 100", "GOOG 200", "Cash 10");
        List<String> sampleTxn1 = List.of("AAPL SL 50 30000", "GOOG BY 10 10000");
        List<String> samplePos1 = List.of("AAPL 50", "GOOG 220", "Cash 20000");
        List<String> reconResult = reconcile(samplePos0, sampleTxn1, samplePos1);
        System.out.println(reconResult);
    }
}
```
