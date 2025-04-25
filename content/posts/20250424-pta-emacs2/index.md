+++
title = "Plain Text Accounting with Emacs – Part 2"
featuredImage = "20250424-share_trading.png"
date = 2025-04-24
lastmod = 2025-04-25T20:22:15+10:00
tags = ["PlainTextAccounting", "Emacs"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

Following up from my [previous article]({{< relref "index.md" >}}) on using Emacs to maintain a plain-text ledger for use with plain text accounting software: I reconfigured my Doom Emacs to use the pre-packaged Ledger mode, and I can confirm that it is much nicer to use than the hledger mode I was using before. This could be because I hadn't set up the `hledger-mode` configuration properly, but with `ledger-mode` I find the autocompletion works much better, and the `M-q` keystroke can be used to nicely line up the amounts in a transaction.

I mentioned last time that I would talk a bit more about doing share trades, so I'll cover that in this article.


## Buying shares {#buying-shares}

When you buy shares, it is important to record the price at which you purchased that lot of shares so that you can correctly calculate capital gains (or losses) at the time you sell them. With HLedger, this is most easily achieved by specifying a subaccount when you purchase a group of shares. For example, lets say I purchased 100 Woolworths shares for $29.55 each on 2 April, 2025. I would create a transaction in my journal like this:

```ledger
2025-04-02 Buy 100 WOW @ $29.55
    assets:shares:WOW:20250402#1           100 WOW @ $29.55
    expenses:brokerage                   $9.95
    liabilities:payable:sharetrades
```

The code `20250402#1` identifies the first lot of shares I bought on 2 April 2025. Maybe the shares come down in price later that day, and I decide to buy 50 more. Then I would create the follow transaction for that next purchase.

```ledger
2025-04-02
   assets:shares:WOW:20250402#2             50 WOW @ $28.50
    expenses:brokerage                   $9.95
    liabilities:payable:sharetrades
```

So, now that I've made those purchaes, let's check out my balance sheet

```text
$ hledger bs
Balance Sheet 2025-04-02

                                 ||          2025-04-02
=================================++=====================
 Assets                          ||
---------------------------------++---------------------
 assets:bank:savings             ||          $10,000.00
 assets:shares:WOW:20250402#1    ||             100 WOW
 assets:shares:WOW:20250402#2    ||              50 WOW
---------------------------------++---------------------
                                 || $10,000.00, 150 WOW
=================================++=====================
 Liabilities                     ||
---------------------------------++---------------------
 liabilities:payable:sharetrades ||           $4,399.90
---------------------------------++---------------------
                                 ||           $4,399.90
=================================++=====================
 Net:                            ||  $5,600.10, 150 WOW
```

So, I have \\(\\$10,000\\) in my savings account, and I am the proud owner of two share lots totaling 150 WOW shares. Well, maybe I speak too soon -- I haven't paid for the shares yet. You can see under Liabilities that I owe my broker \\(\\$4,399.90\\) for the shares plus his brokerage fee. The broker is going to expect that I settle this debt on the second business day after the purchase. We'll get to that in a minute. For now, maybe I want to see how much each share lot cost me in dollars. I can do this using the `-B` option on the hledger command:

```text
$ hledger bs -B
Balance Sheet 2025-04-02, converted to cost

                                 || 2025-04-02
=================================++============
 Assets                          ||
---------------------------------++------------
 assets:bank:savings             || $10,000.00
 assets:shares:WOW:20250402#1    ||  $2,955.00
 assets:shares:WOW:20250402#2    ||  $1,425.00
---------------------------------++------------
                                 || $14,380.00
=================================++============
 Liabilities                     ||
---------------------------------++------------
 liabilities:payable:sharetrades ||  $4,399.90
---------------------------------++------------
                                 ||  $4,399.90
=================================++============
 Net:                            ||  $9,980.10
```

So now I can see how much I paid for each lot. What if I don't care about lots -- I just want to see how much I paid for my Woolworth shares in total. You can hide the lot numbers by saying I only want account names to go to three levels, and that everything below that should be totalled up and summarized. I then see:

```text
$ hledger bs -B -3
Balance Sheet 2025-04-02, converted to cost

                                 || 2025-04-02
=================================++============
 Assets                          ||
---------------------------------++------------
 assets:bank:savings             || $10,000.00
 assets:shares:WOW               ||  $4,380.00
---------------------------------++------------
                                 || $14,380.00
=================================++============
 Liabilities                     ||
---------------------------------++------------
 liabilities:payable:sharetrades ||  $4,399.90
---------------------------------++------------
                                 ||  $4,399.90
=================================++============
 Net:                            ||  $9,980.10
```

So now you can see I own \\(\\$4,380\\) worth of shares, and I owe my broker \\(\\$4,399.90\\). What happened to the brokerage fee? Well, that is an expense, and so it won't appear on the balance sheet. I need to look at an income statement to find it:

```text
$ hledger is
Income Statement 2025-04-01..2025-04-02

                    || 2025-04-01..2025-04-02
====================++========================
 Revenues           ||
--------------------++------------------------
--------------------++------------------------
                    ||
====================++========================
 Expenses           ||
--------------------++------------------------
 expenses:brokerage ||                 $19.90
--------------------++------------------------
                    ||                 $19.90
====================++========================
 Net:               ||                $-19.90
```

Oh dear, I've spent \\(\\$19.90\\) on brokerage fees, but I haven't earned any money yet. So, I am \\(\\$19.90\\) in the hole.

I don't want to get into any more trouble by not paying the broker's bill, so let's create a new transaction where I settle the bill:

```ledger
2025-04-04 ! Settlement for WOW share purchase
    liabilities:payable:sharetrades  $4399.90
    assets:bank:savings
```

After settling my trade, let's take another look at my balance sheet:

```text
$ hledger bs -B -3
Balance Sheet 2025-04-04, converted to cost

                     || 2025-04-04
=====================++============
 Assets              ||
---------------------++------------
 assets:bank:savings ||  $5,600.10
 assets:shares:WOW   ||  $4,380.00
---------------------++------------
                     ||  $9,980.10
=====================++============
 Liabilities         ||
---------------------++------------
---------------------++------------
                     ||
=====================++============
 Net:                ||  $9,980.10
```

So now my savings account has gone down, but I don't have any debt to my broker anymore.

The reason I wanted to buy the shares was to make some money, so let's see how to do that.


## Booking Dividends {#booking-dividends}

One way to make money from shares is to receive dividends. This is very easy to record in my ledger:

```ledger

2025-05-01 WOW dividend
    revenues:shares:wow  $-35
    assets:bank:savings
```

So, I receive some revenue from Woolworths by way of a dividend -- because paying me a dividend causes Woolworths to have less money, the revenue shows as negative. To balance the transaction, positive \\(\\$35\\) is added to my savings account -- score! Let's check my inocme statement again:

```text
$ hledger is
Income Statement 2025-04-01..2025-05-01

                     || 2025-04-01..2025-05-01
=====================++========================
 Revenues            ||
---------------------++------------------------
 revenues:shares:WOW ||                 $35.00
---------------------++------------------------
                     ||                 $35.00
=====================++========================
 Expenses            ||
---------------------++------------------------
 expenses:brokerage  ||                 $19.90
---------------------++------------------------
                     ||                 $19.90
=====================++========================
 Net:                ||                 $15.10
```

Now I'm in the black! The dividend payment has covered the brokerage fees, and I'm now \\(\\$15.10\\) ahead.

Let's see if I can make any more money by selling some shares


## Selling Shares {#selling-shares}

The other way to make money off shares is through capital gains, which means you sell the shares for more than you paid for them. So, let's say that at the end of may, the market price for my shares is \\(\\$32\\). I can let hledger know this using a `P` directive:

```ledger
P 2025-05-31 WOW $32.00
```

So, what is my portfolio now worth? To find out, we apply the `-V` option to my balance sheet. If I'm only interested in the value of my shares and not my bank account, I can add a filter "shares" so that only assets containing that word are shown in the balance sheet.

```text
$ hledger bs shares -V
Balance Sheet 2025-05-31, valued at period ends

                              || 2025-05-31
==============================++============
 Assets                       ||
------------------------------++------------
 assets:shares:WOW:20250402#1 ||  $3,200.00
 assets:shares:WOW:20250402#2 ||  $1,600.00
------------------------------++------------
                              ||  $4,800.00
==============================++============
 Liabilities                  ||
------------------------------++------------
------------------------------++------------
                              ||
==============================++============
 Net:                         ||  $4,800.00
```

Great, I paid \\(\\$4,380\\) for my shares, but now they are worth \\(\\$4,800\\)! Using the `--gain` option on my balance sheet, I can see how much the shares have gone up or down in value:

```text
$ hledger bs --gain
Balance Sheet 2025-05-31 (Historical Gain), valued at period ends

                              || 2025-05-31
==============================++============
 Assets                       ||
------------------------------++------------
 assets:shares:WOW:20250402#1 ||    $245.00
 assets:shares:WOW:20250402#2 ||    $175.00
------------------------------++------------
                              ||    $420.00
==============================++============
 Liabilities                  ||
------------------------------++------------
------------------------------++------------
                              ||
==============================++============
 Net:                         ||    $420.00
```

You can also make reports that show the change in value over time, but for now I just want to make a quick profit. Unfortunately with hledger, the handling of capital gain is a bit manual. This is one way to do it:

```ledger
2025-06-01 Sell all WOW shares
    assets:shares:WOW:20250402#1                -100 WOW @ $29.55
    assets:shares:WOW:20250402#2                 -50 WOW @ $28.50
    revenues:shares:capital_gain               -$420
    expenses:brokerage                         $9.95
    assets:receivable:sharetrades
```

So here, I reduce my share holdings back down to zero, and I declare the capital gain as a revenue. I have to pay a brokerage fee, which I declare as an expense, and so now the share broker owes me some money which I booked as an receivables asset `assets:recelvable:sharetrades`. Let's check the balance sheet again:

```text
$ hledger bs
Balance Sheet 2025-06-01

                               || 2025-06-01
===============================++============
 Assets                        ||
-------------------------------++------------
 assets:bank:savings           ||  $5,635.10
 assets:receivable:sharetrades ||  $4,790.05
-------------------------------++------------
                               || $10,425.15
===============================++============
 Liabilities                   ||
-------------------------------++------------
-------------------------------++------------
                               ||
===============================++============
 Net:                          || $10,425.15
```

You can see I no longer own any shares, but I do have \\(\\$4,790.05\\) owing to me from the broker \\(\\$4,800\\) from the sale of shares less the \\(\\$9.95\\) brokerage fee). In two day's time, the broker will settle the account:

```ledger
2025-06-02 Receive payment from broker
    assets:receivable:sharetrades          -$4790.05
    assets:bank:savings
```

The amount the broker owes me goes down by \\(\\$4790.05\\), which means my bank account should go up by \\(\\$4,790.05\\) to balance the transaction.

The final balance sheet now looks like this:

```text
$ hledger bs
Balance Sheet 2025-06-03

                     || 2025-06-03
=====================++============
 Assets              ||
---------------------++------------
 assets:bank:savings || $10,425.15
---------------------++------------
                     || $10,425.15
=====================++============
 Liabilities         ||
---------------------++------------
---------------------++------------
                     ||
=====================++============
 Net:                || $10,425.15
```

So, I started with \\(\\$10,000\\) in by bank account, and now I have \\(\\$10,425.15\\), a profit of \\(\\$425.15\\). We can also see this on my income statement:

```text
$ hledger is
Income Statement 2025-04-01..2025-06-03

                              || 2025-04-01..2025-06-03
==============================++========================
 Revenues                     ||
------------------------------++------------------------
 revenues:shares:WOW          ||                 $35.00
 revenues:shares:capital_gain ||                $420.00
------------------------------++------------------------
                              ||                $455.00
==============================++========================
 Expenses                     ||
------------------------------++------------------------
 expenses:brokerage           ||                 $29.85
------------------------------++------------------------
                              ||                 $29.85
==============================++========================
 Net:                         ||                $425.15
```

Of course, the tax man will want his cut at the end of the year. However, you can see that I now conveniently have all the information I will need to give to my accountant at Year End.

If I wanted to do a breakdown of all the brokerage fees I paid over the year, I can use HLedger's `reg` report:

```text
$ hledger reg brokerage
2025-04-02 Buy 100 WOW @ $29.55                     expenses:brokerage             $9.95         $9.95
2025-04-02 Buy 50 WOW @ $28.50                      expenses:brokerage             $9.95        $19.90
2025-06-01 Sell all WOW shares                      expenses:brokerage             $9.95        $29.85
```

It shows the date, transaction description, transaction amount and running total for the records shown. I can check that the transactions on my savings bank account match the statement from my bank:

```text
$ hledger reg savings
2025-04-01 Opening Balances                       assets:bank:savings         $10,000.00    $10,000.00
2025-04-04 Settlement for WOW share purchase      assets:bank:savings         $-4,399.90     $5,600.10
2025-05-01 WOW dividend                           assets:bank:savings             $35.00     $5,635.10
2025-06-03 Receive payment from broker            assets:bank:savings          $4,790.05    $10,425.15
```


## Conclusion {#conclusion}

So, this has been a quick walk-through of how you can use plain text accounting to manage your share trading, and make sure you stay on the right side of both the tax man and your broker. Perhaps next time I'll show what happens if you buy a big ticket item for work, such as a laptop, and you need to depreciate the cost over a few years for tax purposes.


## Complete Ledger File {#complete-ledger-file}

If you want the complete ledger file I used for this example (so you can play around with it), you can find it below. There is a bit at the start that I didn't talk about:

-   the `commodity` directive tells HLedger how to format the currency. If you use commas as a decimal separator and periods as thousands separators, you can simply change that line.
-   The initial transaction sets up an initial balance in my savings account of \\(\\$10,000\\). Because money can't come from nowhere, and the transaction has to balance, I am pulling the \\(\\$10,000\\) out of an equity account, showing how much I invested at the start. If I close the books, the assets are all converted back to equity, and you can then see how much I will pocket in the end.

<!--listend-->

```ledger
commodity $1,000.00

2025-04-01 Opening Balances
    assets:bank:savings                    $10000.00
    equity:opening/closing balances

2025-04-02 Buy 100 WOW @ $29.55
    assets:shares:WOW:20250402#1                 100 WOW @ $29.55
    expenses:brokerage                         $9.95
    liabilities:payable:sharetrades

2025-04-02 Buy 50 WOW @ $28.50
    assets:shares:WOW:20250402#2                  50 WOW @ $28.50
    expenses:brokerage                         $9.95
    liabilities:payable:sharetrades

2025-04-04 * Settlement for WOW share purchase
    liabilities:payable:sharetrades         $4399.90
    assets:bank:savings

2025-05-01 WOW dividend
    revenues:shares:WOW                         $-35
    assets:bank:savings

P 2025-05-31 WOW $32.00

2025-06-01 Sell all WOW shares
    assets:shares:WOW:20250402#1                -100 WOW @ $29.55
    assets:shares:WOW:20250402#2                 -50 WOW @ $28.50
    revenues:shares:capital_gain               -$420
    expenses:brokerage                         $9.95
    assets:receivable:sharetrades

2025-06-03 Receive payment from broker
    assets:receivable:sharetrades          -$4790.05
    assets:bank:savings
```
