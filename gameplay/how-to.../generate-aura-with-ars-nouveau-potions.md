# Generate Aura with Ars Nouveau Potions

The Lingering Absorber, also referred to as the Ritual of the Brewer, generates Aura when a Lingering Potion is splashed into the ritual area. The potion must be some sort of positive effect and the amount of Aura generated will be based off of the strength and duration of the potion.&#x20;

This can be automated fairly simply with a Dispenser throwing the potions down periodically, however that requires brewing a potion up to a Splash and then Lingering variant. This requirement can be shortcut through the clever use of Ars Nouveau's potion system.

Ars Nouveau's Potion Jars do not support Splash or Lingering potions directly, but the potions stored in those jars may be converted to Splashing or Lingering with the use of the following spell form:

<mark style="color:purple;">Projectile</mark> > <mark style="color:green;">Infuse</mark> > <mark style="color:orange;">Extend Time</mark>

This spell can be set on a Spell Turret and used to pull potions from a Potion Jar and splash them as a Lingering Potion onto the Lingering Absorber, essentially skipping two brewing steps at the cost of a small amount of Source.

Let's take a look at how to set that up. We'll need a few things to get started:

* 1x Basic Spell Turret or Enchanted Spell Turret
* 1x Spell Parchment inscribed with the form <mark style="color:purple;">Projectile</mark> > <mark style="color:green;">Infuse</mark> > <mark style="color:orange;">Extend Time</mark>
* 1x Source Jar
* 1x Potion Jar

To begin, place the Spell Turret facing the Lingering Absorber and right-click it with the Spell Parchment.  If done properly, the spell form will show when looking at the Turret. Place the Potion Jar and Source Jar nearby. The Potion Jar must be directly adjacent, but the Source Jar can be further away.&#x20;

{% hint style="info" %}
The Spell Turret can be pointed at any face of the Lingering Abosrber and works equally well pointing down as from the side.
{% endhint %}

{% hint style="success" %}
Consider upgrading to an Enchanted Spell Turret when possible to reduce the Source cost of firing the Turret.
{% endhint %}

Applying any redstone signal to the Spell Turret will cause it to fire, generating Aura. We'll want that to happen automatically, of course, so we will need some tools to read the Aura and some redstone logic. There are numerous ways of setting up this logic, but for this example we'll use the Redstone Logic Control from the Redstone Pen mod.&#x20;

* 1x Redstone Logic Control (RLC)
* 1x Aura Detector
* 1x Redstone Pen

Now, the logic we need for this is quite simple: If the Aura Detector has a signal less than 13, we want to output a pulse every 100 ticks. That gives the Lingering Absorber enough time to finish absorbing one potion before another is fired. In terms of the RLC, this can be expressed as follows:

```javascript
R = IF(Y.CO<13, TIV1(100), 0)
```

We'll work through what all of that means in a moment. First, let's look at the setup in game:

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>A friendly Starby appeared to deliver potions. How nice!</p></figcaption></figure>

We've re-arranged things slightly here, moving the Source Jar out of the way to give us easier access to the Turret. The Aura Detector has been placed next to the Lingering Absorber to accurately read the local Aura level. Finally, the RLC has been placed between the Aura Detector and the Redstone Pen line running into the Turret.

{% hint style="info" %}
The colors on the outside of the RLC correspond with the variables used in the expression where R is Red and Y is Yellow. If your RLC is placed differently, be sure to adjust those variables. The appropriate variables can be seen in the GUI itself.
{% endhint %}

So let's walk through that expression. First, the IF statement works as follows `IF(Condition, Output if True, Output if False)` In our condition, we're comparing the comparator output of Y to 13.  If it is greater than or equal to 13, we output 0 on R. If it is less than 13, the `TIV()` function will output a pulse every of 100 ticks.&#x20;

Once the expression has been entered into the RLC, be sure to enable it by clicking the Run/Stop button in the top right corner. It will be a green triangle when running.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Activated RLC</p></figcaption></figure>

{% hint style="warning" %}
Firing a potion at the Lingering Abosorber when Aura is high can lead to some of that potion being wasted as the Lingering Absorber has a cap on how high Aura can go. To remove this cap, be sure to place a Creational Catalyst under your Lingering Absorber!&#x20;
{% endhint %}
