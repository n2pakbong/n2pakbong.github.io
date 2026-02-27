---
title: "How large do numbers get, really?"
---

As I am finally starting my maths blog, I thought it would be fitting for the first post to be on one of the first things that triggered my mathematical curiosity in my younger years: *large* numbers (and no, I will not be talking about the [law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) today, although it will eventually make an appearance on this blog with probability close to 1). More specifically, I want to try to give a somewhat satisfying answer to the following question: *how large can numbers get?* Most of the following discussion will be mostly elementary and not rely on mathematical concepts beyond the high school level, I therefore hope this will be accessible to most curious (and mathematically-inclined) readers!


## 1. The Harmonic Series

There are many ways one might be led to think about concepts like *infinity* and large numbers. For me, one of the most striking such way is the divergence of the [Harmonic Series](https://en.wikipedia.org/wiki/Harmonic_series_(mathematics)), to which I was introduced as a first-year *classe prépa* student. Let's first explain what the Harmonic Series is: consider the (infinite) sequence of inverses of natural numbers, i.e. $1, 1/2, 1/3, 1/4,\ldots$ and so on. Note that these values are decreasing and get closer and closer to zero as we keep going further in the sequence (for instance $1/10 = 0.1 \le 1/100 = 0.01 \le 1/10000= 0.0001$ and so on...). Now let $N$ be a large natural number (say, $N= 9999999$ if you like), and denote the *partial sum* of the Harmonic Series as follows:
$$H_N\equiv \sum_{n=1}^N \frac1n := 1 + \frac12 + \frac 13 + \cdots + \frac1N, \tag1 $$
the left-hand side of $(1)$ is a [conveniently shorter notation](https://en.wikipedia.org/wiki/Summation#Capital-sigma_notation) for its right-hand side.  

So the partial sum of the Harmonic Series is just the sum of the $N$ first inverse natural numbers. Now, what we call the Harmonic Series is the limit of the sum $(1)$ above, as we let $N$ increase to infinity. That is, it is the sum of *all* the inverse natural numbers. Mathematically, we denote it as follows:
$$\lim_{N\to\infty} \sum_{n=1}^N \frac1n \equiv\sum_{n=1}^\infty \frac1n \equiv 1 + \frac12 + \frac 13  + \frac 14 \cdots. \tag2  $$

Now, what is quite remarkable about the above quantity, is that although it is a sum of tiny numbers which are getting closer and closer to $0$, the sum value is actually *infinite*. That is, for any number $M$ you can imagine, you can always find a number $N$ of terms such that $H_N$ is larger than your $M$ of choice. This is what we mean when we say that the Harmonic series *diverges*. This was an absolute shock to me upon learning this for the first time, *"how could tiny numbers possibly add up to any arbitrarily large quantity?"* was my reaction. Perhaps you don't find this particularly shocking, since we're adding up an infinite number of terms after all, but to convince you that it is not such an obvious fact, consider the sequence of inverse powers of two, i.e. $1/2, 1/4, 1/8, 1/16, 1/32 \ldots$ and so on. Now consider the associated *geometric series*:
$$S := \sum_{n=1}^\infty \frac{1}{2^n} = \frac12 + \frac 14 + \frac18+ \frac{1}{16} + \cdots. $$

Here again, the sequence of inverse powers of two is monotonically decreasing towards zero, and although we're adding infinitely many terms, we can show that the sum $S$ does *not* get arbitrarily large as we keep adding more and more terms (in fact, we can show that the sum gets closer and closer to $1$ as we keep adding terms, see [this Wikipedia page](https://en.wikipedia.org/wiki/Geometric_series) for details). 

So then, how do we make sense of the divergence of the Harmonic Series, and how can we even see that the series diverges? There is actually [quite](https://en.wikipedia.org/wiki/Harmonic_series_(mathematics)#Comparison_test) [a number](https://en.wikipedia.org/wiki/Harmonic_series_(mathematics)#Integral_test) [of ways](https://proofwiki.org/wiki/Harmonic_Series_is_Divergent) [one can](https://math.stackexchange.com/questions/255/why-does-the-series-sum-n-1-infty-frac1n-not-converge) [prove](https://scipp-legacy.pbsci.ucsc.edu/~haber/ph116A/harmapa.pdf) that the series diverges, but a standard proof which achieves both aims at once is obtained by noting that the function $x\mapsto 1/x$ is monotone decreasing on the interval $[1,\infty)$, which gives the lower bound (see [here](https://math.stackexchange.com/a/3207608/857384) for details)
$$H_N \ge \int_1^{N+1} \frac{dx}{x} \ge \log N, \tag3 $$
where $\log$ denotes the [natural logarithm](https://en.wikipedia.org/wiki/Natural_logarithm). Hence from Equation $(3)$ we can see that $H_N$ grows like the logarithm of $N$, which indeed grows unboundedly with $N$. Better yet, by the same argument used to obtain $(3)$, we can show that $H_N \le \log N + \gamma $, where $\gamma > 0$ is a [constant](https://en.wikipedia.org/wiki/Euler's_constant) independent of $N$. So now, we know that the Harmonic series diverges, and it does so at a logarithmic speed. We can thus answer the question: for a given number $M$, how many terms $N$ do we need in the sum $H_N$ for it to be larger than $M$? Well, since we've shown that $H_N \approx \log N$, it follows that we can take $N$ such that $ \log N \ge M$ to reach our desired target $M$, which after applying the [exponential](https://en.wikipedia.org/wiki/Exponential_function), tells us that roughly $N\approx \exp(M)$ terms are enough to get $H_N \ge M$.

## 2. From a slowly diverging series to astronomically large numbers

Now that we have some quantitative understanding of why and how the harmonic series diverges to infinity, we can make sense out of why the statement *the harmonic series diverges* may seem so counter-intuitive (to my younger self, at least).

### Mining some "harmonic bitcoin"

Let's do a thought experiment. Imagine we're mining a virtual currency, akin to bitcoin, which works as follows: for every unit of computational resource we use, we are able to compute an additional term of the harmonic series, and we get paid in (Hong Kong) dollars the total sum we have been able to compute. In words, if we have $N=10$ units of computational resources, we can compute the harmonic series up to $1 + 1/2 + \ldots + 1/10 $, and we thus get paid $1 + 1/2 + \ldots + 1/10 \approx 3$ HK\$ for this computational work. Now, how much computational resources would we need to earn 200HK\$ this way? Well, as we've seen in the previous section, it takes roughly $N\approx\exp(200)$ terms for the first $N$ terms of the harmonic series to be larger than $200$. So we would need approximately $N = 7\times 10^86$ (that's a seven with *86 zeroes* after it) units of computational resources to earn 200 dollars! For the record, experts estimate that [the number of atoms in the observable universe is around $10^80$](https://en.wikipedia.org/wiki/Observable_universe#Matter_content%E2%80%94number_of_atoms). Said differently, this means that even if we could magically turn *every single atom of the universe* into a computational unit solely dedicated to our harmonic bitcoin mining objective, we would still need about *a million copies* of the universe to earn 200 dollars!

### Saving up on a mortgage 

Another amusing thought experiment which conveys the same idea is the following: we start a timer at time $T_0=0s$, and every second after that, an amount $1/j$ $ is deposited to our bank account by a (kind, wealthy, and immortal) stranger, where $j$ is the current second. Said differently, at time $T=1s$, we receive 1\$, at time $T=2s$, we receive 1/2\$, and so on, such that after $N$ seconds, our bank account has received $H_N \approx \log N$ HK\$. How long do we need to wait to have received 10\$, 20\$, or \50$? Running the same computations as before, we find that it would take respectively $2.2\times 10^4s\approx$ 6 hours to earn 10\$, $4.8\times 10^8s\approx$ 15 *years* to earn 20\$, and $5.2\times 10^21s\approx 1.6 \times 10^14$ *years* to earn 50\$. For the record, this last one is about [*ten thousand times* greater than the age of the universe](https://81018.com/universeclock/). So yeah, we're not buying a house anytime soon.

### Ubiquity of large numbers in mathematics

Thought experiments like the two above (and many more variants) explain why the divergence of the harmonic series feels like such a counterintuitive fact: the series diverges so slowly that we have no way, within the space and time scales of our universe, to "witness" it reaching those arbitrarily large values. To us, this divergence to infinity, while true in theory, is not physically observable. Slowly (or, viewed conversely, rapidly growing) quantities, such as $\log$ and $\exp$, which lead to the consideration of these "unphysical" are however far being mere curiosities or thought experiments. Indeed, such objects arise naturally to answer deep and important questions in a lot of mathematical subfields. Here are a few examples that I like:
- The partial sum of reciprocals of prime numbers $\sum_{p \le x, p\text{prime}} \frac1p $ is [known](https://en.wikipedia.org/wiki/Divergence_of_the_sum_of_the_reciprocals_of_the_primes) to grow at a rate of $\log\log x$. That is a whole order of magnitude *slower* than $\log$.
- The function $x\mapsto\log\log x$ famously appears in [the law of the iterated logarithm](https://en.wikipedia.org/wiki/Law_of_the_iterated_logarithm) which quantifies the fluctuations of a scaled random walk. 
- The [Moment Generating Function](https://en.wikipedia.org/wiki/Moment-generating_function) of a Poisson random variable with parameter $\lambda > 0$ is given by the function $f:t\mapsto e^{\lambda(\exp(t) - 1)}$. That is *exponentially faster* growth than the function $\exp$!

And these examples do not even remotely scratch the surface of the
---

## 2. How to “reconcile” this fact with my intuition?  
### A “hard threshold” on the computations we can do

- Point 1 : la borne inférieure logarithmique \(\Rightarrow\) divergence « indéniable »
  - Rappeler : si
    \[
    H_N \ge \ln N + \text{(constante)},
    \]
    alors comme \(\ln N \to \infty\), la série diverge.
  - Souligner le lien \(\ln\) / \(\exp\) :
    - \(\ln N \to \infty\) équivaut au fait que \(\exp(M)\) peut atteindre des valeurs aussi grandes qu’on veut quand \(M\) croît.
    - L’exponentielle \(\exp\) a une croissance « indéniable », évidente, même pour l’intuition.
    - Donc, via cette borne logarithmique, la divergence de la série harmonique devient *mathématiquement* évidente, même si elle reste *intuitivement* bizarre.
- Point 2 : regarder la question « à l’envers »  
  **Combien de termes faut-il pour atteindre une valeur donnée \(M\) ?**
  - Utiliser l’approximation/comparaison avec le logarithme :
    - On cherche \(N\) tel que :
      \[
      H_N \approx \ln N \approx M.
      \]
    - Donc :
      \[
      N \approx e^M.
      \]
  - Exemple \(M = 100\) :
    - En première approximation, il « faut » \(N \approx e^{100}\) termes.
    - Idée à faire passer : ce \(N\) est « incroyablement grand ».
    - Comparaison souhaitée dans ton brouillon :
      - **Idée** : dire que \(e^{100}\) est plus grand que le nombre estimé de particules dans l’univers observable.
      - **NOTE pour toi** : vérification numérique :
        - \(e^{100} \approx 3.7 \times 10^{43}\),
        - estimations usuelles pour le nombre de particules dans l’univers observable : \(\sim 10^{80}\)–\(10^{90}\).
        - Donc \(e^{100}\) est *beaucoup plus petit* que \(10^{80}\).
      - Suggestion (à décider toi-même) :
        - soit garder \(M = 100\) et comparer à autre chose,
        - soit prendre un \(M\) plus grand (par ex. \(M \approx 200\)) pour avoir \(e^M\) réellement plus grand que \(10^{80}\).
  - Scénario 1 : assigner chaque terme à une particule de l’univers
    - Idée exacte de ton brouillon :
      - « si on assignait à chaque particule numéro \(j\) de l’univers le calcul du \(j\)-ème terme de la somme et qu’on sommait le tout, on atteindrait même pas 100 ».
    - À détailler :
      - Nombre estimé de particules : \(\sim 10^{80}\) (ordre de grandeur).
      - On calcule et additionne \(\sum_{j=1}^{10^{80}} \frac{1}{j}\).
      - Valeur approchée via le log :
        \[
        H_{10^{80}} \approx \ln(10^{80}) + \gamma \approx 80 \ln 10 + \gamma \approx 80 \times 2.3026 \approx 184.
        \]
      - **NOTE** : du coup, avec \(10^{80}\) particules on dépasse déjà 100 (on arrive vers ~184).
      - Tu peux :
        - soit ajuster la valeur cible \(M\),
        - soit ajuster le nombre de particules / l’ordre de grandeur pour garder l’effet « wow » comme tu le souhaites.
  - Scénario 2 : ajouter un terme toutes les nanosecondes
    - Idée de ton brouillon :
      - « si on ajoutait le \(j\)-ème terme à la somme toutes les nano-secondes, il faudrait des milliards de milliards de milliards de fois l'âge de l'univers pour atteindre 100 ».
    - À faire :
      - Donner un ordre de grandeur de l’âge de l’univers en secondes puis en nanosecondes.
      - Calculer le nombre de termes ajoutés en « un âge de l’univers » au rythme d’un terme par nanoseconde.
      - Approximer la valeur de \(H_N\) correspondante via \(\ln N\).
      - Comparer cette valeur à la cible voulue (par ex. \(M = 100\)).
      - Conclure : même après tout ce temps, on est très en-dessous / on ne l’atteint pas.
  - Scénario 3 : combiner les deux idées
    - Idée de ton brouillon :
      - « on peut combiner les autres en faisant la somme des \(j\) à \(j+10^{80}\) termes de la somme toutes les secondes et regarder la valeur de la somme qu'on atteint après avoir attendu l'âge de l'univers ».
    - Plan :
      - Chaque seconde, tu ajoutes un « bloc » de \(10^{80}\) termes supplémentaires.
      - Tu fais ça pendant un nombre de secondes comparable à l’âge de l’univers.
      - Tu estimes combien de termes au total ont été ajoutés (ordre de grandeur).
      - Tu utilises encore l’approximation logarithmique pour \(H_N\).
      - Objectif : montrer que même en « exploitant » à fond les ressources de l’univers (en caricaturant), tu n’atteins pas des valeurs si grandes que ça pour \(H_N\) (ou en tout cas, que ça reste « raisonnable » par rapport aux cibles \(M\) que tu choisis).
- Message de fond de cette section
  - La divergence de la série harmonique est un phénomène **purement mathématique / abstrait**.
  - Elle n’est pas « physiquement observable » dans aucun scénario réaliste dans notre univers.
  - C’est ce décalage entre monde mathématique (où on peut aller jusqu’à \(N \approx e^{100}\), \(e^{1000}\), etc.) et monde physique (où même \(N \approx 10^{80}\) est un plafond dur) qui rend le résultat contre-intuitif.
  - Tu peux ajouter d’autres exemples « astronomiques » si tu veux :
    - temps nécessaire pour juste *écrire* un nombre comme \(e^{1000}\) en base 10,
    - limites de stockage d’information dans l’univers observable,
    - etc.

---

## 3. So… should we reject large numbers?

- Lien avec l’**ultra-finitisme**
  - Transition :
    - Face à des nombres comme \(e^{100}\), \(e^{1000}\), Graham’s number, TREE(3), etc., on peut commencer à douter du sens de ces objets.
  - Présentation rapide :
    - L’ultra-finitisme est une position philosophique qui va plus loin que le simple finitisme :
      - non seulement sceptique vis-à-vis des infinis « réels »,
      - mais aussi vis-à-vis des entiers **tellement grands** qu’on ne pourra jamais les « construire » concrètement dans l’univers physique.
    - Les considérations précédentes (capacité de calcul limitée par l’univers, etc.) semblent **donner des arguments** en faveur de cette position :
      - si un nombre ne peut jamais être *réalisé* (ni même écrit complètement, ni stocké), a-t-il le même statut ontologique qu’un entier « raisonnable » ?
  - Position personnelle :
    - Mentionner que tu **ne comptes pas** trancher ce débat philosophique.
    - Tu présentes juste la position, sans t’aligner explicitement.
- Une alternative (ta façon « moderne » de résoudre le paradoxe)
  - Problème intuitif qui te gêne :
    - Phrases du type :
      - « Graham’s number is unimaginably huge, **and there are infinitely many numbers even larger!** »
      - « TREE(3) is inconceivably large, and still just one point in an infinite tower of even larger numbers. »
    - Ces phrases t’ont toujours dérangé « intuitivement » :
      - oui, c’est vrai mathématiquement,
      - mais ça semble « déconnecté » de tout ce qu’on peut réellement appréhender ou rencontrer.
  - Idée principale : **mettre une distribution de probabilité sur \(\mathbb{N}\)**
    - On considère une loi de probabilité \(P\) sur les entiers naturels \(\{1,2,3,\dots\}\).
    - Interprétation :
      - cette mesure modélise la **distribution des nombres entiers** qu’un humain pris au hasard a plus ou moins de chances de rencontrer dans sa vie de tous les jours.
        - nombres qui apparaissent dans des problèmes,
        - dans des mesures physiques,
        - dans des discussions,
        - etc.
      - (Tu pourras reformuler ce passage dans ta voix.)
    - Conditions minimales :
      - C’est une **vraie** loi de probabilité :
        \[
        \sum_{n=1}^\infty P(n) = 1.
        \]
      - On postule l’existence d’une telle loi (pas besoin de la décrire précisément).
  - Conséquence clé : masse des très grands nombres \(\to 0\)
    - Du simple fait que la somme des probabilités doit être égale à 1, on a nécessairement :
      - \(\lim_{n \to \infty} P(n) = 0\),
      - et pour tout seuil \(K\), la masse de l’événement \(\{n > K\}\) doit tendre vers 0 quand \(K \to \infty\).
    - Intuition :
      - La probabilité totale « allouée » aux nombres **très grands** doit tendre vers zéro.
      - On ne peut pas avoir « beaucoup » de probabilité concentrée sur une infinité de nombres immenses, sinon la somme dépasserait 1.
  - Comment cela « réconcilie » le paradoxe
    - On n’exclut **pas** l’existence de très grands nombres (Graham, TREE(3), etc.) :
      - ils existent pleinement dans le monde mathématique.
      - ils ont une probabilité **non nulle** (en général) dans cette distribution (sauf cas particulier).
    - Mais :
      - plus un nombre est grand / complexe, plus sa probabilité devient **faible**.
      - les « super grands nombres » sont donc *possibles*, mais statistiquement **très, très peu probables** à rencontrer « dans la nature » (au sens de cette loi).
    - Ça permet de dire :
      - « oui, il y a une infinité de nombres plus grands que Graham’s number »,
      - mais « la probabilité d’en rencontrer un dans une situation réaliste est pratiquement nulle ».
  - Lien avec le rasoir d’Ockham (version bayésienne)
    - Rappel :
      - le rasoir d’Ockham (en version bayésienne) recommande de donner des **probabilités a priori faibles** aux hypothèses plus complexes.
    - L’analogie ici :
      - les nombres très grands / compliqués représentent des « hypothèses » plus complexes.
      - leur attribuer une probabilité faible est cohérent avec cette philosophie bayésienne.
    - Tu peux expliciter un peu :
      - plus la description d’un nombre est longue / compliquée, plus sa probabilité a priori devrait être petite.
- Conclusion de la section (à écrire)
  - Idée générale à transmettre :
    - Cette vision probabiliste te permet de faire la paix avec :
      - l’existence de l’infini,
      - l’existence de nombres *ridiculement* grands,
      - tout en restant en accord avec l’intuition qu’« on ne les verra jamais » dans la pratique.
    - Elle offre une troisième voie entre :
      - accepter naïvement tous les grands nombres comme « aussi concrets » que 42,
      - et les rejeter purement et simplement à la manière ultrafinitiste.
  - **TODO** : trouver une phrase / un paragraphe de conclusion qui te convient.

---

## 4. Some related reading

- Branches des maths / infos sur les **fonctions qui croissent très vite**
  - Fonctions qui « croissent tellement vite qu’elles ne sont même plus calculables ».
    - ex. Busy Beaver function,
    - fast-growing hierarchy,
    - Goodstein sequences,
    - hydra games, etc.
  - Comment ces exemples illustrent :
    - la notion de croissance « au-delà » du calculable,
    - l’apparition de nombres encore plus gigantesques que ceux issus de la série harmonique.
- Distributions de probabilité sur \(\mathbb{N}\)
  - Types d’exemples possibles à mentionner :
    - lois géométriques,
    - lois décroissantes du type \(P(n) \propto 1/n^2\),
    - distributions inspirées de la théorie de l’information algorithmique (Solomonoff prior, etc.).
  - Comment ces lois peuvent modéliser (au moins grossièrement) :
    - la fréquence d’apparition des nombres dans les problèmes, les mesures, le langage, etc.
- Ressources populaires sur les **very large numbers**
  - Vidéos YouTube et billets de blogs sur :
    - Graham’s number,
    - TREE(3),
    - autres constructions classiques de nombres énormes.
  - Tu peux lister des liens sympas :
    - articles de blogs (par ex. *What’s New?*, *Libres pensées d’un mathématicien ordinaire*, etc. si pertinents),
    - vidéos de vulgarisation sur les grands nombres,
    - éventuellement des articles plus techniques pour lecteurs motivés.
- Liens avec la philosophie des maths / ultrafinitisme
  - Quelques références d’intro sur :
    - le finitisme,
    - l’ultra-finitisme,
    - débats sur l’existence des grands nombres / de l’infini,
    - pour les lecteurs qui voudraient aller plus loin côté philosophie.

---

<!-- Optionnel : petite conclusion générale du billet -->

## (Optional) Closing thoughts

- Rappeler en une ou deux phrases :
  - que la série harmonique diverge, mais d’une manière tellement lente qu’on ne la « voit » jamais,
  - que les nombres gigantesques apparaissent naturellement en maths (bien plus grands que tout ce qu’on peut physiquement manipuler),
  - que la perspective probabiliste te permet de garder ces objets mathématiques tout en respectant l’intuition « physique » que notre univers est très finiment limité.
- Remerciements / appel à commentaires :
  - inviter les lecteurs à proposer :
    - d’autres exemples de séries « à divergence lente »,
    - des ressources sur les grands nombres,
    - des points de vue philosophiques sur l’ultra-finitisme, etc.```
