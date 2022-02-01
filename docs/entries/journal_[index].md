## [Tital Goes Here] - Journal Entry Template 
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>


### Sub-Heading

This is a template for journal entries to help keep consistency!  
[Page content goes here]

<br />

### Cites
##### All citations are available in a single [bibtex file](../references.bib)

<p id="c1">
[1] Author, “Title,” [Online]. Available: URL, [Accessed: day Month Year] 
</p>

<br />
<br />

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

<br />
<br />

**Please refer to the [Licences and Sources](../resources/licences-and-sources) document for content used from external sources along with usage and licence infomation**

<br />

<script>
// Store the entry id and published values in a JS script, to make life easier with updateing links.
entry_id  = -1
published = "dd-mm-yy" 
week = -1

document.getElementById("index").innerHTML = entry_id
document.getElementById("published").innerHTML   = `${published} (Week: ${week})`


next_page = "journal_"+ (entry_id + 1)
priv_page = "journal_"+ (entry_id - 1)

next_links = document.getElementsByClassName("next_entry")
priv_links = document.getElementsByClassName("priv_entry")

// atempt to fetch the next page. 
// if we get an ok responce display the next links, 
// otherwise we have most likely reaced the end.
fetch('./'+next_page+'.html')
    .then (
        responce => {
        if ( responce.ok ) 
            for ( let i in next_links )
                next_links[i].innerHTML = '<a href="./'+next_page+'">Next ></a>'
        }
    )

// only display the priv page link if we have gone past the first page.
// theres no need to fetch the prv page, since we know the min id is 0
if (entry_id > 0)
    for ( let i in priv_links )
        priv_links[i].innerHTML = '<a href="./'+priv_page+'">< Priv</a>'


</script>