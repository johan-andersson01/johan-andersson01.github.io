---
layout: post
title:  'A Stab at Transforming Pokemons'
---

In June 2019, Riley Wong wrote a [blog post](https://www.rileynwong.com/blog/2019/5/22/pokemon2pokemon-using-cyclegan-to-generate-pokemon-as-different-elemental-types) describing his attempt at transforming pokemons of one type into another (e.g. water to fire).

To do this, he collected pokemons from generation 1-7 and sorted them by primary type. Using CycleGAN, an image-to-image translation GAN, he then trained the network to transform water to fire, fire to dark, water to grass, etc. Below, I show his variations on the water type pokemon Gyarados.

![Gyarados transformed into fire, grass, and electric type](/media/pokemon/rileywong/gyarados.png)


However, as far as transformations go, this is very weak. We see that the only transformation taking place is a simple color change. Instead of only learning to translate colors, we want a model that is able to translate features as well. For example, leaves turning into flames or wings turning into fins.

CycleGAN is unable to perform such shapeshifting translations, due to its cyclic pixel loss. A more promising architecture is UGATIT, which excels at moderate shapeshifting.

---

### So, how do we turn leaves into flames?

UGATIT is a GAN that was published in July 2019. Images on the left (transparent backgrounds) are inputs and images on the right (black backgrounds) are outputs.

![Too "strong" transformations](/media/pokemon/input/charmander.png)
![Too "strong" transformations](/media/pokemon/ugatit1/charmander.png)
![Too "strong" transformations](/media/pokemon/input/charmeleon.png)
![Too "strong" transformations](/media/pokemon/ugatit1/charmeleon.png)
![Too "strong" transformations](/media/pokemon/input/charizard.png)
![Too "strong" transformations](/media/pokemon/ugatit1/charizard.png)

![Too "strong" transformations](/media/pokemon/input/budew.png)
![Too "strong" transformations](/media/pokemon/ugatit1/budew.png)
![Too "strong" transformations](/media/pokemon/input/bellossom.png)
![Too "strong" transformations](/media/pokemon/ugatit1/bellossom.png)


![Too "strong" transformations](/media/pokemon/input/bulbasaur.png)
![Too "strong" transformations](/media/pokemon/ugatit1/bulbasaur.png)
![Too "strong" transformations](/media/pokemon/input/ivysaur.png)
![Too "strong" transformations](/media/pokemon/ugatit1/ivysaur.png)
![Too "strong" transformations](/media/pokemon/input/venusaur.png)
![Too "strong" transformations](/media/pokemon/ugatit1/venusaur.png)

![Too "strong" transformations](/media/pokemon/input/darumaka.png)
![Too "strong" transformations](/media/pokemon/ugatit1/darumaka.png)
![Too "strong" transformations](/media/pokemon/input/magcargo.png)
![Too "strong" transformations](/media/pokemon/ugatit1/magcargo.png)

In the samples above we see that instead of transforming a pokemon of type A into the same pokemon but of type B, the network transforms a pokemon of type A into a different (existing) pokemon of type B. A classic example of overtraining. Since UGATIT has a global discriminator and since the dataset is quite small, the discriminator is able to learn how all pokemons look like. Thus, for the generator to fool the discriminator it has to generate pokemons that resemble already existing ones. This leads to a deviation from the features of the input pokemon.

It seems that the only time the network kind of does what I want (Charizard and Venusaur; third column), is where it can't find a similar enough corresponding pokemon, i.e. transforming to another pokemon would incur too high a cyclic/identity loss.

To solve this, I could increase the weight for identity and cycle losses, but at the same time I don't want to restrain the network's "creativity". Instead, I thought that removing the global discriminator could solve my problem. The network would then never learn how a full-size pokemon looks like, as long as the pokemon is larger than the discriminator's "field of view".

---

### Removing the global discriminator

Transforming fire to water, I saw some progress. Some of the results weren't recognizable pokemon anymore, but unfortunately they weren't similar to the input either.

![Too "malformed" transformations](/media/pokemon/input/charmander.png)
![Too "malformed" transformations](/media/pokemon/ugatit2_removed_global_discr/charmander.png)
![Too "malformed" transformations](/media/pokemon/input/charmeleon.png)
![Too "malformed" transformations](/media/pokemon/ugatit2_removed_global_discr/charmeleon.png)
![Too "malformed" transformations](/media/pokemon/input/charizard.png)
![Too "malformed" transformations](/media/pokemon/ugatit2_removed_global_discr/charizard.png)

Others were similar to the first results, where real pokemons had been generated and the input practically ignored.

![Too "strong" transformations](/media/pokemon/input/pansear.png)
![Too "strong" transformations](/media/pokemon/ugatit2_removed_global_discr/pansear.png)
![Too "strong" transformations](/media/pokemon/input/torchic.png)
![Too "strong" transformations](/media/pokemon/ugatit2_removed_global_discr/torchic.png)
![Too "strong" transformations](/media/pokemon/input/darmanitan-standard.png)
![Too "strong" transformations](/media/pokemon/ugatit2_removed_global_discr/darmanitan-standard.png)


Transforming water to fire, the network suffered a mode collapse. Most outputs were black, whereas some outputs were blurry versions of the input.

![Mode collapsed transformations](/media/pokemon/input/squirtle.png)
![Mode collapsed transformations](/media/pokemon/ugatit2_removed_global_discr/squirtle.png)
![Mode collapsed transformations](/media/pokemon/input/wartortle.png)
![Mode collapsed transformations](/media/pokemon/ugatit2_removed_global_discr/wartortle.png)
![Mode collapsed transformations](/media/pokemon/input/kingler.png)
![Mode collapsed transformations](/media/pokemon/ugatit2_removed_global_discr/kingler.png)

### Stay tuned

Haven't had the time to fully explore this task, so stay tuned for future updates. If you want to see more samples, you can find all of the outputs [here](https://github.com/johan-andersson01/johan-andersson01.github.io/tree/master/media/pokemon)
