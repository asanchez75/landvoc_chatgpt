# Few-shot training to teach ChatGPT how to build SPARQL queries from natural language questions against LandVoc

The methodology is described here by a collection of [prompts](https://htmlpreview.github.io/?https://raw.githubusercontent.com/asanchez75/landvoc_chatgpt/main/landvoc.html).
I had to shrink the LandVoc taxonomy to its english version in order to deal with the size limitations of context window. The trimming process of the taxonomy is described [here](https://htmlpreview.github.io/?https://github.com/asanchez75/landvoc_chatgpt/blob/main/trimming_landvoc.html)

The question in natural language was `which are all the direct subjects related with the term "land"?`
and the query generated by ChatGPT was

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX skosxl: <http://www.w3.org/2008/05/skos-xl#>

SELECT DISTINCT ?relatedConcept ?label
WHERE {
  ?concept a skos:Concept .
  ?concept skosxl:prefLabel/skosxl:literalForm ?landLabel .
  FILTER(lang(?landLabel) = 'en' && STR(?landLabel) = "land") 

  {
    ?concept skos:broader ?relatedConcept .
  } UNION {
    ?concept skos:narrower ?relatedConcept .
  }

  OPTIONAL { 
    ?relatedConcept skosxl:prefLabel/skosxl:literalForm ?label .
    FILTER(lang(?label) = 'en') 
  }
}

```

To verify the correcteness of the query, you can test it in the SPARQL endpoint provided by AGROVOC.

Just paste the above query here:

https://agrovoc.fao.org/sparql


![AGROVOC-SPARQLEndpoint](https://github.com/asanchez75/landvoc_chatgpt/assets/383566/043e1e5a-d50f-4b3a-b1ea-7ea033c09082)
