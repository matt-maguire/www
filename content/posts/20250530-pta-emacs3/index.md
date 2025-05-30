+++
title = "Plain Text Accounting with Emacs – Part 3"
featuredImage = "20250530-share_trading.png"
date = 2025-05-30
lastmod = 2025-05-30T18:12:00+10:00
tags = ["PlainTextAccounting", "Emacs"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

My journey into using Hledger with Emacs to track my share investments continues. [Last Time]({{< relref "index.md" >}}) I looked at a way to track shares using a plain text file. Since then I have made some tweaks to my workflow as I gain more experience:

-   naming conventions that support multiple stock brokers
-   booking brokerage fees in a better way to support the Australian tax system.


## Naming convention {#naming-convention}

Over the past month I was not overly impressed with my existing stock broker, which led me to explore other stock brokers that may offer a more reliable service at a cheaper rate. It is expensive to transfer stocks over to a new broker, so I decided to cap further investment at my current broker and instead make any new purchases at a different broker. This meant that I would need an account structure that lets me easily identify which share lots were purchased through which broker.

The scheme I came up with to record my share inventory looks like this:

`assets:shares:[stock]:[broker_code][purchase_date]`

So, for example, if I purchase some more Telstra (TLS) shares through my new Cheap Stocks broker (CHP), then I would record the purchase like this:

```ledger
2025-04-25 * (5550001) B 500 TLS @ 4.80
    assets:shares:tls:chp20250425             500 TLS @@ $2400
    equity:shares:tls:chp20250425:brokerage    $5
    liabilities:payable:bank:chp           $-2405
2025-05-25 * (5550002) B 1000 TLS @ 4.60
    assets:shares:tls:chp20250525            1000 TLS @@ $4600
    equity:shares:tls:chp20250525:brokerage   $10
    liabilities:payable:bank:chp           $-4610
```

-   On the first line, I have the trade confirmation number in brackets, then the narrative mentions (B)uy or (S)ell, Units traded, Stock ticker code and average unit price at which the order was executed.
-   On the second line, I decided to avoid using unit price, since this would often result in rounding errors. The unit price is easily available in the narrative, and so in the second line I record the total consideration amount quoted by the stock broker.
-   On the third line I record the brokerage fee. Why do I have an "equity" account here? I'll explain in the next section.
-   On the last line, I record the total amount I owe the broker as an Accounts-Payable Liability, which will be settled in two days' time. I include the sub-account name "bank" so that I can easily see my bank balances and upcoming settlement amounts by querying for the term "bank". I then include the stock broker code so I can see how much I owe to which broker.


## Tax reporting {#tax-reporting}

When I looked a bit further to the taxation rules here in Australia, I discovered that I can't claim brokerage fees as a tax-deductable expense at the time of share purchase. I can however claim it against any capital gain I make when I sell the shares at some point in the future, as it is . This could be a long way down the track, and I may have migrated my ledger across several files in the meantime. So, how can I report this brikerage fee?

My idea is that I don't want to book it as an expense at the time I purchase the shares, but I DO want to expense it when I sell the shares. So, I need to store the value in a longer-term account such as an Asset, Liability or Equity.

I initially thought that I could store the value of the brokerage fee as an asset. However, it is money I have spent on a service, and it isn't really an asset that I can "liquidate" later on. Neither is it a liability, since I have paid the money and I don't owe anyone anything. I then realised paying out money for brokerage fees reduces my equity, and so it makes sense to hold the value of brokerage fees in an equity account, and convert it to an expense when the shares are sold.

So, let's say I sell my Telstra shares a year later for \\(\\$5\\) per share. I would record the sale like this:

```ledger
2026-05-01 * (5550003) S 1500 TLS @ 5.00
    ; lot chp20250430
    assets:shares:tls:chp20250425             -500 TLS @@ $2400
    equity:shares:tls:chp20250525:brokerage   $-5  ; convert loss of equity into an expense
    expenses:shares:tls:brokerage              $5  ; brokerage cost of purchasing shares
    income:shares:tls:capgain:>1y            $-100
    ; lot chp20250530
    assets:shares:tls:chp20250525            -1000 TLS @@ $4600
    equity:shares:tls:chp20250525:brokerage   $-10  ; convert loss of equity into an expense
    expenses:shares:tls:brokerage              $10  ; brokerage cost of purchasing shares
    income:shares:tls:capgain:<1y            $-400
    ; general
    expenses:shares:tls:brokerage              $12  ; brokerage cost of selling shares
    assets:receivable:bank:chp               $7488  ; consideration for sale of all shares less brokerage for sale
```

-   In lines 3 and 8, we reduce my share inventory by the amount of shares I am selling, and we use the original cost base. We could go back to an earlier file where I purchased the shares, but it is easier to simply run `hledger bs -B` on the current ledger to get those values.
-   In lines 4 and 9, we reverse the brokerage equity transaction from when I bought the shares, and convert it to an expense for this current financial year where I need to declare my capital gain (lines 5 and 10). These amounts can easily be found by running `hledger bse -B` to ge tthe balance of the revevant equity accounts.
-   In lines 6 and 11, I declare the capital gain on the shares as income. This can be calculated manually, or if I am only selling a single share lot, I can leave the amount blank and `hledger` will calculate it for me.

    Note with capital gain, I split it into two categories: capital gain where I held the shares for less than one year (in which case Australian tax law says I pay capital gains tax on the full amount), and capital gain where I held the shares for more than 1 year (in which case, I only need to pay tax on half the capital gain).
-   In line 13, I declare the brokerage fee for the sale as an expense.
-   in line 14, I post an Accounts Receivable amount for what my broker owes me, being the consideration of the shares sold less their brokerage fee for the sale.

So, in this way, I have all the information needed for my accountant without having to trawl back through historical files from previous financial years. My closing the books at end of financial year and migrating to a new file for the next financial year, the cost base of the shares and the brokerage fees for the purchases are preserved in the opening balance for the new year.


## Settlements {#settlements}

Just for completeness, I should mention the settlements that go with those three share trades. Each of them is two days after the trade and have the confirmation number issues by my broker for the trade. They will look something like this:

```ledger
2025-04-27 * (5550001) Settle TLS trade
    liabilities:payable:bank:chp    $2405  ; B 500 TLS @ 4.80
    assets:bank:abc:savings        $-2405
2025-05-27 * (5550002) Settle TLS trade
    liabilities:payable:bank:chp    $4610  ; B 1000 TLS @ 4.60
    assets:bank:abc:savings        $-4610

2026-05-03 * (5550003) Settle TLS trade
    assets:receivable:bank:chp     $-7488  ; S 1500 TLS @ 5.00
    assets:bank:abc:savings         $7488
```


## Final Thoughts {#final-thoughts}

I think this new setup provides a bit more flexibility and convenience in understanding capital gains/losses and expenses (cost of ownership) for my share portfolio. You may have noticed that in these entries I use "income" as a top level account name rather than "revenue". This change was because I wanted to better align with the ledger format used by [Beancount](https://beancount.github.io/), an alternative to `hledger`. After playing with `Beancount` for a bit, I decided that I would stick with `hledger` due its greater flexibility and the nice reports it produces. After I go through my next tax return with my accountant, I may find other tweaks to this setup (eg. perhaps I need to apportion the selling brokerage fee across share lots that fall under different capital gains tax rules?). I'll post here any new things I learn along the way.
