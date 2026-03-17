---
title: "Strip out the USBC and what do I have left?"
description: "Last year I wrote about how [USB-C PD isn't just for charging --- it can revolutionize our relationship with electricity](/posts/usb-c-pd-revolutionize-electricity/)\n\nToday I'd like to propose we throw that away."
date: 2026-03-06 00:00:00 +0000
categories: [Technology, Energy]
tags: [poe, dc-power, energy-efficiency, ethernet, sustainability]
---

Last year I wrote about how [USB-C PD isn't just for charging --- it can revolutionize our relationship with electricity](/posts/usb-c-pd-revolutionize-electricity/)

Today I'd like to propose we should throw that away and instead build something that's already ready, already battle tested, already has all the regulatory approvals, already figured out the DC Arc Fault problem.

Recap: Your BLDC ceiling fan, your iPhone charger, your wifi router and LED TV & lights all run on Direct Current (DC). But what's wired to your home is Alternating Current (AC) because Edison tried and failed to run DC long distance. And that was fine because up until recently pretty much all your appliances also ran on AC. That's no longer true.

Fun fact: You've probably heard of the Three Gorges Dam slowing down the earth's rotation. You've probably not heard though that it supplies power all the way from the East China (Shanghai etc) all the way to the Western coast (Guangzhou and Shenzhen). Edison could not make long distance DC transmission work but today a significant part of China depends on High Voltage DC (HVDC) transmission (PS: Kids, don't try HVDC at home!)

But AC to DC.. That's a LOT of adapters. Bad adapters, mind you. Your phone's little charger carries a bunch of scarce resources and does a pretty wasteful job of converting AC to DC, which is fair considering it's been tasked with doing that while staying tiny, lightweight and cheap.

And you might say that Aayush sure we'd love to use DC directly. But it's not like we can switch overnight, there's the fact that you need to develop the relevant infrastructure, the building codes will be a nightmare, fire safety and so on. Not to mention your idea of using USBC directly is vulnerable to ARC faults and even more importantly, cybersecurity.

## That's crazy

What if I said it's all there, all the regulatory approvals exist and it's sitting in plain sight for us to adopt?

- **AC to DC converters** --- Here's the thing --- if you live in 2026 you probably know somebody with an EV. Guess what an electric car charger needs to do before it can charge your lithium ion batteries? It needs to convert AC to DC. And it needs to do that with minimum heat dissipation, minimum conversion loss etc. it's been engineered obsessively and all the safety data and regulatory approvals and building codes already exist. It is already in mass production. It is already capable of high loads.
- **PoE++** --- In my last post I hinted to this but gravitated towards USBC because I assumed it's obvious that protocols are interchangeable. But it needs laying out that Ethernet cables, the same ones running internet to your computer, have every regulatory approval, are designed for sustained operation and crucially --- can carry upto 90 watts over upto 100 meters.
- **PoE injectors** --- funny thing, people already needed to inject power into ethernet to run VoIP phones and security cameras without having to run separate AC power to them. And since they were sitting on very expensive routers they didn't want to buy anew they figured they can just build devices that take a datalink and inject some electricity into it.

## That's still crazy

What's stopping us? Momentum? Me being completely and totally wrong? Or the fact that we simply ran AC to our homes a century ago and never considered the fact that over the last 30 years we switched to DC for pretty much everything inside those homes.

### Google already proved the concept

In 2016. Google's datacenters switched to 48V DC and observed a 30% improvement in energy efficiency.

Also, Unifi already sells switches that can do 60–100W with per-port monitoring, remote restart, scheduling etc. You can buy every component off-the-shelf right now.

### And Starlink did it outdoors

Starlink figured it's cheaper to just run PoE from mesh routers to satellite dishes than the whole AC dance. That's ethernet cables already battle tested to be supplying electricity in rain or shine, freezing snow & unbearable heat.

Starlink used it to eliminate the AC run, the outdoor outlet, the weatherproof GFCI and the dish's power brick to power your internet with.. internet cables.

This article merely says why restrict the mighty ethernet to the networking industry?

### And cybersecurity kills it

There's one really big problem with plugging a bunch of ethernet cables into a router and then letting anybody connect to those cables --- you're gonna get hacked. In particular, those cables are gonna talk to eachother and talk to the router and that means somebody can hack your home by plugging in a device. Or a poisoned router at a hotel can deliver a payload straight through your charger.

So why not just bypass all that and start with PoE injectors that do not even let you plug multiple devices in in the first place.

And once somebody realizes this idea isn't totally bonkers they might realize PoE does not require devices to talk to each other at all. So you can build a router that doesn't have a backplane, just ASICs to negotiate voltages. In technical terms that's a managed switch without any Layer 2/3 forwarding capabilities. In simple terms a PoE injector scaled up to have multiple output ports and no input ports.

### But clean power

A CPAP machine doesn't have the same headroom as an ordinary BLDC ceiling fan.

Except PoE++ is designed for equipment much more delicate than that. Some of the most delicate equipment we've got. And that's been taken into account by the IEEE for every single layer obsessively.

### But prohibitive cost

As of right now FEDUS sells a 100 meter ethernet cable for 1299 INR or about $14.18 USD as of this writing & today's conversion rate.

PoE injectors are hard to source cheap in India but I found them going for about 1039 INR or $11.34 USD for lower voltages upto 12V or upto 1349 INR or $14.73 USD. But that's niche pricing for what's effectively an ASIC negotiating voltage and connecting input DC into copper pairs on an ethernet cable.

### Except

Not every cable needs to be 100 meters. Personally I have never lived in a home that needed a 100 meter cable run and if you do, have you considered more than one utility closets in your palace?

## Caveats

This proposal certainly sounds attractive. Throw away AC and run DC to every home in the world! What could possibly go wrong? Here's what can go wrong:

When plugging my iphone to charge into a hotel how do I know this router has no backplane? Although to be fair USB ports in wall sockets are already proliferating and you can trust them even less. We had a whole panic about connecting to airport Wi-Fi and then we ended up connecting to airport & airplane chargers anyway.

You can't bypass the negotiation step. Which requires a chip. Which means something that talks to something and that opens up an attack surface.

Except.. nobody said it has to be the same chip that connects to your RAM and storage and runs your phone. Modern devices already use ASICs to negotiate USB C charging voltage. Cameras do the same for PoE++. The only requirement here is to make sure you're not plugging a data-carrying cable into a port that does both data and power simultaneously.

But doesn't that mean we're back to square one? Well, are you? The ASICs in your phone for charging exist regardless of whether you use an adapter or a USB-C wall socket. What's eliminated is the adapter that converts AC to DC and talks to your phone with its own ASICs and carries a very tiny amount of rare earths --- scarce materials that stack up fast when every single electronic device in your home has a cheap AC to DC converter.

## Electrocution & Fire Safety

There's an old saying: Safety Codes are written in blood. This is not an exaggeration for these codes are most often updated after an unfortunate disaster proves the last version was inadequate.

Both live AC & DC have the potential to cause electrocution and electrical fires. AC needs to run at much higher voltages (120–240V residential depending on where you live) and the alternating nature is lethal.

**On electrocution:**

DC is objectively safer for electrocution risk. Not perfectly safe. A 48V wall outlet is just about skirting the edges and can be lethal in some conditions (OSHA's cutoff for hazardous is 50V).

AC is objectively more dangerous. Not only does it require much higher voltages but the alternating nature itself causes trouble.

In the past this was a big problem. Even if you did want to run AC to DC adapters what voltage would you decide? 12V is very safe, it will happily run a small LED bulb. But it won't run your laptop. So do you limit yourself to 36V? Or do you run multiple voltage lines in the same home? It's a nightmare.

The magic of both USBC & PoE++ is that they sidestep the problem entirely. Onboard ASICs in your power adapter negotiate voltage with your iPhone. With an ASICs based router you can quite literally cap voltages on a per-device basis and change your mind by flipping a digital or physical switch.

**On fire safety:**

Here's the hardest problem and arguably one that my USBC piece did not sufficiently address. DC Arc faults are harder to put out than AC Arc faults. But both exist and if you live in a modern home chances are you have an AFCI (Arc Fault Circuit Interruptor) --- a device with an onboard ASIC that detects AC Arc Fault signatures to avoid setting your home on fire.

Luckily there's a certain advantage to standing on the shoulders of giants: The giants are absolutely paranoid and the very people with the most experience dealing with electrical fires. I am talking about the IEEE of course.

And the IEEE has spent considerable effort already perfecting PoE++ to minimize fire risk from sustained draw & arc faults with painfully detailed specs. For reference:

<https://www.ieee802.org/3/bt/public/jan15/diminico_01_0115.pdf>

<https://webstore.iec.ch/en/publication/67690> (<https://www.ieee802.org/3/bt/public/oct15/Draft%20of%20IEC%2060512-99-002.pdf>)

The TIA TSB-184-A standard strongly recommends limiting bundles to a maximum of 24 cables when running high power.

NEC Article 725.144 legally dictates how much power you can run based on gauge, temperature rating, and bundle size. If you must run bundles larger than 192 cables, the NEC requires strict adherence to ampacity charts or the mandatory use of LP-rated cables.

Now I don't know about you but quite frankly I cannot think of 24 devices that need power in a single room. And if you do need it, you probably already need to call in an electrician because plugging all that into a regular 6amp AC line will probably burn down your house.

**Continuous resistance monitoring (Free bonus):** The IEEE spec for PoE++ already requires upstream devices to monitor for insulation resistance. Most electrical fires start when your AC wiring insulation degrades from heavy use and you don't know it. PoE++ is capable of detecting failing insulation in real time.

**Short Circuits (Free bonus):** PoE++ already implements per-port current limiting. When the power draw is both capped & monitored a device that told the router I'm expecting about 30 watts and suddenly surges to 80 is cut off. Short circuit protection is built into the very protocol itself.

## Why we shouldn't actually use Ethernet cables (Hear me out)

Wait, didn't I just spend 1000 words convincing you to run internet cables to your TV? Yes and no. The protocol is perfect. The standard Cat5e physical cable is a disaster waiting to happen.

And if you try to do this with regular ethernet cables a lot of people will absolutely go for the lowest bidder who will not know the difference between a Cat 5e and Cat 6 cable (And according to the IEEE you shouldn't run 90W on a Cat 5e, even though they both look the same).

For completion, I will say that the answer here is pretty simple. Use the right cable gauge for the job and a distinct connector so contractors can't possibly mix them up.

## Benefits beyond waste reduction

Here's where the payoffs go beyond just the scarce resource savings and conversion losses:

- **Child proofing** --- Today child proofing electrical outlets is a nightmare. What if however you only ran AC to a single utility closet/spare garage/basement and put your heavy appliances like washing machines and dryers and AC to DC converter in there?
- **Modularity** --- I made the same argument with USBC and it bears repeating now: Imagine me making the claim that we can standardize to yet another standard, fully expecting a good deal of readers to be reminded of XKCD.

About modularity:

- AC to DC converter damaged? Just replace that.
- PoE injector damaged? Just replace that.
- Cable damaged? Yeah you're kind of screwed unless you overprovision --- in which case you can lose a few cables in the bundle and be just fine not tearing down your walls.

And now the cascading benefits:

- Your bathroom today has a GFCI --- a device intended to protect you from electrocution from your own water.
- Your home probably contains a bunch of AFCIs (And if it doesn't and it's a modern home, you know what kind of contractor you got).
- If you've got sensitive equipment there's a good chance there's an internal stabilizer to avoid getting fried by rogue AC voltages. If your sensitive equipment is pretty old or if you're paranoid there's a good chance you've got an external stabilizer sitting upstream of it.

All three eliminated at once. Those also carry scarce resources. And are expensive. And do not cover your whole house. Not only do you spend on a GFCI for everything in your bathroom to avoid electrocution but most building codes don't require a GFCI in your bedroom where the AC plug can electrocute you all the same.

But the buck doesn't stop there. Rather we move on to inverters and UPS. In my last article I pointed out:

1. When charging the battery (A/C to D/C): **15% lost**
2. When the power goes out (D/C to A/C): **15% lost**
3. Your device/adapter again converting power (A/C to D/C): **15% lost**

Compounded? **38%.** Almost 40.

If you just use DC though you don't need steps two and three.

## Observation and control

With each device receiving independent, high quality DC your PoE injector can monitor each connection, automatically acting as a GFCI, AFCI , MCB and usage monitor. Exactly how much power each device is using.

But it also means you can decide which devices go onto your UPS and which go onto your inverter. If you ever decide you want more inverter capacity in your bedroom you do not need to run new wiring, you just remove the ethernet cable from a non-inverter to an inverter switch.

And your switch will complain loudly when the load exceeds what your UPS or inverter can handle. You could even put in a priority order for which devices the inverter can drop if the load exceeds its capacity, the rightmost device being disconnected first. No digital configurations, no app, just physical ordering. Right now companies pay a fortune for inverters that degrade gracefully. And Cisco already came up with a better solution and buried it in a different field calling it "perpetual PoE".

Finally, and perhaps most powerfully, you isolate failure. A bad brick can destroy your TV. A bad power supply can destroy all your equipment. A Unifi router designed to power the most sensitive equipment is probably better at protecting your devices AND isolating failures.

Also, your printer doesn't get bricked if you lose its power brick!

## Why?

To be very clear this post (And the USBC post before it) is meant to demonstrate that we are using outdated technology for modern solutions and that switching is not as difficult as we think. All the relevant problems have been solved, it's just that the people making routers are not the people dealing with fire safety codes are not the people designing EV chargers.

And if they start talking to each other we can:

- Eliminate AC to DC adapters in every household device.
- Eliminate GFCI, AFCI & MCBs in every AC safety circuit.

Not to mention since these adapters & equipment are repeated in every single device & upstream lines manufacturers optimize for cheaper devices even though they are worse at converting and proxying electricity.

So what if these people started talking? We'd eliminate significant waste in both scarce resources and materials in general, improve energy efficiency AND improve safety:

- Ethernet & USBC ports are inert. Sticking your finger in one will not send you to the ER. They run a very tiny voltage and PoE only kicks in when a device explicitly asks for power. And that stream dies the moment it disconnects.
- You get global GFCI coverage. GFCIs measure electricity leaving and electricity returning to break the circuit if there's a leak/electrocution. Right now most countries only mandate GFCI coverage for bathrooms/areas with water. With a 10 port ASICs based injector you don't just generalize to all equipment but you can use higher quality GFCI circuits.

And sometimes building off an existing standard instead of inventing the next universal standard means you can achieve 95% of the gains without the waste of slugging through building and pushing and standardizing and getting approvals for a brand new standard to capture the remaining 5%.

## Where I'm wrong

I'm sure this piece sounds pretty convincing. But here's a few open issues:

- **Contractor reliability & training:** The uptake could be chaotic if training & certifications are not strictly enforced. I'd be terrified of a contractor deciding it's fine to splice off a 5e's RJ45 connector to splice on a power delivery connector.
- **The 48V safety question:** 48V is just skirting the edge of safety. Though the fact that the IEEE have standardized around it implies people who know more about electrical safety see it as an acceptable tradeoff. Technically the IEEE have gone higher, PoE++ can go upto 57V to account for transmission losses. However it is no secret that 48V DC is objectively safer than 120/240/440V AC.
- **The 48V capacity question:** On the other end of the spectrum we've got the fact that current PoE++ can carry up to 90W. Is it a justifiable tradeoff for your LED TV to have two or even three input power ports? Although given current trends (See PoE+++ below) this question could soon become redundant.
- **Heavy appliances:** Your washer & dryer still run on AC and it is probably impractical to stick 24 ethernet cables into them. If this solution is adopted we'd probably have utility closets in our homes where the AC lands and connects to DC injectors. The same place can contain heavy appliances. This also helps with child/pet proofing since you can keep just that one place off limits.

Although as for heavy appliances, unless you've got centralized heating you'd still need a solution for air conditioners & refrigerators. However this is already addressed by current approaches --- In India for example we've got 6amp plugs and 16amp plugs, the 16amp usually near the ceiling and often just one or two in each room for the heavy appliances like air conditioners.

Perhaps we can keep the 16amp lines whose appliances are all pretty much AC appliances anyway and replace the 6amp lines to save some scarce resources, money, lives and the planet.

- **PoE+++:** The assumption that the IEEE could bump up the wattage isn't unreasonable. PoE started at 15.4 Watts (First generation) to PoE+ at 30W (Second generation) to PoE++ at 60 (Third generation) and then 90/100 watts (Fourth/current generation). Each generation roughly doubled wattage and the IEEE is already working on the Fifth generation.

## Compounding benefits

This section is perhaps the most optimistic but I want to stress how an article that started with talking about how EV chargers, PoE switches and ethernet cables exist went into saving materials, making electricity safer & more. However the thing about simplification & efficiency is that they don't just serve as one time bonuses but enable new breakthroughs & capabilities.

Here's one example of what's possible though: A router that knows your ceiling fan's power signature can tell you when something's wrong long before it fails. How much money in predictive maintenance can we save by fixing stuff before it starts smoking?

And how much do we save in external costs if we drastically reduce short circuits & reduce electrocution/electrical fire risk? TP-Link is already circling this with their PoE watchdog. But it's still trapped in marketing to IP cameras rather than predictive maintenance for pretty much all DC equipment.

## Conclusion

How much are we throwing away simply because we made the right call and then the passage of time flipped the board? And how many professionals are rediscovering the same problems and solutions across different fields?

The networking industry solved the supply side. The automobile industry figured out the conversion angle. The solar industry already standardized to 48V that they then convert to AC just to feed it to your home and for your devices to convert it back into DC. Read that again.

Do you see something I missed? Or just want to discuss the piece? Please reach out to me on twitter!

You can also connect with me on X: <a href="https://x.com/AgentAayush?ref_src=twsrc%5Etfw" class="twitter-follow-button" data-size="large" data-show-count="true">Follow @AgentAayush</a>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
