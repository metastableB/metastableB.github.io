---
layout: post_md
key: a5
title: Music Discovery  
date:  2015-11-15 08:30 +0530 
categories: projects
author: Don Dennis
header-img: /img/snake_charmer_original.jpg
bg-class-name: music_discovery
---
**_tldr:_** *I am not really satisfied with existing music recommendation/discovery systems and hence try to find out more about the research being done in the area. I try and see if a system based on a purely taste and context based learning model is feasible. I read a bunch of papers and realize that a lot of work has been done in the field but in a scattered fashion. I try to explore if these works can be combined and improved and eventually stumble upon last.fm and pandora. Pandora and last.fm do most of the things I want in the software I have in mind but when both of them are considered together.*

___

The idea is to see if a learning based music recommendation model is feasible, and compare the existing ones on how they perform. As I discuss below, learning models for suggestions of different kinds - movies, music, book etc, exist and are extensively used. They generally use classifications based on the the usual set of attributes and trends which in some sense can be quantified. I've observed that these models are not always perfect, especially when it comes to music. There is an inherent quality to music that defines a persons taste and that is what I am really interested in. Can we really learn more about this? Can we teach a machine learn your taste and make decisions based on context? I have a strong feeling we can.

Why you ask? Well the way I try to see if a problem is addressable by machine learning techniques is to see if given the same inputs and constraints, a human being can solve it. If yes, I think its an indication that a learning based model can be created for that task.

The task at hand does seem to be something humans are good at. I mean take two people with similar taste in music for example, say Alice and Bob (*shout out to crypto folks*). There is a high probability (intuitive) that as Alice’s likes and dislikes get closer to Bob’s, any song that Alice discovers, Bob will love. Hence, if we can create an Alice for each Bob (cupid are we?), the model might just perform. Again you need a database of songs to explore, like a huge collection of songs you got from your friend or maybe something like a spotify database ? Alice quietly scans the DB, finds something she loves and shares it with Bob. This is not something you can expect instantaneous results out of, but something you can couple up with the existing suggestion systems.



## Existing Technologies

A lot of existing learning, classification, clustering, graph and FOF based models exist and YouTube suggestions is a good example. Though these are either not exactly specialized for discovering music, or depend way too much on attributes rather than song 'fingerprints'. Also, most of these don't take context into account.

Other softwares I know of that includes some suggestion systems are 
    
* Mixxx - AutoDJ, **which I worked on.**
* Smart Play List and song Moods - Clementine
* iTunes DJ - iTunes

**GNOD: Global network of discovery**
Does a brilliant job! They take inputs from you and finds songs/artists that you might like using a map based model they have build. 

If this was integrated to some native Linux music app and used the users library as an input replacing an active user participation, it would have been much more convenient.

They don't take the actual music structure into consideration though, which is what I'm looking for here.

[Here](https://www.quora.com/What-are-some-music-recommendation-websites/answer/Christoph-M%C3%B6ller) you can find a list of similar search and discovery sites which take some kind of user input and then works on it.

**Last FM and Pandora**
[This](http://blog.stevekrause.org/2006/01/pandora-and-lastfm-nature-vs-nurture-in.html) blog gives excellent insights into what lastfm and Pandora does and I must say that those two together more or less does what I had in mind.

At this point I guess I'm going to read more on them and use the technology to see its results before doing more work. 
**<u>UPDATE[14 December 2015 (Monday)]:</u>** Since this is not a well maintained blog and is sort of a temporary report, I thought I'd brain dump more. So as of today 14 December 2015 (Monday), 03:57 am, I am trying to reach out to more people working on feature learning and hoping that someone will give me an opportunity to work with them. Man this is tough! Anyway, for the mean time I've decided to start by general clustering on MP3 waveforms from January. (December is for DJANGO man!). This will teach me more about mp3 compression and how to deal with them. I hope I don't have to start from scratch though. Frankly, I am torn between using raw formats or compressed formats. I am its reasonable to believe that compressed formats will lead to additional complexity. I have no evidence to substantiate this though. Let us see.</p>

*The following content was prepared before I actually encountered the above mentioned blog, and I've decided to keep it here for my own future reference.*

___

### [Music Recommendation Based on Acoustic Features and User Access Patterns][5]
    ~2009
    Bo Shao, Sch. of Comput. Sci., 
    Florida Int. Univ., Miami, FL, USA 


 >Collaborative-filtering and content-based recommendations are two widely used approaches that have been proposed for music recommendation. However, both approaches have their own disadvantages: collaborative-filtering methods need a large collection of user history data and content-based methods lack the ability of understanding the interests and preferences of users. To overcome these limitations, this paper presents a novel dynamic music similarity measurement strategy that utilizes both content features and user access patterns. The seamless integration of them significantly improves the music similarity measurement accuracy and performance. Based on this strategy, recommended songs are obtained by a means of label propagation over a graph representing music similarity. 

This is pretty recent work and what I'm going to start with. The ideas seem sound and the results are encouraging. They underline a method utilizing content feature as well has user access patterns. If I could incorporate the music nature/mood as a music feature and figure out how to use context information as outlines in [3][3] I can make a unsupervised self correcting (secondary correction) plugin for Clementine. Using the mood information and lastfm integration already available in Clementine, this can be possibly extended to using web data as well, possibly improving results.

### [A music recommendation system based on music data grouping and user interests][2] 
    2001
    Hung-Chen Chen  National Tsing Hua University Hsinchu, Taiwan
    Arbee L. P. Chen    National Tsing Hua University Hsinchu, Taiwan


>With the growth of the World Wide Web, a large amount of music data is available on the Internet. In addition to searching expected music objects for users, it becomes necessary to develop a recommendation service. In this paper, we design the Music Recommendation System (MRS) to provide a personalized service of music recommendation. The music objects of MIDI format are first analyzed. For each polyphonic music object, the representative track is first determined, and then six features are extracted from this track. According to the features, the music objects are properly grouped. For users, the access histories are analyzed to derive user interests. The content-based, collaborative and statistics-based recommendation methods are proposed, which are based on the favorite degrees of the users to the music groups. A series of experiments are carried out to show that our approach is feasible.

The only downside I see here is them using **MIDI** objects to do the initial analysis. I am not exactly sure how they are actually doing this but considering the fact that midi outputs of mp3's or other common audio formats are not available easily, this does not seem to be the best way, at least not one your average Joe can use ?

Again, remember that `clementine` is able to plot a music mood graph on a per song basis. Is it possible that similar songs can be clustered on the basis of this mood graph? Maybe, this can even be used in place of the MIDI based model to comparable or even better results? 

### [A Context-Aware Music Recommendation System Using Fuzzy Bayesian Networks with Utility Theory][3]
    ~2006
    Han-Saem Park
    Ji-Oh Yoo
    Sung-Bae Cho


>This paper proposes a context-aware music recommendation system (CA-MRS) that exploits the fuzzy system, Bayesian networks and the utility theory in order to recommend appropriate music with respect to the context.

What was described in [2][2] was a suggestion system based on the attributes ignoring context, or mood as I like to put it. *(note to self : how do you plan to detect mood anyway ? Well I guess using a running history of songs coupled with an adaptive system which changes the suggestions depending on how the user responds to previous suggestions may work.)*

Okay, It seems I should start with researching and working on a simple attribute based classification system first, taking the attributes to be readily available. I mean you can later use other classification and clustering algorithms to modify the attribute set to reflect only those that matter and even go to the extent of making new arbitrary attributes which have no physical sense as long as they contribute to the overall prediction model.

Then after you have established a reasonable model, implement the mood based classification in place of the midi based model but deriving ideas from the same.

The finally, move on to implement a fuzzy decision making model, using context aware systems to finish off what you actually started to build.

### [Foafing the Music : Bridging the Semantic Gap in Music Recommendation][4]
    ~2006
    Oscar Celma, Music Technology Group, 
    University Pompeu Fabra, Barcelona, Spain


>In this paper we give an overview of the Foafing the Music system. The system uses the Friend of a Friend (FOAF) and RDF Site Summary (RSS) vocabularies for recommending music to a user, depending on the user’s musical tastes and listening habits. Music information (new album releases, podcast sessions, audio from MP3 blogs, related artists news and upcoming gigs) is gathered from thousands of RSS feeds.

This is interesting. Creating a FOF graph of users with similar taste can help the learning process speed up. Though this is something I'm getting into only after I have some working system.


## Other Interesting Reads.
**[A Machine Learning Approach to Musical Style Recognition][1]**

    Roger B. Dannenberg
    Belinda Thom
    David Watson

**[Limitations of interactive music recommendation based on audio content][6]**
    
    ~2010
    Arthur Flexer   Austrian Research Institute for Artificial Intelligence
    Martin Gasser   Austrian Research Institute for Artificial Intelligence
    Dominik Schnitzer   Johannes Kepler University, Linz, Austria

**[Location-aware music recommendation][7]**

    Matthias Braunhofer
    Marius Kaminskas 
    Francesco Ricci

** [LEARNING FEATURES FROM MUSIC AUDIO WITH DEEP BELIEF NETWORKS][8]**
   

    Philippe Hamel 
    Douglas Eck
    DIRO, Universite de Montreal 

**AUTOTAGGING MUSIC USING SUPERVISED MACHINE LEARNING**

    ~2006
    Douglas Eck, Sun Labs
    Thierry Bertin-Mahieux, Univ. of Montreal
    Paul Lamere,Sun Labs


[1]: http://repository.cmu.edu/cgi/viewcontent.cgi?article=1496&context=compsci "A machine learning approach to musical style recognition"
[2]: http://dl.acm.org/citation.cfm?id=502625&preflayout=tabs "A music recommendation system based on music data grouping and user interests"
[3]: http://link.springer.com/chapter/10.1007/11881599_121 "A Context-Aware Music Recommendation System Using Fuzzy Bayesian Networks with Utility Theory"
[4]: http://link.springer.com/chapter/10.1007/11926078_67 "Foafing the Music: Bridging the Semantic Gap in Music Recommendation"
[5]: http://dl.acm.org/citation.cfm?id=1859812  "Music Recommendation Based on Acoustic Features and User Access Patterns"
[6]: http://dl.acm.org/citation.cfm?id=1859812 "Limitations of interactive music recommendation based on audio content"
[7]: http://link.springer.com/article/10.1007/s13735-012-0032-2 "Location-aware music recommendation"
[8]: http://ismir2010.ismir.net/proceedings/ismir2010-58.pdf "LEARNING FEATURES FROM MUSIC AUDIO WITH DEEP BELIEF NETWORKS"