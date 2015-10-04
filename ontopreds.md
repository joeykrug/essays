# Ontology based prediction markets

As a long time enthusiast of [prediction markets](https://en.wikipedia.org/wiki/Prediction_market) I am following with great interest the progress of the [Augur Project](http://www.augur.net).

A [recent post](http://www.augur.net/blog/i-robot-will-be-the-judge-the-case-for-artificial-intelligenc) on the Augur blog talks about what it would take to make it as easy and efficient as possible for the market referees to adjudicate predictions, i.e. to judge if a predicted event did in fact happen by the appointed deadline.

To exemplify, imagine Alice establishes a market for the prediction:

"By March 31 2015, Siemens will have become a customer of Neo Technology"

On April 1, 2015 it becomes the task of referees Barbara, Carol and Diana to decide if the event described in Alice's market did, in fact, take place.

Barbara, Carol and Diana are busy people and so would appreciate help in finding and sorting out reliable information sources that contain evidence about that question (for example, official announcements on the web site of Siemens or Neo Technology).

This is particularly important because Barbara, Carol and Diana will have to deal not only with Alice's prediction but with hundreds or thousands of predictions made by many other people on the market.

The Augur post points in an obvious direction: anything that can be automated *should* be automated.

And we know that the referees' task can be automated because we have, *today*, software agents that are reasonably good at

1. performing queries on search engines (e.g. search for: +"Siemens" +"Neo Technology")
2. identifying mentions of the relevant entities in the documents returned by those queries (i.e. Siemens and Neo Technology)
3. determining if the entities mentioned stand in the appropriate relationship to one another (i.e to make sure that the document is about Siemems becoming a customer of Neo Technology and not viceversa)

As the post itself points out, steps 1, 2 and 3 are exactly the steps a system such as [IBM's Watson](https://en.wikipedia.org/wiki/Watson_(computer)) (of Jeopardy fame) goes through in order to answer questions posed in a natural language such as English.

Here I would like to examine the following question: is a natural language such as English really the best medium for Alice to specify her predictions and for (the tools that) Barbara, Carol and Diana (will use) to resolve them?

## Three desiderata for the representation of predictions

Let's consider what "best medium" might mean in the context of a global prediction market.

It is in Alice's interest (and in the interest of the entire prediction market and all its participants):

1. for her prediction to be described as precisely as possible. This means, for example, that if Alice posts a prediction about [Michael Jordan (the former basketball player)](https://en.wikipedia.org/wiki/Michael_Jordan) it should be clear that the prediction is not about [Michael Jordan (the UC Berkeley professor)](https://en.wikipedia.org/wiki/Michael_I._Jordan)
2. for her prediction to be understood by prediction market participants the world over in *their own* language (because this will increase the liquidity of the market by attracting a larger number of participants)
3. for her prediction to be resolved as quickly and as reliably as possible

I am going to go out on a limb here and make the claim that web ontologies (formally specified systems of concepts) such as [schema.org](http://schema.org) will turn out to be an *excellent* tool for stating predictions very precisely on prediction markets.

### Making predictions as precise and unambiguous as possible

Exibit one: here is how you can use [schema.org](http://schema.org) to embed in a regular HTML document a representation of the fact that, when on February 22, 2015 I visited Neo Technology's list of customers, I found TomTom listed among them (it is still there, I just checked):

```
<tr typeof="http://schema.org/BuyAction">
  <td property="http://schema.org/agent" href="http://dbpedia.org/resource/TomTom">
    <a href="http://www.tomtom.com/">TomTom</a>
  </td>
  <td property="http://schema.org/object" href="http://dbpedia.org/resource/Neo4j">
    <a href="http://neo4j.com">has been a paying user of Neo4J,</a>
  </td>
  <td property="http://schema.org/seller" href="http://dbpedia.org/resource/Neo_Technology">
    <a href="http://neo4j.com/company/">a product of Neo Technology,</a>
  </td>
  since at least
  <td property="http://schema.org/startTime">2015-02-22</td>
  <td property="http://purl.org/spar/cito/citesAsEvidence" href="http://neo4j.com/customers/">
    <a href="http://neo4j.com/customers/">as seen on Neo4J's list of customers as of 2015-02-22</a></td>
</tr>
```

This looks very complex, but it really boils down to the text you see when you render the HTML fragment above in a browser:

"TomTom has been a paying user of Neo4J, a product of Neo Technology, since at least 2015-02-22 as seen on Neo4J's list of customers as of 2015-02-22"

This may not be the most fluent prose in the world, but it is well formed English, the kind of English that [Watson](https://en.wikipedia.org/wiki/Watson_(computer)) can get to work on in order to figure out what kind of event is described and who are its participants.

The difference is that Barbara, Carol and Diana don't have to ask [Watson](https://en.wikipedia.org/wiki/Watson_(computer)) or any other natural language processing information extraction system to figure this out for them starting from an English expression: the event type and all its participants are spelled out in full detail and can be directly recovered from the HTML markup (which is mercifully invisible to humans using a browser).

If you want to inspect the definition of "being a paying customer", visit http://schema.org/BuyAction .

If you want to know who exactly is meant by "TomTom" (for all you know, my cousin could also be called "TomTom" and be a customer of Neo Technology; that would make resolving a prediction harder than it should be) visit http://dbpedia.org/resource/TomTom .

And so on, and so on.

So, in this new, more precise, representation medium (surface English enriched with web ontology markup), Alice's prediction

"By March 31 2015 Siemens will have become a customer of Neo Technology"

can be represented as

```
<tr typeof="http://schema.org/BuyAction">
  <td property="http://schema.org/agent" href="http://dbpedia.org/resource/Siemens">
    <a href="http://www.siemens.com/">Siemens</a>
  </td>
  <td property="http://schema.org/object" href="http://dbpedia.org/resource/Neo4j">
    <a href="http://neo4j.com">has been a paying user of Neo4J,</a>
  </td>
  <td property="http://schema.org/seller" href="http://dbpedia.org/resource/Neo_Technology">
    <a href="http://neo4j.com/company/">a product of Neo Technology,</a>
  </td>
  since at least
  <td property="http://schema.org/startTime">2015-03-31</td>
</tr>
```

(here I am admittedly abusing the notation because, as *startTime* is currently defined, the prediction above means that the commercial relationship between Siemens and Neo Technology started *exactly* on March 31, 2015. With a bit more sluething around [schema.org](http://schema.org) or other ontologies it should be relatively easy to find a fix).

This takes care of Alice's first requirement, that predictions be specified as precisely as possible.

Specifying things to this level of precision may not be possible for all predictions (there will certainly be some exotic predictions that cannot be easily represented using widely used ontology) but it is certainly worth doing (also for a number of additional reasons about which below) in all cases where it *can* be done. Very high quality consensus ontologies exist for people, companies, countries, products and many, many other things that are in the news. Some of them are developed by committed communities and maintained by organizations that have a clear business interest in their widespread use.

For example, [schema.org](http://schema.org) (the ontology I use in my markup examples above) is supported by Google, Microsoft, Yahoo and Yandex: it is my understanding that at least some of these search engines actively use the ontology markup in their indices.

The [International Press Telecommunications Council](https://iptc.org) a standards body of the news media actively maintains a [1100-term ontology of media topics](https://iptc.org/standards/media-topics/) and the New York Times has [its own ontology](http://data.nytimes.com) (for example, they refer to Siemens as http://data.nytimes.com/N82918236209763785922 ) which they use to index their [Times Topics page](http://www.nytimes.com/pages/topics/index.html).

Add to this [DBPedia](http://dbpedia.org) (which I use in my examples above), the extremely broad coverage ontology that is systematically harvested from [Wikipedia](http://wikipedia.org) (i.e. it grows as Wikipedia grows) and [OpenCyc](http://opencyc.org) which, to my knowledge, still has the most comprehensive and sophisticated inventory of event types.

So, in the spirit of prediction markets, I am going to predict that using the ontologies I list above it should be possible to represent at least 50% of the first 1000 predictions to appear on [Augur](http://www.augur.net).

Naturally, it would be unrealistic to expect for most market makers to be familiar with the ins-and-outs of these various ontologies in the way that a professional knowledge/ontology engineer would be. But here two things could (and, in my opinion *will*) happen.

The first one is that people will learn how to specify their predictions by inspecting well-formed predictions previously made by others; this is how many people learned HTML: by studying the source code of web pages they liked.

In the Siemens prediction example above it is obvious that the part of the markup you need to edit if you want to change deadline for your prediction is

```
<td property="http://schema.org/startTime">2015-03-31</td>
```

Similarly, if you want to make the prediction about Samsung instead of Siemens you will need to edit

```
  <td property="http://schema.org/agent" href="http://dbpedia.org/resource/Siemens">
```

And so on.

The second (in my opinion) likely development is that some charitable (or self-interested) soul will write a 'prediction editor', i.e. a very simple interface where people will type in their prediction in a natural language such as English and the tool will go out looking for the right ontology terms to map the prediction on. When I was working at [Cycorp](http://www.cyc.com) (1998-2004) I helped develop [a tool that worked exactly like that](http://www.researchgate.net/publication/228729261_Automated_OWL_annotation_assisted_by_a_large_knowledge_base) and I am quite confident that such tools have become a lot better since.

### Resolving prediction quickly and reliably

Moving on to Alice's third desideratum: predictions should be resolved as quickly and reliably as possible.

Let's assume for a moment that there will be someone out there (we'll see who in a second) who goes through the trouble of crawling the web, reading bits of news such as the hypothetical

"On March 28, 2015 Siemens announced that it deploying Neo4J through a contract serviced by Neo Technology"

into

```
<tr typeof="http://schema.org/BuyAction">
  On
  <td property="http://schema.org/startTime">2015-03-28</td>
  <td property="http://schema.org/agent" href="http://dbpedia.org/resource/Siemens">
    <a href="http://www.siemens.com/">Siemens</a>
  <td property="http://purl.org/spar/cito/citesAsEvidence" href="http://www.somenewsoutlet.com/pressreleases/neo4j_siemens_contract.html">
      <a href="http://www.somenewsoutlet.com/pressreleases/neo4j_siemens_contract.html">announced</a></td>
  </td>
  <td property="http://schema.org/object" href="http://dbpedia.org/resource/Neo4j">
    <a href="http://neo4j.com">it is deploying Neo4J</a>
  </td>
  <td property="http://schema.org/seller" href="http://dbpedia.org/resource/Neo_Technology">
    <a href="http://neo4j.com/company/">through a contract serviced by Neo Technology,</a>
  </td>
</tr>
```

and that someone (else) crawls the web and saves all these facts into a [Triplestore](https://en.wikipedia.org/wiki/Triplestore), that is a database specialized in storing facts represented in terms of these ontologies.

Just like databases can answer queries crafted in the [SQL](https://en.wikipedia.org/wiki/SQL) query language, triplestores can be queried using the [SPARQL](https://en.wikipedia.org/wiki/SPARQL) query language.

The following is the SPARQL query that retrieves all events in which Siemens is the buyer, Neo Technology is the seller and the sale took place before March 31, 2015:   

```
PREFIX schema: <http://schema.org/>
PREFIX dpr: <http://schema.org/resource/>
SELECT ?buy
{
  ?buy a schema:BuyAction .
  ?buy schema:seller dpr:Neo_Technology .
  ?buy schema:agent dpr:Siemens .
  ?buy schema:startTime ?time .
  FILTER(?time < "2015-03-31"^^xsd:date) .
}
```

A cursory inspection should convince you that it is possible to generate this query automatically from Alice's *formal* version of her Siemens/Neo Technology prediction (it's just a matter of mechanically mapping certain bits of the prediction into certain bits of the query).

So, *if* Alice goes through the trouble of formally representing her prediction using an appropriate ontology *and* someone (else) goes through the trouble of analyzing newsfeeds and storing the resulting formally represented facts into a triplestore, *then* querying the triplestore will *immediately* resolve the query.

That seems a pretty tall order: who could possibly go through the trouble of analyzing newsfeeds and storing the resulting facts in a triplestore?

Companies or research teams such as the [BBC](http://www.bbc.co.uk/blogs/internet/entries/afdf2190-4e60-3dfc-b15f-fc17f88c85a1), [Ontotext](http://ontotext.com/semantic-solutions/dynamic-semantic-publishing-platform/semantic-news-publishing/) or [EventRegistry](http://eventregistry.org), that's who.

In other words, entities that have independent (business) reasons for wanting to keep track of things that happen in a structured format.

This is the first order approximation of how things could work.

There will of course be difficulties (the query above will not resolve against a triplestore where the relevant event has been encoded using an alternative ontology) but also enormous promise: the query can be resolved not by direct lookup but by *inference* , i.e. by establishing a chain of facts that *jointly* *imply* that Siemens has become a customer of Neo Technology. This sounds a bit vague and optimistic. But if your prediction is about pharmacological development, you will appreciate the power of inference deployed over an [open data pharmacological triple store that contains 10 billion facts](http://linkedlifedata.com).

In the most optimistic scenario, the use of ontologies in the representation of predictions in prediction markets could ignite a positive feedback loop: the existence of certain questions on prediction markets causes news publishers or news intermediaries to publish/extract a formal representation of news about those events. This makes predictions easier, cheaper, faster to resolve, encouraging the creation of new predictions and so on, and so on...

### Multilingual representation from formally specified predictions

A few paragraphs above I mentioned my experience in working on tools that take as input natural language expressions and outputs structured representations of the same.

It should come as no surprise that it is equally possible to take as input structured representations of facts or events expressed in a formal language/ontology and [produce a natural language representation](https://www.jair.org/media/4017/live-4017-7471-jair.pdf), e.g. an English sentence.

And this is where things get interesting for Alice's second desideratum: making predictions multilingual.

You can think of the sentence

"On March 28, 2015 Siemens bought a service contract for Neo4J from Neo Technology"

as the English language 'skin' of the underlying fact

```
<tr typeof="http://schema.org/BuyAction">
  <td property="http://schema.org/startTime">2015-03-28</td>
  <td property="http://schema.org/agent" href="http://dbpedia.org/resource/Siemens"></td>
  <td property="http://schema.org/object" href="http://dbpedia.org/resource/Neo4j"></td>
  <td property="http://schema.org/seller" href="http://dbpedia.org/resource/Neo_Technology"></td>
</tr>
```

If you have a process to generate the English 'skin' from the underlying fact, there is in principle no reason why you couldn't generate from the same fact a Chinese or Hindi or Spanish 'skin' (indeed, this should probably be tackled as a rather nicely constrained problem in machine translation, as done [here](https://sites.google.com/site/spedialproject/)).

And that's what finally closes the circle:

1. Alice writes down in her own language (say, English) the prediction she wants to publish on the prediction market
2. Alice produces a *formal* representation of her prediction (by tweaking the formal representation of someone else's prediction or using a natural language enabled ontology editor)
3. Alice uses a natural language generation module to 'skin' the formal representation of her prediction in as many other languages as possible (say Chinese, Hindi and Spanish)
4. the prediction now become understandable/accessible to speakers of Chinese, Hindi, Spanish that would not have been able to trade on the English-only prediction.
5. the market becomes more liquid
6. the market as a whole becomes better at tracking the probability of events happening

To repeat, the entire scheme of enriching predictions with formal, ontology based, representations is certainly not a pre-requisite for the functioning of prediction markets and need not be an all-or-nothing kind of thing (natural language predictions could peacefully co-exist with formally specified predictions on a prediction markets).

That said, the fairly predictable nature of many predictions (elections, mergers and acquisitions, military events, ...) is such that even a modest investment in formalizing them would go a long way, particularly when the interests of prediction markets in these matters are so very clearly aligned with those of search engines and news organizations.

The question/query remains

```
PREFIX schema: <http://schema.org/>
PREFIX dpr: <http://dbpedia.org/resource/>
SELECT ?time
{
  ?build a schema:CreateAction .
  ?build schema:result dpr:Ontology_editor .
  ?build schema:agent dpr:Augur_(software) .
  ?build schema:startTime ?time .
}
```

i.e. when will [Augur](http://www.augur.net) manage to do (at least part of) this?
