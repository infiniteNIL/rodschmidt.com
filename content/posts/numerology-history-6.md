---
title: "Numerology: 17 Years in the App Store (Part 6)"
date: 2025-12-03T11:43:03-07:00
draft: false
tags: ['Apple Platforms', 'Numerology', 'Software Development']
---
This is part 6 of my blog series on the history of my app Numerology. See Part 5 [here](/posts/numerology-history-5).

When we left off, Numerology was chugging along, slowly making less and less each month. First $300, then $200, sometimes as low as $100. Sometimes it would spurt back up to $300.

This continued until fairly recently. In 2024, I noticed a competitor that always showed up first in the App Store search results when you searched for ‘numerology’. I decided to look into them a little more. According to Sensor Tower, their app was making over $5,000/month on the Apple App Store. They had an Android app as well, and it was making $5,000/month as well.

This got me thinking. If they can do it, why can’t I?

For the longest time, my app was paid up front for $9.99. I resisted the whole subscription movement, because as a consumer myself, I disliked the idea.

Things change though. In March of 2024, I was laid off from my job at Cricut. It didn’t bother me much financially, as I had a lot of savings, and I kind of wanted to take the summer off anyway. It bothered me quite a bit psychologically, and I think I’m still getting over it.

When fall rolled around, I still didn’t know what to do. The job market was terrible. I thought maybe I try to create my own thing, but I couldn’t figure out what to do. This continued all the way into 2025.

## Numerology 2

After reaching a year after being laid off and no end in sight, I started looking at Numerology again and decided I would create a version 2 that would be subscription based.

I rewrote the app with a slightly new design, using SwiftUI and SwiftData. I even included iCloud syncing (with a subscription at first). Customers were always emailing me about how they lost their data because they deleted the app or got a new phone and hadn’t backed up their data, or even used the app’s export feature to save their data.

I also used ChatGPT to generate new readings. Users had complained that the original readings were kind of depressing, so I generated an upbeat style. Users could still choose the original if they wanted to, but the new style was the default. I would eventually had a third style called New Age Guru, and a random style that picked one of the styles at random. The New Age Guru style required a subscription.

Here’s some screenshots of the new version.

{{<img-center src="/images/num2-profile-list.png" width="256" title="Numerology 2's Profile List">}}
{{<img-center src="/images/num2-chart.png" width="256" title="Numerology 2's Chart">}}
{{<img-center src="/images/num2-motivation.png" width="256" title="Numerology 2's Reading View">}}

I also implemented support for Shortcuts, Siri, and Spotlight, as you can see in one of the screenshots above. I advocated for this at Cricut and even implemented a proof of content for Spotlight support, but it went nowhere. Companies often want to increase user engagement and I believe supporting these technologies is a good way to do so. Spotlight search especially, can remind users about your app when the search for something that also happens to have relevant data in your app.

The shortcuts later came in quite handy when I experimented with some workflows that used n8n to automatically get the name and birthdate of a celebrity from Wikipedia, add the profile to the app, extract a reading, and then post a screenshot or text of that reading somewhere. Shortcuts and the Shortcuts app can really be quite powerful, and can be used in AI workflows.

# Release
I released the new version on April 24th, 2025. The app was free to download and you could add as many profiles as you wanted. You could subscribe to get additional features (and upcoming features) such as new voice styles, shortcuts, etc. There’s a monthly subscription, an annual subscription, and a lifetime subscription. Here’s the current paywall.

{{<img-center src="/images/num2-paywall.png" width="256" title="Numerology 2's Paywall">}}

The monthly and annual prices were a little cheaper at first and I later raised them. I also lowered the lifetime price.

# Results
Here’s a graph of sales from April ’25 to December 2nd 2025:
{{<img-center src="/images/num2-sales.png" title="Numerology 2's Sales">}}

Things started off strong, but have pretty much settled back to where they were when I had a paid up-front model, but a little better. I currently have 148 subscribers and a monthly recurring revenue (MRR) of $259.

I know the subscription model is a long play and it takes a long time to grow. I’ve been increasing my marketing efforts, posting on [X](https://x.com/numerologyios), [Instagram](https://www.instagram.com/numerology.app/), [Facebook](https://www.facebook.com/profile.php?id=61576257742749&__cft__%5B0%5D=AZUsHew47MI8CmtrsM9krKutjJkBs6A2hvJiUH05gEdLWbYrSIcdD5TCu7TfMSfyNXCn_fTp1iu6Krp9O87VtTrFsc0V2YNEgV1DzjS9efRuWth-gbvqcrvBatPR64P85nh9ThxyWLcAUsrdOeHKs3zoBDKVSN_UfaRXmCqGi_pSxwLif2AfU1wjBV5-toF6wiBFleMD21AcdETeugkMmuYa&__tn__=-UC%252CP-R), and even [YouTube](https://www.youtube.com/@numerologyapp).

For YouTube, I used AI to help generate personal year forecast videos for all the birthdays in January.

When you search for ‘numerology’ in the App Store, my app shows up as the 3rd result, my highest since the App Store peak days. So my ASO strategies must be working. I know my social marketing efforts aren’t because they have very few views or subscribers.

At this point, I’m not sure the subscription model is any better than the paid up front model. I am making more money, but I’m also charging more. The true test will be when the annual subscriptions near renewal and customers either drop or renew.

In the meantime, Numerology is not going to pay the bills. My savings keeps dwindling and I’m still trying to figure out ways to make more money. I apply for jobs when interesting ones come up, but not even an interview. I've been doing Apple development full-time since 2003. You'd think there would be some value in that.

# That’s It
There you have it. The history of Numerology. I hope you enjoyed this series and learned something. Tell your friends about Numerology. You’d be surprised how accurate it is.

If you need help in your software development efforts, I’m available for consulting, contracts, part-time, or full-time.
