Title: Finding a prime number whose binary representation is a giraffe (or a T-Rex)
Date: 2018-01-21 08:11:32
Modified: 2018-01-21 19:41:32+05:30
Slug: prime-number-binary-trex
Authors: shabda
Tags: python
Summary: Demo
![Odd Number Theorists](https://raw.githubusercontent.com/shabda/experiments/master/prime_dinos/odd-number-theorists.jpg)

First, here is a prime number whose binary representation is a T-Rex.

<script src="https://gist.github.com/shabda/a3fce10a702f9a48c0e7989ac5802739.js"></script>

Math with bad drawings [asked](https://mathwithbaddrawings.com/2017/10/09/insatiable-for-updates/) for a prime number whose binary representation is a giraffe. This lead to discussion on [Math.reddit](https://www.reddit.com/r/math/comments/7qpfls/does_there_exist_a_prime_number_whose/) and [Hacker news](https://news.ycombinator.com/item?id=16192608) which led to finding such a prime number.

342581792649127676198127791406119644054852809184750511204770992210601825938383173228625368612512343524580568135765381129832784929140287102447656231670490278371618738040053550924844754083137995849927287014555199009052294292243605111352278964229602623894816760629354416193979550552423279842373621548435137856781153105076831681645952473068169294190544029391463758663828828100567003458546392021905815042131115480711892076216081858013250696070743624005842779807059777397154653840706692288630135185563366228931093496037459868457738024280865863648682544327375771172685872176976999577303715645442779935499071380556380855234358517399907184246818275843840363379983214925406281243183361618849192180391506653641933784053451121171160334712857092937535606122822893204604775038632348974223351004456787673186165100098223897371450275291114458983950607846718107603195397991880820766444935587675531082421404505700110617860358142315360174185418283092141238404865012380329781867103175076882601367389664213176688432343205501275425887396821357210158983064167712232404771958406106398750988506833703615072967113838953

You can see the binary representation on reddit. 

As Nathaniel Borenstein [said](https://en.wikiquote.org/wiki/Nathaniel_Borenstein) `It should be noted that no ethically-trained software engineer would ever consent to write a DestroyBaghdad procedure. Basic professional ethics would instead require him to write a DestroyCity procedure, to which Baghdad could be given as a parameter.`

So the obvious next step is to generalize this is to program which can take an image and find a primary number whose binary representation is the image.

We run this for an Argentinosaurus image

    python prime_dinosaur.py -f ~/Downloads/argentinosaurus.jpg -s 40

Which gives us

<script src="https://gist.github.com/shabda/ccf7f224df92a1484b46d2d7c28bd37f.js"></script>

### How does this work?

* We read the image and convert to desired size
* The image data is converted to monochrome and pixels darker (lower) than a threshold are converted 1, rest pixels are zero.
* This data is read in in a numpy array
* This 2d array is flattened, and treated as a bitarray to get a number
* We then start incrementing the number until we get a prime.
* The primality is tested using the Miller Rabin test.
* When such a number is found the number is converted to its binary representation, and displayed in a square grid.

The full code is:

<script src="https://gist.github.com/shabda/20dc8dd2665920f857f24b8b224c63ea.js"></script>


